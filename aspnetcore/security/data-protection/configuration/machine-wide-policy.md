---
title: ASP.NET Core 中的資料保護全電腦原則支援
author: rick-anderson
description: 瞭解針對使用 ASP.NET Core 資料保護的所有應用程式，設定預設全電腦原則的支援。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 84f54b37dfff3112ea5ca84f931103624cfde90a
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776834"
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>ASP.NET Core 中的資料保護全電腦原則支援

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在 Windows 上執行時，資料保護系統對於使用 ASP.NET Core 資料保護的所有應用程式，設定預設全電腦原則的支援有限。 一般的概念是，系統管理員可能想要變更預設設定（例如使用的演算法或金鑰存留期），而不需要手動更新電腦上的每個應用程式。

> [!WARNING]
> 系統管理員可以設定預設原則，但無法強制執行。 應用程式開發人員一律可以使用自己選擇的其中一個來覆寫任何值。 預設原則只會影響開發人員未指定設定之明確值的應用程式。

## <a name="setting-default-policy"></a>設定預設原則

若要設定預設原則，管理員可以在系統登錄的下列登錄機碼底下設定已知的值：

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

如果您是在64位的作業系統上，而且想要影響32位應用程式的行為，請記得設定上述金鑰的 Wow6432Node 對等用法。

支援的值如下所示。

| 值              | 類型   | 說明 |
| ------------------ | :----: | ----------- |
| EncryptionType     | 字串 | 指定應該使用哪些演算法來保護資料。 此值必須為 CNG-CBC、CNG-GCM 或 Managed，並會在下面詳細說明。 |
| DefaultKeyLifetime | DWORD  | 指定新產生之金鑰的存留期。 此值以天為單位指定，且必須 >= 7。 |
| KeyEscrowSinks     | 字串 | 指定用於金鑰委付的類型。 此值是以分號分隔的金鑰委付接收清單，其中清單中的每個元素都是實[IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink)類型的元件限定名稱。 |

## <a name="encryption-types"></a>加密類型

如果 EncryptionType 是 CNG-CBC，系統會設定為使用 CBC 模式對稱區塊加密來搭配 Windows CNG 所提供的服務（如需更多詳細資料，請參閱[指定自訂 WINDOWS cng 演算法](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)）以取得機密性和 HMAC 的真實性。 下列為支援的額外值，其中每一個都對應至 CngCbcAuthenticatedEncryptionSettings 類型上的屬性。

| 值                       | 類型   | 說明 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | 字串 | CNG 瞭解的對稱式區塊密碼演算法的名稱。 此演算法會在 CBC 模式中開啟。 |
| EncryptionAlgorithmProvider | 字串 | 可以產生演算法 EncryptionAlgorithm 的 CNG 提供者實作為名稱。 |
| EncryptionAlgorithmKeySize  | DWORD  | 要為對稱區塊加密演算法衍生的金鑰長度（以位為單位）。 |
| HashAlgorithm               | 字串 | CNG 所瞭解之雜湊演算法的名稱。 此演算法會在 HMAC 模式中開啟。 |
| HashAlgorithmProvider       | 字串 | 可以產生演算法 HashAlgorithm 的 CNG 提供者實作為名稱。 |

如果 EncryptionType 是 CNG-GCM，系統會設定為使用 Galois/Counter 模式對稱區塊密碼，以搭配 Windows CNG 提供的服務機密性和真實性（如需詳細資訊，請參閱[指定自訂 WINDOWS CNG 演算法](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)）。 下列為支援的額外值，其中每一個都對應至 CngGcmAuthenticatedEncryptionSettings 類型上的屬性。

| 值                       | 類型   | 說明 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | 字串 | CNG 瞭解的對稱式區塊密碼演算法的名稱。 此演算法會在 Galois/Counter 模式中開啟。 |
| EncryptionAlgorithmProvider | 字串 | 可以產生演算法 EncryptionAlgorithm 的 CNG 提供者實作為名稱。 |
| EncryptionAlgorithmKeySize  | DWORD  | 要為對稱區塊加密演算法衍生的金鑰長度（以位為單位）。 |

如果 EncryptionType 是受管理的，系統會設定為使用受控 System.security.cryptography.symmetricalgorithm 的機密性和 KeyedHashAlgorithm 以提供真實性（如需詳細資訊，請參閱[指定自訂的受控演算法](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms)）。 下列為支援的額外值，其中每一個都對應至 ManagedAuthenticatedEncryptionSettings 類型上的屬性。

| 值                      | 類型   | 說明 |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | 字串 | 實 System.security.cryptography.symmetricalgorithm 之型別的元件限定名稱。 |
| EncryptionAlgorithmKeySize | DWORD  | 要為對稱式加密演算法衍生的金鑰長度（以位為單位）。 |
| ValidationAlgorithmType    | 字串 | 實 KeyedHashAlgorithm 之型別的元件限定名稱。 |

如果 EncryptionType 具有 null 或空白以外的任何其他值，則資料保護系統會在啟動時擲回例外狀況。

> [!WARNING]
> 設定包含型別名稱（EncryptionAlgorithmType、ValidationAlgorithmType、KeyEscrowSinks）的預設原則設定時，應用程式必須能夠使用這些類型。 這表示，對於在桌面 CLR 上執行的應用程式，包含這些類型的元件應該存在於全域組件快取（GAC）中。 針對在 .NET Core 上執行的 ASP.NET Core 應用程式，應該安裝包含這些類型的封裝。
