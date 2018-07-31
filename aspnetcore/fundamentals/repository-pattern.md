---
title: 使用 ASP.NET Core 的存放庫模式
author: ardalis
description: 了解如何在 ASP.NET Core 應用程式中實作存放庫應用程式設計模式。
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342594"
---
# <a name="repository-pattern-with-aspnet-core"></a>使用 ASP.NET Core 的存放庫模式

作者：[Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com) 與 [Luke Latham](https://github.com/guardrex)

*存放庫模式*是一個將資料存取隔離在介面抽象後方的設計模式。 連線到資料庫並處理資料儲存體物件是透過由介面實作所提供的方法來執行。 因此，不需要呼叫程式碼以處理資料庫問題，例如連線、命令與讀者。

使用 ASP.NET Core 的存放庫模式實作有下列優點：

* 應用程式的組織的複雜性較低，而且在商業與資料存取層之間沒有直接相互依存。
* 比較容易重複使用資料庫存取程式碼，因為程式碼是由一或多個存放庫集中管理。
* 可以從資料庫層在不相互依存的情況下針對商業網域進行單元測試。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples)使用存放庫模式來初始化及顯示電影角色名稱清單。 應用程式為其資料持續性使用 [Entity Framework Core](/ef/core/) 與 `ApplicationDbContext` 類別，但資料庫基礎結構在資料存取處並不顯而易見。 資料存取與資料庫物件是從[存放庫](https://martinfowler.com/eaaCatalog/repository.html)後方進行抽象化。

## <a name="repository-interface"></a>存放庫介面

存放庫介面定義實作的屬性與方法。 在範例應用程式中，電影角色資料的存放庫介面是 `ICharacterRepository`。 `ICharacterRepository` 定義 `ListAll` 與 `Add` 方法，它們是在應用程式中搭配 `Character` 執行個體使用的必要項目：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` 會定義為：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>存放庫具象型別

介面是由具象型別所實作。 在範例應用程式中，`CharacterRepository` 會管理資料庫內容並實作 `ListAll` 與 `Add` 方法：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>註冊存放庫服務

存放庫與資料庫內容是使用 `Startup.ConfigureServices` 中的服務容器所註冊。 在範例應用程式中，`ApplicationDbContext`是藉由呼叫擴充方法 [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 來設定。 `ICharacterRepository` 是註冊為具範圍服務：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>插入存放庫的執行個體

在需要資料庫存取的類別中，會透過建構函式要求存放庫的執行個體，並指派給私人欄位以供類別方法使用。 在範例應用程式中，`ICharacterRepository` 是用來：

* 在沒有任何角色存在的情況下填入資料庫。
* 取得要顯示的角色清單。

注意，呼叫程式碼只與介面的實作 `CharacterRepository` 互動。 呼叫程式碼未直接使用 `ApplicationDbContext`：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>一般存放庫介面

此主題與其範例應用程式示範最簡單的存放庫模式實作，其中會為每個商業物件建立一個存放庫。 若應用程式成長超過一些目標，*一般存放庫介面* 可能可以減少實作存放庫模式所需的程式碼量。 如需詳細資訊，請參閱[DevIQ: 存放庫模式: 一般存放庫介面](http://deviq.com/repository-pattern/) \(英文\)。

## <a name="additional-resources"></a>其他資源

* [DevIQ: 存放庫模式](https://deviq.com/repository-pattern/) \(英文\)
* [相依性插入](xref:fundamentals/dependency-injection)
* [在檢視中插入相依性](xref:mvc/views/dependency-injection)
* [在控制器中插入相依性](xref:mvc/controllers/dependency-injection)
* [要求處理常式中的相依性插入](xref:security/authorization/dependencyinjection)
* [逆轉控制容器和相依性插入模式](https://www.martinfowler.com/articles/injection.html) \(英文\)
