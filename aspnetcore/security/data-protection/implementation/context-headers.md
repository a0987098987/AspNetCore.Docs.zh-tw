---
title: ASP.NET Core 中的內容標頭
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護內容標頭的執行詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 518423f5df93924d3df144994e4beb1755cd0bfc
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666576"
---
# <a name="context-headers-in-aspnet-core"></a>ASP.NET Core 中的內容標頭

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>背景和理論

在資料保護系統中，「金鑰」是指可提供已驗證加密服務的物件。 每個金鑰都是以唯一識別碼（GUID）來識別，而且它會攜帶其演算法資訊和 entropic 材質。 它的目的是要讓每個金鑰都具有獨特的熵，但是系統無法強制執行，而且我們也需要將金鑰環中現有金鑰的演算法資訊修改為手動變更金鑰信號的開發人員。 為了達到我們的安全性需求，在這些情況下，資料保護系統具有[密碼](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/)編譯彈性的概念，可讓您在多個密碼編譯演算法中安全使用單一 entropic 值。

大部分支援密碼編譯靈活性的系統，都是在裝載內包含關於演算法的一些識別資訊。 演算法的 OID 通常是很好的候選。 不過，我們遇到的一個問題是，有多種方式可以指定相同的演算法：「AES」（CNG）和受管理的 Aes、AesManaged、AesCryptoServiceProvider、AesCng 和 RijndaelManaged （指定的特定參數）類別實際上是相同的而且我們需要維護所有這些的對應到正確的 OID。 如果開發人員想要提供自訂演算法（或甚至是 AES！的另一個實作為），他們必須告訴我們其 OID。 這個額外的註冊步驟讓系統設定特別困難。

回頭執行後，我們決定我們已從錯誤的方向中接近問題。 OID 會告訴您演算法的意義，但我們並不會特別在意這一點。 如果我們需要以兩種不同的演算法安全地使用單一 entropic 值，我們就不需要知道演算法實際上是什麼。 我們真正在意的是它們的表現方式。 任何適當的對稱式區塊加密演算法也是強式隨機排列（PRP）：修正輸入（金鑰、連結模式、IV、純文字），而加密文字輸出的機率會與任何其他對稱區塊密碼不同提供相同輸入的演算法。 同樣地，任何適當的索引鍵雜湊函式也是強式的偽虛擬函式（PRF），而且在指定固定的輸入集時，其輸出將回應非常正面與任何其他索引雜湊函數不同。

我們使用此強式 Prp 和 PRFs 的概念來建立內容標頭。 此內容標頭基本上會作為任何指定作業所使用之演算法的穩定指紋，並提供資料保護系統所需的密碼編譯靈活性。 此標頭是可重現的，稍後會用來做為子機碼[衍生進程](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)的一部分。 有兩種不同的方式可以建立內容標頭，視基礎演算法的作業模式而定。

## <a name="cbc-mode-encryption--hmac-authentication"></a>CBC 模式加密 + HMAC 驗證

<a name="data-protection-implementation-context-headers-cbc-components"></a>

內容標頭是由下列元件所組成：

* [16 位]值 00 00，這是表示「CBC 加密 + HMAC 驗證」的標記。

* [32 位]對稱區塊加密演算法的索引鍵長度（以位元組為單位）。

* [32 位]對稱區塊加密演算法的區塊大小（以位元組為單位）。

* [32 位]HMAC 演算法的索引鍵長度（以位元組為單位）。 （目前金鑰大小一律符合摘要大小）。

* [32 位]HMAC 演算法的摘要大小（以位元組為單位）。

* EncCBC （K_E，IV，""），這是對稱式區塊加密演算法的輸出，指定空白字串輸入，其中 IV 是全零向量。 K_E 的結構如下所述。

* MAC （K_H，""），這是指定空字串輸入的 HMAC 演算法輸出。 K_H 的結構如下所述。

在理想的情況下，我們可以針對 K_E 和 K_H 傳遞所有-零的向量。 不過，我們想要避免基礎演算法在執行任何作業之前檢查弱式金鑰是否存在（尤其是 DES 和3DES），這種情況會使用簡單或可重複的模式（例如全零向量）來排除。

