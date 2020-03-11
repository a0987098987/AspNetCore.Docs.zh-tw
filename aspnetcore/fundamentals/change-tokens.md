---
title: 在 ASP.NET Core 中使用變更權杖來偵測變更
author: rick-anderson
description: 了解如何使用變更權杖來追蹤變更。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/07/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: 70451e219f1295b854e2f84aac55f0cfd1786b19
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656342"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>在 ASP.NET Core 中使用變更權杖來偵測變更

::: moniker range=">= aspnetcore-3.0"

「變更權杖」是用來追蹤狀態變更的一般用途低階建置組塊。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken 介面

<xref:Microsoft.Extensions.Primitives.IChangeToken> 會傳播已發生變更的通知。 `IChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。 會以隱含方式為 ASP.NET Core 應用程式提供了[Microsoft Extensions 原型](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)NuGet 套件。

`IChangeToken` 有兩個屬性：

* <xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> 指出權杖是否主動引發回呼。 如果 `ActiveChangedCallbacks` 設定為 `false`，則絕不會呼叫回呼，而且應用程式必須輪詢 `HasChanged` 是否有變更。 如果未發生任何變更，或基礎變更接聽程式已遭處置或停用，權杖也可能永遠不會被取消。
* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> 會接收指出是否已發生變更的值。

`IChangeToken` 介面包括 [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) 方法，它會登錄在權杖變更時叫用的回呼。 `HasChanged` 必須在叫用回呼之前設定。

## <a name="changetoken-class"></a>ChangeToken 類別

<xref:Microsoft.Extensions.Primitives.ChangeToken> 靜態類別用來傳播已發生變更的通知。 `ChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。 會以隱含方式為 ASP.NET Core 應用程式提供了[Microsoft Extensions 原型](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)NuGet 套件。

[ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) 方法會登錄每當權杖變更時要呼叫的 `Action`：

* `Func<IChangeToken>` 會產生權杖。
* 在權杖變更時呼叫 `Action`。

[ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) 多載接受傳遞到權杖取用者 `TState` 的額外 `Action` 參數。

`OnChange` 會傳回 <xref:System.IDisposable>。 呼叫 <xref:System.IDisposable.Dispose*> 將阻止權杖接聽其他變更和釋出權杖的資源。

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>ASP.NET Core 中變更權杖的範例用法

變更權杖用於 ASP.NET Core 監視物件變更的重要區域：

* 針對監視檔案變更，<xref:Microsoft.Extensions.FileProviders.IFileProvider> 的 <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> 方法會針對要監看的指定檔案或資料夾建立 `IChangeToken`。
* `IChangeToken` 權杖可新增至快取項目，以在變更時觸發快取收回。
* 針對 `TOptions` 變更，<xref:Microsoft.Extensions.Options.OptionsMonitor`1> 的預設 <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> 實作具有會接受一或多個 <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> 執行個體的多載。 每個執行個體會傳回 `IChangeToken`，以登錄變更通知回呼來追蹤選項變更。

## <a name="monitor-for-configuration-changes"></a>監視設定變更

ASP.NET Core 範本預設使用 [JSON 組態檔](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*、*appsettings.Development.json* 和 *appsettings.Production.json*) 來載入應用程式組態設定。

這些檔案是在接受 [ 參數的 ](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) 擴充方法上使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)`reloadOnChange` 擴充方法來設定的。 `reloadOnChange` 指出組態是否應該在檔案變更時重新載入。 此設定會出在 <xref:Microsoft.Extensions.Hosting.Host> 便利方法 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 中：

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

檔案型設定是以 <xref:Microsoft.Extensions.Configuration.FileConfigurationSource> 代表的。 `FileConfigurationSource` 使用 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 來監視檔案。

根據預設，`IFileMonitor` 由 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供，它會使用 <xref:System.IO.FileSystemWatcher> 來監視設定檔變更。

範例應用程式將示範監視組態變更的兩個實作。 如果任一 *appsettings* 檔案變更，兩個檔案監視實作都會執行自訂程式碼&mdash;範例應用程式會將訊息寫入到主控台。

組態檔的 `FileSystemWatcher` 可以觸發單一組態檔案變更的多個權杖回呼。 為確定自訂程式碼只在觸發多個權杖回呼時執行一次，範例的實作會檢查檔案雜湊。 範例使用 SHA1 檔案雜湊處理。 重試將利用指數倒退來實作。 因為可能發生檔案鎖定而暫時無法對檔案計算新的雜湊，所以會進行重試。

*Utilities/Utilities.cs*：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a>簡易啟動變更權杖

將變更通知的權杖取用者 `Action` 回呼登錄到設定重新載入權杖。

在 `Startup.Configure` 中：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

`config.GetReloadToken()` 提供此權杖。 回呼是 `InvokeChanged` 方法：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

回呼的 `state` 是用來傳入 `IWebHostEnvironment`，它在指定要監視的 *appsettings* 設定檔時非常實用 (例如，開發環境中的 *appsettings.Development.json*)。 檔案雜湊用來防止 `WriteConsole` 陳述式在組態檔只變更過一次時，由於多個權杖回呼而執行多次。

只要應用程式在執行中，此系統就會執行，而且使用者無法停用此系統。

### <a name="monitor-configuration-changes-as-a-service"></a>以服務方式監視設定變更

此範例將實作：

* 基本啟動權杖監視。
* 以服務方式監視。
* 啟用和停用監視的機制。

範例會建立 `IConfigurationMonitor` 介面。

*Extensions/ConfigurationMonitor.cs*：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

已實作類別的建構函式 `ConfigurationMonitor` 登錄變更通知的回呼：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` 提供此權杖。 `InvokeChanged` 是回呼方法。 此執行個體中的 `state` 是用來存取監視狀態的 `IConfigurationMonitor` 執行個體參考。 會使用兩個屬性：

