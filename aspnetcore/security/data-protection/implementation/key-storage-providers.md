---
title: ASP.NET Core 中的金鑰儲存提供者
author: rick-anderson
description: 深入瞭解 ASP.NET Core 中的金鑰儲存提供者，以及如何設定金鑰儲存位置。
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: ec746f383c18ccc7b60c614c990f7577d2d52a20
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2019
ms.locfileid: "74052837"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="4af16-103">ASP.NET Core 中的金鑰儲存提供者</span><span class="sxs-lookup"><span data-stu-id="4af16-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="4af16-104">資料保護系統[預設會採用探索機制](xref:security/data-protection/configuration/default-settings)來判斷密碼編譯金鑰的保存位置。</span><span class="sxs-lookup"><span data-stu-id="4af16-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="4af16-105">開發人員可以覆寫預設探索機制，並手動指定位置。</span><span class="sxs-lookup"><span data-stu-id="4af16-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="4af16-106">如果您指定明確的金鑰持續性位置，資料保護系統會取消註冊待用的預設金鑰加密機制，因此金鑰不會再加密。</span><span class="sxs-lookup"><span data-stu-id="4af16-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="4af16-107">建議您另外針對生產環境部署[指定明確的金鑰加密機制](xref:security/data-protection/implementation/key-encryption-at-rest)。</span><span class="sxs-lookup"><span data-stu-id="4af16-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="4af16-108">檔案系統</span><span class="sxs-lookup"><span data-stu-id="4af16-108">File system</span></span>

<span data-ttu-id="4af16-109">若要設定以檔案系統為基礎的金鑰存放庫，請呼叫[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)設定常式，如下所示。</span><span class="sxs-lookup"><span data-stu-id="4af16-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="4af16-110">提供[DirectoryInfo](/dotnet/api/system.io.directoryinfo) ，指向存放應儲存金鑰的存放庫：</span><span class="sxs-lookup"><span data-stu-id="4af16-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="4af16-111">`DirectoryInfo` 可以指向本機電腦上的目錄，也可以指向網路共用上的資料夾。</span><span class="sxs-lookup"><span data-stu-id="4af16-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="4af16-112">如果指向本機電腦上的目錄（而且案例是只有本機電腦上的應用程式需要存取權才能使用此存放庫），請考慮使用[WINDOWS DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) （在 windows 上）來加密待用金鑰。</span><span class="sxs-lookup"><span data-stu-id="4af16-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="4af16-113">否則，請考慮使用[x.509 憑證](xref:security/data-protection/implementation/key-encryption-at-rest)來加密待用金鑰。</span><span class="sxs-lookup"><span data-stu-id="4af16-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="4af16-114">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="4af16-114">Azure Storage</span></span>

<span data-ttu-id="4af16-115">[AspNetCore. DataProtection. AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/)封裝可讓您將資料保護金鑰儲存在 Azure Blob 儲存體中。</span><span class="sxs-lookup"><span data-stu-id="4af16-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) package allows storing data protection keys in Azure Blob Storage.</span></span> <span data-ttu-id="4af16-116">金鑰可以在 web 應用程式的數個實例之間共用。</span><span class="sxs-lookup"><span data-stu-id="4af16-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="4af16-117">應用程式可以在多部伺服器之間共用驗證 cookie 或 CSRF 保護。</span><span class="sxs-lookup"><span data-stu-id="4af16-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

<span data-ttu-id="4af16-118">若要設定 Azure Blob 儲存體提供者，請呼叫其中一個[PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)多載。</span><span class="sxs-lookup"><span data-stu-id="4af16-118">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

<span data-ttu-id="4af16-119">如果 web 應用程式是以 Azure 服務的形式執行，則可以使用[AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/)自動建立驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="4af16-119">If the web app is running as an Azure service, authentication tokens can be automatically created using [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).</span></span>

