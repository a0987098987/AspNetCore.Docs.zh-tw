---
title: "已驗證的加密的詳細資料。"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 826e6d5d-9620-44e6-ad93-3b1d9969b70b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 146b1cb197766f02bb75559c44b24686898ed4d9
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="authenticated-encryption-details"></a><span data-ttu-id="d2725-103">已驗證的加密的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d2725-103">Authenticated encryption details.</span></span>

<a name=data-protection-implementation-authenticated-encryption-details></a>

<span data-ttu-id="d2725-104">呼叫 IDataProtector.Protect 都已驗證的加密作業。</span><span class="sxs-lookup"><span data-stu-id="d2725-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="d2725-105">保護方法提供機密性和驗證，以及它會連結到目的鏈結，用於從其根 IDataProtectionProvider 衍生此特定 IDataProtector 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d2725-105">The Protect method offers both confidentiality and authenticity, and it is tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="d2725-106">IDataProtector.Protect 會採用 byte [] 的純文字參數，並產生的位元組 [受保護的內容，其格式如下所述。</span><span class="sxs-lookup"><span data-stu-id="d2725-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="d2725-107">（此外還有擴充方法多載會接受字串參數的純文字，並傳回字串的受保護的內容。</span><span class="sxs-lookup"><span data-stu-id="d2725-107">(There is also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="d2725-108">如果單獨使用此應用程式開發介面，還是會有受保護的裝載格式下方結構，但實際上會[base64url 編碼](https://tools.ietf.org/html/rfc4648#section-5)。)</span><span class="sxs-lookup"><span data-stu-id="d2725-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="d2725-109">受保護的裝載格式</span><span class="sxs-lookup"><span data-stu-id="d2725-109">Protected payload format</span></span>

<span data-ttu-id="d2725-110">受保護的裝載格式包含三個主要元件：</span><span class="sxs-lookup"><span data-stu-id="d2725-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="d2725-111">32 位元 magic 的標頭會識別資料保護系統的版本。</span><span class="sxs-lookup"><span data-stu-id="d2725-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="d2725-112">128 位元金鑰的識別碼，可識別用來保護此特定內容的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d2725-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="d2725-113">受保護內容的其餘部分是[特有加密此金鑰由封裝程式](subkeyderivation.md#data-protection-implementation-subkey-derivation)。</span><span class="sxs-lookup"><span data-stu-id="d2725-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="d2725-114">在下列範例中的索引鍵所代表的 256 位 AES-CBC + HMACSHA256 加密程式，並裝載進一步細分，如下所示: * 為 128 位元金鑰的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="d2725-114">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: * A 128-bit key modifier.</span></span> <span data-ttu-id="d2725-115">* 128 位元的初始化向量。</span><span class="sxs-lookup"><span data-stu-id="d2725-115">* A 128-bit initialization vector.</span></span> <span data-ttu-id="d2725-116">* 256-AES-CBC 輸出 48 個位元組。</span><span class="sxs-lookup"><span data-stu-id="d2725-116">* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="d2725-117">* HMACSHA256 驗證的標記。</span><span class="sxs-lookup"><span data-stu-id="d2725-117">* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="d2725-118">下圖說明範例受保護的內容。</span><span class="sxs-lookup"><span data-stu-id="d2725-118">A sample protected payload is illustrated below.</span></span>

<!-- literal_block {"xml:space": "preserve", "source": "security/data-protection/implementation/authenticated-encryption-details/_static/protectedpayload.txt", "ids": [], "linenos": true, "highlight_args": {"linenostart": 1}} -->

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

<span data-ttu-id="d2725-119">從上面的前 32 個位元或 4 個位元組的裝載格式會使用識別的版本 (09 F0 C9 F0) magic 標頭</span><span class="sxs-lookup"><span data-stu-id="d2725-119">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="d2725-120">接下來的 128 位元或 16 個位元組是金鑰識別元 (80 9 C 81 0 C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="d2725-120">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="d2725-121">其餘部分包含裝載，並屬於所使用的格式。</span><span class="sxs-lookup"><span data-stu-id="d2725-121">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="d2725-122">保護指定的索引鍵的所有裝載的都開頭是相同的 20 個位元組 （magic 值，索引鍵識別碼） 標頭。</span><span class="sxs-lookup"><span data-stu-id="d2725-122">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="d2725-123">管理員可以使用這項事實進行診斷，接近產生內容時。</span><span class="sxs-lookup"><span data-stu-id="d2725-123">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="d2725-124">例如，上述內容對應至索引鍵 {0c819c80-6619-4019-9536-53f8aaffee57}。</span><span class="sxs-lookup"><span data-stu-id="d2725-124">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="d2725-125">如果您在檢查索引鍵的儲存機制之後發現此特定機碼啟用日期 2015年-01-01，和其到期日已 2015年-03-01，則很合理假設的裝載 （如果未遭竄改） 產生在該視窗中，提供或需要一個小型是任一邊的比例。</span><span class="sxs-lookup"><span data-stu-id="d2725-125">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it is reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
