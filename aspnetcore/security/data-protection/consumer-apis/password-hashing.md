---
title: 在 ASP.NET Core 雜湊密碼
author: rick-anderson
description: 了解如何使用 ASP.NET Core 資料保護 Api 的密碼雜湊。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: aef22ab91e76afdb5f54dc37bcee7128420b6f3b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272983"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="cf9f8-103">在 ASP.NET Core 雜湊密碼</span><span class="sxs-lookup"><span data-stu-id="cf9f8-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="cf9f8-104">資料保護程式碼基底包含封裝*Microsoft.AspNetCore.Cryptography.KeyDerivation*包含密碼編譯金鑰衍生函式。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="cf9f8-105">此套件是獨立的元件且沒有任何相依性的資料保護系統的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="cf9f8-106">它可以完全獨立使用。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-106">It can be used completely independently.</span></span> <span data-ttu-id="cf9f8-107">其中存在著來源與資料保護程式碼基底，為了方便起見。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="cf9f8-108">封裝目前提供的方法，`KeyDerivation.Pbkdf2`讓雜湊密碼使用[PBKDF2 演算法](https://tools.ietf.org/html/rfc2898#section-5.2)。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="cf9f8-109">這個 API 已非常類似現有的.NET Framework [Rfc2898DeriveBytes 類型](/dotnet/api/system.security.cryptography.rfc2898derivebytes)，但是有三個重要的區別：</span><span class="sxs-lookup"><span data-stu-id="cf9f8-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="cf9f8-110">`KeyDerivation.Pbkdf2`方法支援使用多個 PRFs (目前`HMACSHA1`， `HMACSHA256`，和`HMACSHA512`)，而`Rfc2898DeriveBytes`類型只支援`HMACSHA1`。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="cf9f8-111">`KeyDerivation.Pbkdf2`方法偵測到目前的作業系統，並嘗試選擇最最佳化的常式，並提供較佳的效能，在某些情況下實作。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="cf9f8-112">(在 Windows 8 上，它提供了大約 10 倍的輸送量`Rfc2898DeriveBytes`。)</span><span class="sxs-lookup"><span data-stu-id="cf9f8-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="cf9f8-113">`KeyDerivation.Pbkdf2`方法要求呼叫端指定所有參數 (salt，PRF 和反覆項目計數)。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="cf9f8-114">`Rfc2898DeriveBytes`針對這些類型提供的預設值。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="cf9f8-115">ASP.NET Core 身分識別的請參閱原始程式碼`PasswordHasher`真實世界的類型使用的大小寫。</span><span class="sxs-lookup"><span data-stu-id="cf9f8-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
