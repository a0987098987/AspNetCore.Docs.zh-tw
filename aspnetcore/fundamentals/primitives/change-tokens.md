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
ms.openlocfilehash: a9479e3d676ed4dc880996a4a77de30d82b84cd5
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2017
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="73421-103">在 ASP.NET Core 偵測變更語彙基元的變更</span><span class="sxs-lookup"><span data-stu-id="73421-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="73421-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="73421-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="73421-105">A*變更權杖*是用來追蹤變更的一般用途的低階建置組塊。</span><span class="sxs-lookup"><span data-stu-id="73421-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="73421-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="73421-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="73421-107">IChangeToken 介面</span><span class="sxs-lookup"><span data-stu-id="73421-107">IChangeToken interface</span></span>

<span data-ttu-id="73421-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)傳播已經發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="73421-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="73421-109">`IChangeToken`位於[Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives)命名空間。</span><span class="sxs-lookup"><span data-stu-id="73421-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="73421-110">不使用的應用程式[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage，參考[Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)專案檔中的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="73421-110">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="73421-111">`IChangeToken`有兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="73421-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="73421-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks)表示是否語彙基元會主動引發回呼。</span><span class="sxs-lookup"><span data-stu-id="73421-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="73421-113">如果`ActiveChangedCallbacks`設`false`、 絕不會呼叫回呼，和應用程式都必須輪詢`HasChanged`的變更。</span><span class="sxs-lookup"><span data-stu-id="73421-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="73421-114">它也可能會永遠不會取消如果不發生任何變更，或基礎變更接聽程式已處置或停用的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="73421-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="73421-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged)取得值，指出是否已經發生變更。</span><span class="sxs-lookup"><span data-stu-id="73421-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="73421-116">這個介面具有一種方法， [RegisterChangeCallback (動作&lt;物件&gt;，物件)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)，其會登錄權杖已變更時叫用的回呼。</span><span class="sxs-lookup"><span data-stu-id="73421-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="73421-117">`HasChanged`在叫用回呼之前，必須設定。</span><span class="sxs-lookup"><span data-stu-id="73421-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="73421-118">ChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="73421-118">ChangeToken class</span></span>

