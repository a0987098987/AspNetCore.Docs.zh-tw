---
title: ASP.NET Core 中的內容標頭
author: rick-anderson
description: 了解 ASP.NET Core 資料保護的內容標頭的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 2b8fd594672bf623d38bfae90d05a984f92ce6a3
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087556"
---
# <a name="context-headers-in-aspnet-core"></a>ASP.NET Core 中的內容標頭

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>背景和理論

在資料保護系統中，「 金鑰 」 表示的物件，可提供驗證加密服務。 每個索引鍵由唯一識別碼 (GUID)，但是會有該演算法的資訊和 entropic 材料。 其目的，每個索引鍵包含唯一的 entropy，系統無法強制執行，但我們也要負責開發人員可能會藉由修改金鑰環中的現有金鑰的演算法資訊手動變更金鑰環。 若要達到這種情況下我們的安全性需求的資料保護系統具有的概念[密碼編譯靈活性](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/)，以便安全地跨多個密碼編譯演算法使用單一的 entropic 值。

大部分的系統支援密碼編譯靈活性的作法是包括一些有關在承載內部之演算法的識別資訊。 此演算法的 OID 通常是這個的不錯候選項目。 不過，我們遇到的問題之一是，有多種方式可以指定相同的演算法：「 AES 」 (CNG) 的受管理 Aes、 AesManaged，AesCryptoServiceProvider、 AesCng 和 RijndaelManaged （指定的特定參數） 類別是所有實際相同的作業，而我們需要會維護所有個以正確的 OID 的對應。 如果開發人員想要提供自訂的演算法 （或甚至是另一個實作 AES ！），他們就不必告訴我們 (OID)。 這個額外的註冊步驟可讓系統組態特別麻煩。

回顧一下，我們決定我們已從錯誤方向接近問題。 OID 會告訴您功能演算法，但我們不真正關心這。 如果我們需要在兩個不同的演算法中安全地使用單一 entropic 值，它不需要讓我們知道演算法實際上是。 什麼我們真正關心是它們的行為。 任何適當的對稱區塊加密演算法也是強式的虛擬隨機排列 (PRP): 修正 （索引鍵，鏈結模式中，IV，純文字） 的輸入，然後加密文字輸出會與讀者的機率是不同於任何其他的對稱區塊編碼器指定輸入的相同的演算法。 同樣地，任何適當的索引雜湊函式也是強式的虛擬隨機函式 (PRF)，和指定的一組固定的輸入範例的輸出結果十分會不同於其他任何索引鍵的雜湊函式。

我們可以使用強式 Prp 和 PRFs 此概念來建置的內容標頭。 此內容標頭實質上會當做穩定指紋透過在任何指定的作業中，使用的演算法，並提供所需的資料保護系統密碼編譯靈活性。 此標頭是可重現，並稍後會使用一部分[子機碼衍生的程序](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)。 有兩種不同的方式，來建置內容標頭，根據的作業模式的基礎演算法。

## <a name="cbc-mode-encryption--hmac-authentication"></a>CBC 模式加密 + HMAC 驗證

<a name="data-protection-implementation-context-headers-cbc-components"></a>

內容標頭是由下列元件所組成：

* [16 位元]值為 00 00，也就是使用標記表示 「 CBC 加密 + HMAC 驗證 」。

* [32 位元]（以位元組為單位，big endian） 的對稱區塊加密演算法金鑰長度。

* [32 位元]對稱區塊加密演算法的區塊大小 （以位元組為單位，big endian）。

* [32 位元]HMAC 演算法金鑰長度，（以位元組為單位，big endian）。 （目前的金鑰大小一律符合摘要大小。）

* [32 位元]HMAC 演算法的摘要大小 （以位元組為單位，big endian）。

* EncCBC (K_E、 IV，"")，這是指定為空字串輸入的對稱區塊加密演算法的輸出，並且其中 IV 是全 0 的向量。 以下說明 K_E 建構。

* MAC (K_H，"")，這是根據輸入空字串的 HMAC 演算法的輸出。 以下說明 K_H 建構。

在理想情況下，我們可以 K_E 和 K_H 來傳遞所有零的向量。 不過，我們想要避免這種情況其中的基礎演算法檢查存在的弱式金鑰再執行任何作業 （尤其是 DES 和 3DES），無法讓您使用簡單或可重複的模式，例如全 0 的向量。

