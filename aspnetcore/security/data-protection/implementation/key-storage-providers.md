---
title: "金鑰儲存提供者"
author: rick-anderson
description: "金鑰儲存提供者"
keywords: "加密，ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 1/14/2017
ms.topic: article
ms.assetid: 423e0a79-2f34-44c4-aaf3-146a53c39251
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 0a1798e60b52cbb16b6cb6d41c36200ed629d06d
ms.sourcegitcommit: 1db592cd8f6ea4061b31689f7109356de8f59440
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2017
---
# <a name="key-storage-providers"></a><span data-ttu-id="26e79-104">金鑰儲存提供者</span><span class="sxs-lookup"><span data-stu-id="26e79-104">Key storage providers</span></span>

<a name=data-protection-implementation-key-storage-providers></a>

<span data-ttu-id="26e79-105">根據預設資料保護系統[採用啟發學習法](../configuration/default-settings.md#data-protection-default-settings)來判斷應該保存密碼編譯金鑰內容的位置。</span><span class="sxs-lookup"><span data-stu-id="26e79-105">By default the data protection system [employs a heuristic](../configuration/default-settings.md#data-protection-default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="26e79-106">開發人員可以覆寫啟發學習法，並以手動方式指定的位置。</span><span class="sxs-lookup"><span data-stu-id="26e79-106">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="26e79-107">如果您指定明確的金鑰持續性位置時，資料保護系統將會取消註冊預設金鑰加密，在其餘的機制，提供啟發學習法，讓金鑰無法再進行加密在靜止。</span><span class="sxs-lookup"><span data-stu-id="26e79-107">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="26e79-108">建議您另外[指定明確的金鑰加密機制](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)實際執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="26e79-108">It is recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="26e79-109">資料保護系統隨附數個內建金鑰儲存提供者。</span><span class="sxs-lookup"><span data-stu-id="26e79-109">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="26e79-110">檔案系統</span><span class="sxs-lookup"><span data-stu-id="26e79-110">File system</span></span>

<span data-ttu-id="26e79-111">我們預期許多應用程式將使用的檔案系統以金鑰儲存機制。</span><span class="sxs-lookup"><span data-stu-id="26e79-111">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="26e79-112">若要設定此項，呼叫[PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)組態常式如下所示。</span><span class="sxs-lookup"><span data-stu-id="26e79-112">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="26e79-113">提供`DirectoryInfo`指向儲存金鑰的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="26e79-113">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="26e79-114">`DirectoryInfo`可以指向本機電腦上的目錄，或可以指向網路共用上的資料夾。</span><span class="sxs-lookup"><span data-stu-id="26e79-114">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="26e79-115">如果指向本機電腦上的目錄 （和的案例是在本機電腦上的應用程式必須使用這個儲存機制），請考慮使用[Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)來加密在靜止的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="26e79-115">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="26e79-116">否則，請考慮使用[X.509 憑證](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)來加密在靜止的金鑰。</span><span class="sxs-lookup"><span data-stu-id="26e79-116">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="26e79-117">Azure 與 Redis</span><span class="sxs-lookup"><span data-stu-id="26e79-117">Azure and Redis</span></span>

<span data-ttu-id="26e79-118">`Microsoft.AspNetCore.DataProtection.AzureStorage`和`Microsoft.AspNetCore.DataProtection.Redis`套件允許您的資料保護的金鑰儲存在 Azure 儲存體或 Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="26e79-118">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="26e79-119">可以跨數個執行個體的 web 應用程式共用金鑰。</span><span class="sxs-lookup"><span data-stu-id="26e79-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="26e79-120">ASP.NET Core 應用程式可以在多部伺服器共用的驗證 cookie 或 CSRF 防護。</span><span class="sxs-lookup"><span data-stu-id="26e79-120">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="26e79-121">若要在 Azure 上設定，請呼叫其中一種[PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs)多載，如下所示。</span><span class="sxs-lookup"><span data-stu-id="26e79-121">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="26e79-122">另請參閱[Azure 測試程式碼](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs)。</span><span class="sxs-lookup"><span data-stu-id="26e79-122">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="26e79-123">若要設定 Redis 上，呼叫其中一種[PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs)多載，如下所示。</span><span class="sxs-lookup"><span data-stu-id="26e79-123">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

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

<span data-ttu-id="26e79-124">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="26e79-124">See the following for more information:</span></span>

- [<span data-ttu-id="26e79-125">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="26e79-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="26e79-126">Azure Redis 快取</span><span class="sxs-lookup"><span data-stu-id="26e79-126">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="26e79-127">[Redis 測試程式碼](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs)。</span><span class="sxs-lookup"><span data-stu-id="26e79-127">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="26e79-128">登錄</span><span class="sxs-lookup"><span data-stu-id="26e79-128">Registry</span></span>

<span data-ttu-id="26e79-129">有時候應用程式可能沒有寫入權限到檔案系統。</span><span class="sxs-lookup"><span data-stu-id="26e79-129">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="26e79-130">請考慮為虛擬服務帳戶 （例如 w3wp.exe 的應用程式集區身分識別） 應用程式執行所在的案例。</span><span class="sxs-lookup"><span data-stu-id="26e79-130">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="26e79-131">在這些情況下，系統管理員可能已佈建服務帳戶身分識別的適當 ACLed 登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="26e79-131">In these cases, the administrator may have provisioned a registry key that is appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="26e79-132">呼叫[PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)組態常式如下所示。</span><span class="sxs-lookup"><span data-stu-id="26e79-132">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="26e79-133">提供`RegistryKey`指向儲存密碼編譯的索引鍵/值的位置。</span><span class="sxs-lookup"><span data-stu-id="26e79-133">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="26e79-134">如果您使用系統登錄做為持續性機制，請考慮使用[Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)來加密在靜止的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="26e79-134">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="26e79-135">自訂金鑰儲存機制</span><span class="sxs-lookup"><span data-stu-id="26e79-135">Custom key repository</span></span>

<span data-ttu-id="26e79-136">如果內建機制不適當，開發人員可以指定自己的金鑰的持續性機制藉由提供自訂`IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="26e79-136">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
