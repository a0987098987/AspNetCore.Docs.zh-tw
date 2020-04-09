---
title: 在 ASP.NET Core 中使用變更權杖來偵測變更
author: rick-anderson
description: 了解如何使用變更權杖來追蹤變更。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/07/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: 70451e219f1295b854e2f84aac55f0cfd1786b19
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78656342"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="45870-103">在 ASP.NET Core 中使用變更權杖來偵測變更</span><span class="sxs-lookup"><span data-stu-id="45870-103">Detect changes with change tokens in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="45870-104">「變更權杖」\*\* 是用來追蹤狀態變更的一般用途低階建置組塊。</span><span class="sxs-lookup"><span data-stu-id="45870-104">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="45870-105">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="45870-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="45870-106">IChangeToken 介面</span><span class="sxs-lookup"><span data-stu-id="45870-106">IChangeToken interface</span></span>

<span data-ttu-id="45870-107"><xref:Microsoft.Extensions.Primitives.IChangeToken> 會傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="45870-107"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="45870-108">`IChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。</span><span class="sxs-lookup"><span data-stu-id="45870-108">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="45870-109">[Microsoft.擴展.原始 NuGet](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)包隱式提供給ASP.NET核心應用。</span><span class="sxs-lookup"><span data-stu-id="45870-109">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="45870-110">`IChangeToken` 有兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="45870-110">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="45870-111"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> 指出權杖是否主動引發回呼。</span><span class="sxs-lookup"><span data-stu-id="45870-111"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="45870-112">如果 `ActiveChangedCallbacks` 設定為 `false`，則絕不會呼叫回呼，而且應用程式必須輪詢 `HasChanged` 是否有變更。</span><span class="sxs-lookup"><span data-stu-id="45870-112">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="45870-113">如果未發生任何變更，或基礎變更接聽程式已遭處置或停用，權杖也可能永遠不會被取消。</span><span class="sxs-lookup"><span data-stu-id="45870-113">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="45870-114"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> 會接收指出是否已發生變更的值。</span><span class="sxs-lookup"><span data-stu-id="45870-114"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="45870-115">`IChangeToken` 介面包括 [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) 方法，它會登錄在權杖變更時叫用的回呼。</span><span class="sxs-lookup"><span data-stu-id="45870-115">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="45870-116">`HasChanged` 必須在叫用回呼之前設定。</span><span class="sxs-lookup"><span data-stu-id="45870-116">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="45870-117">ChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="45870-117">ChangeToken class</span></span>

<span data-ttu-id="45870-118"><xref:Microsoft.Extensions.Primitives.ChangeToken> 靜態類別用來傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="45870-118"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="45870-119">`ChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。</span><span class="sxs-lookup"><span data-stu-id="45870-119">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="45870-120">[Microsoft.擴展.原始 NuGet](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)包隱式提供給ASP.NET核心應用。</span><span class="sxs-lookup"><span data-stu-id="45870-120">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="45870-121">[ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) 方法會登錄每當權杖變更時要呼叫的 `Action`：</span><span class="sxs-lookup"><span data-stu-id="45870-121">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="45870-122">`Func<IChangeToken>` 會產生權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-122">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="45870-123">在權杖變更時呼叫 `Action`。</span><span class="sxs-lookup"><span data-stu-id="45870-123">`Action` is called when the token changes.</span></span>

<span data-ttu-id="45870-124">[ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) 多載接受傳遞到權杖取用者 `Action` 的額外 `TState` 參數。</span><span class="sxs-lookup"><span data-stu-id="45870-124">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="45870-125">`OnChange` 會傳回 <xref:System.IDisposable>。</span><span class="sxs-lookup"><span data-stu-id="45870-125">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="45870-126">呼叫 <xref:System.IDisposable.Dispose*> 將阻止權杖接聽其他變更和釋出權杖的資源。</span><span class="sxs-lookup"><span data-stu-id="45870-126">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="45870-127">ASP.NET Core 中變更權杖的範例用法</span><span class="sxs-lookup"><span data-stu-id="45870-127">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="45870-128">變更權杖用於 ASP.NET Core 監視物件變更的重要區域：</span><span class="sxs-lookup"><span data-stu-id="45870-128">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="45870-129">針對監視檔案變更，<xref:Microsoft.Extensions.FileProviders.IFileProvider> 的 <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> 方法會針對要監看的指定檔案或資料夾建立 `IChangeToken`。</span><span class="sxs-lookup"><span data-stu-id="45870-129">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="45870-130">`IChangeToken` 權杖可新增至快取項目，以在變更時觸發快取收回。</span><span class="sxs-lookup"><span data-stu-id="45870-130">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="45870-131">針對 `TOptions` 變更，<xref:Microsoft.Extensions.Options.IOptionsMonitor`1> 的預設 <xref:Microsoft.Extensions.Options.OptionsMonitor`1> 實作具有會接受一或多個 <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> 執行個體的多載。</span><span class="sxs-lookup"><span data-stu-id="45870-131">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="45870-132">每個執行個體會傳回 `IChangeToken`，以登錄變更通知回呼來追蹤選項變更。</span><span class="sxs-lookup"><span data-stu-id="45870-132">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="45870-133">監視設定變更</span><span class="sxs-lookup"><span data-stu-id="45870-133">Monitor for configuration changes</span></span>

