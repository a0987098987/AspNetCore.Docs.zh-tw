---
title: ASP.NET Core 中的內容標頭
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護內容標頭的執行詳細資料。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 0995cd80c10f638c90a60630378518988ffb89ed
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060094"
---
# <a name="context-headers-in-aspnet-core"></a><span data-ttu-id="1cd69-103">ASP.NET Core 中的內容標頭</span><span class="sxs-lookup"><span data-stu-id="1cd69-103">Context headers in ASP.NET Core</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="1cd69-104">背景和理論</span><span class="sxs-lookup"><span data-stu-id="1cd69-104">Background and theory</span></span>

<span data-ttu-id="1cd69-105">在資料保護系統中，「金鑰」是指可提供已驗證加密服務的物件。</span><span class="sxs-lookup"><span data-stu-id="1cd69-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="1cd69-106">每個金鑰都是以唯一識別碼（GUID）來識別，而且它會攜帶其演算法資訊和 entropic 材質。</span><span class="sxs-lookup"><span data-stu-id="1cd69-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="1cd69-107">它的目的是要讓每個金鑰都具有獨特的熵，但是系統無法強制執行，而且我們也需要將金鑰環中現有金鑰的演算法資訊修改為手動變更金鑰信號的開發人員。</span><span class="sxs-lookup"><span data-stu-id="1cd69-107">It's intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="1cd69-108">為了達到我們的安全性需求，在這些情況下，資料保護系統具有[密碼](https://www.microsoft.com/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption)編譯彈性的概念，可讓您在多個密碼編譯演算法中安全使用單一 entropic 值。</span><span class="sxs-lookup"><span data-stu-id="1cd69-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="1cd69-109">大部分支援密碼編譯靈活性的系統，都是在裝載內包含關於演算法的一些識別資訊。</span><span class="sxs-lookup"><span data-stu-id="1cd69-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="1cd69-110">演算法的 OID 通常是很好的候選。</span><span class="sxs-lookup"><span data-stu-id="1cd69-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="1cd69-111">不過，我們遇到的一個問題是，有多種方式可以指定相同的演算法：「AES」（CNG）和受管理的 Aes、AesManaged、AesCryptoServiceProvider、AesCng 和 RijndaelManaged （指定的特定參數）類別實際上都是相同的，因此我們需要維護所有這些的對應到正確的 OID。</span><span class="sxs-lookup"><span data-stu-id="1cd69-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="1cd69-112">如果開發人員想要提供自訂演算法（或甚至是 AES！的另一個實作為），他們必須告訴我們其 OID。</span><span class="sxs-lookup"><span data-stu-id="1cd69-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="1cd69-113">這個額外的註冊步驟讓系統設定特別困難。</span><span class="sxs-lookup"><span data-stu-id="1cd69-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="1cd69-114">回頭執行後，我們決定我們已從錯誤的方向中接近問題。</span><span class="sxs-lookup"><span data-stu-id="1cd69-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="1cd69-115">OID 會告訴您演算法的意義，但我們並不會特別在意這一點。</span><span class="sxs-lookup"><span data-stu-id="1cd69-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="1cd69-116">如果我們需要以兩種不同的演算法安全地使用單一 entropic 值，我們就不需要知道演算法實際上是什麼。</span><span class="sxs-lookup"><span data-stu-id="1cd69-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="1cd69-117">我們真正在意的是它們的表現方式。</span><span class="sxs-lookup"><span data-stu-id="1cd69-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="1cd69-118">任何適當的對稱式區塊加密演算法也是強式隨機排列（PRP）：修正輸入（金鑰、連結模式、IV、純文字），而加密文字輸出的機率會與任何其他對稱式區塊加密演算法（提供相同的輸入）不同。</span><span class="sxs-lookup"><span data-stu-id="1cd69-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="1cd69-119">同樣地，任何適當的索引鍵雜湊函式也是強式的偽虛擬函式（PRF），而且在指定固定的輸入集時，其輸出將回應非常正面與任何其他索引雜湊函數不同。</span><span class="sxs-lookup"><span data-stu-id="1cd69-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="1cd69-120">我們使用此強式 Prp 和 PRFs 的概念來建立內容標頭。</span><span class="sxs-lookup"><span data-stu-id="1cd69-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="1cd69-121">此內容標頭基本上會作為任何指定作業所使用之演算法的穩定指紋，並提供資料保護系統所需的密碼編譯靈活性。</span><span class="sxs-lookup"><span data-stu-id="1cd69-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="1cd69-122">此標頭是可重現的，稍後會用來做為子機碼[衍生進程](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)的一部分。</span><span class="sxs-lookup"><span data-stu-id="1cd69-122">This header is reproducible and is used later as part of the [subkey derivation process](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="1cd69-123">有兩種不同的方式可以建立內容標頭，視基礎演算法的作業模式而定。</span><span class="sxs-lookup"><span data-stu-id="1cd69-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="1cd69-124">CBC 模式加密 + HMAC 驗證</span><span class="sxs-lookup"><span data-stu-id="1cd69-124">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="1cd69-125">內容標頭是由下列元件所組成：</span><span class="sxs-lookup"><span data-stu-id="1cd69-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="1cd69-126">[16 位]值 00 00，這是表示「CBC 加密 + HMAC 驗證」的標記。</span><span class="sxs-lookup"><span data-stu-id="1cd69-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="1cd69-127">[32 位]對稱區塊加密演算法的索引鍵長度（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="1cd69-128">[32 位]對稱區塊加密演算法的區塊大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="1cd69-129">[32 位]HMAC 演算法的索引鍵長度（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="1cd69-130">（目前金鑰大小一律符合摘要大小）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="1cd69-131">[32 位]HMAC 演算法的摘要大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="1cd69-132">`EncCBC(K_E, IV, "")`，這是指定空字串輸入，而且 IV 為全零向量的對稱區塊加密演算法的輸出。</span><span class="sxs-lookup"><span data-stu-id="1cd69-132">`EncCBC(K_E, IV, "")`, which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="1cd69-133">的結構 `K_E` 如下所述。</span><span class="sxs-lookup"><span data-stu-id="1cd69-133">The construction of `K_E` is described below.</span></span>

