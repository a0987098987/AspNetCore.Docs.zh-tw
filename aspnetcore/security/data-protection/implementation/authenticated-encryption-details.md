---
title: ASP.NET Core 中已驗證的加密詳細資料
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護已驗證加密的執行詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 9def03e6b27e19fc34a839e923d6152e086889db
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667759"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a><span data-ttu-id="9bbc9-103">ASP.NET Core 中已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="9bbc9-103">Authenticated encryption details in ASP.NET Core</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="9bbc9-104">Idataprotector 加密的呼叫是已驗證的加密作業。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="9bbc9-105">保護方法提供機密性和真實性，並系結至用來從根 IDataProtectionProvider 衍生這個特定 Idataprotector 加密實例的目的鏈。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-105">The Protect method offers both confidentiality and authenticity, and it's tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="9bbc9-106">Idataprotector 加密。保護採用 byte [] 純文字參數，並產生 byte [] 受保護的承載，其格式如下所述。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="9bbc9-107">（還有一個擴充方法多載，它採用字串純文字參數，並傳回字串保護的裝載。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-107">(There's also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="9bbc9-108">如果使用此 API，受保護的承載格式仍然會有下列結構，但它將會[base64url 編碼](https://tools.ietf.org/html/rfc4648#section-5)。）</span><span class="sxs-lookup"><span data-stu-id="9bbc9-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="9bbc9-109">受保護的承載格式</span><span class="sxs-lookup"><span data-stu-id="9bbc9-109">Protected payload format</span></span>

<span data-ttu-id="9bbc9-110">受保護的承載格式是由三個主要元件所組成：</span><span class="sxs-lookup"><span data-stu-id="9bbc9-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="9bbc9-111">32位的神奇標頭，可識別資料保護系統的版本。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="9bbc9-112">128位金鑰識別碼，用來識別用來保護此特定裝載的金鑰。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="9bbc9-113">受保護承載的其餘部分是[此金鑰所封裝之加密](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)程式的特定。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="9bbc9-114">在下列範例中，金鑰代表 AES-256-CBC + HMACSHA256 加密程式，而裝載會進一步細分，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9bbc9-114">In the example below, the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows:</span></span>
  * <span data-ttu-id="9bbc9-115">128位金鑰修飾詞。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-115">A 128-bit key modifier.</span></span>
  * <span data-ttu-id="9bbc9-116">128位初始化向量。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-116">A 128-bit initialization vector.</span></span>
  * <span data-ttu-id="9bbc9-117">48個位元組的 AES-256-CBC 輸出。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-117">48 bytes of AES-256-CBC output.</span></span>
  * <span data-ttu-id="9bbc9-118">HMACSHA256 authentication 標記。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-118">An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="9bbc9-119">範例受保護的承載如下所示。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-119">A sample protected payload is illustrated below.</span></span>

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

<span data-ttu-id="9bbc9-120">從高於前32位的承載格式，或4個位元組是識別版本的魔術標頭（09 F0 C9 F0）</span><span class="sxs-lookup"><span data-stu-id="9bbc9-120">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="9bbc9-121">下一個128位或16個位元組是金鑰識別碼（80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57）</span><span class="sxs-lookup"><span data-stu-id="9bbc9-121">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="9bbc9-122">餘數包含裝載，且為所用格式的特定內容。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-122">The remainder contains the payload and is specific to the format used.</span></span>

> [!WARNING]
> <span data-ttu-id="9bbc9-123">受指定金鑰保護的所有承載都將以相同的20位元組（魔術值、金鑰識別碼）標頭開頭。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-123">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="9bbc9-124">系統管理員可以使用此事實來進行診斷，以估計承載的產生時間。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-124">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="9bbc9-125">例如，上述裝載對應于 key {0c819c80-6619-4019-9536-53f8aaffee57}。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-125">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="9bbc9-126">如果在檢查金鑰存放庫之後，發現此特定金鑰的啟用日期為2015-01-01，而其到期日為2015-03-01，則合理地假設該視窗內產生的承載（如果未遭修改），請提供或接受小的任一端的巧克力因素。</span><span class="sxs-lookup"><span data-stu-id="9bbc9-126">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it's reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
