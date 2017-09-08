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
# <a name="context-headers"></a>內容標頭

<a name=data-protection-implementation-context-headers></a>

## <a name="background-and-theory"></a>背景和理論上

在資料保護系統中，「 金鑰 」 表示的物件，可提供驗證加密服務。 每個索引鍵由唯一識別碼 (GUID)，它會具有演算法的資訊和 entropic 材料。 並非每個索引鍵執行唯一 entropy，但是系統無法強制執行，而且我們也要負責的開發人員可能會藉由修改現有的索引鍵中鑰匙圈的演算法資訊手動變更鑰匙圈。 若要達到指定這些情況下我們安全性需求的資料保護系統具有的概念[加密彈性](http://research.microsoft.com/apps/pubs/default.aspx?id=121045)，可讓安全地使用單一 entropic 值不同的多個密碼編譯演算法。

大部分的系統支援的加密彈性來達成包括一些有關裝載內部之演算法的識別資訊。 此演算法的 OID 通常是適合此。 不過，我們遇到的一個問題是有多種方式可以指定相同的演算法:"AES 」 (CNG) 和 managed Aes、 AesManaged、 AesCryptoServiceProvider、 AesCng 和 RijndaelManaged （要有特定的參數） 類別所有實際上都相同件事，所以我們需要維護所有這些至正確的 OID 的對應。 如果開發人員想要提供自訂的演算法 （或甚至是另一個實作 AES ！），他們必須告訴 OID。 此額外的註冊步驟可讓系統設定特別人痛苦。

逐步執行，我們決定我們已從方向錯誤接近問題。 OID 告訴您此演算法的是，但我們不實際在意這。 如果我們需要兩個不同的演算法中安全地使用單一 entropic 值，它不需要讓我們知道演算法實際上是。 什麼我們實際關注的是它們的行為。 任何不錯對稱的區塊加密演算法也是強式的虛擬隨機排列 (PRP): 修正 （索引鍵，鏈結模式，IV，純文字） 的輸入，然後加密文字輸出會與過度機率是不同於任何其他對稱的區塊編碼器演算法提供相同的輸入。 同樣地，任何不錯的金鑰雜湊函式也是強式的虛擬隨機函式 (PRF)，而且指定的一組固定的輸入它的輸出會相當不同於任何其他索引鍵的雜湊函式。

我們使用強式 PRPs 和 PRFs 的概念來建立的內容標頭。 此內容標頭實質上會當做穩定的憑證指紋的演算法，對於任何給定的作業，用於透過並提供資料保護系統所需的加密彈性。 這個標頭是可重現和稍後會使用一部分[子機碼衍生的程序](subkeyderivation.md#data-protection-implementation-subkey-derivation)。 有兩種不同的方式來建置內容標頭，根據的作業模式基礎的演算法而定。

## <a name="cbc-mode-encryption--hmac-authentication"></a>CBC 模式加密 + HMAC 驗證

<a name=data-protection-implementation-context-headers-cbc-components></a>

內容標頭包含下列元件：

* [16 位元]值為 00 00，也就是使用標記表示"CBC 加密 + HMAC 驗證 」。

* [32 位元]（以位元組為單位，big endian） 對稱的區塊加密演算法的金鑰長度。

* [32 位元]對稱的區塊加密演算法的區塊大小 （以位元組為單位，big endian）。

* [32 位元]金鑰長度 （以位元組為單位，big endian） 的 HMAC 演算法。 （目前的索引鍵的大小一律符合摘要大小。）

* [32 位元]摘要的大小 （以位元組為單位，big endian） 的 HMAC 演算法。

* EncCBC (K_E、 IV，"")，這是指定為空字串輸入對稱的區塊加密演算法的輸出和 IV 所在的全部為零的向量。 下面會描述 K_E 建構。

* MAC (K_H，"")，這是指定為空字串輸入的 HMAC 演算法的輸出。 下面會描述 K_H 建構。

在理想情況下，我們無法 K_E 和 K_H 傳遞全部為零的向量。 不過，我們想要避免這種情況，基礎的演算法檢查存在的弱式金鑰之前執行任何作業 （尤其是 DES 和 3DES） 無法讓您使用的簡單或可重複的模式，像全部為零的向量。

相反地，我們使用 NIST SP800 108 KDF 計數器模式中 (請參閱[NIST SP800 108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)、 秒。 5.1) 零長度的索引鍵、 標籤和與內容為基礎的 PRF HMACSHA512。 我們衍生 |K_E |+ |K_H |位元組的輸出，然後分解結果 K_E 和 K_H 本身。 在數學上，這被表示，如下所示。

(K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")

### <a name="example-aes-192-cbc--hmacsha256"></a>範例： 192 位 AES-CBC + HMACSHA256

例如，請考慮對稱的區塊加密演算法是 AES-CBC-192 並驗證演算法是 HMACSHA256 的情況。 系統會產生內容標頭使用下列步驟。

首先，讓 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 192 位元和 |K_H |= 每個指定的演算法的 256 位元。 這會導致 K_E = 5BB6...21DD 和 K_H = A04A...下例會 00A9:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
   61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
   49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
   B7 92 3D BF 59 90 00 A9
   ```

接下來，計算 Enc_CBC (K_E、 IV，"") 192 位 AES-CBC 指定 IV = 0 * 和上述 K_E。

結果: = F474B1872B3B53E4721DE19C0841DB6F

接下來，計算 MAC (K_H，"") 的 HMACSHA256 上述指定 K_H。

結果: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

這會產生下列的完整內容標頭：

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
   00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
   DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
   8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
   22 0C
   ```

此內容標頭是已驗證的加密演算法組 （AES-CBC-192 加密 + HMACSHA256 驗證） 的憑證指紋。 所述的元件[上方](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components)是：

* 標記 (00 00)

* 區塊的加密金鑰長度 (00 00 00 18)

* 區塊 cipher 區塊大小 (00 00 00 10)

* HMAC 金鑰長度 (00 00 00 20)

* HMAC 摘要大小 (00 00 00 20)

* 區塊編碼器 PRP 輸出 (F4 74-DB 6F) 和

* HMAC PRF 輸出 (D4 79-結束)。

> [!NOTE]
> CBC 模式加密 + HMAC 驗證內容標頭建立相同的方式，不論透過 Windows CNG 或 managed SymmetricAlgorithm 和 KeyedHashAlgorithm 型別，是否提供演算法實作。 這可讓在不同作業系統上執行的應用程式能夠可靠地產生相同的內容標頭，即使作業系統之間不同的演算法的實作。 （在實務上，KeyedHashAlgorithm 不必是適當的 HMAC。 它可以是任何索引雜湊演算法類型。）

### <a name="example-3des-192-cbc--hmacsha1"></a>範例： 3DES-192 位 CBC + HMACSHA1

首先，讓 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 192 位元和 |K_H |= 160 位元，每個指定的演算法。 這會導致 K_E = A219...E2BB 和 K_H = DC4A...下例會 B464:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
   61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
   D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
   ```

接下來，計算 Enc_CBC (K_E、 IV，"") 3DES-192 位 CBC 指定 IV = 0 * 和上述 K_E。

結果: = ABB100F81E53E10E

接下來，計算 MAC (K_H，"") 的上述指定 K_H HMACSHA1。

結果: = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

這會產生完整的內容標頭的已驗證的指紋即加密演算法組 （3DES-192 位 CBC 加密 + HMACSHA1 驗證），如下所示：

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
   00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
   03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
   ```

如下所示分解元件：

* 標記 (00 00)

* 區塊的加密金鑰長度 (00 00 00 18)

* 區塊 cipher 區塊大小 (00 00 00 08)

* HMAC 金鑰長度 (00 00 00 14)

* HMAC 摘要大小 (00 00 00 14)

* 區塊編碼器 PRP 輸出 (AB B1-E1 0E) 和

* HMAC PRF 輸出 (76 EB-結束)。

## <a name="galoiscounter-mode-encryption--authentication"></a>Galois/計數器模式加密 + 驗證

內容標頭包含下列元件：

* [16 位元]值為 00 01，也就是使用標記表示"GCM 加密 + 驗證 」。

* [32 位元]（以位元組為單位，big endian） 對稱的區塊加密演算法的金鑰長度。

* [32 位元]Nonce 大小 （以位元組為單位，big endian） 已驗證的加密作業期間使用。 (我們系統中，這會固定為 nonce 大小 = 96 位元。)

* [32 位元]對稱的區塊加密演算法的區塊大小 （以位元組為單位，big endian）。 (適用於 GCM，這會固定為區塊大小 = 128 位元。)

* [32 位元]驗證標記大小 （以位元組為單位，big endian） 已驗證的加密函數所產生。 (我們系統中，這會固定為標記大小 = 128 位元。)

* [128 位元]Enc_GCM 標記 (K_E nonce，"")，這是指定為空字串輸入對稱的區塊加密演算法的輸出和 nonce 所在的 96 位元全部為零的向量。

K_E 衍生使用相同的機制，如 CBC 加密 + HMAC 驗證案例所示。 不過，因為沒有 K_H 正在播放這裡，我們基本上都擁有 |K_H |= 0，演算法會摺疊至下列表單。

K_E = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")

### <a name="example-aes-256-gcm"></a>範例： AES 256 GCM

首先，讓 K_E = SP800_108_CTR (prf = HMACSHA512，索引鍵 =""，標籤 =""，內容 ="")，其中 |K_E |= 256 位元。

K_E: = 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

接下來，計算 Enc_GCM 的驗證標記 (K_E nonce，"") AES-256-GCM 提供 nonce = 096 和 K_E 上述。

結果: = E7DCCE66DF855A323A6BB7BD7A59BE45

這會產生下列的完整內容標頭：

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
   00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
   BE 45
   ```

如下所示分解元件：

   * 標記 (00 01)

   * 區塊的加密金鑰長度 (00 00 00 20)

   * nonce 大小 (00 00 00 0 C)

   * 區塊 cipher 區塊大小 (00 00 00 10)

   * 驗證標記大小 (00 00 00 10) 和

   * 執行區塊編碼器的驗證標記 (E7 DC-結束)。
