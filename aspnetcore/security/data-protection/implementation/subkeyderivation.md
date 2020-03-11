---
title: ASP.NET Core 中的子機碼衍生和驗證的加密
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護子機碼衍生和已驗證加密的執行詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: bbfde378755b09cd5b1217b8cf66249b9fa1d6ad
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660549"
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>ASP.NET Core 中的子機碼衍生和驗證的加密

<a name="data-protection-implementation-subkey-derivation"></a>

金鑰通道中的大部分金鑰都會包含某種形式的熵，而且會有一些演算法資訊，說明「CBC 模式加密 + HMAC 驗證」或「GCM 加密 + 驗證」。 在這些情況下，我們會將內嵌的熵稱為此金鑰的主要金鑰資料（或公里），而我們會執行金鑰衍生函式來衍生金鑰，以用於實際的密碼編譯作業。

> [!NOTE]
> 金鑰是抽象的，自訂的執行可能不會如下所示。 如果金鑰提供自己的 `IAuthenticatedEncryptor` 執行，而不是使用其中一個內建工廠，本節所述的機制將不再適用。

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>其他已驗證的資料和子機碼衍生

`IAuthenticatedEncryptor` 介面會作為所有已驗證之加密作業的核心介面。 其 `Encrypt` 方法會採用兩個緩衝區：純文字和 additionalAuthenticatedData （AAD）。 純文字內容不會變更 `IDataProtector.Protect`的呼叫，但是 AAD 是由系統產生，並由三個元件所組成：

1. 32-bit 魔術 header 09 F0 C9 F0，可識別此版本的資料保護系統。

2. 128位金鑰識別碼。

3. 從目的鏈形成的可變長度字串，其會建立執行此作業的 `IDataProtector`。

因為 AAD 對於這三個元件的元組而言是唯一的，所以我們可以使用它來從公里衍生新的金鑰，而不是在所有的密碼編譯作業中使用公里本身。 對於 `IAuthenticatedEncryptor.Encrypt`的每個呼叫，會進行下列金鑰衍生程式：

（K_E，K_H） = SP800_108_CTR_HMACSHA512 （K_M，AAD，coNtextHeader | | keyModifier）

在這裡，我們會在計數器模式下呼叫 NIST SP800-108 KDF （請參閱[NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，Sec. 5.1），並提供下列參數：

* 金鑰衍生金鑰（KDK） = K_M

* PRF = HMACSHA512

* 標籤 = additionalAuthenticatedData

* coNtext = coNtextHeader | |keyModifier

內容標頭的長度是可變的，基本上是做為衍生 K_E 和 K_H 的演算法指紋。 金鑰修飾詞是在每次呼叫 `Encrypt` 時隨機產生的128位字串，可確保在此特定驗證加密作業中，KH 是唯一的，即使 KDF 的所有其他輸入都是常數。

針對 CBC 模式加密 + HMAC 驗證作業，|K_E |這是對稱區塊加密金鑰的長度，而是 |K_H |這是 HMAC 常式的摘要大小。 針對 GCM 加密 + 驗證作業，|K_H |= 0。

## <a name="cbc-mode-encryption--hmac-validation"></a>CBC 模式加密 + HMAC 驗證

透過上述機制產生 K_E 之後，我們會產生隨機的初始化向量，並執行對稱式區塊加密演算法來加密純文字。 接著，初始化向量和加密文字會透過以金鑰 K_H 初始化的 HMAC 常式來執行，以產生 MAC。 這個進程和傳回值的呈現方式如下圖所示。

![CBC 模式處理和傳回](subkeyderivation/_static/cbcprocess.png)

*output： = keyModifier | |iv | |E_cbc （K_E，iv，資料） | |HMAC （K_H，iv | |E_cbc （K_E，iv，資料））*

> [!NOTE]
> `IDataProtector.Protect` 的執行會在將[魔術標頭和金鑰識別碼](xref:security/data-protection/implementation/authenticated-encryption-details)傳回給呼叫端之前，先在輸出前面加上。 由於魔術標頭和金鑰識別碼是[AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad)的隱含部分，而且因為金鑰修飾詞是以輸入的形式送到 KDF，這表示最後傳回的承載的每一個位元組都會由 MAC 驗證。

## <a name="galoiscounter-mode-encryption--validation"></a>Galois/計數器模式加密 + 驗證

透過上述機制產生 K_E 之後，我們會產生隨機的96位 nonce，並執行對稱式區塊加密演算法，以加密純文字並產生128位驗證標記。

![GCM 模式進程和傳回](subkeyderivation/_static/galoisprocess.png)

*output： = keyModifier | |nonce | |E_gcm （K_E，nonce，資料） | |authTag*

> [!NOTE]
> 雖然 GCM 原本就支援 AAD 的概念，但我們仍在將 AAD 只提供給原始 KDF，選擇將空字串傳遞至 GCM 以取得其 AAD 參數。 這種情況的原因是兩折迭。 首先，[為了支援靈活性](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers)，我們不想要直接使用 K_M 做為加密金鑰。 此外，GCM 會對其輸入施加非常嚴格的唯一性需求。 在兩個或多個不同的輸入資料集上，使用相同（金鑰、nonce）配對來叫用 GCM 加密常式的機率，不得超過 2 ^ 32。 如果我們修正 K_E 我們在執行 2 ^-32 限制的 afoul 前，無法執行 2 ^ 32 個以上的加密作業。 這看起來可能像是非常大量的作業，但高流量的 web 伺服器可以只在這些金鑰的正常存留期間，在數天內經歷4000000000個要求。 為了保持符合 2 ^-32 機率限制，我們會繼續使用128位金鑰修飾詞和96位 nonce，以徹底擴充任何指定 K_M 的可用作業計數。 為了簡單起見，我們會在 CBC 與 GCM 作業之間共用 KDF 程式碼路徑，而且由於 AAD 已列入 KDF 中，因此不需要將它轉送到 GCM 常式。