相反地，我們使用 NIST SP800 108 KDF 計數器模式中 (請參閱[NIST SP800 108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，sec 5.1) 使用長度為零的索引鍵、 標籤和內容和為基礎的 PRF HMACSHA512。 我們 |K_E |+ |K_H |位元組的輸出，然後分解成結果 K_E 和 K_H 本身。 以數學方式，這表示，如下所示。

( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>範例：AES-192-CBC + HMACSHA256

例如，請考慮對稱區塊加密演算法是 AES-192-CBC，並驗證演算法是 HMACSHA256 的情況。 系統會產生內容標頭使用下列步驟。

首先，讓 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 192 位元和 |K_H |= 每個指定的演算法的 256 位元。 這會導致 K_E = 5BB6...21DD 和 K_H = A04A...在下列範例中的 00A9:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

接下來，計算 Enc_CBC (K_E、 IV，"") AES-192-CBC 指定 IV = 0 * 和 K_E 如上所述。

result := F474B1872B3B53E4721DE19C0841DB6F

接下來，計算 MAC (K_H，"") 為上述指定 K_H HMACSHA256。

結果: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

這會產生下列完整的內容標頭：

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

此內容標頭是 （AES 192 CBC 加密 + HMACSHA256 驗證），才會進行驗證的加密演算法組指紋。 如所述的元件[上述](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components)是：

* 標記 (00 00)

* 區塊加密金鑰長度 (00 00 00 18)

* 區塊密碼區塊大小 (00 00 00 10)

* HMAC 金鑰長度 (00 00 00 20)

* HMAC 摘要大小 (00 00 00 20)

* 區塊編碼器 PRP 輸出 (F4 74-DB 6F) 和

* HMAC PRF 輸出 (D4 79-結束)。

> [!NOTE]
> CBC 模式加密 + HMAC 驗證內容標頭是相同的方式，不論是否演算法實作由提供 Windows CNG 或由受管理的 SymmetricAlgorithm 和 KeyedHashAlgorithm 型別。 這可讓在不同的作業系統上執行的應用程式可靠地產生相同的內容標頭，即使演算法的實作不同的作業系統。 （實際上，KeyedHashAlgorithm 不一定要適當的 HMAC。 它可以是任何索引鍵的雜湊演算法類型）。

### <a name="example-3des-192-cbc--hmacsha1"></a>範例：3DES-192-CBC + HMACSHA1

首先，讓 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 192 位元和 |K_H |= 每個指定的演算法的 160 位元。 這會導致 K_E = A219...E2BB 和 K_H = DC4A...在下列範例中的 B464:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

接下來，計算 Enc_CBC (K_E、 IV，"") 3DES-192-CBC 指定 IV = 0 * 和 K_E 如上所述。

結果: = ABB100F81E53E10E

接下來，計算 MAC (K_H，"") 為上述指定 K_H HMACSHA1。

result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

這會產生所驗證的憑證指紋的完整內容標頭加密演算法組 （3DES-192-CBC 加密 + HMACSHA1 驗證），如下所示：

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

元件，如下所示分解：

* 標記 (00 00)

* 區塊加密金鑰長度 (00 00 00 18)

* 區塊密碼區塊大小 (00 00 00 08)

* HMAC 金鑰長度 (00 00 00 14)

* HMAC 摘要大小 (00 00 00 14)

* 區塊編碼器 PRP 輸出 (AB B1-E1 0E) 和

* HMAC PRF 輸出 (76 EB-結束)。

## <a name="galoiscounter-mode-encryption--authentication"></a>Galois/計數器模式加密 + 驗證

內容標頭是由下列元件所組成：

* [16 位元]值為 00 01，也就是使用標記表示 [GCM 加密 + 驗證]。

* [32 位元]（以位元組為單位，big endian） 的對稱區塊加密演算法金鑰長度。

* [32 位元]（以位元組為單位，big endian） 要驗證的加密作業期間使用 nonce 的大小。 (如我們的系統，這固定在 nonce 大小 = 96 位元。)

* [32 位元]對稱區塊加密演算法的區塊大小 （以位元組為單位，big endian）。 (適用於 GCM，這會固定區塊大小 = 128 位元。)

* [32 位元]驗證標記大小 （以位元組為單位，big endian） 驗證的加密函數所產生。 (如我們的系統，這會固定標記大小 = 128 位元。)

* [128 位元]Enc_GCM 的標記 (K_E nonce，"")，這是指定為空字串輸入的對稱區塊加密演算法的輸出，並且其中 nonce 是 96 位元全零向量。

K_E 衍生使用相同的機制，如 CBC 加密 + HMAC 驗證案例所示。 不過，因為這裡沒有任何 K_H，基本上我們 |K_H |= 0 時，演算法會摺疊到和表單下方。

K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-256-gcm"></a>範例：AES-256-GCM

First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

接下來，計算 Enc_GCM 的驗證標記 (K_E nonce，"") AES-256-GCM 指定 nonce = 096 和 K_E 如上所述。

結果: = E7DCCE66DF855A323A6BB7BD7A59BE45

這會產生下列完整的內容標頭：

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

元件，如下所示分解：

* 標記 (00 01)

* 區塊加密金鑰長度 (00 00 00 20)

* nonce 的大小 (00 00 00 0 C)

* 區塊密碼區塊大小 (00 00 00 10)

* 驗證標記大小 (00 00 00 10) 和

* 驗證標籤，從執行中的區塊編碼器 (E7 DC-結束)。
