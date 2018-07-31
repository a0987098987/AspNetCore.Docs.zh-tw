---
title: ASP.NET Core 控制器的相依性插入
author: ardalis
description: 了解 ASP.NET Core MVC 控制器如何在 ASP.NET Core 中，透過其含有相依性插入的建構函式明確要求其相依性。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 9dec9807e8fc2883144b2da518f36a7eb8ddc871
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342129"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a>ASP.NET Core 控制器的相依性插入

<a name="dependency-injection-controllers"></a>

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 控制器應該透過其建構函式明確要求相依性。 在某些情況下，個別的控制器動作可能需要某項服務，但在控制器層級提出此要求並不合理。 這時候，您也可以透過動作方法上的參數形式來插入服務。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>相依性插入

相依性插入是一種遵循[相依性反向準則](http://deviq.com/dependency-inversion-principle/)的技術，可讓您使用鬆散耦合的模組組成應用程式。 ASP.NET Core 已內建支援[相依性插入](../../fundamentals/dependency-injection.md)，您可更輕鬆地測試和維護應用程式。

## <a name="constructor-injection"></a>建構函式插入

ASP.NET Core 內建支援以建構函式為基礎的相依性插入，此支援也延伸到 MVC 控制器。 只要您將服務類型以建構函式參數形式新增至控制器，ASP.NET Core 就會嘗試使用其內建服務容器來解析該類型。 一般而言 (但並非絕對)，您可以使用介面來定義服務。 例如，如果您的應用程式有相依於目前時間的商務邏輯，您可以插入服務以擷取時間 (而非將其寫入程式碼)，即可在使用指定時間的實作中成功通過測試。

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


實作這種介面以便在執行階段時使用系統時鐘，這樣會比較簡單：

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


就緒時，我們即可使用控制器中的服務。 在這個案例中，我們將某個邏輯新增至 `HomeController``Index` 方法，以依據一天中的時間向使用者顯示問候語。

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

如果現在執行應用程式，我們很可能會發生錯誤：

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

當我們尚未在 `Startup` 類別的 `ConfigureServices` 方法中設定服務時，就會發生此錯誤。 若要指定系統應該使用 `SystemDateTime` 的執行個體來解析對 `IDateTime` 的要求，請將清單中強調顯示的那一行新增至 `ConfigureServices` 方法下面：

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> 您可以使用任何不同的存留期間選項 (`Transient`、`Scoped` 或 `Singleton`) 來實作這個特定服務。 請參閱[相依性插入](../../fundamentals/dependency-injection.md)，以了解每個範圍選項會如何影響您的服務行為。

一旦服務已設定，在執行應用程式與巡覽到首頁時，應該會如預期般顯示以時間為基礎的訊息：

![伺服器問候語](dependency-injection/_static/server-greeting.png)

>[!TIP]
> 請參閱[測試控制器邏輯](testing.md)，以了解為何在控制器中明確要求相依性 [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) 可讓您更輕鬆地測試程式碼。

ASP.NET Core 內建的相依性插入支援只使用單一建構函式來進行類別的服務要求。 如果您有一個以上的建構函式，可能會收到如下的例外狀況說明：

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

如錯誤訊息所述，使用單一建構函式即可修正這個問題。 您也可以[使用協力廠商的實作來取代預設相依性插入容器](xref:fundamentals/dependency-injection#default-service-container-replacement)，其中有許多實作均支援多個建構函式。

## <a name="action-injection-with-fromservices"></a>使用 FromServices 的動作插入

有時候您不需要針對控制器內的多個動作專設一項服務。 在這種情況下，更適合的做法是以動作方法的參數形式插入服務。 若要這麼做，您可以使用 `[FromServices]` 屬性來標記參數，如下所示：

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>透過控制器存取設定

透過控制器來存取應用程式或組態設定是常見的模式。 您應該使用[組態](xref:fundamentals/configuration/index)中所述的「選項」模式來進行這類存取。 一般來說，您不應該使用相依性插入直接向控制器要求設定。 更理想的方法是要求 `IOptions<T>` 執行個體，其中 `T` 是您需要的組態類別。

若要使用「選項」模式，您必須建立用來代表選項的類別，例如：

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

接著，您必須將應用程式設為使用選項模型，並將組態類別新增至 `ConfigureServices` 中的服務集合：

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> 在上述清單中，我們會設定應用程式以讀取來自 JSON 格式化檔案的設定。 您也可以完全以程式碼方式進行設定，如上方註解的程式碼所示。 如需詳細的組態選項，請參閱[組態](xref:fundamentals/configuration/index)。

一旦您已指定強型別組態物件 (在此例中為 `SampleWebSettings`) 並將它新增至服務集合，即可藉由要求 `IOptions<T>` 的執行個體 (在此例中為 `IOptions<SampleWebSettings>`)，以透過任何控制器或動作方法來要求該物件。 下列程式碼會示範使用者如何透過控制器來要求設定：

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

採用「選項」模式時，可讓設定和組態彼此分離，並確保控制器落實 [Separation of Concerns](http://deviq.com/separation-of-concerns/) (關注點分離) 原則，因為控制器不需要知道如何尋找設定資訊以及到何處尋找。 這麼做也可以確保控制器能更輕鬆針對[測試控制器邏輯](testing.md)進行單元測試，因為控制器類別內的設定類別不會有任何 [Static Cling](http://deviq.com/static-cling/) (非預期耦合) 或直接具現化的問題。
