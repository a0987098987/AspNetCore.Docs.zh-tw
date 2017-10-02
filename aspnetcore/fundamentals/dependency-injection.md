---
title: "在 ASP.NET Core 的相依性插入"
author: ardalis
description: "了解 ASP.NET Core 實作相依性插入的方式以及如何使用它。"
keywords: "ASP.NET Core 相依性插入、 di"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fccd69be-7ad1-47fb-b203-b3633b6b9a9b
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f5c903a72d004afac55fbcc04ad157442e7a18ee
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a>在 ASP.NET Core 的相依性插入的簡介

<a name=fundamentals-dependency-injection></a>

由[Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)

ASP.NET Core 被設計，註冊支援，並利用相依性插入。 ASP.NET Core 應用程式可以利用內建架構服務，讓它們插入到啟動類別中的方法，可以將應用程式服務設定以及資料隱碼。 ASP.NET Core 所提供的預設服務容器會提供最少的功能設定，而不是取代其他容器。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>相依性插入是什麼？

相依性插入 (DI) 是針對達到物件和共同作業者或相依性之間的鬆散偶合的技術。 而非直接具現化共同作業者，或使用靜態參考，才能執行其動作的類別需要的物件會提供以某些方式中的類別。 大多數情況下，類別會宣告它們的相依性，透過其建構函式，讓他們遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。 這種方法稱為 「 建構函式資料隱碼 >。