* <span data-ttu-id="1cd69-134">`MAC(K_H, "")`，這是指定空字串輸入的 HMAC 演算法輸出。</span><span class="sxs-lookup"><span data-stu-id="1cd69-134">`MAC(K_H, "")`, which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="1cd69-135">的結構 `K_H` 如下所述。</span><span class="sxs-lookup"><span data-stu-id="1cd69-135">The construction of `K_H` is described below.</span></span>

<span data-ttu-id="1cd69-136">在理想的情況下，我們可以針對和傳遞所有-零的向量 `K_E` `K_H` 。</span><span class="sxs-lookup"><span data-stu-id="1cd69-136">Ideally, we could pass all-zero vectors for `K_E` and `K_H`.</span></span> <span data-ttu-id="1cd69-137">不過，我們想要避免基礎演算法在執行任何作業之前檢查弱式金鑰是否存在（尤其是 DES 和3DES），這種情況會使用簡單或可重複的模式（例如全零向量）來排除。</span><span class="sxs-lookup"><span data-stu-id="1cd69-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="1cd69-138">相反地，我們使用計數器模式的 NIST SP800-108 KDF （請參閱[NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，Sec. 5.1），其長度為零的索引鍵、標籤和內容，以及 HMACSHA512 作為基礎 PRF。</span><span class="sxs-lookup"><span data-stu-id="1cd69-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="1cd69-139">我們會衍生 `| K_E | + | K_H |` 位元組的輸出，然後將結果分解成 `K_E` 和 `K_H` 自己。</span><span class="sxs-lookup"><span data-stu-id="1cd69-139">We derive `| K_E | + | K_H |` bytes of output, then decompose the result into `K_E` and `K_H` themselves.</span></span> <span data-ttu-id="1cd69-140">這是以數學方式表示，如下所示。</span><span class="sxs-lookup"><span data-stu-id="1cd69-140">Mathematically, this is represented as follows.</span></span>

`( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")`

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="1cd69-141">範例： AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="1cd69-141">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="1cd69-142">例如，假設對稱區塊加密演算法是 AES-192-CBC，而驗證演算法是 HMACSHA256 的情況。</span><span class="sxs-lookup"><span data-stu-id="1cd69-142">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="1cd69-143">系統會使用下列步驟來產生內容標頭。</span><span class="sxs-lookup"><span data-stu-id="1cd69-143">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="1cd69-144">First、let `( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")` 、where `| K_E | = 192 bits` 和 `| K_H | = 256 bits` 依據指定的演算法。</span><span class="sxs-lookup"><span data-stu-id="1cd69-144">First, let `( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")`, where `| K_E | = 192 bits` and `| K_H | = 256 bits` per the specified algorithms.</span></span> <span data-ttu-id="1cd69-145">這會導致 `K_E = 5BB6..21DD` `K_H = A04A..00A9` 下列範例中的和：</span><span class="sxs-lookup"><span data-stu-id="1cd69-145">This leads to `K_E = 5BB6..21DD` and `K_H = A04A..00A9` in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="1cd69-146">接下來， `Enc_CBC (K_E, IV, "")` 為指定和如上所述的 AES-192-CBC 計算 `IV = 0*` `K_E` 。</span><span class="sxs-lookup"><span data-stu-id="1cd69-146">Next, compute `Enc_CBC (K_E, IV, "")` for AES-192-CBC given `IV = 0*` and `K_E` as above.</span></span>

`result := F474B1872B3B53E4721DE19C0841DB6F`

<span data-ttu-id="1cd69-147">接下來，為 `MAC(K_H, "")` 指定的 HMACSHA256 計算 `K_H` 。</span><span class="sxs-lookup"><span data-stu-id="1cd69-147">Next, compute `MAC(K_H, "")` for HMACSHA256 given `K_H` as above.</span></span>

`result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C`

<span data-ttu-id="1cd69-148">這會產生以下的完整內容標頭：</span><span class="sxs-lookup"><span data-stu-id="1cd69-148">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="1cd69-149">此內容標頭是已驗證加密演算法配對的指紋（AES-192-CBC 加密 + HMACSHA256 驗證）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-149">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="1cd69-150">[上述](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components)元件如下所述：</span><span class="sxs-lookup"><span data-stu-id="1cd69-150">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="1cd69-151">標記`(00 00)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-151">the marker `(00 00)`</span></span>

* <span data-ttu-id="1cd69-152">區塊密碼金鑰長度`(00 00 00 18)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-152">the block cipher key length `(00 00 00 18)`</span></span>

* <span data-ttu-id="1cd69-153">區塊加密區塊大小`(00 00 00 10)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-153">the block cipher block size `(00 00 00 10)`</span></span>

* <span data-ttu-id="1cd69-154">HMAC 金鑰長度`(00 00 00 20)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-154">the HMAC key length `(00 00 00 20)`</span></span>

* <span data-ttu-id="1cd69-155">HMAC 摘要大小`(00 00 00 20)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-155">the HMAC digest size `(00 00 00 20)`</span></span>

* <span data-ttu-id="1cd69-156">區塊密碼 PRP 輸出 `(F4 74 - DB 6F)` 和</span><span class="sxs-lookup"><span data-stu-id="1cd69-156">the block cipher PRP output `(F4 74 - DB 6F)` and</span></span>

* <span data-ttu-id="1cd69-157">HMAC PRF 輸出 `(D4 79 - end)` 。</span><span class="sxs-lookup"><span data-stu-id="1cd69-157">the HMAC PRF output `(D4 79 - end)`.</span></span>