```csharp
var tokenProvider = new AzureServiceTokenProvider();
var token = await tokenProvider.GetAccessTokenAsync("https://storage.azure.com/");
var credentials = new StorageCredentials(new TokenCredential(token));
var storageAccount = new CloudStorageAccount(credentials, "mystorageaccount", "core.windows.net", useHttps: true);
var client = storageAccount.CreateCloudBlobClient();
var container = client.GetContainerReference("my-key-container");

// optional - provision the container automatically
await container.CreateIfNotExistsAsync();

services.AddDataProtection()
    .PersistKeysToAzureBlobStorage(container, "keys.xml");
```

<span data-ttu-id="4af16-120">請參閱設定[服務對服務驗證的更多詳細資料。](/azure/key-vault/service-to-service-authentication)</span><span class="sxs-lookup"><span data-stu-id="4af16-120">See [more details about configuring service-to-service authentication.](/azure/key-vault/service-to-service-authentication)</span></span>

## <a name="redis"></a><span data-ttu-id="4af16-121">Redis</span><span class="sxs-lookup"><span data-stu-id="4af16-121">Redis</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4af16-122">[AspNetCore. DataProtection. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/)封裝可讓您將資料保護金鑰儲存在 Redis 快取中。</span><span class="sxs-lookup"><span data-stu-id="4af16-122">The [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="4af16-123">金鑰可以在 web 應用程式的數個實例之間共用。</span><span class="sxs-lookup"><span data-stu-id="4af16-123">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="4af16-124">應用程式可以在多部伺服器之間共用驗證 cookie 或 CSRF 保護。</span><span class="sxs-lookup"><span data-stu-id="4af16-124">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4af16-125">[AspNetCore. DataProtection. Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/)封裝可讓您將資料保護金鑰儲存在 Redis 快取中。</span><span class="sxs-lookup"><span data-stu-id="4af16-125">The [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="4af16-126">金鑰可以在 web 應用程式的數個實例之間共用。</span><span class="sxs-lookup"><span data-stu-id="4af16-126">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="4af16-127">應用程式可以在多部伺服器之間共用驗證 cookie 或 CSRF 保護。</span><span class="sxs-lookup"><span data-stu-id="4af16-127">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4af16-128">若要在 Redis 上設定，請呼叫其中一個[PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis)多載：</span><span class="sxs-lookup"><span data-stu-id="4af16-128">To configure on Redis, call one of the [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) overloads:</span></span>

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

<span data-ttu-id="4af16-129">若要在 Redis 上設定，請呼叫其中一個[PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis)多載：</span><span class="sxs-lookup"><span data-stu-id="4af16-129">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

<span data-ttu-id="4af16-130">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="4af16-130">For more information, see the following topics:</span></span>

