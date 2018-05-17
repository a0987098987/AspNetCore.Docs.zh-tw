---
title: 使用分散式快取中 ASP.NET Core
author: ardalis
description: 了解如何使用 ASP.NET Core 分散式快取以改善應用程式效能和延展性，尤其是在雲端或伺服器的伺服陣列環境。
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: c40209e3b3f2b5bf28450bb2a88cbe40e9e23230
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2018
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>使用分散式快取中 ASP.NET Core

作者：[Steve Smith](https://ardalis.com/)

分散式快取可以改善效能和延展性的 ASP.NET Core 應用程式，尤其是裝載於雲端或伺服器的伺服陣列環境中。 本文說明如何使用 ASP.NET Core 內建的分散式快取的抽象概念和實作。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>什麼是分散式快取

分散式快取由多個應用程式伺服器共用 (請參閱[快取的基本概念](memory.md#caching-basics))。 快取中的資訊不會儲存在個別 web 伺服器的記憶體，而且快取的資料可用於所有應用程式的伺服器。 這會提供數個優點：

1. 快取的資料是一致的所有 web 伺服器上。 使用者沒有看到不同的結果，根據哪個 web 伺服器會處理其要求

2. 快取的資料未 web 伺服器重新啟動並部署。 個別的網頁伺服器可以移除或加入，而不會影響快取。

3. 來源資料存放區有較少對它 （非多個記憶體中快取或完全不需要與快取完全） 提出的要求。

> [!NOTE]
> 如果使用 SQL Server 分散式快取，這些優點有些只比快取中的應用程式的來源資料使用不同的資料庫執行個體，如果為 true。

任何快取中，例如分散式快取可以大幅改善應用程式的回應性，通常比更快，從關聯式資料庫 （或 web 服務） 快取中擷取資料之後。

快取設定是特定的實作。 本文說明如何設定兩者 Redis 和 SQL Server 分散式快取。 無論選取哪一個實作時，應用程式互動使用共同的快取`IDistributedCache`介面。

## <a name="the-idistributedcache-interface"></a>IDistributedCache 介面

`IDistributedCache`介面包括同步和非同步方法。 介面允許加入、 擷取，並從分散式快取實作中移除的項目。 `IDistributedCache`介面包含下列方法：

**Get、 GetAsync**

會採用字串索引鍵，並擷取快取的項目為`byte[]`如果快取中找到。

**集合 SetAsync**

將項目 (做為`byte[]`) 至快取使用字串索引鍵。

**重新整理 RefreshAsync**

重新整理它的金鑰，重設其滑動到期逾時 （如果有的話） 為基礎的快取中的項目。

**Remove、 RemoveAsync**

根據索引鍵的快取項目中移除。

若要使用`IDistributedCache`介面：

   1. 將必要的 NuGet 封裝加入至您的專案檔。

   2. 設定的特定實作`IDistributedCache`中您`Startup`類別的`ConfigureServices`方法，並將它新增至那里容器。

   3. 從應用程式的[中介軟體](xref:fundamentals/middleware/index)MVC 控制器類別中，要求的執行個體或`IDistributedCache`從建構函式。 執行個體將會由提供[相依性插入](../../fundamentals/dependency-injection.md)(DI)。

> [!NOTE]
> 若要使用的單一或 Scoped 存留期不需要`IDistributedCache`執行個體 (至少為內建實作)。 您也可以建立執行個體，只要您可能需要一個 (而不是使用[相依性插入](../../fundamentals/dependency-injection.md))，但是這可以讓您的程式碼不容易進行測試，且違反[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。

下列範例示範如何使用的執行個體`IDistributedCache`簡單的中介軟體元件：

[!code-csharp[](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

在上述程式碼快取的值是讀取，但永遠不會寫入。 在此範例中，當伺服器啟動，而且不會變更時，才會設定值。 在多伺服器案例中，最新的伺服器，準備開始將會覆寫任何先前由其他伺服器設定的值。 `Get`和`Set`方法使用`byte[]`型別。 因此，字串值必須轉換使用`Encoding.UTF8.GetString`(如`Get`) 和`Encoding.UTF8.GetBytes`(如`Set`)。

下列程式碼會從*Startup.cs*示範所設定的值：

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> 因為`IDistributedCache`中設定`ConfigureServices`方法，您就能夠`Configure`做為參數的方法。 將它加入做為參數，可讓透過 DI 提供設定的執行個體。

## <a name="using-a-redis-distributed-cache"></a>使用分散式的 Redis 快取

[Redis](https://redis.io/)是開放原始碼記憶體中的資料存放區，通常是做為分散式快取。 您可以使用在本機，並且可以設定[Azure Redis 快取](https://azure.microsoft.com/services/cache/)Azure 裝載的 ASP.NET Core 應用程式。 您的 ASP.NET Core 應用程式會設定快取實作使用`RedisDistributedCache`執行個體。

設定中的 Redis 實作`ConfigureServices`，而且應用程式程式碼中存取所要求的執行個體`IDistributedCache`（請參閱上面的程式碼）。

範例程式碼，`RedisCache`設定伺服器時，會使用實作`Staging`環境。 因此`ConfigureStagingServices`方法會設定`RedisCache`:

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> 若要安裝 Redis 在本機電腦上，安裝 chocolatey 封裝[ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/)並執行`redis-server`從命令提示字元。

## <a name="using-a-sql-server-distributed-cache"></a>使用 SQL Server 分散式快取

SqlServerCache 實作可讓分散式快取，以使用 SQL Server 資料庫做為其備份存放區。 若要建立 SQL Server 資料表，您可以使用 sql 快取工具，此工具會建立資料表，與您指定的名稱和結構描述。

若要使用 sql 快取工具，加入`SqlConfig.Tools`至`<ItemGroup>`元素 *.csproj*檔，然後執行 dotnet 還原。

[!code-xml[](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

執行下列命令來測試 SqlConfig.Tools

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

sql 快取工具會顯示使用量、 選項和命令的說明，現在您可以建立資料表至 sql server，執行 「 建立 sql 快取 」 的命令：

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

建立的資料表具有下列結構描述：

![SqlServer 快取表格](distributed/_static/SqlServerCacheTable.png)

所有快取實作，例如您的應用程式應該取得並設定使用的執行個體的快取值`IDistributedCache`，而非`SqlServerCache`。 此範例會實作`SqlServerCache`中`Production`環境 (因此中設定它`ConfigureProductionServices`)。

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> `ConnectionString` (並選擇性地`SchemaName`和`TableName`) 應該通常會儲存在外部原始檔控制 （例如 UserSecrets)，因為它們可能包含認證。

## <a name="recommendations"></a>建議

當您決定哪一個實作時`IDistributedCache`是適合您的應用程式中，選擇 Redis 和 SQL Server 根據現有的基礎結構和環境、 您效能需求，以及小組的體驗。 如果您的小組更方便使用 Redis，它會是很好的選擇。 如果小組慣用的 SQL Server，您可以實作以及信心。 請注意傳統快取方案儲存在記憶體資料可快速擷取資料。 您應該在快取中儲存常用的資料，並將整個資料儲存在 SQL Server 或 Azure 儲存體之類的後端持續性存放區。 Redis 快取是快取的解決方案，可提供您高輸送量和低度延遲相較於 SQL 快取。

## <a name="additional-resources"></a>其他資源

* [Redis 快取，在 Azure 上](https://azure.microsoft.com/documentation/services/redis-cache/)
* [在 Azure 上的 SQL 資料庫](https://azure.microsoft.com/documentation/services/sql-database/)
* [記憶體中快取](xref:performance/caching/memory)
* [使用變更權杖來偵測變更](xref:fundamentals/primitives/change-tokens)
* [回應快取](xref:performance/caching/response)
* [回應快取中介軟體](xref:performance/caching/middleware)
* [快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
