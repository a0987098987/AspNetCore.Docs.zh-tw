---
title: 子機碼衍生和驗證的加密，在 ASP.NET Core
author: rick-anderson
description: 了解 ASP.NET Core 資料保護實作詳細資料子機碼衍生和驗證加密。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 37e7b01700e8a6b755b5ed16a9d7d75a9eeb970e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891835"
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>子機碼衍生和驗證的加密，在 ASP.NET Core

<a name="data-protection-implementation-subkey-derivation"></a>

大部分的索引鍵，在 鑰匙圈中會包含某種形式的 entropy，而且必須演算法的資訊指出 CBC 模式加密 + HMAC 驗證 或 GCM 加密 + 驗證。 在這些情況下，我們將內嵌的 entropy 稱為主要金鑰處理內容 （或公里），此索引鍵，而且我們執行金鑰衍生函式來衍生金鑰會用於實際的密碼編譯作業。

> [!NOTE]
> 索引鍵是抽象類別，以及自訂的實作可能無法運作，如下所示。 如果索引鍵能提供自己的實作`IAuthenticatedEncryptor`而不是使用我們的內建 factory 的其中一個，這一節所述的機制就不再適用。

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>其他已驗證的資料和子機碼衍生

`IAuthenticatedEncryptor`介面做為所有已驗證的加密作業的核心介面。 其`Encrypt`方法會採用兩個緩衝區： 純文字和 additionalAuthenticatedData (AAD)。 純文字內容流程不會變更呼叫`IDataProtector.Protect`，但 AAD 由系統產生，並包含三個元件：

1. 32 位元 magic 標頭 09 F0 C9 F0 可識別這個版本的資料保護系統。

2. 128 位元的金鑰識別碼。

3. 可變長度字串，由建立目的鏈結`IDataProtector`，正在執行此作業。

AAD 是針對所有三個元件的元組唯一的因為我們可以使用它從公里衍生新的金鑰，而不是使用金鑰管理本身在所有的密碼編譯作業。 每個呼叫`IAuthenticatedEncryptor.Encrypt`，下列的金鑰衍生程序：

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

在這裡，我們正在撥打 NIST SP800 108 KDF 計數器模式中 (請參閱[NIST SP800 108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，sec 5.1) 使用下列參數：

* 金鑰衍生金鑰 (KDK) = K_M

* PRF = HMACSHA512

* 標籤 = additionalAuthenticatedData

* context = contextHeader || keyModifier

內容標頭是可變長度，基本上會作為指紋，我們要為其衍生 K_E 和 K_H 的演算法。 金鑰的修飾詞是隨機產生的每個呼叫的 128 位元字串`Encrypt`和提供服務，確保與過度負荷，KE 和 KH 是唯一的這個特定的驗證加密作業，即使所有其他輸入至 KDF 保持不變的機率。

CBC 模式加密 + HMAC 驗證作業，|K_E |對稱區塊加密金鑰的長度和 |K_H |是 HMAC 常式摘要大小。 GCM 加密 + 驗證作業 |K_H |= 0。

## <a name="cbc-mode-encryption--hmac-validation"></a>CBC 模式加密 + HMAC 驗證

透過上述的機制產生 K_E 之後, 我們會產生的隨機初始化向量，並執行 encipher 純文字的對稱區塊加密演算法。 初始化向量和加密文字然後執行與索引鍵來產生 MAC K_H 初始化 HMAC 常式 此程序和傳回值是下面以圖形方式表示。

![CBC 模式的程序，並傳回](subkeyderivation/_static/cbcprocess.png)

*output:= keyModifier || iv || E_cbc (K_E,iv,data) || HMAC(K_H, iv || E_cbc (K_E,iv,data))*

> [!NOTE]
> `IDataProtector.Protect`會實作[magic 的標頭和索引鍵的識別碼，前面加上](xref:security/data-protection/implementation/authenticated-encryption-details)輸出傳回給呼叫者之前。 因為神奇的標頭和索引鍵的識別碼都是隱含的一部分[AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad)，做為 KDF 輸入提供給金鑰的修飾詞，因為這也表示，最終傳回裝載的每個位元組由 MAC 驗證

## <a name="galoiscounter-mode-encryption--validation"></a>Galois/計數器模式加密 + 驗證

透過上述的機制產生 K_E 之後, 我們會產生隨機的 96 位元 nonce，並執行 encipher 純文字，並產生 128 位元的驗證標籤的對稱區塊加密演算法。

![GCM 模式程序，並傳回](subkeyderivation/_static/galoisprocess.png)

*輸出: = keyModifier | |nonce | |E_gcm （K_E，nonce 資料） | |authTag*

> [!NOTE]
> 即使 GCM 原生支援 AAD 的概念，我們正在仍提供 AAD 才能原始 KDF 中，選擇其 AAD 參數時，將空字串傳遞至 GCM。 這是兩個方面。 首先，[來支援敏捷度](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers)我們永遠不會想要使用 K_M 直接為加密金鑰。 此外，GCM 來加上其輸入非常嚴格的唯一性。 GCM 加密常式為曾經叫用在兩個或多不同的可能性設定具有相同 （索引鍵，nonce） 的輸入資料組不能超過 2 ^32。 如果我們修正 K_E 我們無法執行多個 2 ^32 的加密作業之前我們進攻的 2 ^-32 限制。 這可能會看起來非常大量的作業，但是高流量網頁伺服器可以透過 4 個要求在 mere 天內，這些機碼的正常存留期內。 若要能持續符合規範的 2 ^-32 機率限制，我們會繼續使用 128 位元金鑰的修飾詞和 96 位元 nonce，徹底擴充任何給定的 K_M 的可用作業計數。 為了簡單起見，設計的我們分享 CBC 和 GCM 作業之間的 KDF 程式碼路徑，因為 AAD KDF 中已被視為不是需要將其轉寄給 GCM 常式。
