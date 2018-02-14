---
title: "在 ASP.NET Core 中使用變更權杖來偵測變更"
author: guardrex
description: "了解如何使用變更權杖來追蹤變更。"
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 94bf356fcbfab3930804485c1b65e4a0f4c52b8e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/31/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="b598b-103">在 ASP.NET Core 中使用變更權杖來偵測變更</span><span class="sxs-lookup"><span data-stu-id="b598b-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="b598b-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b598b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b598b-105">「變更權杖」是用來追蹤變更的一般用途低階建置組塊。</span><span class="sxs-lookup"><span data-stu-id="b598b-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="b598b-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b598b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="b598b-107">IChangeToken 介面</span><span class="sxs-lookup"><span data-stu-id="b598b-107">IChangeToken interface</span></span>

<span data-ttu-id="b598b-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) 可傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="b598b-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="b598b-109">`IChangeToken` 位於 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 命名空間中。</span><span class="sxs-lookup"><span data-stu-id="b598b-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="b598b-110">如需不使用 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) 中繼套件的應用程式，請參考專案檔中的 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="b598b-110">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="b598b-111">`IChangeToken` 有兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="b598b-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="b598b-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) 指出權杖是否主動引發回呼。</span><span class="sxs-lookup"><span data-stu-id="b598b-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="b598b-113">如果 `ActiveChangedCallbacks` 設定為 `false`，則絕不會呼叫回呼，而且應用程式必須輪詢 `HasChanged` 是否有變更。</span><span class="sxs-lookup"><span data-stu-id="b598b-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="b598b-114">如果未發生任何變更，或基礎變更接聽程式已遭處置或停用，權杖也可能永遠不會被取消。</span><span class="sxs-lookup"><span data-stu-id="b598b-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="b598b-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) 取得值，指出是否已發生變更。</span><span class="sxs-lookup"><span data-stu-id="b598b-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="b598b-116">此介面有一種方法 [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)，它會登錄在權杖變更時叫用的回呼。</span><span class="sxs-lookup"><span data-stu-id="b598b-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="b598b-117">`HasChanged` 必須在叫用回呼之前設定。</span><span class="sxs-lookup"><span data-stu-id="b598b-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="b598b-118">ChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="b598b-118">ChangeToken class</span></span>