類別專為 DI 設計，當它們是更鬆散偶合因為他們共同作業者上並沒有直接的硬式編碼相依性。 這會遵循[相依性反向原則](http://deviq.com/dependency-inversion-principle/)，其指出*「 高的層級模組不應該依賴低層級的模組; 兩者都需要具備的抽象概念 」。* 而不是參考特定實作，類別會要求抽象層 (通常`interfaces`) 建構類別時，這提供給它們。 擷取相依性介面，並提供實作這些介面做為參數也是範例[策略設計模式](http://deviq.com/strategy-design-pattern/)。

系統設計為使用 DI，具有許多要求透過其建構函式 （或屬性），及其相依性的類別時有專門用來建立這些類別及其相關聯的相依性的類別幫助。 這些類別統稱為*容器*，更具體來說，[逆轉控制 (IoC)](http://deviq.com/inversion-of-control/)容器或相依性插入 (DI) 容器。 容器是基本上會負責提供向它要求類型的執行個體的 factory。 如果指定的型別已經宣告，它有相依性，容器已設定為提供相依性類型，它會建立在建立要求的執行個體的相依性。 如此一來，可以提供複雜的相依性圖形，而不需要任何硬式編碼的物件建構的類別。 除了建立物件相依性，容器通常會管理應用程式中的物件存留期。

ASP.NET Core 包含簡單的內建容器 (由`IServiceProvider`介面)，根據預設，支援建構函式插入並 ASP.NET 的特定服務提供透過 DI。 ASP。網路的容器做為其所管理的類型是指*服務*。 此篇文章的其餘*服務*會參考受 ASP.NET Core IoC 容器的類型。 設定中的內建的容器服務`ConfigureServices`應用程式中的方法`Startup`類別。

> [!NOTE]
> Martin Fowler 已寫入大量的發行項上[逆轉控制容器和相依性插入模式](https://www.martinfowler.com/articles/injection.html)。 Microsoft Patterns and Practices 也有很棒的描述[相依性插入](https://msdn.microsoft.com/library/hh323705.aspx)。

> [!NOTE]
> 本文涵蓋相依性插入套用到所有的 ASP.NET 應用程式。 MVC 控制器內的相依性插入在講述[相依性插入和控制器](../mvc/controllers/dependency-injection.md)。

### <a name="constructor-injection-behavior"></a>建構函式資料隱碼行為

插入建構函式需要有問題的建構函式是*公用*。 否則，您的應用程式將會擲回`InvalidOperationException`:

> 找不到類型 'YourType' 適合建構函式。 請確定類型是具象，且服務中註冊的所有參數的公用建構函式。


插入建構函式需要該只能有一個適用於建構函式存在。 支援的建構函式多載，但只有一個多載可以存在於其引數可以所有滿足的相依性插入。 如果有多個，您的應用程式將會擲回`InvalidOperationException`:

> 類型 'YourType' 中已找到多個建構函式接受所有指定的引數類型。 只能有一個適用的建構函式。

建構函式可以接受所未提供的相依性插入的引數，但這些項目必須支援的預設值。 例如: 

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>使用 Framework 提供的服務

`ConfigureServices`方法中的`Startup`類別會負責定義將使用的應用程式，包括平台功能，例如 Entity Framework Core 與 ASP.NET Core MVC 的服務。 一開始，`IServiceCollection`提供給`ConfigureServices`具有下列服務定義 (取決於[主應用程式的設定方式](xref:fundamentals/hosting)):

| 服務類型 | 存留期 |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | 單一 |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | 單一 |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | 單一 |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | 暫時性 |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | 暫時性 |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | 單一 |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | 單一 |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | 單一 |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | 暫時性 |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | 單一 |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | 暫時性 |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | 單一 |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | 單一 |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | 單一 |

以下是範例，將其他服務加入至容器使用的擴充方法類似的數字的`AddDbContext`， `AddIdentity`，和`AddMvc`。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

功能與中介軟體提供的 ASP.NET MVC 中，例如依照慣例，以使用單一新增的*ServiceName*來註冊所有該功能所需之服務的擴充方法。

>[!TIP]
> 您可以要求內的特定架構提供服務`Startup`方法，透過其參數清單中-請參閱[應用程式啟動](startup.md)如需詳細資訊。

## <a name="registering-your-own-services"></a>註冊您自己的服務

您可以註冊自己的應用程式服務，如下所示。 第一個泛型型別代表將會要求從容器的類型 （通常的介面）。 第二個泛型型別表示的具象型別會具現化的容器，用來滿足這類要求。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> 每個`services.Add<ServiceName>`擴充方法新增 （並可能設定） 服務。 例如，`services.AddMvc()`新增 MVC 要求的服務。 建議您遵循這個慣例，將中的擴充方法放`Microsoft.Extensions.DependencyInjection`命名空間，來封裝服務註冊的群組。

`AddTransient`方法用來將抽象型別對應至具象執行個體化分開，每個物件所需的服務。 這稱為 「 服務*存留期*，和其他的存留期選項說明如下。 請務必選擇適當的存留時間為每個您註冊服務。 應該用於每個要求的類別提供服務的新執行個體嗎？ 應該有一個執行個體用於給定的 web 要求嗎？ 或應該單一執行個體使用的應用程式的存留期間嗎？

在本文範例中，沒有顯示字元的名稱，稱為簡單控制器`CharactersController`。 其`Index`方法會顯示目前的應用程式中所儲存的字元清單，並初始化集合少數幾個字元，如果皆不存在。 請注意，雖然此應用程式使用 Entity Framework Core 和`ApplicationDbContext`其持續性，該 「 無 」 明顯控制器中的類別。 特定資料存取機制相反地，抽象介面背後`ICharacterRepository`，哪些如下所示[儲存機制模式](http://deviq.com/repository-pattern/)。 執行個體`ICharacterRepository`已透過建構函式要求，並指派給私用欄位，然後用來依需要存取的字元。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository`定義兩個方法，控制站需要搭配`Character`執行個體。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

具象型別，依序實作這個介面`CharacterRepository`，也就是在執行階段使用。

> [!NOTE]
> 搭配使用的方式 DI`CharacterRepository`類別是您可以依照您的應用程式服務，不只是在 「 儲存機制 」 或 「 資料存取類別的所有一般模型。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

請注意，`CharacterRepository`要求`ApplicationDbContext`其建構函式中。 它不是異常相依性插入使用像這樣，與每個要求的相依性，接著要求它自己的相依性鏈結的方式。 容器是負責解決所有圖形中的相依性，並傳回完整解析的服務。

> [!NOTE]
> 建立要求的物件，和所有物件，它需要，以及所有物件，這些需要，有時稱為*物件圖形*。 同樣地，必須先解析的相依性集合組通常稱為*相依性樹狀結構*或*相依性圖形*。

在此情況下，同時`ICharacterRepository`，進而`ApplicationDbContext`必須向 「 服務 」 容器中`ConfigureServices`中`Startup`。 `ApplicationDbContext`設定利用擴充方法呼叫`AddDbContext<T>`。 下列程式碼會顯示註冊`CharacterRepository`型別。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Entity Framework 內容應該加入至服務容器使用`Scoped`存留期。 這是處理自動如果您使用的 helper 方法如上所示。 儲存機制，可讓您使用 Entity Framework 應該使用相同的存留期。

>[!WARNING]
> 正在解析主要危險来謹慎`Scoped`服務從單一子句。 很可能在此情況下處理後續要求時，服務會造成不正確的狀態。

具有相依性服務應該註冊這些伺服器在容器中。 如果服務的建構函式可如需要基本型別`string`，這可以藉由插入[模式和組態選項](configuration.md)。

## <a name="service-lifetimes-and-registration-options"></a>服務的存留期和註冊選項

ASP.NET 服務可以使用下列的存留時間設定：

**暫時性**

暫時性的存留時間服務會建立每次要求。 此存留時間最適合用於輕量型、 無狀態服務。

**範圍**

已設定領域的存留時間服務會建立一次，每個要求。

**單一**

單一存留時間服務會建立第一次要求 (或當`ConfigureServices`如果您指定執行個體執行) 然後每個後續的要求將會使用相同的執行個體。 如果您的應用程式需要單一行為，允許 「 服務 」 容器來管理服務的存留期建議而不是實作單一設計模式，並管理您自己的類別中物件的存留期。

可以與幾種容器登錄服務。 我們已經看過如何藉由指定要使用的具象類型，含指定之類型註冊服務實作。 此外，此處理站可以指定，然後將用來建立隨選執行個體。 第三種方法是直接指定類型的執行個體使用，在其中案例容器將永遠不會嘗試建立執行個體 （也不會將處置的執行個體）。

若要示範這些存留期和註冊的選項之間的差異，請考慮一個簡單的介面，表示為一或多個工作*作業*具有唯一的識別碼， `OperationId`。 根據我們設定此服務的存留期，容器會提供相同或不同的執行個體的服務要求的類別。 若要清除所要求的存留期，我們將建立 lifetime 選項每一種類型：

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

我們會使用單一類別，這些介面實作`Operation`，接受`Guid`在其建構函式或使用新`Guid`若未提供。

接下來，在`ConfigureServices`，每個類型加入至的容器，根據其具名的存留期：

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

請注意，`IOperationSingletonInstance`服務所使用的特定執行個體的已知識別碼`Guid.Empty`會清除此型別時 （其 Guid 會是全部為零） 的使用中。 我們也已註冊`OperationService`相依於每個其他`Operation`類型，因此它會要求內清除這項服務是否即將控制器或新方案，每個作業類型相同的執行個體。 此服務的作用是公開其相依性為屬性，以便在檢視中顯示。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

若要示範物件存留期間內以及個別應用程式的個別要求之間，此範例包含`OperationsController`，每種類型的要求`IOperation`型別，以及`OperationService`。 `Index`動作還會顯示所有的控制站和服務的`OperationId`值。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

現在兩個不同的要求是對此控制器的動作：

![[作業] 檢視顯示暫時性、 Scoped、 單一值，和執行個體控制站和作業的第一個要求的服務作業的作業識別碼值 (GUID) 的 Microsoft Edge 中執行的相依性插入範例 web 應用程式。](dependency-injection/_static/lifetimes_request1.png)

![作業檢視顯示第二個要求的作業識別碼值。](dependency-injection/_static/lifetimes_request2.png)

觀察哪些`OperationId`內要求，並在要求之間的值會不同。

* *暫時性*物件總是不同; 的新執行個體提供給每個控制站與每個服務。

* *範圍*物件是相同的要求，但不同跨不同的要求內

* *單一*都適用於每個物件與每個要求相同的物件 (不論是否會提供執行個體中`ConfigureServices`)

## <a name="request-services"></a>要求的服務

在 ASP.NET 中可用的服務要求從`HttpContext`公開會透過`RequestServices`集合。

![指出要求服務取得或設定提供要求的服務容器的存取權的 IServiceProvider HttpContext 要求服務 Intellisense 的內容對話方塊。](dependency-injection/_static/request-services.png)

要求服務代表的服務，設定要求做為您的應用程式的一部分。 當您的物件指定相依性時，這些由中找到的類型來滿足`RequestServices`，而非`ApplicationServices`。

一般而言，您不應該使用這些屬性直接偏好改為要求的型別必須透過您的類別建構函式，您的類別，並讓架構插入這些相依性。 這會產生較易於測試的類別 (請參閱[測試](../testing/index.md)) 且更鬆散偶合。

> [!NOTE]
> 偏好做為建構函式參數，存取要求的相依性`RequestServices`集合。

## <a name="designing-your-services-for-dependency-injection"></a>設計您的服務相依性插入

您應該設計服務，以取得其共同作業者使用相依性插入。 這表示避免使用可設定狀態的靜態方法呼叫 (這會導致程式碼的氣味稱為[靜態黏貼](http://deviq.com/static-cling/)) 和您的服務內的相依類別的直接具現化。 它有助於記住片語，[新是黏附](https://ardalis.com/new-is-glue)，當您選擇是否要具現化型別，或要求透過相依性插入。 依照[實心原則的物件導向設計](http://deviq.com/solid/)，類別自然會傾向小型、 構造良好且輕鬆地測試。

如果找到您的類別通常有方式媵檔晻太多的相依性？ 這通常是一個符號，嘗試太多，您的類別，並可能違反 SRP-[單一責任原則](http://deviq.com/single-responsibility-principle/)。 如果您能夠重構類別將某些其負責的部分移到新的類別，請參閱。 請注意，您`Controller`類別應該專注於 UI 考量，讓商務規則和資料存取實作詳細資料保存在適用於這些類別[將重點分開](http://deviq.com/separation-of-concerns/)。

資料存取與具體來說，您可以將插入`DbContext`到您的控制站 (假設您已加入中服務容器中的 EF `ConfigureServices`)。 有些開發人員偏好使用至資料庫的儲存機制介面而不是插入`DbContext`直接。 使用介面來封裝資料在一個地方存取邏輯可以減少您必須變更您的資料庫時變更的數位數。

### <a name="disposing-of-services"></a>處置服務

容器會呼叫`Dispose`如`IDisposable`它所建立的類型。 不過，如果您將執行個體加入容器自行，它將不會處置。

範例：

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();

    // container did not create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> 在 1.0 版中，容器會呼叫 dispose 上*所有*`IDisposable`物件，包括未建立。

## <a name="replacing-the-default-services-container"></a>取代預設服務容器

內建的服務容器是要做的架構和建置在其上的大部分取用者應用程式的基本需求。 不過，開發人員可以使用其偏好的容器取代內建的容器。 `ConfigureServices`方法通常會傳回`void`，但是，如果其簽章變更為傳回`IServiceProvider`，可設定與傳回不同的容器。 沒有適用於.NET 的許多 IOC 容器。 在此範例中， [Autofac](https://autofac.org/)使用套件。

首先，安裝適當的容器封裝：

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

接下來，設定中的容器`ConfigureServices`並傳回`IServiceProvider`:

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add other framework services

    // Add Autofac
    var containerBuilder = new ContainerBuilder();
    containerBuilder.RegisterModule<DefaultModule>();
    containerBuilder.Populate(services);
    var container = containerBuilder.Build();
    return new AutofacServiceProvider(container);
}
```

> [!NOTE]
> 當使用協力廠商 DI 容器，您必須變更`ConfigureServices`，使它傳回`IServiceProvider`而不是`void`。

最後，像在平常一樣設定 Autofac `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

在執行階段，Autofac 將用於解析型別，以及將相依性。 [深入了解使用 Autofac 和 ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html)。

### <a name="thread-safety"></a>執行緒安全

單一服務需要具備執行緒安全。 如果單一服務有暫時性服務相依性，暫時性服務可能也需要安全視它由單一執行緒。

## <a name="recommendations"></a>建議

當使用相依性插入，請記住下列建議：

* DI 適用於具有複雜的相依性物件。 控制站、 服務、 配接器，以及儲存機制是所有的範例 DI 可能加入的物件。

* 避免直接在 DI 中儲存資料和設定。 例如，使用者的購物車通常不應該新增至服務容器。 組態應該使用[選項模型](configuration.md#options-config-objects)。 同樣地，避免只存在於以允許存取其他物件的 「 資料持有者 」 物件。 最好是要求透過 DI，盡可能所需的實際項目。

* 避免靜態存取服務。

* 避免在應用程式程式碼中的服務位置。

* 避免靜態存取`HttpContext`。

> [!NOTE]
> 建議的所有集合，例如您可能會遇到的情況下忽略其中一個必要。 我們發現例外狀況很少見-主要是非常特殊的情況下，架構本身內。

請記住，相依性插入是*替代*靜態/全域物件存取模式。 無法實現 DI 的優點，如果您混具有靜態物件的存取權。

## <a name="additional-resources"></a>其他資源

* [應用程式啟動](startup.md)

* [測試](../testing/index.md)

* [相依性插入 (MSDN) 與 ASP.NET Core 中撰寫全新的程式碼](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [容器管理應用程式的設計，Prelude： 位置？ 容器屬於](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)

* [明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)

* [逆轉控制容器和相依性插入模式](https://www.martinfowler.com/articles/injection.html)(Fowler)
