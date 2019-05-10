---
title: ASP.NET Core 中的已驗證的加密詳細資料
author: rick-anderson
description: 了解 ASP.NET Core 資料保護驗證加密的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 9def03e6b27e19fc34a839e923d6152e086889db
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896625"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a><span data-ttu-id="d3ff9-103">ASP.NET Core 中的已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="d3ff9-103">Authenticated encryption details in ASP.NET Core</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="d3ff9-104">呼叫 IDataProtector.Protect 是已驗證的加密作業。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="d3ff9-105">保護方法提供機密性和真實性，以及它會連結到目的鏈結，用來從其根 IDataProtectionProvider 衍生這個特定的 IDataProtector 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-105">The Protect method offers both confidentiality and authenticity, and it's tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="d3ff9-106">IDataProtector.Protect 採用 byte [] 的純文字參數，並且產生 byte [] 受保護的承載，其格式如下所述。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="d3ff9-107">（還有它採用純文字字串參數，並傳回字串的受保護的承載的擴充方法多載。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-107">(There's also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="d3ff9-108">如果使用此 API 的受保護的裝載格式仍然會以下結構，但將予以[base64url 編碼](https://tools.ietf.org/html/rfc4648#section-5)。)</span><span class="sxs-lookup"><span data-stu-id="d3ff9-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="d3ff9-109">受保護的裝載格式</span><span class="sxs-lookup"><span data-stu-id="d3ff9-109">Protected payload format</span></span>

<span data-ttu-id="d3ff9-110">受保護的承載的格式是由三個主要元件所組成：</span><span class="sxs-lookup"><span data-stu-id="d3ff9-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="d3ff9-111">32 位元 magic 的標頭會識別資料保護系統的版本。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="d3ff9-112">128 位元金鑰的識別碼，可識別用來保護此特定的承載的金鑰。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="d3ff9-113">受保護承載的其餘部分[封裝此金鑰的加密程式的特定](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="d3ff9-114">在下列範例中，索引鍵所代表的 AES-256-CBC + HMACSHA256 加密程式，並裝載進一步細分，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d3ff9-114">In the example below, the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows:</span></span>
  * <span data-ttu-id="d3ff9-115">128 位元金鑰的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-115">A 128-bit key modifier.</span></span>
  * <span data-ttu-id="d3ff9-116">128 位元初始向量。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-116">A 128-bit initialization vector.</span></span>
  * <span data-ttu-id="d3ff9-117">AES-256-CBC 輸出 48 個位元組。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-117">48 bytes of AES-256-CBC output.</span></span>
  * <span data-ttu-id="d3ff9-118">HMACSHA256 驗證標記中。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-118">An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="d3ff9-119">範例的受保護的承載如下所示。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-119">A sample protected payload is illustrated below.</span></span>

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

<span data-ttu-id="d3ff9-120">從上面的前 32 個位元或 4 個位元組的承載格式所識別的版本 (09 F0 C9 F0) magic 標頭</span><span class="sxs-lookup"><span data-stu-id="d3ff9-120">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="d3ff9-121">接下來的 128 位元或 16 個位元組是金鑰的識別碼 (80 9 C 81 0c 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="d3ff9-121">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="d3ff9-122">其餘部分包含裝載，並且專屬於使用的格式。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-122">The remainder contains the payload and is specific to the format used.</span></span>

> [!WARNING]
> <span data-ttu-id="d3ff9-123">保護指定的索引鍵的所有裝載會使用相同的 20 個位元組 （magic 的值，金鑰識別碼） 標頭都開始。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-123">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="d3ff9-124">管理員可以使用這項事實，供診斷之用來模擬時所產生的裝載。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-124">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="d3ff9-125">例如，上述承載會對應至索引鍵 {0c819c80-6619-4019-9536-53f8aaffee57}。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-125">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="d3ff9-126">如果您在檢查索引鍵的存放庫之後發現此特定索引鍵的啟動日期是 2015年-01-01，和它的到期日是 2015年-03-01，則很合理假設承載 （如果未遭竄改） 產生該視窗中，提供或採取一個小型任一端餘裕。</span><span class="sxs-lookup"><span data-stu-id="d3ff9-126">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it's reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