相反地，我們使用計數器模式的 NIST SP800-108 KDF （請參閱[NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，Sec. 5.1），其長度為零的索引鍵、標籤和內容，以及 HMACSHA512 作為基礎 PRF。 我們衍生 |K_E |+ |K_H |輸出的位元組數，然後將結果分解成 K_E 並 K_H 本身。 這是以數學方式表示，如下所示。

（K_E | |K_H） = SP800_108_CTR （prf = HMACSHA512，key = ""，label = ""，coNtext = ""）

### <a name="example-aes-192-cbc--hmacsha256"></a>範例： AES-192-CBC + HMACSHA256

例如，假設對稱區塊加密演算法是 AES-192-CBC，而驗證演算法是 HMACSHA256 的情況。 系統會使用下列步驟來產生內容標頭。

首先，let （K_E | |K_H） = SP800_108_CTR （prf = HMACSHA512，key = ""，label = ""，coNtext = ""），where |K_E |= 192 位 and |K_H |= 每個指定演算法256位。 這會導致 K_E = 5BB6。21DD 和 K_H = A04A。下列範例中的00A9：

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

接下來，計算 Enc_CBC （K_E，IV，""）的 AES-192-CBC 指定 IV = 0 *，並 K_E 如上所示。

result： = F474B1872B3B53E4721DE19C0841DB6F

接下來，計算所提供的 HMACSHA256 K_H 的 MAC （K_H，""），如上所示。

result： = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

這會產生以下的完整內容標頭：

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

此內容標頭是已驗證加密演算法配對的指紋（AES-192-CBC 加密 + HMACSHA256 驗證）。 [上述](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components)元件如下所述：

* 標記（00 00）

* 區塊密碼金鑰長度（00 00 00 18）

* 區塊加密區塊大小（00 00 00 10）

* HMAC 金鑰長度（00 00 00 20）

* HMAC 摘要大小（00 00 00 20）

* 區塊密碼 PRP 輸出（F4 74-DB 6F）和

* HMAC PRF 輸出（D4 79-end）。

> [!NOTE]
> 不論演算法實架構是由 Windows CNG 還是由 managed System.security.cryptography.symmetricalgorithm 和 KeyedHashAlgorithm 類型所提供，CBC 模式加密 + HMAC 驗證內容標頭的建立方式都相同。 這可讓在不同作業系統上執行的應用程式可靠地產生相同的內容標頭，即使作業系統的演算法不同也是一樣。 （實際上，KeyedHashAlgorithm 不一定要是適當的 HMAC。 它可以是任何金鑰雜湊演算法類型）。

### <a name="example-3des-192-cbc--hmacsha1"></a>範例： 3DES-192-CBC + HMACSHA1

首先，let （K_E | |K_H） = SP800_108_CTR （prf = HMACSHA512，key = ""，label = ""，coNtext = ""），where |K_E |= 192 位 and |K_H |= 每個指定演算法160位。 這會導致 K_E = A219。E2BB 和 K_H = DC4A。下列範例中的 B464：

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

接下來，計算 Enc_CBC （K_E，IV，""）的 3DES-192-CBC 指定 IV = 0 *，並 K_E 如上所示。

result： = ABB100F81E53E10E

接下來，計算所提供的 HMACSHA1 K_H 的 MAC （K_H，""），如上所示。

result： = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

這會產生完整的內容標頭，這是已驗證加密演算法配對的指紋（3DES-192-CBC 加密 + HMACSHA1 驗證），如下所示：

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

元件會依照下列方式細分：

* 標記（00 00）

* 區塊密碼金鑰長度（00 00 00 18）

* 區塊加密區塊大小（00 00 00 08）

* HMAC 金鑰長度（00 00 00 14）

* HMAC 摘要大小（00 00 00 14）

* 區塊密碼 PRP 輸出（AB B1-E1 0E）和

* HMAC PRF 輸出（76 EB-end）。

## <a name="galoiscounter-mode-encryption--authentication"></a>Galois/計數器模式加密 + 驗證

內容標頭是由下列元件所組成：

* [16 位]值 00 01，這是表示「GCM 加密 + 驗證」的標記。

* [32 位]對稱區塊加密演算法的索引鍵長度（以位元組為單位）。

* [32 位]已驗證的加密作業期間使用的 nonce 大小（以位元組為單位）。 （在我們的系統中，這是在 nonce 大小 = 96 位）。

* [32 位]對稱區塊加密演算法的區塊大小（以位元組為單位）。 （對於 GCM 而言，這是在區塊大小 = 128 位）。

* [32 位]已驗證的加密函式所產生的驗證標記大小（以位元組為單位）。 （在我們的系統中，這是在標記大小 = 128 位）。

* [128 位]Enc_GCM （K_E，nonce，""）的標籤，這是對稱式區塊密碼演算法的輸出，指定空白字串輸入，其中 nonce 是96位的全部-零向量。

K_E 是使用與 CBC 加密 + HMAC 驗證案例中相同的機制所衍生。 不過，由於這裡沒有 K_H 播放，我們基本上有 |K_H |= 0，而演算法折迭為下列表單。

K_E = SP800_108_CTR （prf = HMACSHA512，key = ""，label = ""，coNtext = ""）

### <a name="example-aes-256-gcm"></a>範例： AES-256-GCM

首先，let K_E = SP800_108_CTR （prf = HMACSHA512，key = ""，label = ""，coNtext = ""），where |K_E |= 256 位。

K_E： = 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

接下來，計算 Enc_GCM （K_E，nonce，""）的驗證標記，以取得指定 nonce = 096 和 K_E 如上所述的 AES-256-GCM。

result： = E7DCCE66DF855A323A6BB7BD7A59BE45

這會產生以下的完整內容標頭：

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

元件會依照下列方式細分：

* 標記（00 01）

* 區塊密碼金鑰長度（00 00 00 20）

* nonce 大小（00 00 00 0C）

* 區塊加密區塊大小（00 00 00 10）

* 驗證標記大小（00 00 00 10）和

* 執行區塊密碼的驗證標記（E7 DC 端）。
