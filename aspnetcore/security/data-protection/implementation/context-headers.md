---
title: ASP.NET Core 中的內容標頭
author: rick-anderson
description: 了解 ASP.NET Core 資料保護的內容標頭的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 518423f5df93924d3df144994e4beb1755cd0bfc
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814020"
---
# <a name="context-headers-in-aspnet-core"></a><span data-ttu-id="4551d-103">ASP.NET Core 中的內容標頭</span><span class="sxs-lookup"><span data-stu-id="4551d-103">Context headers in ASP.NET Core</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="4551d-104">背景和理論</span><span class="sxs-lookup"><span data-stu-id="4551d-104">Background and theory</span></span>

<span data-ttu-id="4551d-105">在資料保護系統中，「 金鑰 」 表示的物件，可提供驗證加密服務。</span><span class="sxs-lookup"><span data-stu-id="4551d-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="4551d-106">每個索引鍵由唯一識別碼 (GUID)，但是會有該演算法的資訊和 entropic 材料。</span><span class="sxs-lookup"><span data-stu-id="4551d-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="4551d-107">其目的，每個索引鍵包含唯一的 entropy，系統無法強制執行，但我們也要負責開發人員可能會藉由修改金鑰環中的現有金鑰的演算法資訊手動變更金鑰環。</span><span class="sxs-lookup"><span data-stu-id="4551d-107">It's intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="4551d-108">若要達到這種情況下我們的安全性需求的資料保護系統具有的概念[密碼編譯靈活性](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/)，以便安全地跨多個密碼編譯演算法使用單一的 entropic 值。</span><span class="sxs-lookup"><span data-stu-id="4551d-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="4551d-109">大部分的系統支援密碼編譯靈活性的作法是包括一些有關在承載內部之演算法的識別資訊。</span><span class="sxs-lookup"><span data-stu-id="4551d-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="4551d-110">此演算法的 OID 通常是這個的不錯候選項目。</span><span class="sxs-lookup"><span data-stu-id="4551d-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="4551d-111">不過，我們遇到的問題之一是，有多種方式可以指定相同的演算法：「 AES 」 (CNG) 的受管理 Aes、 AesManaged，AesCryptoServiceProvider、 AesCng 和 RijndaelManaged （指定的特定參數） 類別是所有實際相同的作業，而我們需要會維護所有個以正確的 OID 的對應。</span><span class="sxs-lookup"><span data-stu-id="4551d-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="4551d-112">如果開發人員想要提供自訂的演算法 （或甚至是另一個實作 AES ！），他們就不必告訴我們 (OID)。</span><span class="sxs-lookup"><span data-stu-id="4551d-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="4551d-113">這個額外的註冊步驟可讓系統組態特別麻煩。</span><span class="sxs-lookup"><span data-stu-id="4551d-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="4551d-114">回顧一下，我們決定我們已從錯誤方向接近問題。</span><span class="sxs-lookup"><span data-stu-id="4551d-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="4551d-115">OID 會告訴您功能演算法，但我們不真正關心這。</span><span class="sxs-lookup"><span data-stu-id="4551d-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="4551d-116">如果我們需要在兩個不同的演算法中安全地使用單一 entropic 值，它不需要讓我們知道演算法實際上是。</span><span class="sxs-lookup"><span data-stu-id="4551d-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="4551d-117">什麼我們真正關心是它們的行為。</span><span class="sxs-lookup"><span data-stu-id="4551d-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="4551d-118">任何適當的對稱區塊加密演算法也是強式的虛擬隨機排列 (PRP): 修正 （索引鍵，鏈結模式中，IV，純文字） 的輸入，然後加密文字輸出會與讀者的機率是不同於任何其他的對稱區塊編碼器指定輸入的相同的演算法。</span><span class="sxs-lookup"><span data-stu-id="4551d-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="4551d-119">同樣地，任何適當的索引雜湊函式也是強式的虛擬隨機函式 (PRF)，和指定的一組固定的輸入範例的輸出結果十分會不同於其他任何索引鍵的雜湊函式。</span><span class="sxs-lookup"><span data-stu-id="4551d-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="4551d-120">我們可以使用強式 Prp 和 PRFs 此概念來建置的內容標頭。</span><span class="sxs-lookup"><span data-stu-id="4551d-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="4551d-121">此內容標頭實質上會當做穩定指紋透過在任何指定的作業中，使用的演算法，並提供所需的資料保護系統密碼編譯靈活性。</span><span class="sxs-lookup"><span data-stu-id="4551d-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="4551d-122">此標頭是可重現，並稍後會使用一部分[子機碼衍生的程序](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)。</span><span class="sxs-lookup"><span data-stu-id="4551d-122">This header is reproducible and is used later as part of the [subkey derivation process](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="4551d-123">有兩種不同的方式，來建置內容標頭，根據的作業模式的基礎演算法。</span><span class="sxs-lookup"><span data-stu-id="4551d-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="4551d-124">CBC 模式加密 + HMAC 驗證</span><span class="sxs-lookup"><span data-stu-id="4551d-124">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="4551d-125">內容標頭是由下列元件所組成：</span><span class="sxs-lookup"><span data-stu-id="4551d-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="4551d-126">[16 位元]值為 00 00，也就是使用標記表示 「 CBC 加密 + HMAC 驗證 」。</span><span class="sxs-lookup"><span data-stu-id="4551d-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="4551d-127">[32 位元]（以位元組為單位，big endian） 的對稱區塊加密演算法金鑰長度。</span><span class="sxs-lookup"><span data-stu-id="4551d-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="4551d-128">[32 位元]對稱區塊加密演算法的區塊大小 （以位元組為單位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="4551d-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="4551d-129">[32 位元]HMAC 演算法金鑰長度，（以位元組為單位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="4551d-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="4551d-130">（目前的金鑰大小一律符合摘要大小。）</span><span class="sxs-lookup"><span data-stu-id="4551d-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="4551d-131">[32 位元]HMAC 演算法的摘要大小 （以位元組為單位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="4551d-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="4551d-132">EncCBC (K_E、 IV，"")，這是指定為空字串輸入的對稱區塊加密演算法的輸出，並且其中 IV 是全 0 的向量。</span><span class="sxs-lookup"><span data-stu-id="4551d-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="4551d-133">以下說明 K_E 建構。</span><span class="sxs-lookup"><span data-stu-id="4551d-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="4551d-134">MAC (K_H，"")，這是根據輸入空字串的 HMAC 演算法的輸出。</span><span class="sxs-lookup"><span data-stu-id="4551d-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="4551d-135">以下說明 K_H 建構。</span><span class="sxs-lookup"><span data-stu-id="4551d-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="4551d-136">在理想情況下，我們可以 K_E 和 K_H 來傳遞所有零的向量。</span><span class="sxs-lookup"><span data-stu-id="4551d-136">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="4551d-137">不過，我們想要避免這種情況其中的基礎演算法檢查存在的弱式金鑰再執行任何作業 （尤其是 DES 和 3DES），無法讓您使用簡單或可重複的模式，例如全 0 的向量。</span><span class="sxs-lookup"><span data-stu-id="4551d-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="4551d-138">相反地，我們使用 NIST SP800 108 KDF 計數器模式中 (請參閱[NIST SP800 108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，sec 5.1) 使用長度為零的索引鍵、 標籤和內容和為基礎的 PRF HMACSHA512。</span><span class="sxs-lookup"><span data-stu-id="4551d-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="4551d-139">我們 |K_E |+ |K_H |位元組的輸出，然後分解成結果 K_E 和 K_H 本身。</span><span class="sxs-lookup"><span data-stu-id="4551d-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="4551d-140">以數學方式，這表示，如下所示。</span><span class="sxs-lookup"><span data-stu-id="4551d-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="4551d-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span><span class="sxs-lookup"><span data-stu-id="4551d-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="4551d-142">範例：AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="4551d-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="4551d-143">例如，請考慮對稱區塊加密演算法是 AES-192-CBC，並驗證演算法是 HMACSHA256 的情況。</span><span class="sxs-lookup"><span data-stu-id="4551d-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="4551d-144">系統會產生內容標頭使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="4551d-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="4551d-145">首先，讓 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 192 位元和 |K_H |= 每個指定的演算法的 256 位元。</span><span class="sxs-lookup"><span data-stu-id="4551d-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="4551d-146">這會導致 K_E = 5BB6...21DD 和 K_H = A04A...在下列範例中的 00A9:</span><span class="sxs-lookup"><span data-stu-id="4551d-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="4551d-147">接下來，計算 Enc_CBC (K_E、 IV，"") AES-192-CBC 指定 IV = 0 \* 和 K_E 如上所述。</span><span class="sxs-lookup"><span data-stu-id="4551d-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="4551d-148">result := F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="4551d-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="4551d-149">接下來，計算 MAC (K_H，"") 為上述指定 K_H HMACSHA256。</span><span class="sxs-lookup"><span data-stu-id="4551d-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="4551d-150">結果: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="4551d-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="4551d-151">這會產生下列完整的內容標頭：</span><span class="sxs-lookup"><span data-stu-id="4551d-151">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="4551d-152">此內容標頭是 （AES 192 CBC 加密 + HMACSHA256 驗證），才會進行驗證的加密演算法組指紋。</span><span class="sxs-lookup"><span data-stu-id="4551d-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="4551d-153">如所述的元件[上述](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components)是：</span><span class="sxs-lookup"><span data-stu-id="4551d-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="4551d-154">標記 (00 00)</span><span class="sxs-lookup"><span data-stu-id="4551d-154">the marker (00 00)</span></span>

