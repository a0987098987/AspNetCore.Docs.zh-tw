---
title: ASP.NET Core 控制器的相依性插入
author: ardalis
description: 了解 ASP.NET Core MVC 控制器如何在 ASP.NET Core 中，透過其含有相依性插入的建構函式明確要求其相依性。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 9d9d0a68927da62fad8df72c868eaf4b8ada440d
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410267"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="961f7-103">ASP.NET Core 控制器的相依性插入</span><span class="sxs-lookup"><span data-stu-id="961f7-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="961f7-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="961f7-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="961f7-105">ASP.NET Core MVC 控制器應該透過其建構函式明確要求相依性。</span><span class="sxs-lookup"><span data-stu-id="961f7-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="961f7-106">在某些情況下，個別的控制器動作可能需要某項服務，但在控制器層級提出這項要求並不合理。</span><span class="sxs-lookup"><span data-stu-id="961f7-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="961f7-107">這時候，您也可以透過動作方法上的參數形式來插入服務。</span><span class="sxs-lookup"><span data-stu-id="961f7-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="961f7-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="961f7-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="961f7-109">相依性插入</span><span class="sxs-lookup"><span data-stu-id="961f7-109">Dependency Injection</span></span>

<span data-ttu-id="961f7-110">ASP.NET Core 已內建支援[相依性插入](../../fundamentals/dependency-injection.md)，您可更輕鬆地測試和維護應用程式。</span><span class="sxs-lookup"><span data-stu-id="961f7-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="961f7-111">建構函式插入</span><span class="sxs-lookup"><span data-stu-id="961f7-111">Constructor Injection</span></span>

<span data-ttu-id="961f7-112">ASP.NET Core 內建支援以建構函式為基礎的相依性插入，這項支援也延伸到 MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="961f7-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="961f7-113">只要您將服務類型以建構函式參數形式新增至控制器，ASP.NET Core 就會嘗試使用其內建服務容器來解析該類型。</span><span class="sxs-lookup"><span data-stu-id="961f7-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="961f7-114">一般而言 (但並非絕對)，您可以使用介面來定義服務。</span><span class="sxs-lookup"><span data-stu-id="961f7-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="961f7-115">例如，如果您的應用程式有相依於目前時間的商務邏輯，您可以插入服務以擷取時間 (而非將其寫入程式碼)，即可在使用指定時間的實作中成功通過測試。</span><span class="sxs-lookup"><span data-stu-id="961f7-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="961f7-116">實作這種介面以便在執行階段時使用系統時鐘，這樣會比較簡單：</span><span class="sxs-lookup"><span data-stu-id="961f7-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="961f7-117">就緒時，我們即可使用控制器中的服務。</span><span class="sxs-lookup"><span data-stu-id="961f7-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="961f7-118">在這個案例中，我們將某個邏輯新增至 `HomeController``Index` 方法，以依據一天中的時間向使用者顯示問候語。</span><span class="sxs-lookup"><span data-stu-id="961f7-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="961f7-119">如果現在執行應用程式，我們很可能會發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="961f7-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="961f7-120">當我們尚未在 `Startup` 類別的 `ConfigureServices` 方法中設定服務時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="961f7-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="961f7-121">若要指定系統應該使用 `SystemDateTime` 的執行個體來解析對 `IDateTime` 的要求，請將清單中強調顯示的那一行新增至 `ConfigureServices` 方法下面：</span><span class="sxs-lookup"><span data-stu-id="961f7-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="961f7-122">您可以使用任何不同的存留期間選項 (`Transient`、`Scoped` 或 `Singleton`) 來實作這個特定服務。</span><span class="sxs-lookup"><span data-stu-id="961f7-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="961f7-123">請參閱[相依性插入](../../fundamentals/dependency-injection.md)，以了解每個範圍選項會如何影響您的服務行為。</span><span class="sxs-lookup"><span data-stu-id="961f7-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="961f7-124">一旦服務已設定，在執行應用程式與巡覽到首頁時，應該會如預期般顯示以時間為基礎的訊息：</span><span class="sxs-lookup"><span data-stu-id="961f7-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![伺服器問候語](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="961f7-126">請參閱[測試控制器邏輯](testing.md)以了解如何透過明確地要求控制器中的相依性，以讓程式碼更容易測試。</span><span class="sxs-lookup"><span data-stu-id="961f7-126">See [Test controller logic](testing.md) to learn how to make code easier to test by explicitly requesting dependencies in controllers.</span></span>

