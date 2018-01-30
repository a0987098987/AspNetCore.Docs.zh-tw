---
title: "金鑰加密在靜止"
author: rick-anderson
description: "本文概述 ASP.NET Core 資料保護加密在靜止的實作詳細資料。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: c66430bfe547cf061e9e79a703ac665a968bbe0b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="key-encryption-at-rest"></a>金鑰加密在靜止

<a name="data-protection-implementation-key-encryption-at-rest"></a>

根據預設，資料保護系統[採用啟發學習法](xref:security/data-protection/configuration/default-settings)來判斷如何密碼編譯金鑰內容應該加密在靜止。 開發人員可以覆寫啟發學習法，並以手動方式指定索引鍵應該如何加密在靜止。

> [!NOTE]
> 如果您指定明確的金鑰加密，在其餘的機制，資料保護系統將會取消註冊啟發學習法所提供的預設金鑰儲存機制。 您必須[指定明確的金鑰儲存機制](key-storage-providers.md#data-protection-implementation-key-storage-providers)，否則資料保護系統將無法啟動。

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

資料保護系統隨附三種內建金鑰加密機制。

## <a name="windows-dpapi"></a>Windows DPAPI

*這項機制可只在 Windows 上。*

使用 Windows DPAPI 時，會透過加密金錀[CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx)之前要保存至儲存體。 DPAPI 是適當的加密機制將永遠不會讀取目前電腦以外的資料 (雖然它可備份到 Active Directory 這些機碼，請參閱 < [DPAPI 和漫遊設定檔](https://support.microsoft.com/kb/309408/#6))。 例如若要設定 DPAPI 金鑰靜態加密。

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

如果`ProtectKeysWithDpapi`是不使用參數呼叫，只有目前 Windows 使用者帳戶可以破解保存的金鑰內容。 您可以選擇性地指定中所示，電腦 （不只是目前的使用者帳戶） 上的任何使用者帳戶，應該可以進行破解金鑰的內容中，下列範例。

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a>X.509 憑證

*這項機制未提供`.NET Core 1.0`或`1.1`。*

如果您的應用程式則會分散到多部電腦，可能會很方便分散的機器上的共用的 X.509 憑證，並設定應用程式使用此憑證進行加密在靜止的索引鍵。 如需範例，請參閱下方內容。

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

由於.NET Framework 限制支援只使用 CAPI 私用金鑰的憑證。 請參閱[憑證為基礎的加密與 Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng)下面這些限制可能的解決方法。

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

*這項機制是僅適用於 Windows 8 / Windows Server 2012 及更新版本。*

從 Windows 8 開始，作業系統支援 DPAPI NG （也稱為 CNG DPAPI）。 Microsoft 會配置其使用案例，如下所示。

   雲端運算，不過，通常需要該內容的加密在一部電腦會在另一台解密。 因此，從 Windows 8 開始，Microsoft 擴充至涵蓋雲端案例使用相當簡單的應用程式開發介面的概念。 這個新的 API，稱為 DPAPI NG，可讓您安全地共用機密資料 （金鑰、 密碼、 金鑰內容） 和訊息，藉由一組可用來取消它們在不同電腦上保護之後適當的驗證和授權的主體來保護它們。

   從[CNG Dpapi](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

主體會編碼成保護描述項規則。 請考慮下列範例中，其加密金鑰內容，只有已加入網域的使用者具有指定 SID 可以解密金鑰資料。

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

也是無參數多載`ProtectKeysWithDpapiNG`。 這是便利的方法來指定規則 」 SID = 採擷"，其中我是目前的 Windows 使用者帳戶的 SID。

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

在此案例中，AD 網域控制站會負責將 DPAPI NG 作業所使用的加密金鑰。 目標使用者將能夠解密從任何已加入網域的電腦的加密的內容 （前提是處理程序以其識別執行）。

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>憑證為基礎的加密與 Windows DPAPI-NG 中

如果您執行 Windows 8.1 / Windows Server 2012 R2 或更新版本中，您可以使用 Windows DPAPI NG 執行憑證架構的加密，即使應用程式執行[.NET Core](https://www.microsoft.com/net/core)。 若要利用這一點，使用 規則描述項字串"憑證 = HashId:thumbprint"，其中的憑證指紋是要使用的憑證十六進位編碼 SHA1 指紋。 如需範例，請參閱下方內容。

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

任何指向這個儲存機制的應用程式必須執行 Windows 8.1 / Windows Server 2012 R2 或更新版本能夠解密此機碼。

## <a name="custom-key-encryption"></a>自訂金鑰加密

如果內建機制不適當，開發人員可以指定自己的金鑰加密機制藉由提供自訂`IXmlEncryptor`。