* [<span data-ttu-id="4af16-131">Stackexchange.redis. Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="4af16-131">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="4af16-132">Azure Redis 快取</span><span class="sxs-lookup"><span data-stu-id="4af16-132">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="4af16-133">aspnet/DataProtection 範例</span><span class="sxs-lookup"><span data-stu-id="4af16-133">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a><span data-ttu-id="4af16-134">登錄</span><span class="sxs-lookup"><span data-stu-id="4af16-134">Registry</span></span>

<span data-ttu-id="4af16-135">**僅適用于 Windows 部署。**</span><span class="sxs-lookup"><span data-stu-id="4af16-135">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="4af16-136">有時候應用程式可能沒有檔案系統的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="4af16-136">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="4af16-137">假設應用程式是以虛擬服務帳戶（例如*w3wp.exe*的應用程式集區身分識別）執行的案例。</span><span class="sxs-lookup"><span data-stu-id="4af16-137">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="4af16-138">在這些情況下，系統管理員可以布建可由服務帳戶身分識別存取的登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="4af16-138">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="4af16-139">呼叫[PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry)擴充方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="4af16-139">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="4af16-140">提供指向應儲存密碼編譯金鑰之位置的[RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) ：</span><span class="sxs-lookup"><span data-stu-id="4af16-140">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="4af16-141">我們建議使用[WINDOWS DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest)來加密待用金鑰。</span><span class="sxs-lookup"><span data-stu-id="4af16-141">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="4af16-142">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4af16-142">Entity Framework Core</span></span>

<span data-ttu-id="4af16-143">[AspNetCore. DataProtection. microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/)套件提供使用 Entity Framework Core 將資料保護金鑰儲存至資料庫的機制。</span><span class="sxs-lookup"><span data-stu-id="4af16-143">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="4af16-144">`Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet 套件必須新增至專案檔，而不是[AspNetCore. 應用程式中繼套件](xref:fundamentals/metapackage-app)的一部分。</span><span class="sxs-lookup"><span data-stu-id="4af16-144">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4af16-145">使用此套件，可以在多個 web 應用程式實例之間共用金鑰。</span><span class="sxs-lookup"><span data-stu-id="4af16-145">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="4af16-146">若要設定 EF Core 提供者，請呼叫[`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext)方法：</span><span class="sxs-lookup"><span data-stu-id="4af16-146">To configure the EF Core provider, call the [`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-20)]

<span data-ttu-id="4af16-147">泛型參數（`TContext`）必須繼承自[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)並執行[IDataProtectionKeyCoNtext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext)：</span><span class="sxs-lookup"><span data-stu-id="4af16-147">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and implement [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

<span data-ttu-id="4af16-148">建立 `DataProtectionKeys` 資料表。</span><span class="sxs-lookup"><span data-stu-id="4af16-148">Create the `DataProtectionKeys` table.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4af16-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4af16-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4af16-150">在 [**套件管理員主控台**] （PMC）視窗中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4af16-150">Execute the following commands in the **Package Manager Console** (PMC) window:</span></span>

```PowerShell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4af16-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4af16-151">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4af16-152">在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4af16-152">Execute the following commands in a command shell:</span></span>

```dotnetcli
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

<span data-ttu-id="4af16-153">`MyKeysContext` 是上述程式碼範例中定義的 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="4af16-153">`MyKeysContext` is the `DbContext` defined in the preceding code sample.</span></span> <span data-ttu-id="4af16-154">如果您使用不同名稱的 `DbContext`，請以您的 `DbContext` 名稱取代 `MyKeysContext`。</span><span class="sxs-lookup"><span data-stu-id="4af16-154">If you're using a `DbContext` with a different name, substitute your `DbContext` name for `MyKeysContext`.</span></span>

<span data-ttu-id="4af16-155">`DataProtectionKeys` 類別/實體會採用下表所示的結構。</span><span class="sxs-lookup"><span data-stu-id="4af16-155">The `DataProtectionKeys` class/entity adopts the structure shown in the following table.</span></span>

| <span data-ttu-id="4af16-156">屬性/欄位</span><span class="sxs-lookup"><span data-stu-id="4af16-156">Property/Field</span></span> | <span data-ttu-id="4af16-157">CLR 型別</span><span class="sxs-lookup"><span data-stu-id="4af16-157">CLR Type</span></span> | <span data-ttu-id="4af16-158">SQL 類型</span><span class="sxs-lookup"><span data-stu-id="4af16-158">SQL Type</span></span>              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | <span data-ttu-id="4af16-159">`int`、PK、not null</span><span class="sxs-lookup"><span data-stu-id="4af16-159">`int`, PK, not null</span></span>   |
| `FriendlyName` | `string` | <span data-ttu-id="4af16-160">`nvarchar(MAX)`、null</span><span class="sxs-lookup"><span data-stu-id="4af16-160">`nvarchar(MAX)`, null</span></span> |
| `Xml`          | `string` | <span data-ttu-id="4af16-161">`nvarchar(MAX)`、null</span><span class="sxs-lookup"><span data-stu-id="4af16-161">`nvarchar(MAX)`, null</span></span> |

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="4af16-162">自訂金鑰存放庫</span><span class="sxs-lookup"><span data-stu-id="4af16-162">Custom key repository</span></span>

<span data-ttu-id="4af16-163">如果不適合使用內建機制，開發人員可以藉由提供自訂[IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository)來指定自己的金鑰持續性機制。</span><span class="sxs-lookup"><span data-stu-id="4af16-163">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