<span data-ttu-id="961f7-127">ASP.NET Core 內建的相依性插入支援只使用單一建構函式來進行類別的服務要求。</span><span class="sxs-lookup"><span data-stu-id="961f7-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="961f7-128">如果您有一個以上的建構函式，可能會收到如下的例外狀況說明：</span><span class="sxs-lookup"><span data-stu-id="961f7-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="961f7-129">如錯誤訊息所述，使用單一建構函式即可修正這個問題。</span><span class="sxs-lookup"><span data-stu-id="961f7-129">As the error message states, you can correct this problem with the use of a single constructor.</span></span> <span data-ttu-id="961f7-130">您也可以[使用協力廠商的實作來取代預設相依性插入容器](xref:fundamentals/dependency-injection#default-service-container-replacement)，其中有許多實作均支援多個建構函式。</span><span class="sxs-lookup"><span data-stu-id="961f7-130">You can also [replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="961f7-131">使用 FromServices 的動作插入</span><span class="sxs-lookup"><span data-stu-id="961f7-131">Action Injection with FromServices</span></span>

<span data-ttu-id="961f7-132">有時候您不需要針對控制器內的多個動作專設一項服務。</span><span class="sxs-lookup"><span data-stu-id="961f7-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="961f7-133">在這種情況下，更適合的做法是以動作方法的參數形式插入服務。</span><span class="sxs-lookup"><span data-stu-id="961f7-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="961f7-134">若要這麼做，您可以使用 `[FromServices]` 屬性來標記參數，如下所示：</span><span class="sxs-lookup"><span data-stu-id="961f7-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="961f7-135">透過控制器存取設定</span><span class="sxs-lookup"><span data-stu-id="961f7-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="961f7-136">透過控制器來存取應用程式或組態設定是常見的模式。</span><span class="sxs-lookup"><span data-stu-id="961f7-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="961f7-137">您應該使用[組態](xref:fundamentals/configuration/index)中所述的「選項」模式來進行這類存取。</span><span class="sxs-lookup"><span data-stu-id="961f7-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="961f7-138">一般來說，您不應該使用相依性插入直接向控制器要求設定。</span><span class="sxs-lookup"><span data-stu-id="961f7-138">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="961f7-139">更理想的方法是要求 `IOptions<T>` 執行個體，其中 `T` 是您需要的組態類別。</span><span class="sxs-lookup"><span data-stu-id="961f7-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="961f7-140">若要使用「選項」模式，您必須建立用來代表選項的類別，例如：</span><span class="sxs-lookup"><span data-stu-id="961f7-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="961f7-141">接著，您必須將應用程式設為使用選項模型，並將組態類別新增至 `ConfigureServices` 中的服務集合：</span><span class="sxs-lookup"><span data-stu-id="961f7-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="961f7-142">在上述清單中，我們會設定應用程式以讀取來自 JSON 格式化檔案的設定。</span><span class="sxs-lookup"><span data-stu-id="961f7-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="961f7-143">您也可以完全以程式碼方式進行設定，如上方註解的程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="961f7-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="961f7-144">如需詳細的組態選項，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="961f7-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="961f7-145">一旦您已指定強型別組態物件 (在此例中為 `SampleWebSettings`) 並將它新增至服務集合，即可藉由要求 `IOptions<T>` 的執行個體 (在此例中為 `IOptions<SampleWebSettings>`)，以透過任何控制器或動作方法來要求該物件。</span><span class="sxs-lookup"><span data-stu-id="961f7-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="961f7-146">下列程式碼會示範使用者如何透過控制器來要求設定：</span><span class="sxs-lookup"><span data-stu-id="961f7-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="961f7-147">採用「選項」模式時，可讓設定和組態彼此分離，並確保控制器落實 [Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (關注點分離) 原則，因為控制器不需要知道如何尋找設定資訊以及到何處尋找。</span><span class="sxs-lookup"><span data-stu-id="961f7-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="961f7-148">這麼做也可以確保控制器能更輕鬆地執行[單元測試](testing.md)，因為控制器類別內的設定類別不會有任何直接具現化的問題。</span><span class="sxs-lookup"><span data-stu-id="961f7-148">It also makes the controller easier to [unit test](testing.md), since there's no direct instantiation of settings classes within the controller class.</span></span>