* `MonitoringEnabled` &ndash; 指出回呼是否應執行其自訂程式碼。
* `CurrentState` &ndash; 描述在 UI 中使用的目前監視狀態。

`InvokeChanged` 方法類似於之前的方法，不同之處在於：

* 不會執行其程式碼，除非 `MonitoringEnabled` 是 `true`。
* 在其 `state` 輸出中輸出目前的 `WriteConsole`。

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

執行個體 `ConfigurationMonitor` 會在 `Startup.ConfigureServices` 中登錄為服務：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

[索引] 頁面提供對組態監視的使用者控制。 `IConfigurationMonitor` 的執行個體會插入到 `IndexModel` 中。

*Pages/Index.cshtml.cs*：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

設定監視器 (`_monitor`) 是用來啟用或停用監視並設定 UI 回饋的目前狀態：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

當觸發 `OnPostStartMonitoring` 時，會啟用監視並清除目前的狀態。 當觸發 `OnPostStopMonitoring` 時，則會停用監視，且狀態會設定為反映未進行監視。

UI 啟用和停用監視中的按鈕。

*Pages/Index.cshtml*：

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a>監視快取的檔案變更

檔案內容可以使用 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 在記憶體內部快取。 記憶體內部快取將在[記憶體內部快取](xref:performance/caching/memory)主題中描述。 若不採取額外的步驟 (例如下述實作)，當來源資料變更時，將會從快取傳回「過時」(過期) 資料。

例如，更新[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)時，若不考慮快取原始程式檔的狀態，將導致過時的快取檔案資料。 資料的每個要求都會更新滑動期限，但檔案永遠不會重新載入至快取。 使用檔案快取內容的任何應用程式功能都可能會收到過時的內容。

在檔案快取案例中使用變更權杖可防止快取中出現過時的檔案內容。 範例應用程式將示範該方法的實作。

此範例會使用 `GetFileContent` 來：

* 傳回檔案內容。
* 實作使用指數倒退的重試演算法，以涵蓋檔案鎖定暫時阻止讀取檔案的情況。

*Utilities/Utilities.cs*：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

`FileService` 是建立來處理快取的檔案查閱。 服務的 `GetFileContent` 方法呼叫會嘗試從記憶體內部快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。

如果使用快取索引鍵找不到快取的內容，則會採取下列動作：

