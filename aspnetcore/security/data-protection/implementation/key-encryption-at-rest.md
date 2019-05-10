---
title: ASP.NET Core 中的待用的金鑰加密
author: rick-anderson
description: 了解 ASP.NET Core 資料保護待用的金鑰加密的實作詳細資料。
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892305"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>ASP.NET Core 中的待用的金鑰加密

資料保護系統[採用預設的探索機制](xref:security/data-protection/configuration/default-settings)來判斷如何密碼編譯金鑰應進行待用加密。 開發人員可以覆寫的探索機制，並以手動方式指定如何應該進行待用加密金鑰。

> [!WARNING]
> 如果您指定的明確[機碼的持續性位置](xref:security/data-protection/implementation/key-storage-providers)，資料保護系統的預設金鑰加密，在 rest 機制可以用來移除。 因此，索引鍵不會再加密靜止。 我們建議您[指定明確的金鑰加密機制](xref:security/data-protection/implementation/key-encryption-at-rest)生產環境部署。 待用加密機制選項是以本主題所述。

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a>Azure Key Vault

金鑰存放在[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)，設定與系統[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)在`Startup`類別：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護：ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault)。

::: moniker-end

## <a name="windows-dpapi"></a>Windows DPAPI

**僅適用於 Windows 的部署。**

使用 Windows DPAPI 時，使用加密金鑰材料[CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata)之前要保存至儲存體。 DPAPI 是永遠不會讀取目前的電腦以外的資料的適當的加密機制 (不過它可以備份到 Active Directory 的這些金鑰，請參閱[DPAPI 和漫遊設定檔](https://support.microsoft.com/kb/309408/#6))。 若要設定 DPAPI 金鑰待用加密，請呼叫其中一個[ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi)擴充方法：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

如果`ProtectKeysWithDpapi`呼叫沒有參數，只有目前的 Windows 使用者帳戶可解密保存的金鑰環。 您可以選擇性地指定任何電腦 （不只是目前的使用者帳戶） 上的使用者帳戶能夠解密金鑰環：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a>X.509 憑證

如果應用程式則會分散到多部電腦，可能會方便分散機器上的共用的 X.509 憑證來設定裝載的應用程式，以使用憑證來加密待用的金鑰：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

由於.NET Framework 的限制，支援只使用 CAPI 私用金鑰的憑證。 請參閱下面的內容，這些限制可能的因應措施。

::: moniker-end

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

**這項機制是僅適用於 Windows 8/Windows Server 2012 或更新版本。**

從 Windows 8 開始，Windows 作業系統支援 DPAPI NG （也稱為 CNG DPAPI）。 如需詳細資訊，請參閱 <<c0> [ 需 CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi)。

主體會編碼為保護描述項規則。 在下列範例會呼叫[ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping)，只有指定的 SID 的已加入網域的使用者可以解密金鑰環：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

另外還有的無參數多載`ProtectKeysWithDpapiNG`。 使用這個便利方法，以指定的規則"SID = {CURRENT_ACCOUNT_SID}"，其中*CURRENT_ACCOUNT_SID*是目前的 Windows 使用者帳戶的 SID:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

在此案例中，AD 網域控制站負責散發 DPAPI NG 作業所使用的加密金鑰。 目標使用者可解密加密的裝載，從任何已加入網域的電腦 （前提是處理序正在執行的身分識別）。

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>使用憑證式加密使用 Windows DPAPI NG

如果應用程式在 Windows 8.1 / Windows Server 2012 R2 上執行，或更新版本中，您可以使用 Windows DPAPI NG，來執行憑證為基礎的加密。 使用 規則描述項字串 「 憑證 = HashId:THUMBPRINT"，其中*指紋*是十六進位編碼的 SHA1 指紋的憑證：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

任何指向這個存放庫的應用程式必須執行 Windows 8.1 / Windows Server 2012 R2 或更新版本，才能解密金鑰。

## <a name="custom-key-encryption"></a>自訂金鑰加密

如果內建機制不適當，開發人員可以指定自己的金鑰加密機制藉由提供自訂[IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)。
