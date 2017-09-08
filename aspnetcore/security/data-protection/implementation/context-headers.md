---
title: "內容標頭"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 16da0a4f78875ee26fa9ca7c9920b8dafd0ce417
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="context-headers"></a><span data-ttu-id="6a5df-103">內容標頭</span><span class="sxs-lookup"><span data-stu-id="6a5df-103">Context headers</span></span>

<a name=data-protection-implementation-context-headers></a>

## <a name="background-and-theory"></a><span data-ttu-id="6a5df-104">背景和理論上</span><span class="sxs-lookup"><span data-stu-id="6a5df-104">Background and theory</span></span>

<span data-ttu-id="6a5df-105">在資料保護系統中，「 金鑰 」 表示的物件，可提供驗證加密服務。</span><span class="sxs-lookup"><span data-stu-id="6a5df-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="6a5df-106">每個索引鍵由唯一識別碼 (GUID)，它會具有演算法的資訊和 entropic 材料。</span><span class="sxs-lookup"><span data-stu-id="6a5df-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="6a5df-107">並非每個索引鍵執行唯一 entropy，但是系統無法強制執行，而且我們也要負責的開發人員可能會藉由修改現有的索引鍵中鑰匙圈的演算法資訊手動變更鑰匙圈。</span><span class="sxs-lookup"><span data-stu-id="6a5df-107">It is intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="6a5df-108">若要達到指定這些情況下我們安全性需求的資料保護系統具有的概念[加密彈性](http://research.microsoft.com/apps/pubs/default.aspx?id=121045)，可讓安全地使用單一 entropic 值不同的多個密碼編譯演算法。</span><span class="sxs-lookup"><span data-stu-id="6a5df-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](http://research.microsoft.com/apps/pubs/default.aspx?id=121045), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="6a5df-109">大部分的系統支援的加密彈性來達成包括一些有關裝載內部之演算法的識別資訊。</span><span class="sxs-lookup"><span data-stu-id="6a5df-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="6a5df-110">此演算法的 OID 通常是適合此。</span><span class="sxs-lookup"><span data-stu-id="6a5df-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="6a5df-111">不過，我們遇到的一個問題是有多種方式可以指定相同的演算法:"AES 」 (CNG) 和 managed Aes、 AesManaged、 AesCryptoServiceProvider、 AesCng 和 RijndaelManaged （要有特定的參數） 類別所有實際上都相同件事，所以我們需要維護所有這些至正確的 OID 的對應。</span><span class="sxs-lookup"><span data-stu-id="6a5df-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="6a5df-112">如果開發人員想要提供自訂的演算法 （或甚至是另一個實作 AES ！），他們必須告訴 OID。</span><span class="sxs-lookup"><span data-stu-id="6a5df-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="6a5df-113">此額外的註冊步驟可讓系統設定特別人痛苦。</span><span class="sxs-lookup"><span data-stu-id="6a5df-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="6a5df-114">逐步執行，我們決定我們已從方向錯誤接近問題。</span><span class="sxs-lookup"><span data-stu-id="6a5df-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="6a5df-115">OID 告訴您此演算法的是，但我們不實際在意這。</span><span class="sxs-lookup"><span data-stu-id="6a5df-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="6a5df-116">如果我們需要兩個不同的演算法中安全地使用單一 entropic 值，它不需要讓我們知道演算法實際上是。</span><span class="sxs-lookup"><span data-stu-id="6a5df-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="6a5df-117">什麼我們實際關注的是它們的行為。</span><span class="sxs-lookup"><span data-stu-id="6a5df-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="6a5df-118">任何不錯對稱的區塊加密演算法也是強式的虛擬隨機排列 (PRP): 修正 （索引鍵，鏈結模式，IV，純文字） 的輸入，然後加密文字輸出會與過度機率是不同於任何其他對稱的區塊編碼器演算法提供相同的輸入。</span><span class="sxs-lookup"><span data-stu-id="6a5df-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="6a5df-119">同樣地，任何不錯的金鑰雜湊函式也是強式的虛擬隨機函式 (PRF)，而且指定的一組固定的輸入它的輸出會相當不同於任何其他索引鍵的雜湊函式。</span><span class="sxs-lookup"><span data-stu-id="6a5df-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="6a5df-120">我們使用強式 PRPs 和 PRFs 的概念來建立的內容標頭。</span><span class="sxs-lookup"><span data-stu-id="6a5df-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="6a5df-121">此內容標頭實質上會當做穩定的憑證指紋的演算法，對於任何給定的作業，用於透過並提供資料保護系統所需的加密彈性。</span><span class="sxs-lookup"><span data-stu-id="6a5df-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="6a5df-122">這個標頭是可重現和稍後會使用一部分[子機碼衍生的程序](subkeyderivation.md#data-protection-implementation-subkey-derivation)。</span><span class="sxs-lookup"><span data-stu-id="6a5df-122">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="6a5df-123">有兩種不同的方式來建置內容標頭，根據的作業模式基礎的演算法而定。</span><span class="sxs-lookup"><span data-stu-id="6a5df-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="6a5df-124">CBC 模式加密 + HMAC 驗證</span><span class="sxs-lookup"><span data-stu-id="6a5df-124">CBC-mode encryption + HMAC authentication</span></span>

<a name=data-protection-implementation-context-headers-cbc-components></a>

<span data-ttu-id="6a5df-125">內容標頭包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="6a5df-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="6a5df-126">[16 位元]值為 00 00，也就是使用標記表示"CBC 加密 + HMAC 驗證 」。</span><span class="sxs-lookup"><span data-stu-id="6a5df-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="6a5df-127">[32 位元]（以位元組為單位，big endian） 對稱的區塊加密演算法的金鑰長度。</span><span class="sxs-lookup"><span data-stu-id="6a5df-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="6a5df-128">[32 位元]對稱的區塊加密演算法的區塊大小 （以位元組為單位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="6a5df-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="6a5df-129">[32 位元]金鑰長度 （以位元組為單位，big endian） 的 HMAC 演算法。</span><span class="sxs-lookup"><span data-stu-id="6a5df-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="6a5df-130">（目前的索引鍵的大小一律符合摘要大小。）</span><span class="sxs-lookup"><span data-stu-id="6a5df-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="6a5df-131">[32 位元]摘要的大小 （以位元組為單位，big endian） 的 HMAC 演算法。</span><span class="sxs-lookup"><span data-stu-id="6a5df-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="6a5df-132">EncCBC (K_E、 IV，"")，這是指定為空字串輸入對稱的區塊加密演算法的輸出和 IV 所在的全部為零的向量。</span><span class="sxs-lookup"><span data-stu-id="6a5df-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="6a5df-133">下面會描述 K_E 建構。</span><span class="sxs-lookup"><span data-stu-id="6a5df-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="6a5df-134">MAC (K_H，"")，這是指定為空字串輸入的 HMAC 演算法的輸出。</span><span class="sxs-lookup"><span data-stu-id="6a5df-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="6a5df-135">下面會描述 K_H 建構。</span><span class="sxs-lookup"><span data-stu-id="6a5df-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="6a5df-136">在理想情況下，我們無法 K_E 和 K_H 傳遞全部為零的向量。</span><span class="sxs-lookup"><span data-stu-id="6a5df-136">Ideally we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="6a5df-137">不過，我們想要避免這種情況，基礎的演算法檢查存在的弱式金鑰之前執行任何作業 （尤其是 DES 和 3DES） 無法讓您使用的簡單或可重複的模式，像全部為零的向量。</span><span class="sxs-lookup"><span data-stu-id="6a5df-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="6a5df-138">相反地，我們使用 NIST SP800 108 KDF 計數器模式中 (請參閱[NIST SP800 108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)、 秒。</span><span class="sxs-lookup"><span data-stu-id="6a5df-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec.</span></span> <span data-ttu-id="6a5df-139">5.1) 零長度的索引鍵、 標籤和與內容為基礎的 PRF HMACSHA512。</span><span class="sxs-lookup"><span data-stu-id="6a5df-139">5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="6a5df-140">我們衍生 |K_E |+ |K_H |位元組的輸出，然後分解結果 K_E 和 K_H 本身。</span><span class="sxs-lookup"><span data-stu-id="6a5df-140">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="6a5df-141">在數學上，這被表示，如下所示。</span><span class="sxs-lookup"><span data-stu-id="6a5df-141">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="6a5df-142">(K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")</span><span class="sxs-lookup"><span data-stu-id="6a5df-142">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="6a5df-143">範例： 192 位 AES-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="6a5df-143">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="6a5df-144">例如，請考慮對稱的區塊加密演算法是 AES-CBC-192 並驗證演算法是 HMACSHA256 的情況。</span><span class="sxs-lookup"><span data-stu-id="6a5df-144">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="6a5df-145">系統會產生內容標頭使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="6a5df-145">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="6a5df-146">首先，讓 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 192 位元和 |K_H |= 每個指定的演算法的 256 位元。</span><span class="sxs-lookup"><span data-stu-id="6a5df-146">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="6a5df-147">這會導致 K_E = 5BB6...21DD 和 K_H = A04A...下例會 00A9:</span><span class="sxs-lookup"><span data-stu-id="6a5df-147">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
   61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
   49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
   B7 92 3D BF 59 90 00 A9
   ```

<span data-ttu-id="6a5df-148">接下來，計算 Enc_CBC (K_E、 IV，"") 192 位 AES-CBC 指定 IV = 0 * 和上述 K_E。</span><span class="sxs-lookup"><span data-stu-id="6a5df-148">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="6a5df-149">結果: = F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="6a5df-149">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="6a5df-150">接下來，計算 MAC (K_H，"") 的 HMACSHA256 上述指定 K_H。</span><span class="sxs-lookup"><span data-stu-id="6a5df-150">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="6a5df-151">結果: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="6a5df-151">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="6a5df-152">這會產生下列的完整內容標頭：</span><span class="sxs-lookup"><span data-stu-id="6a5df-152">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
   00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
   DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
   8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
   22 0C
   ```

<span data-ttu-id="6a5df-153">此內容標頭是已驗證的加密演算法組 （AES-CBC-192 加密 + HMACSHA256 驗證） 的憑證指紋。</span><span class="sxs-lookup"><span data-stu-id="6a5df-153">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="6a5df-154">所述的元件[上方](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components)是：</span><span class="sxs-lookup"><span data-stu-id="6a5df-154">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="6a5df-155">標記 (00 00)</span><span class="sxs-lookup"><span data-stu-id="6a5df-155">the marker (00 00)</span></span>

* <span data-ttu-id="6a5df-156">區塊的加密金鑰長度 (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="6a5df-156">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="6a5df-157">區塊 cipher 區塊大小 (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="6a5df-157">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="6a5df-158">HMAC 金鑰長度 (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="6a5df-158">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="6a5df-159">HMAC 摘要大小 (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="6a5df-159">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="6a5df-160">區塊編碼器 PRP 輸出 (F4 74-DB 6F) 和</span><span class="sxs-lookup"><span data-stu-id="6a5df-160">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="6a5df-161">HMAC PRF 輸出 (D4 79-結束)。</span><span class="sxs-lookup"><span data-stu-id="6a5df-161">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="6a5df-162">CBC 模式加密 + HMAC 驗證內容標頭建立相同的方式，不論透過 Windows CNG 或 managed SymmetricAlgorithm 和 KeyedHashAlgorithm 型別，是否提供演算法實作。</span><span class="sxs-lookup"><span data-stu-id="6a5df-162">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="6a5df-163">這可讓在不同作業系統上執行的應用程式能夠可靠地產生相同的內容標頭，即使作業系統之間不同的演算法的實作。</span><span class="sxs-lookup"><span data-stu-id="6a5df-163">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="6a5df-164">（在實務上，KeyedHashAlgorithm 不必是適當的 HMAC。</span><span class="sxs-lookup"><span data-stu-id="6a5df-164">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="6a5df-165">它可以是任何索引雜湊演算法類型。）</span><span class="sxs-lookup"><span data-stu-id="6a5df-165">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="6a5df-166">範例： 3DES-192 位 CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="6a5df-166">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="6a5df-167">首先，讓 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 192 位元和 |K_H |= 160 位元，每個指定的演算法。</span><span class="sxs-lookup"><span data-stu-id="6a5df-167">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="6a5df-168">這會導致 K_E = A219...E2BB 和 K_H = DC4A...下例會 B464:</span><span class="sxs-lookup"><span data-stu-id="6a5df-168">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
   61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
   D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
   ```

<span data-ttu-id="6a5df-169">接下來，計算 Enc_CBC (K_E、 IV，"") 3DES-192 位 CBC 指定 IV = 0 * 和上述 K_E。</span><span class="sxs-lookup"><span data-stu-id="6a5df-169">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="6a5df-170">結果: = ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="6a5df-170">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="6a5df-171">接下來，計算 MAC (K_H，"") 的上述指定 K_H HMACSHA1。</span><span class="sxs-lookup"><span data-stu-id="6a5df-171">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="6a5df-172">結果: = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="6a5df-172">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="6a5df-173">這會產生完整的內容標頭的已驗證的指紋即加密演算法組 （3DES-192 位 CBC 加密 + HMACSHA1 驗證），如下所示：</span><span class="sxs-lookup"><span data-stu-id="6a5df-173">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
   00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
   03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
   ```

<span data-ttu-id="6a5df-174">如下所示分解元件：</span><span class="sxs-lookup"><span data-stu-id="6a5df-174">The components break down as follows:</span></span>

* <span data-ttu-id="6a5df-175">標記 (00 00)</span><span class="sxs-lookup"><span data-stu-id="6a5df-175">the marker (00 00)</span></span>

* <span data-ttu-id="6a5df-176">區塊的加密金鑰長度 (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="6a5df-176">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="6a5df-177">區塊 cipher 區塊大小 (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="6a5df-177">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="6a5df-178">HMAC 金鑰長度 (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="6a5df-178">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="6a5df-179">HMAC 摘要大小 (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="6a5df-179">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="6a5df-180">區塊編碼器 PRP 輸出 (AB B1-E1 0E) 和</span><span class="sxs-lookup"><span data-stu-id="6a5df-180">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="6a5df-181">HMAC PRF 輸出 (76 EB-結束)。</span><span class="sxs-lookup"><span data-stu-id="6a5df-181">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="6a5df-182">Galois/計數器模式加密 + 驗證</span><span class="sxs-lookup"><span data-stu-id="6a5df-182">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="6a5df-183">內容標頭包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="6a5df-183">The context header consists of the following components:</span></span>

* <span data-ttu-id="6a5df-184">[16 位元]值為 00 01，也就是使用標記表示"GCM 加密 + 驗證 」。</span><span class="sxs-lookup"><span data-stu-id="6a5df-184">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="6a5df-185">[32 位元]（以位元組為單位，big endian） 對稱的區塊加密演算法的金鑰長度。</span><span class="sxs-lookup"><span data-stu-id="6a5df-185">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="6a5df-186">[32 位元]Nonce 大小 （以位元組為單位，big endian） 已驗證的加密作業期間使用。</span><span class="sxs-lookup"><span data-stu-id="6a5df-186">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="6a5df-187">(我們系統中，這會固定為 nonce 大小 = 96 位元。)</span><span class="sxs-lookup"><span data-stu-id="6a5df-187">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="6a5df-188">[32 位元]對稱的區塊加密演算法的區塊大小 （以位元組為單位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="6a5df-188">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="6a5df-189">(適用於 GCM，這會固定為區塊大小 = 128 位元。)</span><span class="sxs-lookup"><span data-stu-id="6a5df-189">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="6a5df-190">[32 位元]驗證標記大小 （以位元組為單位，big endian） 已驗證的加密函數所產生。</span><span class="sxs-lookup"><span data-stu-id="6a5df-190">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="6a5df-191">(我們系統中，這會固定為標記大小 = 128 位元。)</span><span class="sxs-lookup"><span data-stu-id="6a5df-191">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="6a5df-192">[128 位元]Enc_GCM 標記 (K_E nonce，"")，這是指定為空字串輸入對稱的區塊加密演算法的輸出和 nonce 所在的 96 位元全部為零的向量。</span><span class="sxs-lookup"><span data-stu-id="6a5df-192">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="6a5df-193">K_E 衍生使用相同的機制，如 CBC 加密 + HMAC 驗證案例所示。</span><span class="sxs-lookup"><span data-stu-id="6a5df-193">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="6a5df-194">不過，因為沒有 K_H 正在播放這裡，我們基本上都擁有 |K_H |= 0，演算法會摺疊至下列表單。</span><span class="sxs-lookup"><span data-stu-id="6a5df-194">However, since there is no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="6a5df-195">K_E = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")</span><span class="sxs-lookup"><span data-stu-id="6a5df-195">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="6a5df-196">範例： AES 256 GCM</span><span class="sxs-lookup"><span data-stu-id="6a5df-196">Example: AES-256-GCM</span></span>

<span data-ttu-id="6a5df-197">首先，讓 K_E = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 256 位元。</span><span class="sxs-lookup"><span data-stu-id="6a5df-197">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="6a5df-198">K_E: = 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="6a5df-198">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="6a5df-199">接下來，計算 Enc_GCM 的驗證標記 (K_E nonce，"") AES-256-GCM 提供 nonce = 096 和 K_E 上述。</span><span class="sxs-lookup"><span data-stu-id="6a5df-199">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="6a5df-200">結果: = E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="6a5df-200">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="6a5df-201">這會產生下列的完整內容標頭：</span><span class="sxs-lookup"><span data-stu-id="6a5df-201">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
   00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
   BE 45
   ```

<span data-ttu-id="6a5df-202">如下所示分解元件：</span><span class="sxs-lookup"><span data-stu-id="6a5df-202">The components break down as follows:</span></span>

   * <span data-ttu-id="6a5df-203">標記 (00 01)</span><span class="sxs-lookup"><span data-stu-id="6a5df-203">the marker (00 01)</span></span>

   * <span data-ttu-id="6a5df-204">區塊的加密金鑰長度 (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="6a5df-204">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="6a5df-205">nonce 大小 (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="6a5df-205">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="6a5df-206">區塊 cipher 區塊大小 (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="6a5df-206">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="6a5df-207">驗證標記大小 (00 00 00 10) 和</span><span class="sxs-lookup"><span data-stu-id="6a5df-207">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="6a5df-208">執行區塊編碼器的驗證標記 (E7 DC-結束)。</span><span class="sxs-lookup"><span data-stu-id="6a5df-208">the authentication tag from running the block cipher (E7 DC - end).</span></span>
