---
title: 在 ASP.NET Core 中使用變更權杖來偵測變更
author: guardrex
description: 了解如何使用變更權杖來追蹤變更。
ms.author: riande
ms.date: 11/10/2017
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 165602587d73907416f47a7ce82a3081e8d74c4b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276889"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="74880-103">在 ASP.NET Core 中使用變更權杖來偵測變更</span><span class="sxs-lookup"><span data-stu-id="74880-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="74880-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="74880-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="74880-105">「變更權杖」是用來追蹤變更的一般用途低階建置組塊。</span><span class="sxs-lookup"><span data-stu-id="74880-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="74880-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74880-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="74880-107">IChangeToken 介面</span><span class="sxs-lookup"><span data-stu-id="74880-107">IChangeToken interface</span></span>

<span data-ttu-id="74880-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) 可傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="74880-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="74880-109">`IChangeToken` 位於 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 命名空間中。</span><span class="sxs-lookup"><span data-stu-id="74880-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="74880-110">如需不使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本) 的應用程式，請參考專案檔中的 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="74880-110">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="74880-111">`IChangeToken` 有兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="74880-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="74880-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) 指出權杖是否主動引發回呼。</span><span class="sxs-lookup"><span data-stu-id="74880-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="74880-113">如果 `ActiveChangedCallbacks` 設定為 `false`，則絕不會呼叫回呼，而且應用程式必須輪詢 `HasChanged` 是否有變更。</span><span class="sxs-lookup"><span data-stu-id="74880-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="74880-114">如果未發生任何變更，或基礎變更接聽程式已遭處置或停用，權杖也可能永遠不會被取消。</span><span class="sxs-lookup"><span data-stu-id="74880-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="74880-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) 取得值，指出是否已發生變更。</span><span class="sxs-lookup"><span data-stu-id="74880-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="74880-116">此介面有一種方法 [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)，它會登錄在權杖變更時叫用的回呼。</span><span class="sxs-lookup"><span data-stu-id="74880-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="74880-117">`HasChanged` 必須在叫用回呼之前設定。</span><span class="sxs-lookup"><span data-stu-id="74880-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="74880-118">ChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="74880-118">ChangeToken class</span></span>

<span data-ttu-id="74880-119">`ChangeToken` 靜態類別用來傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="74880-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="74880-120">`ChangeToken` 位於 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 命名空間中。</span><span class="sxs-lookup"><span data-stu-id="74880-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="74880-121">如需不使用[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)的應用程式，請參考專案檔中的 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="74880-121">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="74880-122">`ChangeToken` 的 [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) 方法將登錄每當權杖變更時要呼叫的 `Action`：</span><span class="sxs-lookup"><span data-stu-id="74880-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="74880-123">`Func<IChangeToken>` 會產生權杖。</span><span class="sxs-lookup"><span data-stu-id="74880-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="74880-124">在權杖變更時呼叫 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74880-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="74880-125">`ChangeToken` 具有 [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) 多載，其採用傳入權杖取用者 `Action` 的額外 `TState` 參數。</span><span class="sxs-lookup"><span data-stu-id="74880-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="74880-126">`OnChange` 會傳回 [IDisposable](/dotnet/api/system.idisposable)。</span><span class="sxs-lookup"><span data-stu-id="74880-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="74880-127">呼叫 [Dispose](/dotnet/api/system.idisposable.dispose) 將阻止權杖接聽其他變更和釋出權杖的資源。</span><span class="sxs-lookup"><span data-stu-id="74880-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="74880-128">ASP.NET Core 中變更權杖的範例用法</span><span class="sxs-lookup"><span data-stu-id="74880-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="74880-129">變更權杖用於 ASP.NET Core 監視物件變更的重要區域：</span><span class="sxs-lookup"><span data-stu-id="74880-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="74880-130">對於監視檔案變更，[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) 的 [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 方法會針對要監看的指定檔案或資料夾建立 `IChangeToken`。</span><span class="sxs-lookup"><span data-stu-id="74880-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="74880-131">`IChangeToken` 權杖可新增至快取項目，以在變更時觸發快取收回。</span><span class="sxs-lookup"><span data-stu-id="74880-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="74880-132">對於 `TOptions` 變更，[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 的預設 [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) 實作具有可接受一或多個 [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) 執行個體的多載。</span><span class="sxs-lookup"><span data-stu-id="74880-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="74880-133">每個執行個體會傳回 `IChangeToken`，以登錄變更通知回呼來追蹤選項變更。</span><span class="sxs-lookup"><span data-stu-id="74880-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="74880-134">監視組態變更</span><span class="sxs-lookup"><span data-stu-id="74880-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="74880-135">ASP.NET Core 範本預設使用 [JSON 組態檔](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*、*appsettings.Development.json* 和 *appsettings.Production.json*) 來載入應用程式組態設定。</span><span class="sxs-lookup"><span data-stu-id="74880-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="74880-136">這些檔案在接受 `reloadOnChange` 參數的 [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 上使用 [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) 擴充方法加以設定 (ASP.NET Core 1.1 和更新版本)。</span><span class="sxs-lookup"><span data-stu-id="74880-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="74880-137">`reloadOnChange` 指出組態是否應該在檔案變更時重新載入。</span><span class="sxs-lookup"><span data-stu-id="74880-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="74880-138">請參閱 [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) 的便利方法 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 中的此設定：</span><span class="sxs-lookup"><span data-stu-id="74880-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="74880-139">檔案型組態是以 [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource) 表示。</span><span class="sxs-lookup"><span data-stu-id="74880-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="74880-140">`FileConfigurationSource` 會使用 [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) 來監視檔案。</span><span class="sxs-lookup"><span data-stu-id="74880-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) to monitor files.</span></span>

