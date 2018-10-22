---
title: 在 ASP.NET Core 中使用變更權杖來偵測變更
author: guardrex
description: 了解如何使用變更權杖來追蹤變更。
ms.author: riande
ms.date: 11/10/2017
uid: fundamentals/change-tokens
ms.openlocfilehash: 1cf3693764919a8fd064584ab7b7ad237e8465b3
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391374"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>在 ASP.NET Core 中使用變更權杖來偵測變更

作者：[Luke Latham](https://github.com/guardrex)

「變更權杖」是用來追蹤變更的一般用途低階建置組塊。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/change-tokens/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken 介面

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) 可傳播已發生變更的通知。 `IChangeToken` 位於 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 命名空間中。 如需不使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本) 的應用程式，請參考專案檔中的 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件。

`IChangeToken` 有兩個屬性：

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) 指出權杖是否主動引發回呼。 如果 `ActiveChangedCallbacks` 設定為 `false`，則絕不會呼叫回呼，而且應用程式必須輪詢 `HasChanged` 是否有變更。 如果未發生任何變更，或基礎變更接聽程式已遭處置或停用，權杖也可能永遠不會被取消。
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) 取得值，指出是否已發生變更。

此介面有一種方法 [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)，它會登錄在權杖變更時叫用的回呼。 `HasChanged` 必須在叫用回呼之前設定。

## <a name="changetoken-class"></a>ChangeToken 類別

`ChangeToken` 靜態類別用來傳播已發生變更的通知。 `ChangeToken` 位於 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 命名空間中。 如需不使用[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)的應用程式，請參考專案檔中的 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件。

`ChangeToken` 的 [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) 方法將登錄每當權杖變更時要呼叫的 `Action`：

* `Func<IChangeToken>` 會產生權杖。
* 在權杖變更時呼叫 `Action`。

`ChangeToken` 具有 [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) 多載，其採用傳入權杖取用者 `Action` 的額外 `TState` 參數。

`OnChange` 會傳回 [IDisposable](/dotnet/api/system.idisposable)。 呼叫 [Dispose](/dotnet/api/system.idisposable.dispose) 將阻止權杖接聽其他變更和釋出權杖的資源。

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>ASP.NET Core 中變更權杖的範例用法

變更權杖用於 ASP.NET Core 監視物件變更的重要區域：