* <span data-ttu-id="4551d-155">區塊加密金鑰長度 (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="4551d-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="4551d-156">區塊密碼區塊大小 (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="4551d-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="4551d-157">HMAC 金鑰長度 (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="4551d-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="4551d-158">HMAC 摘要大小 (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="4551d-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="4551d-159">區塊編碼器 PRP 輸出 (F4 74-DB 6F) 和</span><span class="sxs-lookup"><span data-stu-id="4551d-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="4551d-160">HMAC PRF 輸出 (D4 79-結束)。</span><span class="sxs-lookup"><span data-stu-id="4551d-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="4551d-161">CBC 模式加密 + HMAC 驗證內容標頭是相同的方式，不論是否演算法實作由提供 Windows CNG 或由受管理的 SymmetricAlgorithm 和 KeyedHashAlgorithm 型別。</span><span class="sxs-lookup"><span data-stu-id="4551d-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="4551d-162">這可讓在不同的作業系統上執行的應用程式可靠地產生相同的內容標頭，即使演算法的實作不同的作業系統。</span><span class="sxs-lookup"><span data-stu-id="4551d-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="4551d-163">（實際上，KeyedHashAlgorithm 不一定要適當的 HMAC。</span><span class="sxs-lookup"><span data-stu-id="4551d-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="4551d-164">它可以是任何索引鍵的雜湊演算法類型）。</span><span class="sxs-lookup"><span data-stu-id="4551d-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="4551d-165">範例：3DES-192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="4551d-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="4551d-166">首先，讓 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 192 位元和 |K_H |= 每個指定的演算法的 160 位元。</span><span class="sxs-lookup"><span data-stu-id="4551d-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="4551d-167">這會導致 K_E = A219...E2BB 和 K_H = DC4A...在下列範例中的 B464:</span><span class="sxs-lookup"><span data-stu-id="4551d-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="4551d-168">接下來，計算 Enc_CBC (K_E、 IV，"") 3DES-192-CBC 指定 IV = 0 \* 和 K_E 如上所述。</span><span class="sxs-lookup"><span data-stu-id="4551d-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="4551d-169">結果: = ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="4551d-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="4551d-170">接下來，計算 MAC (K_H，"") 為上述指定 K_H HMACSHA1。</span><span class="sxs-lookup"><span data-stu-id="4551d-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="4551d-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="4551d-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="4551d-172">這會產生所驗證的憑證指紋的完整內容標頭加密演算法組 （3DES-192-CBC 加密 + HMACSHA1 驗證），如下所示：</span><span class="sxs-lookup"><span data-stu-id="4551d-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="4551d-173">元件，如下所示分解：</span><span class="sxs-lookup"><span data-stu-id="4551d-173">The components break down as follows:</span></span>

* <span data-ttu-id="4551d-174">標記 (00 00)</span><span class="sxs-lookup"><span data-stu-id="4551d-174">the marker (00 00)</span></span>

* <span data-ttu-id="4551d-175">區塊加密金鑰長度 (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="4551d-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="4551d-176">區塊密碼區塊大小 (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="4551d-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="4551d-177">HMAC 金鑰長度 (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="4551d-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="4551d-178">HMAC 摘要大小 (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="4551d-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="4551d-179">區塊編碼器 PRP 輸出 (AB B1-E1 0E) 和</span><span class="sxs-lookup"><span data-stu-id="4551d-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="4551d-180">HMAC PRF 輸出 (76 EB-結束)。</span><span class="sxs-lookup"><span data-stu-id="4551d-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="4551d-181">Galois/計數器模式加密 + 驗證</span><span class="sxs-lookup"><span data-stu-id="4551d-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="4551d-182">內容標頭是由下列元件所組成：</span><span class="sxs-lookup"><span data-stu-id="4551d-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="4551d-183">[16 位元]值為 00 01，也就是使用標記表示 [GCM 加密 + 驗證]。</span><span class="sxs-lookup"><span data-stu-id="4551d-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="4551d-184">[32 位元]（以位元組為單位，big endian） 的對稱區塊加密演算法金鑰長度。</span><span class="sxs-lookup"><span data-stu-id="4551d-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="4551d-185">[32 位元]（以位元組為單位，big endian） 要驗證的加密作業期間使用 nonce 的大小。</span><span class="sxs-lookup"><span data-stu-id="4551d-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="4551d-186">(如我們的系統，這固定在 nonce 大小 = 96 位元。)</span><span class="sxs-lookup"><span data-stu-id="4551d-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="4551d-187">[32 位元]對稱區塊加密演算法的區塊大小 （以位元組為單位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="4551d-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="4551d-188">(適用於 GCM，這會固定區塊大小 = 128 位元。)</span><span class="sxs-lookup"><span data-stu-id="4551d-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="4551d-189">[32 位元]驗證標記大小 （以位元組為單位，big endian） 驗證的加密函數所產生。</span><span class="sxs-lookup"><span data-stu-id="4551d-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="4551d-190">(如我們的系統，這會固定標記大小 = 128 位元。)</span><span class="sxs-lookup"><span data-stu-id="4551d-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="4551d-191">[128 位元]Enc_GCM 的標記 (K_E nonce，"")，這是指定為空字串輸入的對稱區塊加密演算法的輸出，並且其中 nonce 是 96 位元全零向量。</span><span class="sxs-lookup"><span data-stu-id="4551d-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="4551d-192">K_E 衍生使用相同的機制，如 CBC 加密 + HMAC 驗證案例所示。</span><span class="sxs-lookup"><span data-stu-id="4551d-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="4551d-193">不過，因為這裡沒有任何 K_H，基本上我們 |K_H |= 0 時，演算法會摺疊到和表單下方。</span><span class="sxs-lookup"><span data-stu-id="4551d-193">However, since there's no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="4551d-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span><span class="sxs-lookup"><span data-stu-id="4551d-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="4551d-195">範例：AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="4551d-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="4551d-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span><span class="sxs-lookup"><span data-stu-id="4551d-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="4551d-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="4551d-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="4551d-198">接下來，計算 Enc_GCM 的驗證標記 (K_E nonce，"") AES-256-GCM 指定 nonce = 096 和 K_E 如上所述。</span><span class="sxs-lookup"><span data-stu-id="4551d-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="4551d-199">結果: = E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="4551d-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="4551d-200">這會產生下列完整的內容標頭：</span><span class="sxs-lookup"><span data-stu-id="4551d-200">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="4551d-201">元件，如下所示分解：</span><span class="sxs-lookup"><span data-stu-id="4551d-201">The components break down as follows:</span></span>

* <span data-ttu-id="4551d-202">標記 (00 01)</span><span class="sxs-lookup"><span data-stu-id="4551d-202">the marker (00 01)</span></span>

* <span data-ttu-id="4551d-203">區塊加密金鑰長度 (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="4551d-203">the block cipher key length (00 00 00 20)</span></span>

* <span data-ttu-id="4551d-204">nonce 的大小 (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="4551d-204">the nonce size (00 00 00 0C)</span></span>

* <span data-ttu-id="4551d-205">區塊密碼區塊大小 (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="4551d-205">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="4551d-206">驗證標記大小 (00 00 00 10) 和</span><span class="sxs-lookup"><span data-stu-id="4551d-206">the authentication tag size (00 00 00 10) and</span></span>

* <span data-ttu-id="4551d-207">驗證標籤，從執行中的區塊編碼器 (E7 DC-結束)。</span><span class="sxs-lookup"><span data-stu-id="4551d-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
