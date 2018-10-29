---
title: 在 ASP.NET Core 中以協力廠商容器啟用中介軟體
author: guardrex
description: 了解如何在 ASP.NET Core 中搭配 Factory 啟用和協力廠商容器來使用強型別中介軟體。
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2018
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 6af775c66a1de7f1a4f06a4a639ade20c6493b2a
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206805"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="b925a-103">在 ASP.NET Core 中以協力廠商容器啟用中介軟體</span><span class="sxs-lookup"><span data-stu-id="b925a-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="b925a-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b925a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b925a-105">本文示範如何使用 [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) 和 [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware)，作為以協力廠商容器啟動[中介軟體](xref:fundamentals/middleware/index)的擴充點。</span><span class="sxs-lookup"><span data-stu-id="b925a-105">This article demonstrates how to use [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) and [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="b925a-106">如需 `IMiddlewareFactory` 和 `IMiddleware` 的簡介資訊，請參閱 [Factory 中介軟體啟用](xref:fundamentals/middleware/extensibility)主題。</span><span class="sxs-lookup"><span data-stu-id="b925a-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see the [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) topic.</span></span>

<span data-ttu-id="b925a-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b925a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b925a-108">範例應用程式會示範如何透過 `IMiddlewareFactory` 的實作 `SimpleInjectorMiddlewareFactory` 來啟用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b925a-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="b925a-109">此範例會使用 [Simple Injector](https://simpleinjector.org) 相依性插入 (DI) 容器。</span><span class="sxs-lookup"><span data-stu-id="b925a-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="b925a-110">範例的中介軟體實作會記錄查詢字串參數 (`key`) 所提供的值。</span><span class="sxs-lookup"><span data-stu-id="b925a-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="b925a-111">中介軟體會使用插入的資料庫內容 (範圍服務)，以記錄記憶體內部資料庫的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="b925a-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="b925a-112">範例應用程式純粹基於示範目的使用 [Simple Injector](https://github.com/simpleinjector/SimpleInjector)。</span><span class="sxs-lookup"><span data-stu-id="b925a-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="b925a-113">使用 Simple Injector 並不表示為其背書。</span><span class="sxs-lookup"><span data-stu-id="b925a-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="b925a-114">Simple Injector 文件和 GitHub 問題中所述的中介軟體啟用方法是 Simple Injector 維護人員建議使用的方法。</span><span class="sxs-lookup"><span data-stu-id="b925a-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="b925a-115">如需詳細資訊，請參閱 [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) (Simple Injector 文件) 和 [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector) (Simple Injector GitHub 存放庫)。</span><span class="sxs-lookup"><span data-stu-id="b925a-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="b925a-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="b925a-116">IMiddlewareFactory</span></span>

<span data-ttu-id="b925a-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) 可提供建立中介軟體的方法。</span><span class="sxs-lookup"><span data-stu-id="b925a-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span>

<span data-ttu-id="b925a-118">在範例應用程式中，將實作中介軟體 Factory 來建立 `SimpleInjectorActivatedMiddleware` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="b925a-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="b925a-119">中介軟體 Factory 會使用 Simple Injector 容器來解析中介軟體：</span><span class="sxs-lookup"><span data-stu-id="b925a-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="b925a-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="b925a-120">IMiddleware</span></span>

<span data-ttu-id="b925a-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 可定義應用程式要求管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b925a-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="b925a-122">由 `IMiddlewareFactory` 實作啟動的中介軟體 (*Middleware/SimpleInjectorActivatedMiddleware.cs*)：</span><span class="sxs-lookup"><span data-stu-id="b925a-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="b925a-123">將會針對此中介軟體建立延伸模組 (*Middleware/MiddlewareExtensions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="b925a-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="b925a-124">`Startup.ConfigureServices` 必須執行幾項工作：</span><span class="sxs-lookup"><span data-stu-id="b925a-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="b925a-125">設定 Simple Injector 容器。</span><span class="sxs-lookup"><span data-stu-id="b925a-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="b925a-126">註冊 Factory 和中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b925a-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="b925a-127">讓應用程式的資料庫內容可從 Razor 頁面的 Simple Injector 容器使用。</span><span class="sxs-lookup"><span data-stu-id="b925a-127">Make the app's database context available from the Simple Injector container for a Razor Page.</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="b925a-128">此中介軟體會在要求處理管線的 `Startup.Configure` 中註冊：</span><span class="sxs-lookup"><span data-stu-id="b925a-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a><span data-ttu-id="b925a-129">其他資源</span><span class="sxs-lookup"><span data-stu-id="b925a-129">Additional resources</span></span>

* [<span data-ttu-id="b925a-130">中介軟體</span><span class="sxs-lookup"><span data-stu-id="b925a-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="b925a-131">Factory 式中介軟體啟用</span><span class="sxs-lookup"><span data-stu-id="b925a-131">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* <span data-ttu-id="b925a-132">[Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector) (Simple Injector GitHub 存放庫)</span><span class="sxs-lookup"><span data-stu-id="b925a-132">[Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector)</span></span>
* <span data-ttu-id="b925a-133">[Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) (Simple Injector 文件)</span><span class="sxs-lookup"><span data-stu-id="b925a-133">[Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html)</span></span>
