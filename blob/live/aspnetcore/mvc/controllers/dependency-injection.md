---
title: "相依性插入控制器"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 21cffcd285879fdca81cb7d92d0f079d4bf7756c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="fe89f-102">相依性插入控制器</span><span class="sxs-lookup"><span data-stu-id="fe89f-102">Dependency injection into controllers</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="fe89f-103">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="fe89f-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fe89f-104">ASP.NET Core MVC 控制器應該要求明確透過其建構函式及其相依性。</span><span class="sxs-lookup"><span data-stu-id="fe89f-104">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="fe89f-105">在某些情況下，個別的控制器動作可能需要一項服務，並不合理，要求在控制器層級。</span><span class="sxs-lookup"><span data-stu-id="fe89f-105">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="fe89f-106">在此情況下，您也可以選擇將做為參數，動作方法上的服務。</span><span class="sxs-lookup"><span data-stu-id="fe89f-106">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="fe89f-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fe89f-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="fe89f-108">相依性插入</span><span class="sxs-lookup"><span data-stu-id="fe89f-108">Dependency Injection</span></span>

<span data-ttu-id="fe89f-109">相依性插入是一種技術，遵循[相依性反向原則](http://deviq.com/dependency-inversion-principle/)，以允許鬆散偶合的模組組成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe89f-109">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="fe89f-110">ASP.NET Core 已內建支援[相依性插入](../../fundamentals/dependency-injection.md)，這讓您更輕鬆地測試和維護應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe89f-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="fe89f-111">建構函式插入</span><span class="sxs-lookup"><span data-stu-id="fe89f-111">Constructor Injection</span></span>

<span data-ttu-id="fe89f-112">ASP.NET Core 建構函式為基礎的相依性插入的內建支援會延伸到 MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="fe89f-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="fe89f-113">只要加入您的控制站做為建構函式參數的服務類型，ASP.NET Core 將嘗試解析該服務容器中使用內建的類型。</span><span class="sxs-lookup"><span data-stu-id="fe89f-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="fe89f-114">服務是一般而言，但不是一定定義介面。</span><span class="sxs-lookup"><span data-stu-id="fe89f-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="fe89f-115">例如，如果您的應用程式有相依於目前時間的商務邏輯，您可以將插入的服務，擷取的時間 （而非硬式編碼它），這可讓您在實作中使用固定的時間，成功的測試。</span><span class="sxs-lookup"><span data-stu-id="fe89f-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="fe89f-116">實作這類介面，以便讓它在執行階段使用系統時鐘是 trivial:</span><span class="sxs-lookup"><span data-stu-id="fe89f-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="fe89f-117">使用此位置中，我們可以使用我們控制器中的服務。</span><span class="sxs-lookup"><span data-stu-id="fe89f-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="fe89f-118">在此情況下，我們新增了某個邏輯來`HomeController``Index`向使用者顯示問候語方法為基礎的當日時間。</span><span class="sxs-lookup"><span data-stu-id="fe89f-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="fe89f-119">當我們執行應用程式現在，我們很可能會發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="fe89f-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="fe89f-120">我們還沒有設定中的服務，就會發生此錯誤`ConfigureServices`方法中的我們`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="fe89f-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="fe89f-121">若要指定要求的`IDateTime`應使用的執行個體解析`SystemDateTime`，強調顯示的行會加入下面清單，以便在您`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="fe89f-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="fe89f-122">無法使用任何存留期不同選項來實作此特定服務 (`Transient`， `Scoped`，或`Singleton`)。</span><span class="sxs-lookup"><span data-stu-id="fe89f-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="fe89f-123">請參閱[相依性插入](../../fundamentals/dependency-injection.md)來了解每個範圍選項會如何影響您的服務行為。</span><span class="sxs-lookup"><span data-stu-id="fe89f-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="fe89f-124">一旦服務已設定，執行應用程式，以及巡覽至 [首頁] 頁面應該會顯示時間為基礎之訊息如預期般：</span><span class="sxs-lookup"><span data-stu-id="fe89f-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![伺服器問候語](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="fe89f-126">請參閱[測試控制器邏輯](testing.md)以了解如何明確地要求的相依性[http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/)控制站可以更輕鬆地測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fe89f-126">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="fe89f-127">ASP.NET Core 內建的相依性插入支援只有單一建構函式類別要求服務。</span><span class="sxs-lookup"><span data-stu-id="fe89f-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="fe89f-128">如果您有一個以上的建構函式，您可能會收到例外狀況的說明：</span><span class="sxs-lookup"><span data-stu-id="fe89f-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="fe89f-129">如錯誤訊息所述，您可以修正這個問題只單一的建構函式。</span><span class="sxs-lookup"><span data-stu-id="fe89f-129">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="fe89f-130">您也可以[預設相依性插入支援取代的第三方實作](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)，許多支援的多個建構函式。</span><span class="sxs-lookup"><span data-stu-id="fe89f-130">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="fe89f-131">與 FromServices 動作資料隱碼</span><span class="sxs-lookup"><span data-stu-id="fe89f-131">Action Injection with FromServices</span></span>

<span data-ttu-id="fe89f-132">有時候您不需要多個動作的服務在您的控制器。</span><span class="sxs-lookup"><span data-stu-id="fe89f-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="fe89f-133">在此情況下，可能會更有意義將服務作為動作方法的參數。</span><span class="sxs-lookup"><span data-stu-id="fe89f-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="fe89f-134">這樣做，將標記與屬性參數`[FromServices]`如下所示：</span><span class="sxs-lookup"><span data-stu-id="fe89f-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="fe89f-135">存取控制站的設定</span><span class="sxs-lookup"><span data-stu-id="fe89f-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="fe89f-136">存取應用程式或組態設定從控制器內是常見的模式。</span><span class="sxs-lookup"><span data-stu-id="fe89f-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="fe89f-137">此存取都應該使用中所述的選項模式[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="fe89f-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="fe89f-138">您通常應該不會要求設定直接從您使用相依性插入的控制器。</span><span class="sxs-lookup"><span data-stu-id="fe89f-138">You generally should not request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="fe89f-139">更好的方法是要求`IOptions<T>`執行個體，其中`T`是您需要的組態類別。</span><span class="sxs-lookup"><span data-stu-id="fe89f-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="fe89f-140">若要使用的選項模式，您需要建立表示的選項，這種類別：</span><span class="sxs-lookup"><span data-stu-id="fe89f-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="fe89f-141">然後您必須設定應用程式使用選項模型，並將您的組態類別加入至服務集合`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fe89f-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="fe89f-142">在上述清單中，我們正在設定應用程式讀取從 JSON 格式化檔案的設定。</span><span class="sxs-lookup"><span data-stu-id="fe89f-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="fe89f-143">您也可以進行設定完全以程式碼，如上述加上註解的程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="fe89f-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="fe89f-144">請參閱[組態](xref:fundamentals/configuration/index)進一步設定選項。</span><span class="sxs-lookup"><span data-stu-id="fe89f-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="fe89f-145">一旦您所指定的強型別組態物件 (在此情況下， `SampleWebSettings`) 並將它新增至服務的集合，您可要求從任何控制器或動作方法所要求的執行個體`IOptions<T>`(在此情況下， `IOptions<SampleWebSettings>`).</span><span class="sxs-lookup"><span data-stu-id="fe89f-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="fe89f-146">下列程式碼會示範一個控制器從要求設定的方式：</span><span class="sxs-lookup"><span data-stu-id="fe89f-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="fe89f-147">遵循選項模式可讓設定和組態分離，並控制站之後，可確保[的重要性分離](http://deviq.com/separation-of-concerns/)，因為它不需要知道方式或位置以尋找設定資訊。</span><span class="sxs-lookup"><span data-stu-id="fe89f-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="fe89f-148">它也可以控制站更輕鬆地單元測試[測試控制器邏輯](testing.md)，因為沒有任何[靜態黏貼](http://deviq.com/static-cling/)或直接具現化控制器類別內的設定類別。</span><span class="sxs-lookup"><span data-stu-id="fe89f-148">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there is no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
