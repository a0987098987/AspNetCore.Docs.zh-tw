---
title: "在 ASP.NET Core 偵測變更語彙基元的變更"
author: guardrex
description: "了解如何使用語彙基元變更追蹤的變更。"
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 94bf356fcbfab3930804485c1b65e4a0f4c52b8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>在 ASP.NET Core 偵測變更語彙基元的變更

作者：[Luke Latham](https://github.com/guardrex)

A*變更權杖*是用來追蹤變更的一般用途的低階建置組塊。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken 介面

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)傳播已經發生變更的通知。 `IChangeToken`位於[Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives)命名空間。 不使用的應用程式[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage，參考[Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)專案檔中的 NuGet 封裝。

`IChangeToken`有兩個屬性：

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks)表示是否語彙基元會主動引發回呼。 如果`ActiveChangedCallbacks`設`false`、 絕不會呼叫回呼，和應用程式都必須輪詢`HasChanged`的變更。 它也可能會永遠不會取消如果不發生任何變更，或基礎變更接聽程式已處置或停用的語彙基元。
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged)取得值，指出是否已經發生變更。

這個介面具有一種方法， [RegisterChangeCallback (動作&lt;物件&gt;，物件)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)，其會登錄權杖已變更時叫用的回呼。 `HasChanged`在叫用回呼之前，必須設定。

## <a name="changetoken-class"></a>ChangeToken 類別

