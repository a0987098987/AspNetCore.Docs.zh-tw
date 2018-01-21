---
title: "子機碼衍生和已驗證的加密"
author: rick-anderson
description: "本文件說明 ASP.NET Core 資料保護的實作詳細資料衍生子機碼，驗證加密。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 3927678b7b67b0e521a961e363200bdfe0bdaeb3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="subkey-derivation-and-authenticated-encryption"></a>子機碼衍生和已驗證的加密

<a name="data-protection-implementation-subkey-derivation"></a>

會包含某種形式的 entropy 鑰匙圈中大部分的索引鍵，而且必須演算資訊指出"CBC 模式加密 + HMAC 驗證 」 或 「 GCM 加密 + 驗證 」。 在這些情況下，我們將內嵌的 entropy 稱為主要金鑰處理內容 （或金鑰管理），此索引鍵，和我們執行金鑰衍生功能來衍生金鑰會用於實際的密碼編譯作業。

> [!NOTE]
> 索引鍵是抽象類別，並自訂實作可能無法運作，如下所示。 如果索引鍵提供自己的實作`IAuthenticatedEncryptor`而不是使用我們的內建處理站的其中一個，這一節所述的機制就不再適用。

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>其他的已驗證的資料和子機碼衍生

`IAuthenticatedEncryptor`介面做為所有已驗證的加密作業的核心介面。 其`Encrypt`方法會採用兩個緩衝區： 純文字和 additionalAuthenticatedData (AAD)。 純文字內容流量未變更的呼叫`IDataProtector.Protect`，但 AAD 由系統產生，並包含三個元件：

1. 32 位元 magic 標頭 09 F0 C9 F0 可識別此版本的資料保護系統。

2. 128 位元的金鑰識別碼。

3. 可變長度字串所組成的用途鏈結建立`IDataProtector`，正在執行此作業。

因為 AAD 都是唯一的所有三個元件的 tuple，我們可以用它來衍生自金鑰管理的新機碼，而不是使用金鑰管理本身在所有的密碼編譯作業。 若要每次呼叫`IAuthenticatedEncryptor.Encrypt`，下列的金鑰衍生處理序會發生：

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

在這裡，我們正在撥打 NIST SP800 108 KDF 計數器模式中 (請參閱[NIST SP800 108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，秒 5.1) 使用下列參數：

* 金鑰衍生金鑰 (KDK) = K_M

* PRF = HMACSHA512

* 標籤 = additionalAuthenticatedData

* 內容 = contextHeader | |keyModifier

內容標頭是可變長度，基本上是我們要為其衍生 K_E 與 K_H 演算法的憑證指紋。 索引鍵的修飾詞是隨機產生的每個呼叫的 128 位元字串`Encrypt`，並提供服務，以確保與過度機率，KE 和 KH 是唯一的這項特定的驗證加密作業，即使所有其他輸入 KDF 是常數。

CBC 模式加密 + HMAC 驗證作業，|K_E |這是對稱的區塊加密金鑰的長度和 |K_H |是摘要的大小 HMAC 常式。 GCM 加密 + 驗證作業 |K_H |= 0。

## <a name="cbc-mode-encryption--hmac-validation"></a>CBC 模式加密 + HMAC 驗證

一旦 K_E 透過上述的機制來產生，我們會產生隨機初始化向量，並執行 encipher 純文字的對稱的區塊加密演算法。 初始化向量和加密文字然後透過具有索引鍵來產生 MAC K_H 初始化 HMAC 常式執行 此程序以及傳回值會以圖形方式以下表示。

![CBC 模式的程序，並傳回](subkeyderivation/_static/cbcprocess.png)

*輸出: = keyModifier | |iv | |E_cbc （K_E，iv，資料） | |HMAC (K_H、 iv | |E_cbc （K_E，iv，資料）)*

> [!NOTE]
> `IDataProtector.Protect`實作將[magic 標頭和索引鍵的識別碼，前面加上](authenticated-encryption-details.md)輸出傳回給呼叫者之前。 因為 magic 標頭和索引鍵識別碼是隱含的一部分[AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad)，因為索引鍵的修飾詞做為輸入至 KDF 回饋，這也表示，每個單一位元組的最後一個傳回的內容由 MAC 驗證

## <a name="galoiscounter-mode-encryption--validation"></a>Galois/計數器模式加密 + 驗證

一旦 K_E 透過上述的機制來產生，我們會產生隨機的 96 位元 nonce，並執行 encipher 純文字，並產生 128 位元的驗證標記對稱的區塊加密演算法。

![GCM 模式程序，並傳回](subkeyderivation/_static/galoisprocess.png)

*輸出: = keyModifier | |nonce | |E_gcm （K_E，nonce，資料） | |authTag*

> [!NOTE]
> 即使 GCM 原生支援 AAD 的概念，我們正在仍饋送 AAD 只以原始 KDF，選擇將其 AAD 參數，傳遞至 GCM 的空字串。 這是一體兩面。 首先，[支援靈活度](context-headers.md#data-protection-implementation-context-headers)我們永遠不會想要直接將 K_M 作為加密金鑰。 此外，GCM 會加諸其輸入上非常嚴格的唯一性。 GCM 加密常式為曾經叫用在兩個或更多相異的可能性設定具有相同 （金鑰、 nonce） 的輸入資料組不能超過 2 ^32。 如果我們修正 K_E 我們無法執行超過 2 ^32 的加密作業之前，我們執行與的 2 ^-32 限制。 這看起來像是非常大量的作業，但高流量的網頁伺服器可以透過 4 10 億個要求，這些機碼正常的存留期間內只天數。 若要保持相容的 2 ^-32 機率限制，我們將繼續使用 128 位元金鑰的修飾詞和 96 位元 nonce，徹底擴充任何給定 K_M 的可用作業計數。 我們在設計的簡易性共用 CBC 及 GCM 作業之間的 KDF 程式碼路徑，因為 AAD KDF 中已被視為不是需要將其轉寄給 GCM 常式。
