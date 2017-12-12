---
title: "相依性插入控制器"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bc8b4ba3-e9ba-48fd-b1eb-cd48ff6bc7a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 46b92a1cab6fb2cd06eff44feb6a55788fca5c2a
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2017
---
# <a name="dependency-injection-into-controllers"></a>相依性插入控制器

<a name="dependency-injection-controllers"></a>

由[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 控制器應該要求明確透過其建構函式及其相依性。 在某些情況下，個別的控制器動作可能需要一項服務，並不合理，要求在控制器層級。 在此情況下，您也可以選擇將做為參數，動作方法上的服務。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>相依性插入

相依性插入是一種技術，遵循[相依性反向原則](http://deviq.com/dependency-inversion-principle/)，以允許鬆散偶合的模組組成的應用程式。 ASP.NET Core 已內建支援[相依性插入](../../fundamentals/dependency-injection.md)，這讓您更輕鬆地測試和維護應用程式。

## <a name="constructor-injection"></a>建構函式插入

ASP.NET Core 建構函式為基礎的相依性插入的內建支援會延伸到 MVC 控制器。 只要加入您的控制站做為建構函式參數的服務類型，ASP.NET Core 將嘗試解析該服務容器中使用內建的類型。 服務是一般而言，但不是一定定義介面。 例如，如果您的應用程式有相依於目前時間的商務邏輯，您可以將插入的服務，擷取的時間 （而非硬式編碼它），這可讓您在實作中使用固定的時間，成功的測試。

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


實作這類介面，以便讓它在執行階段使用系統時鐘是 trivial:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


使用此位置中，我們可以使用我們控制器中的服務。 在此情況下，我們新增了某個邏輯來`HomeController``Index`向使用者顯示問候語方法為基礎的當日時間。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

當我們執行應用程式現在，我們很可能會發生錯誤：

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

我們還沒有設定中的服務，就會發生此錯誤`ConfigureServices`方法中的我們`Startup`類別。 若要指定要求的`IDateTime`應使用的執行個體解析`SystemDateTime`，強調顯示的行會加入下面清單，以便在您`ConfigureServices`方法：

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> 無法使用任何存留期不同選項來實作此特定服務 (`Transient`， `Scoped`，或`Singleton`)。 請參閱[相依性插入](../../fundamentals/dependency-injection.md)來了解每個範圍選項會如何影響您的服務行為。

一旦服務已設定，執行應用程式，以及巡覽至 [首頁] 頁面應該會顯示時間為基礎之訊息如預期般：

![伺服器問候語](dependency-injection/_static/server-greeting.png)

>[!TIP]
> 請參閱[測試控制器邏輯](testing.md)以了解如何明確地要求的相依性[http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/)控制站可以更輕鬆地測試的程式碼。

ASP.NET Core 內建的相依性插入支援只有單一建構函式類別要求服務。 如果您有一個以上的建構函式，您可能會收到例外狀況的說明：

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

如錯誤訊息所述，您可以修正這個問題只單一的建構函式。 您也可以[預設相依性插入支援取代的第三方實作](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)，許多支援的多個建構函式。

## <a name="action-injection-with-fromservices"></a>與 FromServices 動作資料隱碼

有時候您不需要多個動作的服務在您的控制器。 在此情況下，可能會更有意義將服務作為動作方法的參數。 這樣做，將標記與屬性參數`[FromServices]`如下所示：

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>存取控制站的設定

存取應用程式或組態設定從控制器內是常見的模式。 此存取都應該使用中所述的選項模式[組態](xref:fundamentals/configuration/index)。 您通常應該不會要求設定直接從您使用相依性插入的控制器。 更好的方法是要求`IOptions<T>`執行個體，其中`T`是您需要的組態類別。

若要使用的選項模式，您需要建立表示的選項，這種類別：

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

然後您必須設定應用程式使用選項模型，並將您的組態類別加入至服務集合`ConfigureServices`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> 在上述清單中，我們正在設定應用程式讀取從 JSON 格式化檔案的設定。 您也可以進行設定完全以程式碼，如上述加上註解的程式碼所示。 請參閱[組態](xref:fundamentals/configuration/index)進一步設定選項。

一旦您所指定的強型別組態物件 (在此情況下， `SampleWebSettings`) 並將它新增至服務的集合，您可要求從任何控制器或動作方法所要求的執行個體`IOptions<T>`(在此情況下， `IOptions<SampleWebSettings>`). 下列程式碼會示範一個控制器從要求設定的方式：

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

遵循選項模式可讓設定和組態分離，並控制站之後，可確保[的重要性分離](http://deviq.com/separation-of-concerns/)，因為它不需要知道方式或位置以尋找設定資訊。 它也可以控制站更輕鬆地單元測試[測試控制器邏輯](testing.md)，因為沒有任何[靜態黏貼](http://deviq.com/static-cling/)或直接具現化控制器類別內的設定類別。