`ChangeToken`靜態類別用來傳播已經發生變更的通知。 `ChangeToken`位於[Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives)命名空間。 不使用的應用程式[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage，參考[Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)專案檔中的 NuGet 封裝。

`ChangeToken` [OnChange (Func&lt;IChangeToken&gt;，動作)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_)方法暫存器`Action`呼叫當權杖變更時：
* `Func<IChangeToken>`產生的語彙基元。
* `Action`語彙基元變更時所呼叫。

`ChangeToken`具有[OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;，動作&lt;TState&gt;，TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_)多載，其他的`TState`參數傳遞至權杖取用者`Action`。

`OnChange`傳回[IDisposable](/dotnet/api/system.idisposable)。 呼叫[處置](/dotnet/api/system.idisposable.dispose)停止接聽是否有其他變更的語彙基元和權杖的資源釋出。

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>範例會使用 ASP.NET Core 中的變更語彙基元的

變更權杖用於重要區域的 ASP.NET 核心監視物件的變更：

* 監視檔案變更， [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)的[監看式](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch)方法會建立`IChangeToken`針對指定的檔案或要監控的資料夾。
* `IChangeToken`語彙基元可以加入至觸發程序所變更的快取收回快取項目。
* 如`TOptions`變更時，預設值[OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1)實作[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)具有可接受一或多個多載[IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)執行個體。 每個執行個體傳回`IChangeToken`註冊變更通知回呼追蹤選項的變更。

## <a name="monitoring-for-configuration-changes"></a>監視設定變更

根據預設，使用 ASP.NET Core 範本[JSON 組態檔](xref:fundamentals/configuration/index#json-configuration)(*appsettings.json*， *appsettings。Development.json*，和*appsettings。Production.json*) 載入應用程式組態設定。

這些檔案使用設定[AddJsonFile IConfigurationBuilder、 字串、 布林值 (Boolean）](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_)上的擴充方法[ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder)接受`reloadOnChange`參數 (ASP.NET核心 1.1 版和更新版本）。 `reloadOnChange`指出是否應該重新載入檔案的變更。 請參閱此設定在[WebHost](/dotnet/api/microsoft.aspnetcore.webhost)便利的方法， [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([參考來源](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

以檔案為基礎的組態由[FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource)。 `FileConfigurationSource`使用[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([參考來源](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) 來監視檔案。

根據預設，`IFileMonitor`係由[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([參考來源](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82))，它會使用[FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher)来監視的組態檔變更。

範例應用程式會示範兩個實作的監視設定變更。 如果有任一個*appsettings.json*變更檔案或檔案的環境版本變更時，每個實作會執行自訂程式碼。 範例應用程式會將訊息寫入主控台。

組態檔的`FileSystemWatcher`可以觸發單一組態檔案變更的多個權杖回呼。 範例的實作會藉由檢查組態檔上的檔案雜湊避免發生這個問題。 檢查檔案雜湊，可確保至少一個組態檔已變更之前執行的自訂程式碼。 此範例會使用 SHA1 雜湊檔案 (*Utilities/Utilities.cs*):

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   利用指數型撤退實作重試。 因為檔案鎖定可能會發生，暫時避免計算新的雜湊，在其中一個檔案，再試一次會出現。

### <a name="simple-startup-change-token"></a>簡單啟動變更語彙基元

註冊權杖取用者`Action`回呼，以變更通知組態重新載入語彙基元 (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()`提供的語彙基元。 回呼是`InvokeChanged`方法：

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

`state`的回呼用來傳入`IHostingEnvironment`。 這可用來判斷正確*appsettings*組態 JSON 檔來監視， *appsettings。&lt;環境&gt;.json*。 檔案雜湊用來防止`WriteConsole`由於一次只能變更組態檔時的多個權杖回呼而多次執行陳述式。

此系統執行只要在應用程式正在執行，與使用者無法停用。

### <a name="monitoring-configuration-changes-as-a-service"></a>以服務的監視設定變更

此範例會實作：

* 基本啟動語彙基元監視。
* 以服務的監視。
* 啟用和停用監視機制。

此範例會建立`IConfigurationMonitor`介面 (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

實作的類別的建構函式`ConfigurationMonitor`，註冊變更通知的回呼：

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()`提供的語彙基元。 `InvokeChanged`是回呼方法。 `state`這個執行個體中是描述監視狀態的字串。 會使用兩個屬性：

* `MonitoringEnabled`指出是否回呼應該執行的自訂程式碼。
* `CurrentState`描述在 UI 中用於目前的監視狀態。

`InvokeChanged`較早的方法，只是它的方法如下：

* 無法執行其程式碼，除非`MonitoringEnabled`是`true`。
* 設定`CurrentState`屬性字串與程式碼執行的時間會記錄的描述性訊息。
* 附註目前`state`中其`WriteConsole`輸出。

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

執行個體`ConfigurationMonitor`登錄中的服務為`ConfigureServices`的*Startup.cs*:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

索引頁提供使用者控制組態監視。 執行個體`IConfigurationMonitor`插入到`IndexModel`:

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

按鈕會啟用和停用監視：

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

當`OnPostStartMonitoring`是觸發，啟用監視，並清除目前的狀態。 當`OnPostStopMonitoring`是監視已停用觸發，，和狀態會設為反映不進行監視。

## <a name="monitoring-cached-file-changes"></a>監視快取的檔案變更

檔案內容可以是快取記憶體中使用[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)。 記憶體中快取述[記憶體中快取](xref:performance/caching/memory)主題。 但不採取額外的步驟，實作如下所述，例如*過時*的來源資料變更時，將會傳回快取中的 （過時） 資料。

不考慮快取的原始程式檔的狀態更新時[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間通往過時的快取資料。 每個要求的資料更新的滑動逾期期限，但檔案永遠不會重新載入至快取。 使用檔案的快取的內容的任何應用程式功能受限於可能接收過時的內容。

使用檔案快取案例變更語彙基元，會讓快取中過時的檔案內容。 範例應用程式將示範實作的方法。

此範例會使用`GetFileContent`至：

* 傳回檔案內容。
* 實作重試演算法使用指數型撤退封面案例，其中檔案鎖定暫時防止檔案被讀取。

*Utilities/Utilities.cs*:

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

A`FileService`是用來處理快取的檔案對應。 `GetFileContent`方法呼叫的服務會嘗試從記憶體中快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。

如果找不到使用快取索引鍵的快取的內容，會採取下列動作：

1. 使用取得的檔案內容`GetFileContent`。
1. 變更語彙基元取自檔案提供者與[IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch)。 修改檔案時，會觸發語彙基元的回呼。
1. 檔案內容會使用快取[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間。 變更語彙基元附有[MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken)收回快取項目，如果變更的檔案時就會快取。

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService`註冊服務容器，以及快取服務的記憶體中 (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

頁面模型載入檔案的內容使用服務 (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken 類別

表示一或多個`IChangeToken`執行個體的單一物件中，使用[CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken)類別 ([參考來源](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs))。

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

`HasChanged`在複合權杖報表`true`如果任何表示語彙基元`HasChanged`是`true`。 `ActiveChangeCallbacks`在複合權杖報表`true`如果任何表示語彙基元`ActiveChangeCallbacks`是`true`。 如果多個並行的變更事件發生時，複合變更回撥會叫用剛好一次。

## <a name="see-also"></a>另請參閱

* [記憶體內部快取](xref:performance/caching/memory)
* [使用分散式快取](xref:performance/caching/distributed)
* [使用變更權杖來偵測變更](xref:fundamentals/primitives/change-tokens)
* [回應快取](xref:performance/caching/response)
* [回應快取中介軟體](xref:performance/caching/middleware)
* [快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