<span data-ttu-id="73421-119">`ChangeToken`靜態類別用來傳播已經發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="73421-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="73421-120">`ChangeToken`位於[Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives)命名空間。</span><span class="sxs-lookup"><span data-stu-id="73421-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="73421-121">不使用的應用程式[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage，參考[Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)專案檔中的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="73421-121">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="73421-122">`ChangeToken` [OnChange (Func&lt;IChangeToken&gt;，動作)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_)方法暫存器`Action`呼叫當權杖變更時：</span><span class="sxs-lookup"><span data-stu-id="73421-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>
* <span data-ttu-id="73421-123">`Func<IChangeToken>`產生的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="73421-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="73421-124">`Action`語彙基元變更時所呼叫。</span><span class="sxs-lookup"><span data-stu-id="73421-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="73421-125">`ChangeToken`具有[OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;，動作&lt;TState&gt;，TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_)多載，其他的`TState`參數傳遞至權杖取用者`Action`。</span><span class="sxs-lookup"><span data-stu-id="73421-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="73421-126">`OnChange`傳回[IDisposable](/dotnet/api/system.idisposable)。</span><span class="sxs-lookup"><span data-stu-id="73421-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="73421-127">呼叫[處置](/dotnet/api/system.idisposable.dispose)停止接聽是否有其他變更的語彙基元和權杖的資源釋出。</span><span class="sxs-lookup"><span data-stu-id="73421-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="73421-128">範例會使用 ASP.NET Core 中的變更語彙基元的</span><span class="sxs-lookup"><span data-stu-id="73421-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="73421-129">變更權杖用於重要區域的 ASP.NET 核心監視物件的變更：</span><span class="sxs-lookup"><span data-stu-id="73421-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="73421-130">監視檔案變更， [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)的[監看式](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch)方法會建立`IChangeToken`針對指定的檔案或要監控的資料夾。</span><span class="sxs-lookup"><span data-stu-id="73421-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="73421-131">`IChangeToken`語彙基元可以加入至觸發程序所變更的快取收回快取項目。</span><span class="sxs-lookup"><span data-stu-id="73421-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="73421-132">如`TOptions`變更時，預設值[OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1)實作[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)具有可接受一或多個多載[IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)執行個體。</span><span class="sxs-lookup"><span data-stu-id="73421-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="73421-133">每個執行個體傳回`IChangeToken`註冊變更通知回呼追蹤選項的變更。</span><span class="sxs-lookup"><span data-stu-id="73421-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="73421-134">監視設定變更</span><span class="sxs-lookup"><span data-stu-id="73421-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="73421-135">根據預設，使用 ASP.NET Core 範本[JSON 組態檔](xref:fundamentals/configuration/index#json-configuration)(*appsettings.json*， *appsettings。Development.json*，和*appsettings。Production.json*) 載入應用程式組態設定。</span><span class="sxs-lookup"><span data-stu-id="73421-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="73421-136">這些檔案使用設定[AddJsonFile IConfigurationBuilder、 字串、 布林值 (Boolean）](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_)上的擴充方法[ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder)接受`reloadOnChange`參數 (ASP.NET核心 1.1 版和更新版本）。</span><span class="sxs-lookup"><span data-stu-id="73421-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="73421-137">`reloadOnChange`指出是否應該重新載入檔案的變更。</span><span class="sxs-lookup"><span data-stu-id="73421-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="73421-138">請參閱此設定在[WebHost](/dotnet/api/microsoft.aspnetcore.webhost)便利的方法， [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([參考來源](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):</span><span class="sxs-lookup"><span data-stu-id="73421-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([reference source](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="73421-139">以檔案為基礎的組態由[FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource)。</span><span class="sxs-lookup"><span data-stu-id="73421-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="73421-140">`FileConfigurationSource`使用[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([參考來源](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) 來監視檔案。</span><span class="sxs-lookup"><span data-stu-id="73421-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([reference source](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) to monitor files.</span></span>

<span data-ttu-id="73421-141">根據預設，`IFileMonitor`係由[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([參考來源](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82))，它會使用[FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher)来監視的組態檔變更。</span><span class="sxs-lookup"><span data-stu-id="73421-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([reference source](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="73421-142">範例應用程式會示範兩個實作的監視設定變更。</span><span class="sxs-lookup"><span data-stu-id="73421-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="73421-143">如果有任一個*appsettings.json*變更檔案或檔案的環境版本變更時，每個實作會執行自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="73421-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="73421-144">範例應用程式會將訊息寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="73421-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="73421-145">組態檔的`FileSystemWatcher`可以觸發單一組態檔案變更的多個權杖回呼。</span><span class="sxs-lookup"><span data-stu-id="73421-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="73421-146">範例的實作會藉由檢查組態檔上的檔案雜湊避免發生這個問題。</span><span class="sxs-lookup"><span data-stu-id="73421-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="73421-147">檢查檔案雜湊，可確保至少一個組態檔已變更之前執行的自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="73421-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="73421-148">此範例會使用 SHA1 雜湊檔案 (*Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="73421-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="73421-149">利用指數型撤退實作重試。</span><span class="sxs-lookup"><span data-stu-id="73421-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="73421-150">因為檔案鎖定可能會發生，暫時避免計算新的雜湊，在其中一個檔案，再試一次會出現。</span><span class="sxs-lookup"><span data-stu-id="73421-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="73421-151">簡單啟動變更語彙基元</span><span class="sxs-lookup"><span data-stu-id="73421-151">Simple startup change token</span></span>

<span data-ttu-id="73421-152">註冊權杖取用者`Action`回呼，以變更通知組態重新載入語彙基元 (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="73421-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="73421-153">`config.GetReloadToken()`提供的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="73421-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="73421-154">回呼是`InvokeChanged`方法：</span><span class="sxs-lookup"><span data-stu-id="73421-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="73421-155">`state`的回呼用來傳入`IHostingEnvironment`。</span><span class="sxs-lookup"><span data-stu-id="73421-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="73421-156">這可用來判斷正確*appsettings*組態 JSON 檔來監視， *appsettings。&lt;環境&gt;.json*。</span><span class="sxs-lookup"><span data-stu-id="73421-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="73421-157">檔案雜湊用來防止`WriteConsole`由於一次只能變更組態檔時的多個權杖回呼而多次執行陳述式。</span><span class="sxs-lookup"><span data-stu-id="73421-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="73421-158">此系統執行只要在應用程式正在執行，與使用者無法停用。</span><span class="sxs-lookup"><span data-stu-id="73421-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="73421-159">以服務的監視設定變更</span><span class="sxs-lookup"><span data-stu-id="73421-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="73421-160">此範例會實作：</span><span class="sxs-lookup"><span data-stu-id="73421-160">The sample implements:</span></span>

* <span data-ttu-id="73421-161">基本啟動語彙基元監視。</span><span class="sxs-lookup"><span data-stu-id="73421-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="73421-162">以服務的監視。</span><span class="sxs-lookup"><span data-stu-id="73421-162">Monitoring as a service.</span></span>
* <span data-ttu-id="73421-163">啟用和停用監視機制。</span><span class="sxs-lookup"><span data-stu-id="73421-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="73421-164">此範例會建立`IConfigurationMonitor`介面 (*Extensions/ConfigurationMonitor.cs*):</span><span class="sxs-lookup"><span data-stu-id="73421-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="73421-165">實作的類別的建構函式`ConfigurationMonitor`，註冊變更通知的回呼：</span><span class="sxs-lookup"><span data-stu-id="73421-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="73421-166">`config.GetReloadToken()`提供的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="73421-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="73421-167">`InvokeChanged`是回呼方法。</span><span class="sxs-lookup"><span data-stu-id="73421-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="73421-168">`state`這個執行個體中是描述監視狀態的字串。</span><span class="sxs-lookup"><span data-stu-id="73421-168">The `state` in this instance is a string that describes the monitoring state.</span></span> <span data-ttu-id="73421-169">會使用兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="73421-169">Two properties are used:</span></span>

* <span data-ttu-id="73421-170">`MonitoringEnabled`指出是否回呼應該執行的自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="73421-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="73421-171">`CurrentState`描述在 UI 中用於目前的監視狀態。</span><span class="sxs-lookup"><span data-stu-id="73421-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="73421-172">`InvokeChanged`較早的方法，只是它的方法如下：</span><span class="sxs-lookup"><span data-stu-id="73421-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="73421-173">無法執行其程式碼，除非`MonitoringEnabled`是`true`。</span><span class="sxs-lookup"><span data-stu-id="73421-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="73421-174">設定`CurrentState`屬性字串與程式碼執行的時間會記錄的描述性訊息。</span><span class="sxs-lookup"><span data-stu-id="73421-174">Sets the `CurrentState` property string to a descriptive message that records the time that the code ran.</span></span>
* <span data-ttu-id="73421-175">附註目前`state`中其`WriteConsole`輸出。</span><span class="sxs-lookup"><span data-stu-id="73421-175">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="73421-176">執行個體`ConfigurationMonitor`登錄中的服務為`ConfigureServices`的*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="73421-176">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="73421-177">索引頁提供使用者控制組態監視。</span><span class="sxs-lookup"><span data-stu-id="73421-177">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="73421-178">執行個體`IConfigurationMonitor`插入到`IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="73421-178">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="73421-179">按鈕會啟用和停用監視：</span><span class="sxs-lookup"><span data-stu-id="73421-179">A button enables and disables monitoring:</span></span>

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="73421-180">當`OnPostStartMonitoring`是觸發，啟用監視，並清除目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="73421-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="73421-181">當`OnPostStopMonitoring`是監視已停用觸發，，和狀態會設為反映未進行監視。</span><span class="sxs-lookup"><span data-stu-id="73421-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring is not occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="73421-182">監視快取的檔案變更</span><span class="sxs-lookup"><span data-stu-id="73421-182">Monitoring cached file changes</span></span>

<span data-ttu-id="73421-183">檔案內容可以是快取記憶體中使用[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)。</span><span class="sxs-lookup"><span data-stu-id="73421-183">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="73421-184">記憶體中快取述[記憶體中快取](xref:performance/caching/memory)主題。</span><span class="sxs-lookup"><span data-stu-id="73421-184">In-memory caching is described in the [In-memory caching](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="73421-185">但不採取額外的步驟，實作如下所述，例如*過時*的來源資料變更時，將會傳回快取中的 （過時） 資料。</span><span class="sxs-lookup"><span data-stu-id="73421-185">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="73421-186">不考慮快取的原始程式檔的狀態更新時[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間通往過時的快取資料。</span><span class="sxs-lookup"><span data-stu-id="73421-186">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="73421-187">每個要求的資料更新的滑動逾期期限，但檔案永遠不會重新載入至快取。</span><span class="sxs-lookup"><span data-stu-id="73421-187">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="73421-188">使用檔案的快取的內容的任何應用程式功能受限於可能接收過時的內容。</span><span class="sxs-lookup"><span data-stu-id="73421-188">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="73421-189">使用檔案快取案例變更語彙基元，會讓快取中過時的檔案內容。</span><span class="sxs-lookup"><span data-stu-id="73421-189">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="73421-190">範例應用程式將示範實作的方法。</span><span class="sxs-lookup"><span data-stu-id="73421-190">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="73421-191">此範例會使用`GetFileContent`至：</span><span class="sxs-lookup"><span data-stu-id="73421-191">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="73421-192">傳回檔案內容。</span><span class="sxs-lookup"><span data-stu-id="73421-192">Return file content.</span></span>
* <span data-ttu-id="73421-193">實作重試演算法使用指數型撤退封面案例，其中檔案鎖定暫時防止檔案被讀取。</span><span class="sxs-lookup"><span data-stu-id="73421-193">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="73421-194">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="73421-194">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="73421-195">A`FileService`是用來處理快取的檔案對應。</span><span class="sxs-lookup"><span data-stu-id="73421-195">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="73421-196">`GetFileContent`方法呼叫的服務會嘗試從記憶體中快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。</span><span class="sxs-lookup"><span data-stu-id="73421-196">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="73421-197">如果找不到使用快取索引鍵的快取的內容，會採取下列動作：</span><span class="sxs-lookup"><span data-stu-id="73421-197">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="73421-198">使用取得的檔案內容`GetFileContent`。</span><span class="sxs-lookup"><span data-stu-id="73421-198">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="73421-199">變更語彙基元取自檔案提供者與[IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch)。</span><span class="sxs-lookup"><span data-stu-id="73421-199">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="73421-200">修改檔案時，會觸發語彙基元的回呼。</span><span class="sxs-lookup"><span data-stu-id="73421-200">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="73421-201">檔案內容會使用快取[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間。</span><span class="sxs-lookup"><span data-stu-id="73421-201">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="73421-202">變更語彙基元附有[MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken)收回快取項目，如果變更的檔案時就會快取。</span><span class="sxs-lookup"><span data-stu-id="73421-202">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="73421-203">`FileService`註冊服務容器，以及快取服務的記憶體中 (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="73421-203">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="73421-204">頁面模型載入檔案的內容使用服務 (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="73421-204">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="73421-205">CompositeChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="73421-205">CompositeChangeToken class</span></span>

<span data-ttu-id="73421-206">表示一或多個`IChangeToken`執行個體的單一物件中，使用[CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken)類別 ([參考來源](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs))。</span><span class="sxs-lookup"><span data-stu-id="73421-206">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class ([reference source](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).</span></span>

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

<span data-ttu-id="73421-207">`HasChanged`在複合權杖報表`true`如果任何表示語彙基元`HasChanged`是`true`。</span><span class="sxs-lookup"><span data-stu-id="73421-207">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="73421-208">`ActiveChangeCallbacks`在複合權杖報表`true`如果任何表示語彙基元`ActiveChangeCallbacks`是`true`。</span><span class="sxs-lookup"><span data-stu-id="73421-208">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="73421-209">如果多個並行的變更事件發生時，複合變更回撥會叫用剛好一次。</span><span class="sxs-lookup"><span data-stu-id="73421-209">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="see-also"></a><span data-ttu-id="73421-210">請參閱</span><span class="sxs-lookup"><span data-stu-id="73421-210">See also</span></span>

* [<span data-ttu-id="73421-211">記憶體中快取</span><span class="sxs-lookup"><span data-stu-id="73421-211">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="73421-212">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="73421-212">Working with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="73421-213">偵測變更語彙基元的變更</span><span class="sxs-lookup"><span data-stu-id="73421-213">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="73421-214">回應快取</span><span class="sxs-lookup"><span data-stu-id="73421-214">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="73421-215">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="73421-215">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="73421-216">快取標記協助程式</span><span class="sxs-lookup"><span data-stu-id="73421-216">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="73421-217">分散式快取標記協助程式</span><span class="sxs-lookup"><span data-stu-id="73421-217">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