* 對於監視檔案變更，[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) 的 [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 方法會針對要監看的指定檔案或資料夾建立 `IChangeToken`。
* `IChangeToken` 權杖可新增至快取項目，以在變更時觸發快取收回。
* 對於 `TOptions` 變更，[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 的預設 [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) 實作具有可接受一或多個 [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) 執行個體的多載。 每個執行個體會傳回 `IChangeToken`，以登錄變更通知回呼來追蹤選項變更。

## <a name="monitoring-for-configuration-changes"></a>監視組態變更

ASP.NET Core 範本預設使用 [JSON 組態檔](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*、*appsettings.Development.json* 和 *appsettings.Production.json*) 來載入應用程式組態設定。

這些檔案在接受 `reloadOnChange` 參數的 [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 上使用 [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) 擴充方法加以設定 (ASP.NET Core 1.1 和更新版本)。 `reloadOnChange` 指出組態是否應該在檔案變更時重新載入。 請參閱 [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) 的便利方法 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 中的此設定：

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

檔案型組態是以 [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource) 表示。 `FileConfigurationSource` 會使用 [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) 來監視檔案。

根據預設，`IFileMonitor` 由 [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) 提供，它會使用 [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) 來監視組態檔變更。

範例應用程式將示範監視組態變更的兩個實作。 如果有任一個 *appsettings.json* 檔案變更或檔案的環境版本變更時，每個實作都會執行自訂程式碼。 範例應用程式會將訊息寫入主控台。

組態檔的 `FileSystemWatcher` 可以觸發單一組態檔案變更的多個權杖回呼。 範例的實作會檢查組態檔的檔案雜湊，以避免發生這個問題。 檢查檔案雜湊，可確保至少其中一個組態檔已在執行自訂程式碼之前變更。 此範例會使用 SHA1 檔案雜湊 (*Utilities/Utilities.cs*)：

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   重試將利用指數倒退來實作。 因為可能發生檔案鎖定而暫時無法對其中一個檔案計算新的雜湊，所以會進行重試。

### <a name="simple-startup-change-token"></a>簡易啟動變更權杖

將變更通知的權杖取用者 `Action` 回呼登錄到組態重新載入權杖 (*Startup.cs*)：

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` 提供此權杖。 回呼是 `InvokeChanged` 方法：

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

`state` 的回呼用來傳入 `IHostingEnvironment`。 這可用來判斷要監視的正確 *appsettings* 組態 JSON 檔案 *appsettings.&lt;環境&gt;.json*。 檔案雜湊用來防止 `WriteConsole` 陳述式在組態檔只變更過一次時，由於多個權杖回呼而執行多次。

只要應用程式在執行中，此系統就會執行，而且使用者無法停用此系統。

### <a name="monitoring-configuration-changes-as-a-service"></a>以服務方式監視組態變更

此範例將實作：

* 基本啟動權杖監視。
* 以服務方式監視。
* 啟用和停用監視的機制。

此範例會建立 `IConfigurationMonitor` 介面 (*Extensions/ConfigurationMonitor.cs*)：

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

已實作類別的建構函式 `ConfigurationMonitor` 登錄變更通知的回呼：

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` 提供此權杖。 `InvokeChanged` 是回呼方法。 這個執行個體中的 `state` 是用來存取監視狀態的 `IConfigurationMonitor` 執行個體參考。 會使用兩個屬性：

* `MonitoringEnabled` 指出回呼是否應該執行自訂程式碼。
* `CurrentState` 描述要用於 UI 的目前監視狀態。

`InvokeChanged` 方法類似於之前的方法，不同之處在於：

* 不會執行其程式碼，除非 `MonitoringEnabled` 是 `true`。
* 在其 `WriteConsole` 輸出中記錄目前的 `state`。

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

執行個體 `ConfigurationMonitor` 會在 *Startup.cs* 的 `ConfigureServices` 中登錄為服務：

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

[索引] 頁面提供對組態監視的使用者控制。 `IConfigurationMonitor` 的執行個體會插入到 `IndexModel` 中：

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

按鈕可啟用和停用監視：

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

當觸發 `OnPostStartMonitoring` 時，會啟用監視並清除目前的狀態。 當觸發 `OnPostStopMonitoring` 時，則會停用監視，且狀態會設定為反映未進行監視。

## <a name="monitoring-cached-file-changes"></a>監視快取的檔案變更

檔案內容可以使用 [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) 在記憶體內部快取。 記憶體內部快取將在[記憶體內部快取](xref:performance/caching/memory)主題中描述。 若不採取額外的步驟 (例如下述實作)，當來源資料變更時，將會從快取傳回「過時」(過期) 資料。

更新[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)時，若不考慮快取原始程式檔的狀態，將導致過時的快取資料。 資料的每個要求都會更新滑動期限，但檔案永遠不會重新載入至快取。 使用檔案快取內容的任何應用程式功能都可能會收到過時的內容。

在檔案快取案例中使用變更權杖，即可防止快取中出現過時的檔案內容。 範例應用程式將示範該方法的實作。

此範例會使用 `GetFileContent` 來：

* 傳回檔案內容。
* 實作使用指數倒退的重試演算法，以涵蓋檔案鎖定暫時阻止讀取檔案的情況。

*Utilities/Utilities.cs*：

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

`FileService` 是建立來處理快取的檔案查閱。 服務的 `GetFileContent` 方法呼叫會嘗試從記憶體內部快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。

如果使用快取索引鍵找不到快取的內容，則會採取下列動作：

1. 使用 `GetFileContent` 取得檔案內容。
1. 使用 [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 從檔案提供者取得變更權杖。 修改檔案時，就會觸發權杖的回呼。
1. 使用[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)快取檔案內容。 變更權杖附有 [MemoryCacheEntryExtensions.AddExpirationToke](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken)，可在快取的檔案變更時收回快取項目。

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` 登錄在服務容器以及記憶體快取服務 (*Startup.cs*) 中：

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

頁面模型會使用服務 (*Pages/Index.cshtml.cs*) 載入檔案的內容：

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken 類別

為了表示單一物件中的一或多個 `IChangeToken` 執行個體，請使用 [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) 類別。

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

如果任何表示的權杖 `HasChanged` 是 `true`，複合權杖上的 `HasChanged` 會報告 `true`。 如果任何表示的權杖 `ActiveChangeCallbacks` 是 `true`，複合權杖上的 `ActiveChangeCallbacks` 會報告 `true`。 如果發生多個同時變更事件，會叫用複合變更回呼剛好一次。

## <a name="additional-resources"></a>其他資源

* [記憶體中快取](xref:performance/caching/memory)
* [使用分散式快取](xref:performance/caching/distributed)
* [回應快取](xref:performance/caching/response)
* [回應快取中介軟體](xref:performance/caching/middleware)
* [快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