1. 使用 `GetFileContent` 取得檔案內容。
1. 使用 [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 從檔案提供者取得變更權杖。 修改檔案時，就會觸發權杖的回呼。
1. 使用[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)快取檔案內容。 變更權杖附有 [MemoryCacheEntryExtensions.AddExpirationToke](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*)，可在快取的檔案變更時收回快取項目。

在下列範例中，檔案會儲存在應用程式的[內容根目錄](xref:fundamentals/index#content-root)中。 `IWebHostEnvironment.ContentRootFileProvider` 可用來取得指向應用程式 `IWebHostEnvironment.ContentRootPath`的 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。 `filePath` 是使用 [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath) 取得的。

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

`FileService` 是在服務容器以及記憶體快取服務中登錄的。

在 `Startup.ConfigureServices` 中：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

頁面模型會使用服務載入檔案的內容。

在索引頁面的 `OnGet` 方法 (*Pages/Index.cshtml.cs*) 中：

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken 類別

為了表示單一物件中的一或多個 `IChangeToken` 執行個體，請使用 <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> 類別。

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

如果任何表示的權杖 `HasChanged` 是 `true`，複合權杖上的 `HasChanged` 會報告 `true`。 如果任何表示的權杖 `ActiveChangeCallbacks` 是 `true`，複合權杖上的 `ActiveChangeCallbacks` 會報告 `true`。 如果發生多個同時變更事件，會叫用複合變更回呼一次。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

「變更權杖」是用來追蹤狀態變更的一般用途低階建置組塊。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken 介面

<xref:Microsoft.Extensions.Primitives.IChangeToken> 會傳播已發生變更的通知。 `IChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。 如需不使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)的應用程式，請建立 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件的套件參考。

`IChangeToken` 有兩個屬性：

* <xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> 指出權杖是否主動引發回呼。 如果 `ActiveChangedCallbacks` 設定為 `false`，則絕不會呼叫回呼，而且應用程式必須輪詢 `HasChanged` 是否有變更。 如果未發生任何變更，或基礎變更接聽程式已遭處置或停用，權杖也可能永遠不會被取消。
* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> 會接收指出是否已發生變更的值。

`IChangeToken` 介面包括 [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) 方法，它會登錄在權杖變更時叫用的回呼。 `HasChanged` 必須在叫用回呼之前設定。

## <a name="changetoken-class"></a>ChangeToken 類別

<xref:Microsoft.Extensions.Primitives.ChangeToken> 靜態類別用來傳播已發生變更的通知。 `ChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。 如需不使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)的應用程式，請建立 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件的套件參考。

[ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) 方法會登錄每當權杖變更時要呼叫的 `Action`：

* `Func<IChangeToken>` 會產生權杖。
* 在權杖變更時呼叫 `Action`。

[ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) 多載接受傳遞到權杖取用者 `TState` 的額外 `Action` 參數。

`OnChange` 會傳回 <xref:System.IDisposable>。 呼叫 <xref:System.IDisposable.Dispose*> 將阻止權杖接聽其他變更和釋出權杖的資源。

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>ASP.NET Core 中變更權杖的範例用法

變更權杖用於 ASP.NET Core 監視物件變更的重要區域：

* 針對監視檔案變更，<xref:Microsoft.Extensions.FileProviders.IFileProvider> 的 <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> 方法會針對要監看的指定檔案或資料夾建立 `IChangeToken`。
* `IChangeToken` 權杖可新增至快取項目，以在變更時觸發快取收回。
* 針對 `TOptions` 變更，<xref:Microsoft.Extensions.Options.OptionsMonitor`1> 的預設 <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> 實作具有會接受一或多個 <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> 執行個體的多載。 每個執行個體會傳回 `IChangeToken`，以登錄變更通知回呼來追蹤選項變更。

## <a name="monitor-for-configuration-changes"></a>監視設定變更

ASP.NET Core 範本預設使用 [JSON 組態檔](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*、*appsettings.Development.json* 和 *appsettings.Production.json*) 來載入應用程式組態設定。

這些檔案是在接受 [ 參數的 ](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) 擴充方法上使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)`reloadOnChange` 擴充方法來設定的。 `reloadOnChange` 指出組態是否應該在檔案變更時重新載入。 此設定會出在 <xref:Microsoft.AspNetCore.WebHost> 便利方法 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 中：

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

檔案型設定是以 <xref:Microsoft.Extensions.Configuration.FileConfigurationSource> 代表的。 `FileConfigurationSource` 使用 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 來監視檔案。

根據預設，`IFileMonitor` 由 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供，它會使用 <xref:System.IO.FileSystemWatcher> 來監視設定檔變更。

範例應用程式將示範監視組態變更的兩個實作。 如果任一 *appsettings* 檔案變更，兩個檔案監視實作都會執行自訂程式碼&mdash;範例應用程式會將訊息寫入到主控台。

組態檔的 `FileSystemWatcher` 可以觸發單一組態檔案變更的多個權杖回呼。 為確定自訂程式碼只在觸發多個權杖回呼時執行一次，範例的實作會檢查檔案雜湊。 範例使用 SHA1 檔案雜湊處理。 重試將利用指數倒退來實作。 因為可能發生檔案鎖定而暫時無法對檔案計算新的雜湊，所以會進行重試。

*Utilities/Utilities.cs*：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a>簡易啟動變更權杖

將變更通知的權杖取用者 `Action` 回呼登錄到設定重新載入權杖。

在 `Startup.Configure` 中：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

`config.GetReloadToken()` 提供此權杖。 回呼是 `InvokeChanged` 方法：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

回呼的 `state` 是用來傳入 `IHostingEnvironment`，它在指定要監視的 *appsettings* 設定檔時非常實用 (例如，開發環境中的 *appsettings.Development.json*)。 檔案雜湊用來防止 `WriteConsole` 陳述式在組態檔只變更過一次時，由於多個權杖回呼而執行多次。

只要應用程式在執行中，此系統就會執行，而且使用者無法停用此系統。

### <a name="monitor-configuration-changes-as-a-service"></a>以服務方式監視設定變更

此範例將實作：

* 基本啟動權杖監視。
* 以服務方式監視。
* 啟用和停用監視的機制。

範例會建立 `IConfigurationMonitor` 介面。

*Extensions/ConfigurationMonitor.cs*：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

已實作類別的建構函式 `ConfigurationMonitor` 登錄變更通知的回呼：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` 提供此權杖。 `InvokeChanged` 是回呼方法。 此執行個體中的 `state` 是用來存取監視狀態的 `IConfigurationMonitor` 執行個體參考。 會使用兩個屬性：

* `MonitoringEnabled` &ndash; 指出回呼是否應執行其自訂程式碼。
* `CurrentState` &ndash; 描述在 UI 中使用的目前監視狀態。

`InvokeChanged` 方法類似於之前的方法，不同之處在於：

* 不會執行其程式碼，除非 `MonitoringEnabled` 是 `true`。
* 在其 `state` 輸出中輸出目前的 `WriteConsole`。

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

執行個體 `ConfigurationMonitor` 會在 `Startup.ConfigureServices` 中登錄為服務：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

[索引] 頁面提供對組態監視的使用者控制。 `IConfigurationMonitor` 的執行個體會插入到 `IndexModel` 中。

*Pages/Index.cshtml.cs*：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

設定監視器 (`_monitor`) 是用來啟用或停用監視並設定 UI 回饋的目前狀態：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

當觸發 `OnPostStartMonitoring` 時，會啟用監視並清除目前的狀態。 當觸發 `OnPostStopMonitoring` 時，則會停用監視，且狀態會設定為反映未進行監視。

UI 啟用和停用監視中的按鈕。

*Pages/Index.cshtml*：

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a>監視快取的檔案變更

檔案內容可以使用 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 在記憶體內部快取。 記憶體內部快取將在[記憶體內部快取](xref:performance/caching/memory)主題中描述。 若不採取額外的步驟 (例如下述實作)，當來源資料變更時，將會從快取傳回「過時」(過期) 資料。

例如，更新[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)時，若不考慮快取原始程式檔的狀態，將導致過時的快取檔案資料。 資料的每個要求都會更新滑動期限，但檔案永遠不會重新載入至快取。 使用檔案快取內容的任何應用程式功能都可能會收到過時的內容。

在檔案快取案例中使用變更權杖可防止快取中出現過時的檔案內容。 範例應用程式將示範該方法的實作。

此範例會使用 `GetFileContent` 來：

* 傳回檔案內容。
* 實作使用指數倒退的重試演算法，以涵蓋檔案鎖定暫時阻止讀取檔案的情況。

*Utilities/Utilities.cs*：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

`FileService` 是建立來處理快取的檔案查閱。 服務的 `GetFileContent` 方法呼叫會嘗試從記憶體內部快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。

如果使用快取索引鍵找不到快取的內容，則會採取下列動作：

1. 使用 `GetFileContent` 取得檔案內容。
1. 使用 [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 從檔案提供者取得變更權杖。 修改檔案時，就會觸發權杖的回呼。
1. 使用[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)快取檔案內容。 變更權杖附有 [MemoryCacheEntryExtensions.AddExpirationToke](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*)，可在快取的檔案變更時收回快取項目。

在下列範例中，檔案會儲存在應用程式的[內容根目錄](xref:fundamentals/index#content-root)中。 [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) 是用來取得指向應用程式 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 的 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>。 `filePath` 是使用 [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath) 取得的。

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

`FileService` 是在服務容器以及記憶體快取服務中登錄的。

在 `Startup.ConfigureServices` 中：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

頁面模型會使用服務載入檔案的內容。

在索引頁面的 `OnGet` 方法 (*Pages/Index.cshtml.cs*) 中：

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken 類別

為了表示單一物件中的一或多個 `IChangeToken` 執行個體，請使用 <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> 類別。

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

如果任何表示的權杖 `HasChanged` 是 `true`，複合權杖上的 `HasChanged` 會報告 `true`。 如果任何表示的權杖 `ActiveChangeCallbacks` 是 `true`，複合權杖上的 `ActiveChangeCallbacks` 會報告 `true`。 如果發生多個同時變更事件，會叫用複合變更回呼一次。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