<span data-ttu-id="45870-134">ASP.NET Core 範本預設使用 [JSON 組態檔](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*、*appsettings.Development.json* 和 *appsettings.Production.json*) 來載入應用程式組態設定。</span><span class="sxs-lookup"><span data-stu-id="45870-134">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="45870-135">這些檔案是在接受 `reloadOnChange` 參數的 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 擴充方法上使用 [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) 擴充方法來設定的。</span><span class="sxs-lookup"><span data-stu-id="45870-135">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="45870-136">`reloadOnChange` 指出組態是否應該在檔案變更時重新載入。</span><span class="sxs-lookup"><span data-stu-id="45870-136">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="45870-137">此設定會出在 <xref:Microsoft.Extensions.Hosting.Host> 便利方法 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 中：</span><span class="sxs-lookup"><span data-stu-id="45870-137">This setting appears in the <xref:Microsoft.Extensions.Hosting.Host> convenience method <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="45870-138">檔案型設定是以 <xref:Microsoft.Extensions.Configuration.FileConfigurationSource> 代表的。</span><span class="sxs-lookup"><span data-stu-id="45870-138">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="45870-139">`FileConfigurationSource` 使用 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 來監視檔案。</span><span class="sxs-lookup"><span data-stu-id="45870-139">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="45870-140">根據預設，`IFileMonitor` 由 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供，它會使用 <xref:System.IO.FileSystemWatcher> 來監視設定檔變更。</span><span class="sxs-lookup"><span data-stu-id="45870-140">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="45870-141">範例應用程式將示範監視組態變更的兩個實作。</span><span class="sxs-lookup"><span data-stu-id="45870-141">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="45870-142">如果任一 *appsettings* 檔案變更，兩個檔案監視實作都會執行自訂程式碼&mdash;範例應用程式會將訊息寫入到主控台。</span><span class="sxs-lookup"><span data-stu-id="45870-142">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="45870-143">組態檔的 `FileSystemWatcher` 可以觸發單一組態檔案變更的多個權杖回呼。</span><span class="sxs-lookup"><span data-stu-id="45870-143">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="45870-144">為確定自訂程式碼只在觸發多個權杖回呼時執行一次，範例的實作會檢查檔案雜湊。</span><span class="sxs-lookup"><span data-stu-id="45870-144">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="45870-145">範例使用 SHA1 檔案雜湊處理。</span><span class="sxs-lookup"><span data-stu-id="45870-145">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="45870-146">重試將利用指數倒退來實作。</span><span class="sxs-lookup"><span data-stu-id="45870-146">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="45870-147">因為可能發生檔案鎖定而暫時無法對檔案計算新的雜湊，所以會進行重試。</span><span class="sxs-lookup"><span data-stu-id="45870-147">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="45870-148">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="45870-148">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="45870-149">簡易啟動變更權杖</span><span class="sxs-lookup"><span data-stu-id="45870-149">Simple startup change token</span></span>