<span data-ttu-id="b598b-119">`ChangeToken` 靜態類別用來傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="b598b-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="b598b-120">`ChangeToken` 位於 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 命名空間中。</span><span class="sxs-lookup"><span data-stu-id="b598b-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="b598b-121">如需不使用 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) 中繼套件的應用程式，請參考專案檔中的 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="b598b-121">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="b598b-122">`ChangeToken` 的 [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) 方法將登錄每當權杖變更時要呼叫的 `Action`：</span><span class="sxs-lookup"><span data-stu-id="b598b-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>
* <span data-ttu-id="b598b-123">`Func<IChangeToken>` 會產生權杖。</span><span class="sxs-lookup"><span data-stu-id="b598b-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="b598b-124">在權杖變更時呼叫 `Action`。</span><span class="sxs-lookup"><span data-stu-id="b598b-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="b598b-125">`ChangeToken` 具有 [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) 多載，其採用傳入權杖取用者 `Action` 的額外 `TState` 參數。</span><span class="sxs-lookup"><span data-stu-id="b598b-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="b598b-126">`OnChange` 會傳回 [IDisposable](/dotnet/api/system.idisposable)。</span><span class="sxs-lookup"><span data-stu-id="b598b-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="b598b-127">呼叫 [Dispose](/dotnet/api/system.idisposable.dispose) 將阻止權杖接聽其他變更和釋出權杖的資源。</span><span class="sxs-lookup"><span data-stu-id="b598b-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="b598b-128">ASP.NET Core 中變更權杖的範例用法</span><span class="sxs-lookup"><span data-stu-id="b598b-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="b598b-129">變更權杖用於 ASP.NET Core 監視物件變更的重要區域：</span><span class="sxs-lookup"><span data-stu-id="b598b-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="b598b-130">對於監視檔案變更，[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) 的 [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 方法會針對要監看的指定檔案或資料夾建立 `IChangeToken`。</span><span class="sxs-lookup"><span data-stu-id="b598b-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="b598b-131">`IChangeToken` 權杖可新增至快取項目，以在變更時觸發快取收回。</span><span class="sxs-lookup"><span data-stu-id="b598b-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="b598b-132">對於 `TOptions` 變更，[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 的預設 [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) 實作具有可接受一或多個 [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) 執行個體的多載。</span><span class="sxs-lookup"><span data-stu-id="b598b-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="b598b-133">每個執行個體會傳回 `IChangeToken`，以登錄變更通知回呼來追蹤選項變更。</span><span class="sxs-lookup"><span data-stu-id="b598b-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="b598b-134">監視組態變更</span><span class="sxs-lookup"><span data-stu-id="b598b-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="b598b-135">ASP.NET Core 範本預設使用 [JSON 組態檔](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*、*appsettings.Development.json* 和 *appsettings.Production.json*) 來載入應用程式組態設定。</span><span class="sxs-lookup"><span data-stu-id="b598b-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="b598b-136">這些檔案在接受 `reloadOnChange` 參數的 [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 上使用 [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) 擴充方法加以設定 (ASP.NET Core 1.1 和更新版本)。</span><span class="sxs-lookup"><span data-stu-id="b598b-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="b598b-137">`reloadOnChange` 指出組態是否應該在檔案變更時重新載入。</span><span class="sxs-lookup"><span data-stu-id="b598b-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="b598b-138">請參閱 [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) 的便利方法 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([參考來源](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)) 中的此設定：</span><span class="sxs-lookup"><span data-stu-id="b598b-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([reference source](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="b598b-139">檔案型組態是以 [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource) 表示。</span><span class="sxs-lookup"><span data-stu-id="b598b-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="b598b-140">`FileConfigurationSource` 會使用 [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([參考來源](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) 來監視檔案。</span><span class="sxs-lookup"><span data-stu-id="b598b-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([reference source](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) to monitor files.</span></span>

<span data-ttu-id="b598b-141">根據預設，`IFileMonitor` 由 [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([參考來源](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82))　提供，它會使用 [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) 來監視組態檔變更。</span><span class="sxs-lookup"><span data-stu-id="b598b-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([reference source](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="b598b-142">範例應用程式將示範監視組態變更的兩個實作。</span><span class="sxs-lookup"><span data-stu-id="b598b-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="b598b-143">如果有任一個 *appsettings.json* 檔案變更或檔案的環境版本變更時，每個實作都會執行自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="b598b-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="b598b-144">範例應用程式會將訊息寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="b598b-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="b598b-145">組態檔的 `FileSystemWatcher` 可以觸發單一組態檔案變更的多個權杖回呼。</span><span class="sxs-lookup"><span data-stu-id="b598b-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="b598b-146">範例的實作會檢查組態檔的檔案雜湊，以避免發生這個問題。</span><span class="sxs-lookup"><span data-stu-id="b598b-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="b598b-147">檢查檔案雜湊，可確保至少其中一個組態檔已在執行自訂程式碼之前變更。</span><span class="sxs-lookup"><span data-stu-id="b598b-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="b598b-148">此範例會使用 SHA1 檔案雜湊 (*Utilities/Utilities.cs*)：</span><span class="sxs-lookup"><span data-stu-id="b598b-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="b598b-149">重試將利用指數倒退來實作。</span><span class="sxs-lookup"><span data-stu-id="b598b-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="b598b-150">因為可能發生檔案鎖定而暫時無法對其中一個檔案計算新的雜湊，所以會進行重試。</span><span class="sxs-lookup"><span data-stu-id="b598b-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="b598b-151">簡易啟動變更權杖</span><span class="sxs-lookup"><span data-stu-id="b598b-151">Simple startup change token</span></span>

<span data-ttu-id="b598b-152">將變更通知的權杖取用者 `Action` 回呼登錄到組態重新載入權杖 (*Startup.cs*)：</span><span class="sxs-lookup"><span data-stu-id="b598b-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="b598b-153">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="b598b-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="b598b-154">回呼是 `InvokeChanged` 方法：</span><span class="sxs-lookup"><span data-stu-id="b598b-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="b598b-155">`state` 的回呼用來傳入 `IHostingEnvironment`。</span><span class="sxs-lookup"><span data-stu-id="b598b-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="b598b-156">這可用來判斷要監視的正確 *appsettings* 組態 JSON 檔案 *appsettings.&lt;環境&gt;.json*。</span><span class="sxs-lookup"><span data-stu-id="b598b-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="b598b-157">檔案雜湊用來防止 `WriteConsole` 陳述式在組態檔只變更過一次時，由於多個權杖回呼而執行多次。</span><span class="sxs-lookup"><span data-stu-id="b598b-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="b598b-158">只要應用程式在執行中，此系統就會執行，而且使用者無法停用此系統。</span><span class="sxs-lookup"><span data-stu-id="b598b-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="b598b-159">以服務方式監視組態變更</span><span class="sxs-lookup"><span data-stu-id="b598b-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="b598b-160">此範例將實作：</span><span class="sxs-lookup"><span data-stu-id="b598b-160">The sample implements:</span></span>

* <span data-ttu-id="b598b-161">基本啟動權杖監視。</span><span class="sxs-lookup"><span data-stu-id="b598b-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="b598b-162">以服務方式監視。</span><span class="sxs-lookup"><span data-stu-id="b598b-162">Monitoring as a service.</span></span>
* <span data-ttu-id="b598b-163">啟用和停用監視的機制。</span><span class="sxs-lookup"><span data-stu-id="b598b-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="b598b-164">此範例會建立 `IConfigurationMonitor` 介面 (*Extensions/ConfigurationMonitor.cs*)：</span><span class="sxs-lookup"><span data-stu-id="b598b-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="b598b-165">已實作類別的建構函式 `ConfigurationMonitor` 登錄變更通知的回呼：</span><span class="sxs-lookup"><span data-stu-id="b598b-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="b598b-166">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="b598b-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="b598b-167">`InvokeChanged` 是回呼方法。</span><span class="sxs-lookup"><span data-stu-id="b598b-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="b598b-168">這個執行個體中的 `state` 是描述監視狀態的字串。</span><span class="sxs-lookup"><span data-stu-id="b598b-168">The `state` in this instance is a string that describes the monitoring state.</span></span> <span data-ttu-id="b598b-169">會使用兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="b598b-169">Two properties are used:</span></span>

* <span data-ttu-id="b598b-170">`MonitoringEnabled` 指出回呼是否應該執行自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="b598b-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="b598b-171">`CurrentState` 描述要用於 UI 的目前監視狀態。</span><span class="sxs-lookup"><span data-stu-id="b598b-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="b598b-172">`InvokeChanged` 方法類似於之前的方法，不同之處在於：</span><span class="sxs-lookup"><span data-stu-id="b598b-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="b598b-173">不會執行其程式碼，除非 `MonitoringEnabled` 是 `true`。</span><span class="sxs-lookup"><span data-stu-id="b598b-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="b598b-174">將 `CurrentState` 屬性字串設定為記錄程式碼執行時間的描述性訊息。</span><span class="sxs-lookup"><span data-stu-id="b598b-174">Sets the `CurrentState` property string to a descriptive message that records the time that the code ran.</span></span>
* <span data-ttu-id="b598b-175">在其 `WriteConsole` 輸出中記錄目前的 `state`。</span><span class="sxs-lookup"><span data-stu-id="b598b-175">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="b598b-176">執行個體 `ConfigurationMonitor` 會在 *Startup.cs* 的 `ConfigureServices` 中登錄為服務：</span><span class="sxs-lookup"><span data-stu-id="b598b-176">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="b598b-177">[索引] 頁面提供對組態監視的使用者控制。</span><span class="sxs-lookup"><span data-stu-id="b598b-177">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="b598b-178">`IConfigurationMonitor` 的執行個體會插入到 `IndexModel` 中：</span><span class="sxs-lookup"><span data-stu-id="b598b-178">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="b598b-179">按鈕可啟用和停用監視：</span><span class="sxs-lookup"><span data-stu-id="b598b-179">A button enables and disables monitoring:</span></span>

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="b598b-180">當觸發 `OnPostStartMonitoring` 時，會啟用監視並清除目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="b598b-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="b598b-181">當觸發 `OnPostStopMonitoring` 時，則會停用監視，且狀態會設定為反映未進行監視。</span><span class="sxs-lookup"><span data-stu-id="b598b-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="b598b-182">監視快取的檔案變更</span><span class="sxs-lookup"><span data-stu-id="b598b-182">Monitoring cached file changes</span></span>

<span data-ttu-id="b598b-183">檔案內容可以使用 [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) 在記憶體內部快取。</span><span class="sxs-lookup"><span data-stu-id="b598b-183">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="b598b-184">記憶體內部快取將在[記憶體內部快取](xref:performance/caching/memory)主題中描述。</span><span class="sxs-lookup"><span data-stu-id="b598b-184">In-memory caching is described in the [In-memory caching](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="b598b-185">若不採取額外的步驟 (例如下述實作)，當來源資料變更時，將會從快取傳回「過時」(過期) 資料。</span><span class="sxs-lookup"><span data-stu-id="b598b-185">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="b598b-186">更新[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)時，若不考慮快取原始程式檔的狀態，將導致過時的快取資料。</span><span class="sxs-lookup"><span data-stu-id="b598b-186">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="b598b-187">資料的每個要求都會更新滑動期限，但檔案永遠不會重新載入至快取。</span><span class="sxs-lookup"><span data-stu-id="b598b-187">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="b598b-188">使用檔案快取內容的任何應用程式功能都可能會收到過時的內容。</span><span class="sxs-lookup"><span data-stu-id="b598b-188">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="b598b-189">在檔案快取案例中使用變更權杖，即可防止快取中出現過時的檔案內容。</span><span class="sxs-lookup"><span data-stu-id="b598b-189">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="b598b-190">範例應用程式將示範該方法的實作。</span><span class="sxs-lookup"><span data-stu-id="b598b-190">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="b598b-191">此範例會使用 `GetFileContent` 來：</span><span class="sxs-lookup"><span data-stu-id="b598b-191">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="b598b-192">傳回檔案內容。</span><span class="sxs-lookup"><span data-stu-id="b598b-192">Return file content.</span></span>
* <span data-ttu-id="b598b-193">實作使用指數倒退的重試演算法，以涵蓋檔案鎖定暫時阻止讀取檔案的情況。</span><span class="sxs-lookup"><span data-stu-id="b598b-193">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="b598b-194">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="b598b-194">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="b598b-195">`FileService` 是建立來處理快取的檔案查閱。</span><span class="sxs-lookup"><span data-stu-id="b598b-195">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="b598b-196">服務的 `GetFileContent` 方法呼叫會嘗試從記憶體內部快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。</span><span class="sxs-lookup"><span data-stu-id="b598b-196">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="b598b-197">如果使用快取索引鍵找不到快取的內容，則會採取下列動作：</span><span class="sxs-lookup"><span data-stu-id="b598b-197">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="b598b-198">使用 `GetFileContent` 取得檔案內容。</span><span class="sxs-lookup"><span data-stu-id="b598b-198">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="b598b-199">使用 [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 從檔案提供者取得變更權杖。</span><span class="sxs-lookup"><span data-stu-id="b598b-199">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="b598b-200">修改檔案時，就會觸發權杖的回呼。</span><span class="sxs-lookup"><span data-stu-id="b598b-200">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="b598b-201">使用[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)快取檔案內容。</span><span class="sxs-lookup"><span data-stu-id="b598b-201">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="b598b-202">變更權杖附有 [MemoryCacheEntryExtensions.AddExpirationToke](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken)，可在快取的檔案變更時收回快取項目。</span><span class="sxs-lookup"><span data-stu-id="b598b-202">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="b598b-203">`FileService` 登錄在服務容器以及記憶體快取服務 (*Startup.cs*) 中：</span><span class="sxs-lookup"><span data-stu-id="b598b-203">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="b598b-204">頁面模型會使用服務 (*Pages/Index.cshtml.cs*) 載入檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="b598b-204">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="b598b-205">CompositeChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="b598b-205">CompositeChangeToken class</span></span>

<span data-ttu-id="b598b-206">為了表示單一物件中的一或多個 `IChangeToken` 執行個體，請使用 [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) 類別 ([參考來源](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs))。</span><span class="sxs-lookup"><span data-stu-id="b598b-206">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class ([reference source](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).</span></span>

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

<span data-ttu-id="b598b-207">如果任何表示的權杖 `HasChanged` 是 `true`，複合權杖上的 `HasChanged` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="b598b-207">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="b598b-208">如果任何表示的權杖 `ActiveChangeCallbacks` 是 `true`，複合權杖上的 `ActiveChangeCallbacks` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="b598b-208">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="b598b-209">如果發生多個同時變更事件，會叫用複合變更回呼剛好一次。</span><span class="sxs-lookup"><span data-stu-id="b598b-209">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="see-also"></a><span data-ttu-id="b598b-210">另請參閱</span><span class="sxs-lookup"><span data-stu-id="b598b-210">See also</span></span>

* [<span data-ttu-id="b598b-211">記憶體內部快取</span><span class="sxs-lookup"><span data-stu-id="b598b-211">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="b598b-212">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="b598b-212">Working with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="b598b-213">使用變更權杖來偵測變更</span><span class="sxs-lookup"><span data-stu-id="b598b-213">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="b598b-214">回應快取</span><span class="sxs-lookup"><span data-stu-id="b598b-214">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="b598b-215">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="b598b-215">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="b598b-216">快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="b598b-216">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="b598b-217">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="b598b-217">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
