---
title: 在 ASP.NET Core 支援的資料保護整部機器的原則
author: rick-anderson
description: 了解支援的設定預設的全機器原則針對取用 ASP.NET Core 資料保護的所有應用程式。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: c2d5760cd18f4e3ecaf0261f36414c9298e3f4c5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>在 ASP.NET Core 支援的資料保護整部機器的原則

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

Windows 上執行時，資料保護系統具有有限的支援設定預設的全機器原則針對取用 ASP.NET Core 資料保護的所有應用程式。 常見用法是系統管理員可能會想要變更預設設定，例如使用的演算法或金鑰存留期，而不需要手動更新每個應用程式在電腦上。

> [!WARNING]
> 系統管理員可以設定預設原則，但無法強制執行它。 應用程式開發人員可以永遠會覆寫以自己選擇的任何值。 預設原則只會影響應用程式開發人員尚未在其中指定明確的值設定。

## <a name="setting-default-policy"></a>設定預設原則

若要設定預設原則，系統管理員可以設定下列登錄機碼下的系統登錄中的已知的值：

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

如果您在 64 位元作業系統上並想要影響的 32 位元應用程式行為，請記得設定 Wow6432Node 相當於上述機碼。

支援的值如下所示。

| 值              | 類型   | 描述 |
| ------------------ | :----: | ----------- |
| EncryptionType     | 字串 | 指定哪些演算法適用於資料保護。 值必須是 CNG CBC、 CNG GCM 或受管理，並且會在下面將更多詳細描述。 |
| DefaultKeyLifetime | DWORD  | 指定新產生的索引鍵的存留期。 指定以天為單位的值，而且必須是 > = 7。 |
| KeyEscrowSinks     | 字串 | 指定類型所使用的金鑰委付。 值是以分號分隔清單的金鑰委付接收，清單中的每個項目所在的組件限定名稱之類型的實作[IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink)。 |

## <a name="encryption-types"></a>加密類型

如果 EncryptionType CNG CBC，系統設定成搭配 Windows CNG 提供服務的真實性使用 CBC 模式對稱的區塊編碼器的機密性和 HMAC (請參閱[指定自訂的 Windows CNG 演算法](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)的詳細資料）。 支援下列的其他值，其中每一個都對應至 CngCbcAuthenticatedEncryptionSettings 類型上的屬性。

| 值                       | 類型   | 描述 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | 字串 | 了解的 CNG 對稱的區塊加密演算法的名稱。 CBC 模式中開啟此演算法。 |
| EncryptionAlgorithmProvider | 字串 | 可以產生 EncryptionAlgorithm 演算法的 CNG 提供者實作的名稱。 |
| EncryptionAlgorithmKeySize  | DWORD  | 要衍生對稱的區塊加密演算法的金鑰長度 （以位元為單位）。 |
| HashAlgorithm               | 字串 | 了解 CNG 的雜湊演算法名稱。 此演算法 HMAC 模式中開啟。 |
| HashAlgorithmProvider       | 字串 | 可以產生 HashAlgorithm 演算法的 CNG 提供者實作的名稱。 |

如果 EncryptionType CNG GCM，系統會設定要用於 Galois/計數器模式對稱的區塊編碼器機密性與驗證與 Windows CNG 所提供的服務 (請參閱[指定自訂的 Windows CNG 演算法](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)如需詳細資訊）。 支援下列的其他值，其中每一個都對應至 CngGcmAuthenticatedEncryptionSettings 類型上的屬性。

| 值                       | 類型   | 描述 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | 字串 | 了解的 CNG 對稱的區塊加密演算法的名稱。 此演算法 Galois/計數器模式中開啟。 |
| EncryptionAlgorithmProvider | 字串 | 可以產生 EncryptionAlgorithm 演算法的 CNG 提供者實作的名稱。 |
| EncryptionAlgorithmKeySize  | DWORD  | 要衍生對稱的區塊加密演算法的金鑰長度 （以位元為單位）。 |

如果 EncryptionType 為 Managed，系統會設定要用於受管理的 SymmetricAlgorithm 機密性和 KeyedHashAlgorithm 的真實性 (請參閱[指定自訂管理演算法](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms)如需詳細資訊)。 支援下列的其他值，其中每一個都對應至 ManagedAuthenticatedEncryptionSettings 類型上的屬性。

| 值                      | 類型   | 描述 |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | 字串 | 實作 SymmetricAlgorithm 之型別的組件限定名稱。 |
| EncryptionAlgorithmKeySize | DWORD  | 要衍生的對稱式加密演算法的金鑰長度 （以位元為單位）。 |
| ValidationAlgorithmType    | 字串 | 實作 KeyedHashAlgorithm 之型別的組件限定名稱。 |

如果 EncryptionType null 以外的任何其他值或空的資料保護系統會擲回例外狀況在啟動時。

> [!WARNING]
> 時設定預設原則設定，包括 EncryptionAlgorithmType、 ValidationAlgorithmType （KeyEscrowSinks） 的型別名稱，類型必須是可用的應用程式。 這表示，如桌面 CLR 上執行的應用程式，包含這些類型的組件應會出現在全域組件快取 (GAC) 中。 針對.NET 核心上執行的 ASP.NET Core 應用程式，應安裝包含這些類型的封裝。
