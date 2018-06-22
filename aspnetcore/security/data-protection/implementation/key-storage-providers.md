---
title: 在 ASP.NET Core 金鑰儲存提供者
author: rick-anderson
description: 深入了解 ASP.NET Core 及如何設定金鑰的儲存體位置中的金鑰儲存提供者。
ms.author: riande
ms.date: 01/14/2017
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 432c2690f216325470bbea9b974ea772bcdc39ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273765"
---
# <a name="key-storage-providers-in-aspnet-core"></a>在 ASP.NET Core 金鑰儲存提供者

<a name="data-protection-implementation-key-storage-providers"></a>

根據預設資料保護系統[採用啟發學習法](xref:security/data-protection/configuration/default-settings)來判斷應該保存密碼編譯金鑰內容的位置。 開發人員可以覆寫啟發學習法，並以手動方式指定的位置。

> [!NOTE]
> 如果您指定明確的金鑰持續性位置時，資料保護系統將會取消註冊預設金鑰加密，在其餘的機制，提供啟發學習法，讓金鑰無法再進行加密在靜止。 建議您另外[指定明確的金鑰加密機制](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers)實際執行應用程式。

資料保護系統隨附數個內建金鑰儲存提供者。

## <a name="file-system"></a>檔案系統

我們預期許多應用程式將使用的檔案系統以金鑰儲存機制。 若要設定此項，呼叫[PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)組態常式如下所示。 提供`DirectoryInfo`指向儲存金鑰的儲存機制。

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

`DirectoryInfo`可以指向本機電腦上的目錄，或可以指向網路共用上的資料夾。 如果指向本機電腦上的目錄 （和的案例是在本機電腦上的應用程式必須使用這個儲存機制），請考慮使用[Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)來加密在靜止的索引鍵。 否則，請考慮使用[X.509 憑證](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)來加密在靜止的金鑰。

## <a name="azure-and-redis"></a>Azure 與 Redis

`Microsoft.AspNetCore.DataProtection.AzureStorage`和`Microsoft.AspNetCore.DataProtection.Redis`套件允許您的資料保護的金鑰儲存在 Azure 儲存體或 Redis 快取。 可以跨數個執行個體的 web 應用程式共用金鑰。 ASP.NET Core 應用程式可以在多部伺服器共用的驗證 cookie 或 CSRF 防護。 若要在 Azure 上設定，請呼叫其中一種[PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs)多載，如下所示。

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

另請參閱[Azure 測試程式碼](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs)。

若要設定 Redis 上，呼叫其中一種[PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs)多載，如下所示。

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

如需詳細資訊，請參閱下列主題：

- [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [Azure Redis 快取](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- [Redis 測試程式碼](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs)。

## <a name="registry"></a>登錄

有時候應用程式可能沒有寫入權限到檔案系統。 請考慮為虛擬服務帳戶 （例如 w3wp.exe 的應用程式集區身分識別） 應用程式執行所在的案例。 在這些情況下，系統管理員可能已佈建服務帳戶身分識別的適當 ACLed 登錄機碼。 呼叫[PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)組態常式如下所示。 提供`RegistryKey`指向儲存密碼編譯的索引鍵/值的位置。

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

如果您使用系統登錄做為持續性機制，請考慮使用[Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)來加密在靜止的索引鍵。

## <a name="custom-key-repository"></a>自訂金鑰儲存機制

如果內建機制不適當，開發人員可以指定自己的金鑰的持續性機制藉由提供自訂`IXmlRepository`。
