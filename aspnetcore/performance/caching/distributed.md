---
title: 使用 ASP.NET Core 中的分散式快取
author: ardalis
description: 了解如何使用 ASP.NET Core 分散式快取以改善應用程式效能和延展性，特別是在雲端或伺服器陣列環境中。
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 861664fcad576c11abe052837b72367eb2b9479a
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095677"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>使用 ASP.NET Core 中的分散式快取

作者：[Steve Smith](https://ardalis.com/)

分散式快取可以改善的效能和延展性的 ASP.NET Core 應用程式，尤其是裝載於雲端或伺服器陣列。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>什麼是分散式快取

分散式快取會共用由多個應用程式伺服器 (請參閱[快取基本知識](memory.md#caching-basics))。 快取中的資訊不會儲存在個別的 web 伺服器的記憶體和快取的資料可供所有的應用程式的伺服器。 這會提供數個優點：

1. 快取的資料是所有的 web 伺服器上的一致。 使用者不會看見不同的結果取決於 web 伺服器會處理其要求

2. Web 伺服器重新啟動並部署，不受影響的快取的資料。 可以移除或新增而不會影響快取個別的 web 伺服器。

3. 來源資料存放區會具有對它 （非具有多個記憶體中快取或沒有快取完全） 所做的要求較少。

> [!NOTE]
> 如果使用 SQL Server 分散式快取，這些優點有些只当比快取中的應用程式的來源資料使用個別的資料庫執行個體，則為 true。

任何快取中，例如分散式快取可大幅提升應用程式的回應性，因為通常遠快於從關聯式資料庫 （或 web 服務） 快取中擷取資料。

實作特定快取設定。 本文說明如何設定兩者 Redis 和 SQL Server 分散式快取。 無論選取哪一個實作時，應用程式互動使用共同的快取`IDistributedCache`介面。

## <a name="the-idistributedcache-interface"></a>IDistributedCache 介面

`IDistributedCache`介面包含同步和非同步方法。 介面允許新增、 擷取，並從分散式快取實作中移除的項目。 `IDistributedCache`介面包括下列方法：

**Get、 GetAsync**

接受字串索引鍵，並擷取快取的項目為`byte[]`如果快取中找到。

**集合 SetAsync**

將項目 (做為`byte[]`) 來使用字串索引鍵的快取。

**重新整理 RefreshAsync**

根據索引鍵，（如果有的話），重設其滑動的到期逾時的快取中的項目會重新整理。

**移除，RemoveAsync**

移除其索引鍵為基礎的快取項目。

若要使用`IDistributedCache`介面：

   1. 將必要的 NuGet 套件新增至專案檔。

   2. 設定的特定實作`IDistributedCache`在您`Startup`類別的`ConfigureServices`方法，並將它新增至那里的容器。

   3. 從應用程式的[中介軟體](xref:fundamentals/middleware/index)MVC 控制器類別中，要求的執行個體或`IDistributedCache`從建構函式。 將所提供的執行個體[相依性插入](../../fundamentals/dependency-injection.md)(DI)。

> [!NOTE]
> 若要使用的單一或範圍存留期不需要`IDistributedCache`執行個體 (至少為內建實作)。 您也可以建立執行個體，每當您可能需要其中一個 (而不是使用[相依性插入](../../fundamentals/dependency-injection.md))，但這會讓您的程式碼難測試，且違反[明確相依性準則](http://deviq.com/explicit-dependencies-principle/)。

下列範例示範如何使用的執行個體`IDistributedCache`中簡單的中介軟體元件：

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

上面的程式碼，快取的值是讀取，但永遠不會寫入。 在此範例中，當伺服器啟動，而且不會變更時，才會設定值。 在多伺服器案例中，最新的伺服器，準備開始將會覆寫任何先前的值所設定的其他伺服器。 `Get`並`Set`方法會使用`byte[]`型別。 因此，字串值必須使用來轉換`Encoding.UTF8.GetString`(如`Get`) 和`Encoding.UTF8.GetBytes`(如`Set`)。

下列程式碼會從*Startup.cs*示範所設定的值：

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> 由於`IDistributedCache`中設定`ConfigureServices`方法，您就能夠`Configure`做為參數的方法。 將它加入做為參數，可讓透過 DI 提供設定的執行個體。

## <a name="using-a-redis-distributed-cache"></a>使用分散式的 Redis 快取

[Redis](https://redis.io/)是開放原始碼記憶體中的資料存放區，通常是做為分散式快取。 您可以使用在本機，而且您可以設定[Azure Redis 快取](https://azure.microsoft.com/services/cache/)Azure 託管的 ASP.NET Core 應用程式。 您的 ASP.NET Core 應用程式會設定快取實作使用`RedisDistributedCache`執行個體。

設定中的 Redis 實作`ConfigureServices`，而且應用程式程式碼存取所要求的執行個體`IDistributedCache`（請參閱上面的程式碼）。

在範例程式碼`RedisCache`針對設定伺服器時，會使用實作`Staging`環境。 因此`ConfigureStagingServices`方法會設定`RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> 若要在本機電腦上安裝 Redis，安裝 chocolatey 封裝[ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/)並執行`redis-server`從命令提示字元。

## <a name="using-a-sql-server-distributed-cache"></a>使用 SQL Server 分散式快取

SqlServerCache 實作可讓分散式快取，以使用 SQL Server 資料庫作為其備份存放區。 若要建立 SQL Server 資料表，您可以使用 sql 快取工具，此工具會建立具有您指定的名稱和結構描述資料表。

::: moniker range="< aspnetcore-2.1"

新增`SqlConfig.Tools`要`<ItemGroup>`項目的專案檔與執行`dotnet restore`。

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

測試 SqlConfig.Tools 藉由執行下列命令：

```console
dotnet sql-cache create --help
```

SqlConfig.Tools 顯示使用量、 選項和命令說明。

SQL Server 中建立資料表，藉由執行`sql-cache create`命令：

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

建立的資料表具有下列結構描述：

![Sql Server 快取表格](distributed/_static/SqlServerCacheTable.png)

所有的快取實作，例如您的應用程式應該取得及設定使用的執行個體的快取值`IDistributedCache`，而非`SqlServerCache`。 此範例會實作`SqlServerCache`生產環境中 (因此它設定在`ConfigureProductionServices`)。

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> `ConnectionString` (並選擇性地`SchemaName`和`TableName`) 應該通常儲存在外部原始檔控制 （例如 UserSecrets)，因為它們可能包含認證。

## <a name="recommendations"></a>建議

當您決定哪一個實作時`IDistributedCache`是適合您的應用程式中，選擇 Redis 和 SQL Server 會根據您現有的基礎結構和環境、 您效能需求，以及您的小組的經驗。 如果您的小組更方便使用的 Redis，它會是一個絕佳選擇。 如果小組慣用的 SQL Server，您可以實作以及信心。 請注意，傳統的快取解決方案會將儲存記憶體中資料可用來快速擷取資料。 您應該在快取中儲存常用的資料，並將整個資料儲存在 SQL Server 或 Azure 儲存體之類的後端持續性存放區。 Redis 快取是快取的解決方案，可提供高輸送量和低延遲相較於 SQL 快取。

## <a name="additional-resources"></a>其他資源

* [Redis 快取，在 Azure 上](https://azure.microsoft.com/documentation/services/redis-cache/)
* [在 Azure 上的 SQL 資料庫](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