<span data-ttu-id="74880-141">根據預設，`IFileMonitor` 由 [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) 提供，它會使用 [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) 來監視組態檔變更。</span><span class="sxs-lookup"><span data-stu-id="74880-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="74880-142">範例應用程式將示範監視組態變更的兩個實作。</span><span class="sxs-lookup"><span data-stu-id="74880-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="74880-143">如果有任一個 *appsettings.json* 檔案變更或檔案的環境版本變更時，每個實作都會執行自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="74880-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="74880-144">範例應用程式會將訊息寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="74880-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="74880-145">組態檔的 `FileSystemWatcher` 可以觸發單一組態檔案變更的多個權杖回呼。</span><span class="sxs-lookup"><span data-stu-id="74880-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="74880-146">範例的實作會檢查組態檔的檔案雜湊，以避免發生這個問題。</span><span class="sxs-lookup"><span data-stu-id="74880-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="74880-147">檢查檔案雜湊，可確保至少其中一個組態檔已在執行自訂程式碼之前變更。</span><span class="sxs-lookup"><span data-stu-id="74880-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="74880-148">此範例會使用 SHA1 檔案雜湊 (*Utilities/Utilities.cs*)：</span><span class="sxs-lookup"><span data-stu-id="74880-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="74880-149">重試將利用指數倒退來實作。</span><span class="sxs-lookup"><span data-stu-id="74880-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="74880-150">因為可能發生檔案鎖定而暫時無法對其中一個檔案計算新的雜湊，所以會進行重試。</span><span class="sxs-lookup"><span data-stu-id="74880-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="74880-151">簡易啟動變更權杖</span><span class="sxs-lookup"><span data-stu-id="74880-151">Simple startup change token</span></span>

<span data-ttu-id="74880-152">將變更通知的權杖取用者 `Action` 回呼登錄到組態重新載入權杖 (*Startup.cs*)：</span><span class="sxs-lookup"><span data-stu-id="74880-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="74880-153">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="74880-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="74880-154">回呼是 `InvokeChanged` 方法：</span><span class="sxs-lookup"><span data-stu-id="74880-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="74880-155">`state` 的回呼用來傳入 `IHostingEnvironment`。</span><span class="sxs-lookup"><span data-stu-id="74880-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="74880-156">這可用來判斷要監視的正確 *appsettings* 組態 JSON 檔案 *appsettings.&lt;環境&gt;.json*。</span><span class="sxs-lookup"><span data-stu-id="74880-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="74880-157">檔案雜湊用來防止 `WriteConsole` 陳述式在組態檔只變更過一次時，由於多個權杖回呼而執行多次。</span><span class="sxs-lookup"><span data-stu-id="74880-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="74880-158">只要應用程式在執行中，此系統就會執行，而且使用者無法停用此系統。</span><span class="sxs-lookup"><span data-stu-id="74880-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="74880-159">以服務方式監視組態變更</span><span class="sxs-lookup"><span data-stu-id="74880-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="74880-160">此範例將實作：</span><span class="sxs-lookup"><span data-stu-id="74880-160">The sample implements:</span></span>

