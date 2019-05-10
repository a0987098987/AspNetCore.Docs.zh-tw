---
title: ASP.NET Core 控制器的相依性插入
author: ardalis
description: 了解 ASP.NET Core MVC 控制器如何在 ASP.NET Core 中，透過其含有相依性插入的建構函式明確要求其相依性。
ms.author: riande
ms.date: 02/24/2019
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 6b08c321f4cae1f4efd8ea40300eaf4dfc2f63a1
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64890933"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="d0ac8-103">ASP.NET Core 控制器的相依性插入</span><span class="sxs-lookup"><span data-stu-id="d0ac8-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="d0ac8-104">作者：[Shadi Namrouti](https://github.com/shadinamrouti)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Steve Smith](https://github.com/ardalis)</span><span class="sxs-lookup"><span data-stu-id="d0ac8-104">By [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://github.com/ardalis)</span></span>

<span data-ttu-id="d0ac8-105">ASP.NET Core MVC 控制器會透過建構函式明確地要求相依性。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-105">ASP.NET Core MVC controllers request dependencies explicitly via constructors.</span></span> <span data-ttu-id="d0ac8-106">ASP.NET Core 內建[相依性插入 (DI)](xref:fundamentals/dependency-injection) 支援。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-106">ASP.NET Core has built-in support for [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d0ac8-107">DI 可讓您更輕鬆地測試和維護應用程式。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-107">DI makes apps easier to test and maintain.</span></span>

<span data-ttu-id="d0ac8-108">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d0ac8-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="d0ac8-109">建構函式插入</span><span class="sxs-lookup"><span data-stu-id="d0ac8-109">Constructor Injection</span></span>

<span data-ttu-id="d0ac8-110">服務會新增來作為建構函式參數，而執行階段會解析來自服務容器的服務。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-110">Services are added as a constructor parameter, and the runtime resolves the service from the service container.</span></span> <span data-ttu-id="d0ac8-111">通常可以使用介面來定義服務。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-111">Services are typically defined using interfaces.</span></span> <span data-ttu-id="d0ac8-112">例如，假設應用程式需要目前的時間。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-112">For example, consider an app that requires the current time.</span></span> <span data-ttu-id="d0ac8-113">下列介面會公開 `IDateTime` 服務：</span><span class="sxs-lookup"><span data-stu-id="d0ac8-113">The following interface exposes the `IDateTime` service:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Interfaces/IDateTime.cs?name=snippet)]

<span data-ttu-id="d0ac8-114">下列程式碼會實作 `IDateTime` 介面：</span><span class="sxs-lookup"><span data-stu-id="d0ac8-114">The following code implements the `IDateTime` interface:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Services/SystemDateTime.cs?name=snippet)]

<span data-ttu-id="d0ac8-115">將服務新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="d0ac8-115">Add the service to the service container:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup1.cs?name=snippet&highlight=3)]

<span data-ttu-id="d0ac8-116">如需 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*> 的詳細資訊，請參閱 [DI 服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-116">For more information on <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, see [DI service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="d0ac8-117">下列程式碼會根據時段來向使用者顯示問候語：</span><span class="sxs-lookup"><span data-stu-id="d0ac8-117">The following code displays a greeting to the user based on the time of day:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="d0ac8-118">執行應用程式，並根據時間顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-118">Run the app and a message is displayed based on the time.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="d0ac8-119">使用 FromServices 進行動作插入</span><span class="sxs-lookup"><span data-stu-id="d0ac8-119">Action injection with FromServices</span></span>

<span data-ttu-id="d0ac8-120"><xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> 能夠將服務直接插入至動作方法，而不需使用建構函式插入：</span><span class="sxs-lookup"><span data-stu-id="d0ac8-120">The <xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> enables injecting a service directly into an action method without using constructor injection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet2)]

## <a name="access-settings-from-a-controller"></a><span data-ttu-id="d0ac8-121">從控制器存取設定</span><span class="sxs-lookup"><span data-stu-id="d0ac8-121">Access settings from a controller</span></span>

<span data-ttu-id="d0ac8-122">從控制器存取應用程式或組態設定是常見的模式。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-122">Accessing app or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="d0ac8-123"><xref:fundamentals/configuration/options> 中所述的「選項模式」是管理設定的慣用方法。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-123">The *options pattern* described in <xref:fundamentals/configuration/options> is the preferred approach to manage settings.</span></span> <span data-ttu-id="d0ac8-124">一般而言，不要將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 直接插入至控制器。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-124">Generally, don't directly inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into a controller.</span></span>

<span data-ttu-id="d0ac8-125">建立要代表選項的類別。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-125">Create a class that represents the options.</span></span> <span data-ttu-id="d0ac8-126">例如：</span><span class="sxs-lookup"><span data-stu-id="d0ac8-126">For example:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Models/SampleWebSettings.cs?name=snippet)]

<span data-ttu-id="d0ac8-127">將設定類別新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="d0ac8-127">Add the configuration class to the services collection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup.cs?highlight=4&name=snippet1)]

<span data-ttu-id="d0ac8-128">設定應用程式以從 JSON 格式的檔案中讀取設定：</span><span class="sxs-lookup"><span data-stu-id="d0ac8-128">Configure the app to read the settings from a JSON-formatted file:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Program.cs?name=snippet&range=10-15)]

<span data-ttu-id="d0ac8-129">下列程式碼會向服務容器要求 `IOptions<SampleWebSettings>` 設定，並在 `Index` 方法中使用它們：</span><span class="sxs-lookup"><span data-stu-id="d0ac8-129">The following code requests the `IOptions<SampleWebSettings>` settings from the service container and uses them in the `Index` method:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/SettingsController.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="d0ac8-130">其他資源</span><span class="sxs-lookup"><span data-stu-id="d0ac8-130">Additional resources</span></span>

* <span data-ttu-id="d0ac8-131">請參閱 <xref:mvc/controllers/testing>，以了解如何透過明確地要求控制器中的相依性，讓程式碼更容易測試。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-131">See <xref:mvc/controllers/testing> to learn how to make code easier to test by explicitly requesting dependencies in controllers.</span></span>

* <span data-ttu-id="d0ac8-132">[使用協力廠商實作來取代預設的相依性插入容器](xref:fundamentals/dependency-injection#default-service-container-replacement)。</span><span class="sxs-lookup"><span data-stu-id="d0ac8-132">[Replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement).</span></span>
