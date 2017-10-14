---
title: "寬型電腦的原則"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: fde8f75422c9dd84311a65b21e1e38b47fbe0306
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="machine-wide-policy"></a>寬型電腦的原則

<a name="data-protection-configuration-machinewidepolicy"></a>

Windows 上執行時，資料保護系統具有有限的支援設定預設全機器原則的所有的應用程式的使用資料保護。 常見用法是系統管理員可能會想要變更的某些預設設定 （例如演算法或金鑰使用存留期） 而不需要手動更新每個應用程式在電腦上。

>[!WARNING]
> 系統管理員可以設定預設原則，但無法強制執行它。 應用程式開發人員可以永遠會覆寫以自己選擇的任何值。 預設原則只會影響應用程式開發人員尚未指定某些特定設定的外顯值。

## <a name="setting-default-policy"></a>設定預設原則

若要設定預設原則，系統管理員可以設定下列機碼下的系統登錄中的已知的值。

登錄機碼：`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`

如果您在 64 位元作業系統上並想要影響的 32 位元應用程式行為，請記得也設定 Wow6432Node 相當於上述機碼。

支援的值為：

* EncryptionType [字串]-指定哪些演算法適用於資料保護。 此值必須是 「 CNG CBC 」、 「 CNG-GCM 」 或 「 受管理 」，並且會更詳細地描述[下方](#data-protection-encryption-types)。

* DefaultKeyLifetime [DWORD]-指定新產生的索引鍵的存留期。 這個值會指定以天為單位，而且必須 ≥ 7。

* KeyEscrowSinks [字串]-指定將使用金鑰委付的類型。 這個值是以分號分隔清單的金鑰委付接收，其中每個清單中的項目是實作 IKeyEscrowSink 類型的組件限定的名稱。

<a name="data-protection-encryption-types"></a>

### <a name="encryption-types"></a>加密類型

如果 「 CNG CBC"EncryptionType，系統將設定為搭配 Windows CNG 提供服務的真實性使用 CBC 模式對稱的區塊編碼器的機密性和 HMAC (請參閱[指定自訂的 Windows CNG 演算法](overview.md#data-protection-changing-algorithms-cng)如需詳細資訊)。 支援下列的其他值，其中每一個都對應至 CngCbcAuthenticatedEncryptionSettings 類型上的屬性：

* EncryptionAlgorithm [字串]-了解的 CNG 對稱的區塊加密演算法的名稱。 此演算法會 CBC 模式中開啟。

* EncryptionAlgorithmProvider [字串]-這可能會產生 EncryptionAlgorithm 演算法的 CNG 提供者實作的名稱。

* EncryptionAlgorithmKeySize [DWORD]-要衍生對稱的區塊加密演算法的金鑰長度 （以位元）。

* HashAlgorithm [字串]-了解 CNG 的雜湊演算法的名稱。 此演算法將 HMAC 模式中開啟。

* HashAlgorithmProvider [字串]-這可能會產生 HashAlgorithm 演算法的 CNG 提供者實作的名稱。

如果 EncryptionType 為 「 CNG GCM"，系統將會設定用於 Galois/計數器模式對稱的區塊編碼器機密性與驗證與 Windows CNG 所提供的服務 (請參閱[指定自訂的 Windows CNG 演算法](overview.md#data-protection-changing-algorithms-cng)如需詳細資訊)。 支援下列的其他值，其中每一個都對應至 CngGcmAuthenticatedEncryptionSettings 類型上的屬性：

* EncryptionAlgorithm [字串]-了解的 CNG 對稱的區塊加密演算法的名稱。 此演算法將 Galois/計數器模式中開啟。

* EncryptionAlgorithmProvider [字串]-這可能會產生 EncryptionAlgorithm 演算法的 CNG 提供者實作的名稱。

* EncryptionAlgorithmKeySize [DWORD]-要衍生對稱的區塊加密演算法的金鑰長度 （以位元）。

如果 EncryptionType 為 「 受管理 」，系統將會設定要用於受管理的 SymmetricAlgorithm 機密性和 KeyedHashAlgorithm 的真實性 (請參閱[指定自訂管理演算法](overview.md#data-protection-changing-algorithms-custom-managed)如需詳細資訊)。 支援下列的其他值，其中每一個都對應至 ManagedAuthenticatedEncryptionSettings 類型上的屬性：

* EncryptionAlgorithmType [字串]-實作 SymmetricAlgorithm 類型的組件限定名稱。

* EncryptionAlgorithmKeySize [DWORD]-要衍生的對稱式加密演算法的金鑰長度 （以位元為單位）。

* ValidationAlgorithmType [字串]-實作 KeyedHashAlgorithm 類型的組件限定名稱。

如果 EncryptionType 有任何其他值 （null 或空白），資料保護系統會在啟動時擲回例外狀況。

>[!WARNING]
> 時設定預設原則設定，包括 EncryptionAlgorithmType、 ValidationAlgorithmType （KeyEscrowSinks） 的型別名稱，類型必須是可用於應用程式。 在實務上，這表示，若為桌面 CLR 上執行的應用程式，其中包含這些類型的組件應該是 Gac 中。 ASP.NET Core 應用程式上執行[.NET Core](https://www.microsoft.com/net/core)，應安裝包含這些類型的套件。
