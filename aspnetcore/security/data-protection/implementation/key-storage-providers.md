---
title: ASP.NET Core 中的金鑰儲存提供者
author: rick-anderson
description: 深入了解 ASP.NET Core，以及如何設定金鑰的儲存體位置中的金鑰儲存提供者。
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 35e2cea4b6404af94de95352dc6ebf3071925cb1
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477146"
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

::: moniker range=">= aspnetcore-2.2"

[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/)並[Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/)套件可讓您允許將資料保護金鑰儲存在 Azure 儲存體或 Redis快取。 可以跨數個執行個體的 web 應用程式共用金鑰。 應用程式可以共用驗證 cookie 或 CSRF 防護，在多部伺服器。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/)並[Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/)套件可讓您允許將資料保護金鑰儲存在 Azure 儲存體或 Redis 快取。 可以跨數個執行個體的 web 應用程式共用金鑰。 應用程式可以共用驗證 cookie 或 CSRF 防護，在多部伺服器。

::: moniker-end

若要設定 Azure Blob 儲存體提供者，呼叫其中一種[PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)多載：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

::: moniker range=">= aspnetcore-2.2"

若要設定 Redis，呼叫其中一種[PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis)多載：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToStackExchangeRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

若要設定 Redis，呼叫其中一種[PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis)多載：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

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

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

[Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/)封裝提供一個機制，來儲存至資料庫，使用 Entity Framework Core 資料保護金鑰。 `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet 套件必須新增至專案檔，並不屬於[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。

透過此封裝，可以跨多個 web 應用程式執行個體共用金鑰。

若要設定 EF Core 提供者，請呼叫[ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext)方法：

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

泛型參數`TContext`，必須繼承自[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)並[IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a>自訂金鑰存放庫

如果內建機制不適當，開發人員可以指定自己的金鑰持續性機制藉由提供自訂[IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository)。