* <span data-ttu-id="74880-161">基本啟動權杖監視。</span><span class="sxs-lookup"><span data-stu-id="74880-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="74880-162">以服務方式監視。</span><span class="sxs-lookup"><span data-stu-id="74880-162">Monitoring as a service.</span></span>
* <span data-ttu-id="74880-163">啟用和停用監視的機制。</span><span class="sxs-lookup"><span data-stu-id="74880-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="74880-164">此範例會建立 `IConfigurationMonitor` 介面 (*Extensions/ConfigurationMonitor.cs*)：</span><span class="sxs-lookup"><span data-stu-id="74880-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="74880-165">已實作類別的建構函式 `ConfigurationMonitor` 登錄變更通知的回呼：</span><span class="sxs-lookup"><span data-stu-id="74880-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="74880-166">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="74880-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="74880-167">`InvokeChanged` 是回呼方法。</span><span class="sxs-lookup"><span data-stu-id="74880-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="74880-168">這個執行個體中的 `state` 是用來存取監視狀態的 `IConfigurationMonitor` 執行個體參考。</span><span class="sxs-lookup"><span data-stu-id="74880-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that is used to access the monitoring state.</span></span> <span data-ttu-id="74880-169">會使用兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="74880-169">Two properties are used:</span></span>

* <span data-ttu-id="74880-170">`MonitoringEnabled` 指出回呼是否應該執行自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="74880-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="74880-171">`CurrentState` 描述要用於 UI 的目前監視狀態。</span><span class="sxs-lookup"><span data-stu-id="74880-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="74880-172">`InvokeChanged` 方法類似於之前的方法，不同之處在於：</span><span class="sxs-lookup"><span data-stu-id="74880-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="74880-173">不會執行其程式碼，除非 `MonitoringEnabled` 是 `true`。</span><span class="sxs-lookup"><span data-stu-id="74880-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="74880-174">在其 `WriteConsole` 輸出中記錄目前的 `state`。</span><span class="sxs-lookup"><span data-stu-id="74880-174">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="74880-175">執行個體 `ConfigurationMonitor` 會在 *Startup.cs* 的 `ConfigureServices` 中登錄為服務：</span><span class="sxs-lookup"><span data-stu-id="74880-175">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="74880-176">[索引] 頁面提供對組態監視的使用者控制。</span><span class="sxs-lookup"><span data-stu-id="74880-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="74880-177">`IConfigurationMonitor` 的執行個體會插入到 `IndexModel` 中：</span><span class="sxs-lookup"><span data-stu-id="74880-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="74880-178">按鈕可啟用和停用監視：</span><span class="sxs-lookup"><span data-stu-id="74880-178">A button enables and disables monitoring:</span></span>

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="74880-179">當觸發 `OnPostStartMonitoring` 時，會啟用監視並清除目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="74880-179">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="74880-180">當觸發 `OnPostStopMonitoring` 時，則會停用監視，且狀態會設定為反映未進行監視。</span><span class="sxs-lookup"><span data-stu-id="74880-180">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="74880-181">監視快取的檔案變更</span><span class="sxs-lookup"><span data-stu-id="74880-181">Monitoring cached file changes</span></span>