> [!NOTE]
> <span data-ttu-id="1cd69-158">不論演算法實架構是由 Windows CNG 還是由 managed System.security.cryptography.symmetricalgorithm 和 KeyedHashAlgorithm 類型所提供，CBC 模式加密 + HMAC 驗證內容標頭的建立方式都相同。</span><span class="sxs-lookup"><span data-stu-id="1cd69-158">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="1cd69-159">這可讓在不同作業系統上執行的應用程式可靠地產生相同的內容標頭，即使作業系統的演算法不同也是一樣。</span><span class="sxs-lookup"><span data-stu-id="1cd69-159">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="1cd69-160">（實際上，KeyedHashAlgorithm 不一定要是適當的 HMAC。</span><span class="sxs-lookup"><span data-stu-id="1cd69-160">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="1cd69-161">它可以是任何金鑰雜湊演算法類型）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-161">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="1cd69-162">範例： 3DES-192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="1cd69-162">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="1cd69-163">First、let `( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")` 、where `| K_E | = 192 bits` 和 `| K_H | = 160 bits` 依據指定的演算法。</span><span class="sxs-lookup"><span data-stu-id="1cd69-163">First, let `( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")`, where `| K_E | = 192 bits` and `| K_H | = 160 bits` per the specified algorithms.</span></span> <span data-ttu-id="1cd69-164">這會導致 `K_E = A219..E2BB` `K_H = DC4A..B464` 下列範例中的和：</span><span class="sxs-lookup"><span data-stu-id="1cd69-164">This leads to `K_E = A219..E2BB` and `K_H = DC4A..B464` in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="1cd69-165">接下來，計算 `Enc_CBC (K_E, IV, "")` 3des-192-CBC， `IV = 0*` 並如上所述提供 `K_E` 。</span><span class="sxs-lookup"><span data-stu-id="1cd69-165">Next, compute `Enc_CBC (K_E, IV, "")` for 3DES-192-CBC given `IV = 0*` and `K_E` as above.</span></span>

`result := ABB100F81E53E10E`

<span data-ttu-id="1cd69-166">接下來，為 `MAC(K_H, "")` 指定的 HMACSHA1 計算 `K_H` 。</span><span class="sxs-lookup"><span data-stu-id="1cd69-166">Next, compute `MAC(K_H, "")` for HMACSHA1 given `K_H` as above.</span></span>

`result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555`

<span data-ttu-id="1cd69-167">這會產生完整的內容標頭，這是已驗證加密演算法配對的指紋（3DES-192-CBC 加密 + HMACSHA1 驗證），如下所示：</span><span class="sxs-lookup"><span data-stu-id="1cd69-167">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="1cd69-168">元件會依照下列方式細分：</span><span class="sxs-lookup"><span data-stu-id="1cd69-168">The components break down as follows:</span></span>

* <span data-ttu-id="1cd69-169">標記`(00 00)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-169">the marker `(00 00)`</span></span>

* <span data-ttu-id="1cd69-170">區塊密碼金鑰長度`(00 00 00 18)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-170">the block cipher key length `(00 00 00 18)`</span></span>

* <span data-ttu-id="1cd69-171">區塊加密區塊大小`(00 00 00 08)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-171">the block cipher block size `(00 00 00 08)`</span></span>

* <span data-ttu-id="1cd69-172">HMAC 金鑰長度`(00 00 00 14)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-172">the HMAC key length `(00 00 00 14)`</span></span>

* <span data-ttu-id="1cd69-173">HMAC 摘要大小`(00 00 00 14)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-173">the HMAC digest size `(00 00 00 14)`</span></span>

* <span data-ttu-id="1cd69-174">區塊密碼 PRP 輸出 `(AB B1 - E1 0E)` 和</span><span class="sxs-lookup"><span data-stu-id="1cd69-174">the block cipher PRP output `(AB B1 - E1 0E)` and</span></span>

* <span data-ttu-id="1cd69-175">HMAC PRF 輸出 `(76 EB - end)` 。</span><span class="sxs-lookup"><span data-stu-id="1cd69-175">the HMAC PRF output `(76 EB - end)`.</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="1cd69-176">Galois/計數器模式加密 + 驗證</span><span class="sxs-lookup"><span data-stu-id="1cd69-176">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="1cd69-177">內容標頭是由下列元件所組成：</span><span class="sxs-lookup"><span data-stu-id="1cd69-177">The context header consists of the following components:</span></span>

* <span data-ttu-id="1cd69-178">[16 位]值 00 01，這是表示「GCM 加密 + 驗證」的標記。</span><span class="sxs-lookup"><span data-stu-id="1cd69-178">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="1cd69-179">[32 位]對稱區塊加密演算法的索引鍵長度（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-179">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="1cd69-180">[32 位]已驗證的加密作業期間使用的 nonce 大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-180">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="1cd69-181">（在我們的系統中，這是在 nonce 大小 = 96 位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-181">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="1cd69-182">[32 位]對稱區塊加密演算法的區塊大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-182">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="1cd69-183">（對於 GCM 而言，這是在區塊大小 = 128 位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-183">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="1cd69-184">[32 位]已驗證的加密函式所產生的驗證標記大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-184">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="1cd69-185">（在我們的系統中，這是在標記大小 = 128 位）。</span><span class="sxs-lookup"><span data-stu-id="1cd69-185">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="1cd69-186">[128 位]的標記 `Enc_GCM (K_E, nonce, "")` ，這是對稱區塊加密演算法的輸出，指定空白字串輸入，其中 nonce 是96位的全部-零向量。</span><span class="sxs-lookup"><span data-stu-id="1cd69-186">[128 bits] The tag of `Enc_GCM (K_E, nonce, "")`, which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="1cd69-187">`K_E`是使用與 CBC 加密 + HMAC 驗證案例中相同的機制來衍生。</span><span class="sxs-lookup"><span data-stu-id="1cd69-187">`K_E` is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="1cd69-188">不過，由於這裡沒有任何 `K_H` 播放，我們基本上有 `| K_H | = 0` ，而演算法折迭為下列表單。</span><span class="sxs-lookup"><span data-stu-id="1cd69-188">However, since there's no `K_H` in play here, we essentially have `| K_H | = 0`, and the algorithm collapses to the below form.</span></span>

`K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")`

### <a name="example-aes-256-gcm"></a><span data-ttu-id="1cd69-189">範例： AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="1cd69-189">Example: AES-256-GCM</span></span>

<span data-ttu-id="1cd69-190">首先，讓我們來看 `K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")` `| K_E | = 256 bits` 。</span><span class="sxs-lookup"><span data-stu-id="1cd69-190">First, let `K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")`, where `| K_E | = 256 bits`.</span></span>

`K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8`

<span data-ttu-id="1cd69-191">接下來， `Enc_GCM (K_E, nonce, "")` 針對指定的 AES-256-GCM 計算的驗證標記， `nonce = 096` 並 `K_E` 如上所述。</span><span class="sxs-lookup"><span data-stu-id="1cd69-191">Next, compute the authentication tag of `Enc_GCM (K_E, nonce, "")` for AES-256-GCM given `nonce = 096` and `K_E` as above.</span></span>

`result := E7DCCE66DF855A323A6BB7BD7A59BE45`

<span data-ttu-id="1cd69-192">這會產生以下的完整內容標頭：</span><span class="sxs-lookup"><span data-stu-id="1cd69-192">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="1cd69-193">元件會依照下列方式細分：</span><span class="sxs-lookup"><span data-stu-id="1cd69-193">The components break down as follows:</span></span>

* <span data-ttu-id="1cd69-194">標記`(00 01)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-194">the marker `(00 01)`</span></span>

* <span data-ttu-id="1cd69-195">區塊密碼金鑰長度`(00 00 00 20)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-195">the block cipher key length `(00 00 00 20)`</span></span>

* <span data-ttu-id="1cd69-196">nonce 大小`(00 00 00 0C)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-196">the nonce size `(00 00 00 0C)`</span></span>

* <span data-ttu-id="1cd69-197">區塊加密區塊大小`(00 00 00 10)`</span><span class="sxs-lookup"><span data-stu-id="1cd69-197">the block cipher block size `(00 00 00 10)`</span></span>

* <span data-ttu-id="1cd69-198">驗證標記大小 `(00 00 00 10)` 和</span><span class="sxs-lookup"><span data-stu-id="1cd69-198">the authentication tag size `(00 00 00 10)` and</span></span>

* <span data-ttu-id="1cd69-199">執行區塊密碼的驗證標記 `(E7 DC - end)` 。</span><span class="sxs-lookup"><span data-stu-id="1cd69-199">the authentication tag from running the block cipher `(E7 DC - end)`.</span></span>
