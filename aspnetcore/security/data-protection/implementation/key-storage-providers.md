---
title: ASP.NET Core 中的金鑰儲存提供者
author: rick-anderson
description: 深入了解 ASP.NET Core，以及如何設定金鑰的儲存體位置中的金鑰儲存提供者。
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: e712ff09b5306bc4481c4cc105448d7cbfa39f3a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356762"
---
# <a name="key-storage-providers-in-aspnet-core"></a>ASP.NET Core 中的金鑰儲存提供者

資料保護系統[採用預設的探索機制](xref:security/data-protection/configuration/default-settings)來判斷應該保存密碼編譯金鑰的位置。 開發人員可以覆寫預設的探索機制，並以手動方式指定的位置。

> [!WARNING]
> 如果您指定明確的金鑰持續性位置，讓金鑰不會再加密待用資料保護系統取消登錄 rest 機制，在預設金鑰加密。 建議您另外[指定明確的金鑰加密機制](xref:security/data-protection/implementation/key-encryption-at-rest)生產環境部署。

## <a name="file-system"></a>檔案系統

若要設定的檔案系統為基礎金鑰存放庫，呼叫[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)設定常式，如下所示。 提供[DirectoryInfo](/dotnet/api/system.io.directoryinfo)指向存放庫儲存金鑰的位置：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

`DirectoryInfo`可以指向本機電腦上的目錄，或它可以指向網路共用上的資料夾。 如果指向本機電腦上的目錄 （並的案例是在本機電腦上的應用程式需要使用此存放庫的存取權），請考慮使用[Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (在 Windows) 來加密待用的金鑰。 否則，請考慮使用[X.509 憑證](xref:security/data-protection/implementation/key-encryption-at-rest)來加密待用的金鑰。

## <a name="azure-and-redis"></a>Azure 與 Redis

[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/)並[Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/)套件可讓您允許將資料保護金鑰儲存在 Azure 儲存體或 Redis 快取。 可以跨數個執行個體的 web 應用程式共用金鑰。 應用程式可以共用驗證 cookie 或 CSRF 防護，在多部伺服器。 若要設定 Azure Blob 儲存體提供者，呼叫其中一種[PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)多載：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

若要設定 Redis，呼叫其中一種[PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis)多載：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

如需詳細資訊，請參閱下列主題：

* [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Azure Redis 快取](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [aspnet/DataProtection 範例](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a>登錄

**僅適用於 Windows 的部署。**

有時候應用程式可能沒有在檔案系統的 「 寫入 」 權限。 請考慮應用程式執行的虛擬服務帳戶的案例 (例如*w3wp.exe*的應用程式集區身分識別)。 在這些情況下，系統管理員可以佈建服務帳戶身分識別可以存取的登錄機碼。 呼叫[PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry)擴充方法，如下所示。 提供[登錄機碼](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey)指向儲存密碼編譯金鑰的位置：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> 我們建議您使用[Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest)來加密待用的金鑰。

## <a name="custom-key-repository"></a>自訂金鑰存放庫

如果內建機制不適當，開發人員可以指定自己的金鑰持續性機制藉由提供自訂[IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository)。
