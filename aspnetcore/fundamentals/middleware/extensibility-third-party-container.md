---
title: 在 ASP.NET Core 中以協力廠商容器啟用中介軟體
author: guardrex
description: 了解如何在 ASP.NET Core 中搭配 Factory 啟用和協力廠商容器來使用強型別中介軟體。
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: b6fd02d1863efe571bafe1344fb34939883e6cb5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>在 ASP.NET Core 中以協力廠商容器啟用中介軟體

作者：[Luke Latham](https://github.com/guardrex)

本文示範如何使用 [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) 和 [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware)，作為以協力廠商容器啟動[中介軟體](xref:fundamentals/middleware/index)的擴充點。 如需 `IMiddlewareFactory` 和 `IMiddleware` 的簡介資訊，請參閱 [Factory 中介軟體啟用](xref:fundamentals/middleware/extensibility)主題。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

範例應用程式會示範如何透過 `IMiddlewareFactory` 的實作 `SimpleInjectorMiddlewareFactory` 來啟用中介軟體。 此範例會使用 [Simple Injector](https://simpleinjector.org) 相依性插入 (DI) 容器。

範例的中介軟體實作會記錄查詢字串參數 (`key`) 所提供的值。 中介軟體會使用插入的資料庫內容 (範圍服務)，以記錄記憶體內部資料庫的查詢字串值。

> [!NOTE]
> 範例應用程式純粹基於示範目的使用 [Simple Injector](https://github.com/simpleinjector/SimpleInjector)。 使用 Simple Injector 並不表示為其背書。 Simple Injector 文件和 GitHub 問題中所述的中介軟體啟用方法是 Simple Injector 維護人員建議使用的方法。 如需詳細資訊，請參閱 [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) (Simple Injector 文件) 和 [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector) (Simple Injector GitHub 存放庫)。

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) 可提供建立中介軟體的方法。

在範例應用程式中，將實作中介軟體 Factory 來建立 `SimpleInjectorActivatedMiddleware` 執行個體。 中介軟體 Factory 會使用 Simple Injector 容器來解析中介軟體：

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 可定義應用程式要求管線的中介軟體。

由 `IMiddlewareFactory` 實作啟動的中介軟體 (*Middleware/SimpleInjectorActivatedMiddleware.cs*)：

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

將會針對此中介軟體建立延伸模組 (*Middleware/MiddlewareExtensions.cs*)：

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` 必須執行幾項工作：

* 設定 Simple Injector 容器。
* 註冊 Factory 和中介軟體。
* 讓應用程式的資料庫內容可從 Razor 頁面的 Simple Injector 容器使用。

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

此中介軟體會在要求處理管線的 `Startup.Configure` 中註冊：

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=12)]

## <a name="additional-resources"></a>其他資源

* [中介軟體](xref:fundamentals/middleware/index)
* [Factory 式中介軟體啟用](xref:fundamentals/middleware/extensibility)
* [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector) (Simple Injector GitHub 存放庫)
* [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) (Simple Injector 文件)
