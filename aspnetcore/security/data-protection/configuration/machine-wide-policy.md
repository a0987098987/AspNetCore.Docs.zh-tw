---
title: 在 ASP.NET Core 支援的資料保護整部機器的原則
author: rick-anderson
description: 了解支援的設定預設的全機器原則針對取用 ASP.NET Core 資料保護的所有應用程式。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 70aaca7afcd3df22cebb4466fbd9845a2277688c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897265"
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>在 ASP.NET Core 支援的資料保護整部機器的原則

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

Windows 上執行時，資料保護系統具有有限的支援設定預設的全機器原則針對取用 ASP.NET Core 資料保護的所有應用程式。 一般概念是系統管理員可能會想要變更預設設定，例如使用的演算法或金鑰的存留期，而不需要手動更新電腦上的每個應用程式。

> [!WARNING]
> 系統管理員可以設定預設原則，但無法強制執行它。 應用程式開發人員一律可以覆寫其中一個自己選擇的任何值。 預設的原則只會影響應用程式開發人員尚未在其中指定明確的值設定。

## <a name="setting-default-policy"></a>設定預設原則

若要設定預設原則，系統管理員可以設定下列登錄機碼下的系統登錄中的已知的值：

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

如果您是在 64 位元作業系統上，而且想要影響 32 位元應用程式的行為，請記得設定適用 Wow6432Node 相當於上述的機碼。

支援的值如下所示。

| 值              | 類型   | 描述 |
| ------------------ | :----: | ----------- |
| EncryptionType     | 字串 | 指定應該使用資料保護的哪些演算法。 值必須是 CNG CBC、 CNG-GCM 或受管理，並詳細說明如下所述。 |
| DefaultKeyLifetime | DWORD  | 指定新產生的索引鍵的存留期。 值會指定以天為單位，且必須是 > = 7。 |
| KeyEscrowSinks     | 字串 | 指定用於金鑰委付的類型。 值是以分號分隔的清單金鑰委付接收器而定，清單中的每個項目所在的組件限定名稱的型別可實作[IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink)。 |

## <a name="encryption-types"></a>加密類型

如果 EncryptionType CNG CBC，系統會設定成搭配 Windows CNG 提供的服務驗證使用 CBC 模式的對稱區塊編碼器的機密性和 HMAC (請參閱[指定自訂的 Windows CNG 演算法](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)的詳細資料）。 支援下列的其他值，其中每個對應於 CngCbcAuthenticatedEncryptionSettings 類型上的屬性。

| 值                       | 類型   | 描述 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | 字串 | 了解 CNG 的對稱區塊加密演算法的名稱。 CBC 模式中開啟此演算法。 |
| EncryptionAlgorithmProvider | 字串 | 可能會產生 EncryptionAlgorithm 的演算法的 CNG 提供者實作的名稱。 |
| EncryptionAlgorithmKeySize  | DWORD  | 要衍生的對稱區塊加密演算法的金鑰長度 （以位元為單位）。 |
| HashAlgorithm               | 字串 | 了解 CNG 的雜湊演算法名稱。 此演算法是以 HMAC 模式開啟。 |
| HashAlgorithmProvider       | 字串 | 可能會產生演算法 HashAlgorithm CNG 提供者實作的名稱。 |

系統如果 EncryptionType CNG GCM，設定要用於 Galois/計數器模式的對稱區塊編碼器機密性與驗證與 Windows CNG 提供的服務 (請參閱[指定自訂的 Windows CNG 演算法](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)如需詳細資訊）。 支援下列的其他值，其中每個對應於 CngGcmAuthenticatedEncryptionSettings 類型上的屬性。

| 值                       | 類型   | 描述 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | 字串 | 了解 CNG 的對稱區塊加密演算法的名稱。 此演算法是以 Galois/計數器模式開啟。 |
| EncryptionAlgorithmProvider | 字串 | 可能會產生 EncryptionAlgorithm 的演算法的 CNG 提供者實作的名稱。 |
| EncryptionAlgorithmKeySize  | DWORD  | 要衍生的對稱區塊加密演算法的金鑰長度 （以位元為單位）。 |

系統如果 EncryptionType 為 Managed，設定要用於受管理的 SymmetricAlgorithm 機密性和 KeyedHashAlgorithm 驗證 (請參閱[指定自訂管理演算法](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms)如需詳細資訊)。 支援下列的其他值，其中每個對應於 ManagedAuthenticatedEncryptionSettings 類型上的屬性。

| 值                      | 類型   | 描述 |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | 字串 | 實作 SymmetricAlgorithm 型別的組件限定名稱。 |
| EncryptionAlgorithmKeySize | DWORD  | 要衍生的對稱式加密演算法的金鑰長度 （以位元為單位）。 |
| ValidationAlgorithmType    | 字串 | 型別的 KeyedHashAlgorithm 的實作組件限定名稱。 |

如果 EncryptionType 具有 null 以外的任何其他值或空的資料保護系統擲回例外狀況在啟動時。

> [!WARNING]
> 設定時牽涉到 EncryptionAlgorithmType、 ValidationAlgorithmType （KeyEscrowSinks） 的型別名稱的預設原則設定，類型必須能夠使用應用程式。 這表示，針對桌面 CLR 上執行的應用程式，包含這些類型的組件應該會出現在全域組件快取 (GAC) 中。 針對.NET Core 上執行的 ASP.NET Core 應用程式，應安裝包含這些類型的封裝。