<span data-ttu-id="45870-150">將變更通知的權杖取用者 `Action` 回呼登錄到設定重新載入權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-150">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="45870-151">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="45870-151">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="45870-152">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-152">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="45870-153">回呼是 `InvokeChanged` 方法：</span><span class="sxs-lookup"><span data-stu-id="45870-153">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="45870-154">回呼的 `state` 是用來傳入 `IWebHostEnvironment`，它在指定要監視的 *appsettings* 設定檔時非常實用 (例如，開發環境中的 *appsettings.Development.json*)。</span><span class="sxs-lookup"><span data-stu-id="45870-154">The `state` of the callback is used to pass in the `IWebHostEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="45870-155">檔案雜湊用來防止 `WriteConsole` 陳述式在組態檔只變更過一次時，由於多個權杖回呼而執行多次。</span><span class="sxs-lookup"><span data-stu-id="45870-155">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="45870-156">只要應用程式在執行中，此系統就會執行，而且使用者無法停用此系統。</span><span class="sxs-lookup"><span data-stu-id="45870-156">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="45870-157">以服務方式監視設定變更</span><span class="sxs-lookup"><span data-stu-id="45870-157">Monitor configuration changes as a service</span></span>

<span data-ttu-id="45870-158">此範例將實作：</span><span class="sxs-lookup"><span data-stu-id="45870-158">The sample implements:</span></span>

* <span data-ttu-id="45870-159">基本啟動權杖監視。</span><span class="sxs-lookup"><span data-stu-id="45870-159">Basic startup token monitoring.</span></span>
* <span data-ttu-id="45870-160">以服務方式監視。</span><span class="sxs-lookup"><span data-stu-id="45870-160">Monitoring as a service.</span></span>
* <span data-ttu-id="45870-161">啟用和停用監視的機制。</span><span class="sxs-lookup"><span data-stu-id="45870-161">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="45870-162">範例會建立 `IConfigurationMonitor` 介面。</span><span class="sxs-lookup"><span data-stu-id="45870-162">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="45870-163">*Extensions/ConfigurationMonitor.cs*：</span><span class="sxs-lookup"><span data-stu-id="45870-163">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="45870-164">已實作類別的建構函式 `ConfigurationMonitor` 登錄變更通知的回呼：</span><span class="sxs-lookup"><span data-stu-id="45870-164">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="45870-165">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-165">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="45870-166">`InvokeChanged` 是回呼方法。</span><span class="sxs-lookup"><span data-stu-id="45870-166">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="45870-167">此執行個體中的 `state` 是用來存取監視狀態的 `IConfigurationMonitor` 執行個體參考。</span><span class="sxs-lookup"><span data-stu-id="45870-167">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="45870-168">會使用兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="45870-168">Two properties are used:</span></span>

* <span data-ttu-id="45870-169">`MonitoringEnabled`&ndash;指示回調是否應運行其自定義代碼。</span><span class="sxs-lookup"><span data-stu-id="45870-169">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="45870-170">`CurrentState`&ndash;描述用於 UI 的當前監視狀態。</span><span class="sxs-lookup"><span data-stu-id="45870-170">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="45870-171">`InvokeChanged` 方法類似於之前的方法，不同之處在於：</span><span class="sxs-lookup"><span data-stu-id="45870-171">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="45870-172">不會執行其程式碼，除非 `MonitoringEnabled` 是 `true`。</span><span class="sxs-lookup"><span data-stu-id="45870-172">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="45870-173">在其 `WriteConsole` 輸出中輸出目前的 `state`。</span><span class="sxs-lookup"><span data-stu-id="45870-173">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="45870-174">執行個體 `ConfigurationMonitor` 會在 `Startup.ConfigureServices` 中登錄為服務：</span><span class="sxs-lookup"><span data-stu-id="45870-174">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="45870-175">[索引] 頁面提供對組態監視的使用者控制。</span><span class="sxs-lookup"><span data-stu-id="45870-175">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="45870-176">`IConfigurationMonitor` 的執行個體會插入到 `IndexModel` 中。</span><span class="sxs-lookup"><span data-stu-id="45870-176">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="45870-177">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="45870-177">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="45870-178">設定監視器 (`_monitor`) 是用來啟用或停用監視並設定 UI 回饋的目前狀態：</span><span class="sxs-lookup"><span data-stu-id="45870-178">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="45870-179">當觸發 `OnPostStartMonitoring` 時，會啟用監視並清除目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="45870-179">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="45870-180">當觸發 `OnPostStopMonitoring` 時，則會停用監視，且狀態會設定為反映未進行監視。</span><span class="sxs-lookup"><span data-stu-id="45870-180">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="45870-181">UI 啟用和停用監視中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="45870-181">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="45870-182">*頁面/索引.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="45870-182">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="45870-183">監視快取的檔案變更</span><span class="sxs-lookup"><span data-stu-id="45870-183">Monitor cached file changes</span></span>

<span data-ttu-id="45870-184">檔案內容可以使用 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 在記憶體內部快取。</span><span class="sxs-lookup"><span data-stu-id="45870-184">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="45870-185">記憶體內部快取將在[記憶體內部快取](xref:performance/caching/memory)主題中描述。</span><span class="sxs-lookup"><span data-stu-id="45870-185">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="45870-186">若不採取額外的步驟 (例如下述實作)，當來源資料變更時，將會從快取傳回「過時」\*\*(過期) 資料。</span><span class="sxs-lookup"><span data-stu-id="45870-186">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="45870-187">例如，更新[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)時，若不考慮快取原始程式檔的狀態，將導致過時的快取檔案資料。</span><span class="sxs-lookup"><span data-stu-id="45870-187">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="45870-188">資料的每個要求都會更新滑動期限，但檔案永遠不會重新載入至快取。</span><span class="sxs-lookup"><span data-stu-id="45870-188">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="45870-189">使用檔案快取內容的任何應用程式功能都可能會收到過時的內容。</span><span class="sxs-lookup"><span data-stu-id="45870-189">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="45870-190">在檔案快取案例中使用變更權杖可防止快取中出現過時的檔案內容。</span><span class="sxs-lookup"><span data-stu-id="45870-190">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="45870-191">範例應用程式將示範該方法的實作。</span><span class="sxs-lookup"><span data-stu-id="45870-191">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="45870-192">此範例會使用 `GetFileContent` 來：</span><span class="sxs-lookup"><span data-stu-id="45870-192">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="45870-193">傳回檔案內容。</span><span class="sxs-lookup"><span data-stu-id="45870-193">Return file content.</span></span>
* <span data-ttu-id="45870-194">實作使用指數倒退的重試演算法，以涵蓋檔案鎖定暫時阻止讀取檔案的情況。</span><span class="sxs-lookup"><span data-stu-id="45870-194">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="45870-195">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="45870-195">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="45870-196">`FileService` 是建立來處理快取的檔案查閱。</span><span class="sxs-lookup"><span data-stu-id="45870-196">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="45870-197">服務的 `GetFileContent` 方法呼叫會嘗試從記憶體內部快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。</span><span class="sxs-lookup"><span data-stu-id="45870-197">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="45870-198">如果使用快取索引鍵找不到快取的內容，則會採取下列動作：</span><span class="sxs-lookup"><span data-stu-id="45870-198">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="45870-199">使用 `GetFileContent` 取得檔案內容。</span><span class="sxs-lookup"><span data-stu-id="45870-199">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="45870-200">使用 [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 從檔案提供者取得變更權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-200">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="45870-201">修改檔案時，就會觸發權杖的回呼。</span><span class="sxs-lookup"><span data-stu-id="45870-201">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="45870-202">使用[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)快取檔案內容。</span><span class="sxs-lookup"><span data-stu-id="45870-202">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="45870-203">變更權杖附有 [MemoryCacheEntryExtensions.AddExpirationToke](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*)，可在快取的檔案變更時收回快取項目。</span><span class="sxs-lookup"><span data-stu-id="45870-203">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="45870-204">在下面的範例中,檔案存儲在應用[的內容根目錄](xref:fundamentals/index#content-root)中。</span><span class="sxs-lookup"><span data-stu-id="45870-204">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="45870-205">`IWebHostEnvironment.ContentRootFileProvider`取得指向<xref:Microsoft.Extensions.FileProviders.IFileProvider>應用程式的`IWebHostEnvironment.ContentRootPath`。</span><span class="sxs-lookup"><span data-stu-id="45870-205">`IWebHostEnvironment.ContentRootFileProvider` is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's `IWebHostEnvironment.ContentRootPath`.</span></span> <span data-ttu-id="45870-206">`filePath` 是使用 [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath) 取得的。</span><span class="sxs-lookup"><span data-stu-id="45870-206">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="45870-207">`FileService` 是在服務容器以及記憶體快取服務中登錄的。</span><span class="sxs-lookup"><span data-stu-id="45870-207">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="45870-208">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="45870-208">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="45870-209">頁面模型會使用服務載入檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="45870-209">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="45870-210">在索引頁面的 `OnGet` 方法 (*Pages/Index.cshtml.cs*) 中：</span><span class="sxs-lookup"><span data-stu-id="45870-210">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="45870-211">CompositeChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="45870-211">CompositeChangeToken class</span></span>

<span data-ttu-id="45870-212">為了表示單一物件中的一或多個 `IChangeToken` 執行個體，請使用 <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> 類別。</span><span class="sxs-lookup"><span data-stu-id="45870-212">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="45870-213">如果任何表示的權杖 `HasChanged` 是 `true`，複合權杖上的 `HasChanged` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="45870-213">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="45870-214">如果任何表示的權杖 `ActiveChangeCallbacks` 是 `true`，複合權杖上的 `ActiveChangeCallbacks` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="45870-214">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="45870-215">如果發生多個同時變更事件，會叫用複合變更回呼一次。</span><span class="sxs-lookup"><span data-stu-id="45870-215">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="45870-216">「變更權杖」\*\* 是用來追蹤狀態變更的一般用途低階建置組塊。</span><span class="sxs-lookup"><span data-stu-id="45870-216">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="45870-217">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="45870-217">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="45870-218">IChangeToken 介面</span><span class="sxs-lookup"><span data-stu-id="45870-218">IChangeToken interface</span></span>

<span data-ttu-id="45870-219"><xref:Microsoft.Extensions.Primitives.IChangeToken> 會傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="45870-219"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="45870-220">`IChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。</span><span class="sxs-lookup"><span data-stu-id="45870-220">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="45870-221">如需不使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)的應用程式，請建立 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="45870-221">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="45870-222">`IChangeToken` 有兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="45870-222">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="45870-223"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> 指出權杖是否主動引發回呼。</span><span class="sxs-lookup"><span data-stu-id="45870-223"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="45870-224">如果 `ActiveChangedCallbacks` 設定為 `false`，則絕不會呼叫回呼，而且應用程式必須輪詢 `HasChanged` 是否有變更。</span><span class="sxs-lookup"><span data-stu-id="45870-224">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="45870-225">如果未發生任何變更，或基礎變更接聽程式已遭處置或停用，權杖也可能永遠不會被取消。</span><span class="sxs-lookup"><span data-stu-id="45870-225">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="45870-226"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> 會接收指出是否已發生變更的值。</span><span class="sxs-lookup"><span data-stu-id="45870-226"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="45870-227">`IChangeToken` 介面包括 [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) 方法，它會登錄在權杖變更時叫用的回呼。</span><span class="sxs-lookup"><span data-stu-id="45870-227">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="45870-228">`HasChanged` 必須在叫用回呼之前設定。</span><span class="sxs-lookup"><span data-stu-id="45870-228">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="45870-229">ChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="45870-229">ChangeToken class</span></span>