<span data-ttu-id="74880-182">檔案內容可以使用 [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) 在記憶體內部快取。</span><span class="sxs-lookup"><span data-stu-id="74880-182">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="74880-183">記憶體內部快取將在[記憶體內部快取](xref:performance/caching/memory)主題中描述。</span><span class="sxs-lookup"><span data-stu-id="74880-183">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="74880-184">若不採取額外的步驟 (例如下述實作)，當來源資料變更時，將會從快取傳回「過時」(過期) 資料。</span><span class="sxs-lookup"><span data-stu-id="74880-184">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="74880-185">更新[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)時，若不考慮快取原始程式檔的狀態，將導致過時的快取資料。</span><span class="sxs-lookup"><span data-stu-id="74880-185">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="74880-186">資料的每個要求都會更新滑動期限，但檔案永遠不會重新載入至快取。</span><span class="sxs-lookup"><span data-stu-id="74880-186">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="74880-187">使用檔案快取內容的任何應用程式功能都可能會收到過時的內容。</span><span class="sxs-lookup"><span data-stu-id="74880-187">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="74880-188">在檔案快取案例中使用變更權杖，即可防止快取中出現過時的檔案內容。</span><span class="sxs-lookup"><span data-stu-id="74880-188">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="74880-189">範例應用程式將示範該方法的實作。</span><span class="sxs-lookup"><span data-stu-id="74880-189">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="74880-190">此範例會使用 `GetFileContent` 來：</span><span class="sxs-lookup"><span data-stu-id="74880-190">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="74880-191">傳回檔案內容。</span><span class="sxs-lookup"><span data-stu-id="74880-191">Return file content.</span></span>
* <span data-ttu-id="74880-192">實作使用指數倒退的重試演算法，以涵蓋檔案鎖定暫時阻止讀取檔案的情況。</span><span class="sxs-lookup"><span data-stu-id="74880-192">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="74880-193">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="74880-193">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="74880-194">`FileService` 是建立來處理快取的檔案查閱。</span><span class="sxs-lookup"><span data-stu-id="74880-194">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="74880-195">服務的 `GetFileContent` 方法呼叫會嘗試從記憶體內部快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。</span><span class="sxs-lookup"><span data-stu-id="74880-195">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="74880-196">如果使用快取索引鍵找不到快取的內容，則會採取下列動作：</span><span class="sxs-lookup"><span data-stu-id="74880-196">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="74880-197">使用 `GetFileContent` 取得檔案內容。</span><span class="sxs-lookup"><span data-stu-id="74880-197">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="74880-198">使用 [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 從檔案提供者取得變更權杖。</span><span class="sxs-lookup"><span data-stu-id="74880-198">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="74880-199">修改檔案時，就會觸發權杖的回呼。</span><span class="sxs-lookup"><span data-stu-id="74880-199">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="74880-200">使用[滑動期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)快取檔案內容。</span><span class="sxs-lookup"><span data-stu-id="74880-200">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="74880-201">變更權杖附有 [MemoryCacheEntryExtensions.AddExpirationToke](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken)，可在快取的檔案變更時收回快取項目。</span><span class="sxs-lookup"><span data-stu-id="74880-201">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="74880-202">`FileService` 登錄在服務容器以及記憶體快取服務 (*Startup.cs*) 中：</span><span class="sxs-lookup"><span data-stu-id="74880-202">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="74880-203">頁面模型會使用服務 (*Pages/Index.cshtml.cs*) 載入檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="74880-203">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="74880-204">CompositeChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="74880-204">CompositeChangeToken class</span></span>

<span data-ttu-id="74880-205">為了表示單一物件中的一或多個 `IChangeToken` 執行個體，請使用 [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) 類別。</span><span class="sxs-lookup"><span data-stu-id="74880-205">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class.</span></span>

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

<span data-ttu-id="74880-206">如果任何表示的權杖 `HasChanged` 是 `true`，複合權杖上的 `HasChanged` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="74880-206">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="74880-207">如果任何表示的權杖 `ActiveChangeCallbacks` 是 `true`，複合權杖上的 `ActiveChangeCallbacks` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="74880-207">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="74880-208">如果發生多個同時變更事件，會叫用複合變更回呼剛好一次。</span><span class="sxs-lookup"><span data-stu-id="74880-208">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="74880-209">其他資源</span><span class="sxs-lookup"><span data-stu-id="74880-209">Additional resources</span></span>

* [<span data-ttu-id="74880-210">記憶體中快取</span><span class="sxs-lookup"><span data-stu-id="74880-210">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="74880-211">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="74880-211">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="74880-212">回應快取</span><span class="sxs-lookup"><span data-stu-id="74880-212">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="74880-213">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="74880-213">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="74880-214">快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="74880-214">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="74880-215">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="74880-215">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