<span data-ttu-id="45870-230"><xref:Microsoft.Extensions.Primitives.ChangeToken> 靜態類別用來傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="45870-230"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="45870-231">`ChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。</span><span class="sxs-lookup"><span data-stu-id="45870-231">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="45870-232">如需不使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)的應用程式，請建立 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="45870-232">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="45870-233">[ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) 方法會登錄每當權杖變更時要呼叫的 `Action`：</span><span class="sxs-lookup"><span data-stu-id="45870-233">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="45870-234">`Func<IChangeToken>` 會產生權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-234">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="45870-235">在權杖變更時呼叫 `Action`。</span><span class="sxs-lookup"><span data-stu-id="45870-235">`Action` is called when the token changes.</span></span>

<span data-ttu-id="45870-236">[ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) 多載接受傳遞到權杖取用者 `Action` 的額外 `TState` 參數。</span><span class="sxs-lookup"><span data-stu-id="45870-236">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="45870-237">`OnChange` 會傳回 <xref:System.IDisposable>。</span><span class="sxs-lookup"><span data-stu-id="45870-237">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="45870-238">呼叫 <xref:System.IDisposable.Dispose*> 將阻止權杖接聽其他變更和釋出權杖的資源。</span><span class="sxs-lookup"><span data-stu-id="45870-238">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="45870-239">ASP.NET Core 中變更權杖的範例用法</span><span class="sxs-lookup"><span data-stu-id="45870-239">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="45870-240">變更權杖用於 ASP.NET Core 監視物件變更的重要區域：</span><span class="sxs-lookup"><span data-stu-id="45870-240">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="45870-241">針對監視檔案變更，<xref:Microsoft.Extensions.FileProviders.IFileProvider> 的 <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> 方法會針對要監看的指定檔案或資料夾建立 `IChangeToken`。</span><span class="sxs-lookup"><span data-stu-id="45870-241">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="45870-242">`IChangeToken` 權杖可新增至快取項目，以在變更時觸發快取收回。</span><span class="sxs-lookup"><span data-stu-id="45870-242">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="45870-243">針對 `TOptions` 變更，<xref:Microsoft.Extensions.Options.IOptionsMonitor`1> 的預設 <xref:Microsoft.Extensions.Options.OptionsMonitor`1> 實作具有會接受一或多個 <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> 執行個體的多載。</span><span class="sxs-lookup"><span data-stu-id="45870-243">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="45870-244">每個執行個體會傳回 `IChangeToken`，以登錄變更通知回呼來追蹤選項變更。</span><span class="sxs-lookup"><span data-stu-id="45870-244">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="45870-245">監視設定變更</span><span class="sxs-lookup"><span data-stu-id="45870-245">Monitor for configuration changes</span></span>

<span data-ttu-id="45870-246">ASP.NET Core 範本預設使用 [JSON 組態檔](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*、*appsettings.Development.json* 和 *appsettings.Production.json*) 來載入應用程式組態設定。</span><span class="sxs-lookup"><span data-stu-id="45870-246">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="45870-247">這些檔案是在接受 `reloadOnChange` 參數的 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 擴充方法上使用 [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) 擴充方法來設定的。</span><span class="sxs-lookup"><span data-stu-id="45870-247">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="45870-248">`reloadOnChange` 指出組態是否應該在檔案變更時重新載入。</span><span class="sxs-lookup"><span data-stu-id="45870-248">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="45870-249">此設定會出在 <xref:Microsoft.AspNetCore.WebHost> 便利方法 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 中：</span><span class="sxs-lookup"><span data-stu-id="45870-249">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="45870-250">檔案型設定是以 <xref:Microsoft.Extensions.Configuration.FileConfigurationSource> 代表的。</span><span class="sxs-lookup"><span data-stu-id="45870-250">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="45870-251">`FileConfigurationSource` 使用 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 來監視檔案。</span><span class="sxs-lookup"><span data-stu-id="45870-251">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="45870-252">根據預設，`IFileMonitor` 由 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供，它會使用 <xref:System.IO.FileSystemWatcher> 來監視設定檔變更。</span><span class="sxs-lookup"><span data-stu-id="45870-252">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="45870-253">範例應用程式將示範監視組態變更的兩個實作。</span><span class="sxs-lookup"><span data-stu-id="45870-253">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="45870-254">如果任一 *appsettings* 檔案變更，兩個檔案監視實作都會執行自訂程式碼&mdash;範例應用程式會將訊息寫入到主控台。</span><span class="sxs-lookup"><span data-stu-id="45870-254">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="45870-255">組態檔的 `FileSystemWatcher` 可以觸發單一組態檔案變更的多個權杖回呼。</span><span class="sxs-lookup"><span data-stu-id="45870-255">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="45870-256">為確定自訂程式碼只在觸發多個權杖回呼時執行一次，範例的實作會檢查檔案雜湊。</span><span class="sxs-lookup"><span data-stu-id="45870-256">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="45870-257">範例使用 SHA1 檔案雜湊處理。</span><span class="sxs-lookup"><span data-stu-id="45870-257">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="45870-258">重試將利用指數倒退來實作。</span><span class="sxs-lookup"><span data-stu-id="45870-258">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="45870-259">因為可能發生檔案鎖定而暫時無法對檔案計算新的雜湊，所以會進行重試。</span><span class="sxs-lookup"><span data-stu-id="45870-259">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="45870-260">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="45870-260">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="45870-261">簡易啟動變更權杖</span><span class="sxs-lookup"><span data-stu-id="45870-261">Simple startup change token</span></span>

<span data-ttu-id="45870-262">將變更通知的權杖取用者 `Action` 回呼登錄到設定重新載入權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-262">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="45870-263">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="45870-263">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="45870-264">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-264">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="45870-265">回呼是 `InvokeChanged` 方法：</span><span class="sxs-lookup"><span data-stu-id="45870-265">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="45870-266">回呼的 `state` 是用來傳入 `IHostingEnvironment`，它在指定要監視的 *appsettings* 設定檔時非常實用 (例如，開發環境中的 *appsettings.Development.json*)。</span><span class="sxs-lookup"><span data-stu-id="45870-266">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="45870-267">檔案雜湊用來防止 `WriteConsole` 陳述式在組態檔只變更過一次時，由於多個權杖回呼而執行多次。</span><span class="sxs-lookup"><span data-stu-id="45870-267">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="45870-268">只要應用程式在執行中，此系統就會執行，而且使用者無法停用此系統。</span><span class="sxs-lookup"><span data-stu-id="45870-268">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="45870-269">以服務方式監視設定變更</span><span class="sxs-lookup"><span data-stu-id="45870-269">Monitor configuration changes as a service</span></span>

<span data-ttu-id="45870-270">此範例將實作：</span><span class="sxs-lookup"><span data-stu-id="45870-270">The sample implements:</span></span>

* <span data-ttu-id="45870-271">基本啟動權杖監視。</span><span class="sxs-lookup"><span data-stu-id="45870-271">Basic startup token monitoring.</span></span>
* <span data-ttu-id="45870-272">以服務方式監視。</span><span class="sxs-lookup"><span data-stu-id="45870-272">Monitoring as a service.</span></span>
* <span data-ttu-id="45870-273">啟用和停用監視的機制。</span><span class="sxs-lookup"><span data-stu-id="45870-273">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="45870-274">範例會建立 `IConfigurationMonitor` 介面。</span><span class="sxs-lookup"><span data-stu-id="45870-274">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="45870-275">*Extensions/ConfigurationMonitor.cs*：</span><span class="sxs-lookup"><span data-stu-id="45870-275">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="45870-276">已實作類別的建構函式 `ConfigurationMonitor` 登錄變更通知的回呼：</span><span class="sxs-lookup"><span data-stu-id="45870-276">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="45870-277">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-277">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="45870-278">`InvokeChanged` 是回呼方法。</span><span class="sxs-lookup"><span data-stu-id="45870-278">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="45870-279">此執行個體中的 `state` 是用來存取監視狀態的 `IConfigurationMonitor` 執行個體參考。</span><span class="sxs-lookup"><span data-stu-id="45870-279">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="45870-280">會使用兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="45870-280">Two properties are used:</span></span>

* <span data-ttu-id="45870-281">`MonitoringEnabled`&ndash;指示回調是否應運行其自定義代碼。</span><span class="sxs-lookup"><span data-stu-id="45870-281">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="45870-282">`CurrentState`&ndash;描述用於 UI 的當前監視狀態。</span><span class="sxs-lookup"><span data-stu-id="45870-282">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="45870-283">`InvokeChanged` 方法類似於之前的方法，不同之處在於：</span><span class="sxs-lookup"><span data-stu-id="45870-283">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="45870-284">不會執行其程式碼，除非 `MonitoringEnabled` 是 `true`。</span><span class="sxs-lookup"><span data-stu-id="45870-284">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="45870-285">在其 `WriteConsole` 輸出中輸出目前的 `state`。</span><span class="sxs-lookup"><span data-stu-id="45870-285">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="45870-286">執行個體 `ConfigurationMonitor` 會在 `Startup.ConfigureServices` 中登錄為服務：</span><span class="sxs-lookup"><span data-stu-id="45870-286">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="45870-287">[索引] 頁面提供對組態監視的使用者控制。</span><span class="sxs-lookup"><span data-stu-id="45870-287">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="45870-288">`IConfigurationMonitor` 的執行個體會插入到 `IndexModel` 中。</span><span class="sxs-lookup"><span data-stu-id="45870-288">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="45870-289">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="45870-289">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="45870-290">設定監視器 (`_monitor`) 是用來啟用或停用監視並設定 UI 回饋的目前狀態：</span><span class="sxs-lookup"><span data-stu-id="45870-290">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="45870-291">當觸發 `OnPostStartMonitoring` 時，會啟用監視並清除目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="45870-291">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="45870-292">當觸發 `OnPostStopMonitoring` 時，則會停用監視，且狀態會設定為反映未進行監視。</span><span class="sxs-lookup"><span data-stu-id="45870-292">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="45870-293">UI 啟用和停用監視中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="45870-293">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="45870-294">*頁面/索引.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="45870-294">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="45870-295">監視快取的檔案變更</span><span class="sxs-lookup"><span data-stu-id="45870-295">Monitor cached file changes</span></span>

<span data-ttu-id="45870-296">檔案內容可以使用 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 在記憶體內部快取。</span><span class="sxs-lookup"><span data-stu-id="45870-296">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="45870-297">記憶體內部快取將在[記憶體內部快取](xref:performance/caching/memory)主題中描述。</span><span class="sxs-lookup"><span data-stu-id="45870-297">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="45870-298">若不採取額外的步驟 (例如下述實作)，當來源資料變更時，將會從快取傳回「過時」\*\*(過期) 資料。</span><span class="sxs-lookup"><span data-stu-id="45870-298">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="45870-299">例如，更新[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)時，若不考慮快取原始程式檔的狀態，將導致過時的快取檔案資料。</span><span class="sxs-lookup"><span data-stu-id="45870-299">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="45870-300">資料的每個要求都會更新滑動期限，但檔案永遠不會重新載入至快取。</span><span class="sxs-lookup"><span data-stu-id="45870-300">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="45870-301">使用檔案快取內容的任何應用程式功能都可能會收到過時的內容。</span><span class="sxs-lookup"><span data-stu-id="45870-301">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="45870-302">在檔案快取案例中使用變更權杖可防止快取中出現過時的檔案內容。</span><span class="sxs-lookup"><span data-stu-id="45870-302">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="45870-303">範例應用程式將示範該方法的實作。</span><span class="sxs-lookup"><span data-stu-id="45870-303">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="45870-304">此範例會使用 `GetFileContent` 來：</span><span class="sxs-lookup"><span data-stu-id="45870-304">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="45870-305">傳回檔案內容。</span><span class="sxs-lookup"><span data-stu-id="45870-305">Return file content.</span></span>
* <span data-ttu-id="45870-306">實作使用指數倒退的重試演算法，以涵蓋檔案鎖定暫時阻止讀取檔案的情況。</span><span class="sxs-lookup"><span data-stu-id="45870-306">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="45870-307">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="45870-307">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="45870-308">`FileService` 是建立來處理快取的檔案查閱。</span><span class="sxs-lookup"><span data-stu-id="45870-308">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="45870-309">服務的 `GetFileContent` 方法呼叫會嘗試從記憶體內部快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。</span><span class="sxs-lookup"><span data-stu-id="45870-309">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="45870-310">如果使用快取索引鍵找不到快取的內容，則會採取下列動作：</span><span class="sxs-lookup"><span data-stu-id="45870-310">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="45870-311">使用 `GetFileContent` 取得檔案內容。</span><span class="sxs-lookup"><span data-stu-id="45870-311">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="45870-312">使用 [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 從檔案提供者取得變更權杖。</span><span class="sxs-lookup"><span data-stu-id="45870-312">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="45870-313">修改檔案時，就會觸發權杖的回呼。</span><span class="sxs-lookup"><span data-stu-id="45870-313">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="45870-314">使用[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)快取檔案內容。</span><span class="sxs-lookup"><span data-stu-id="45870-314">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="45870-315">變更權杖附有 [MemoryCacheEntryExtensions.AddExpirationToke](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*)，可在快取的檔案變更時收回快取項目。</span><span class="sxs-lookup"><span data-stu-id="45870-315">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="45870-316">在下面的範例中,檔案存儲在應用[的內容根目錄](xref:fundamentals/index#content-root)中。</span><span class="sxs-lookup"><span data-stu-id="45870-316">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="45870-317">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) 是用來取得指向應用程式 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath> 的 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="45870-317">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="45870-318">`filePath` 是使用 [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath) 取得的。</span><span class="sxs-lookup"><span data-stu-id="45870-318">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="45870-319">`FileService` 是在服務容器以及記憶體快取服務中登錄的。</span><span class="sxs-lookup"><span data-stu-id="45870-319">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="45870-320">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="45870-320">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="45870-321">頁面模型會使用服務載入檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="45870-321">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="45870-322">在索引頁面的 `OnGet` 方法 (*Pages/Index.cshtml.cs*) 中：</span><span class="sxs-lookup"><span data-stu-id="45870-322">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="45870-323">CompositeChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="45870-323">CompositeChangeToken class</span></span>

<span data-ttu-id="45870-324">為了表示單一物件中的一或多個 `IChangeToken` 執行個體，請使用 <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> 類別。</span><span class="sxs-lookup"><span data-stu-id="45870-324">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="45870-325">如果任何表示的權杖 `HasChanged` 是 `true`，複合權杖上的 `HasChanged` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="45870-325">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="45870-326">如果任何表示的權杖 `ActiveChangeCallbacks` 是 `true`，複合權杖上的 `ActiveChangeCallbacks` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="45870-326">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="45870-327">如果發生多個同時變更事件，會叫用複合變更回呼一次。</span><span class="sxs-lookup"><span data-stu-id="45870-327">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="45870-328">其他資源</span><span class="sxs-lookup"><span data-stu-id="45870-328">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
