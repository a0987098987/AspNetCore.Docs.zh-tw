---
<span data-ttu-id="5ce99-101">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="5ce99-101">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="5ce99-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="5ce99-102">'Blazor'</span></span>
- <span data-ttu-id="5ce99-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="5ce99-103">'Identity'</span></span>
- <span data-ttu-id="5ce99-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="5ce99-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="5ce99-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="5ce99-105">'Razor'</span></span>
- <span data-ttu-id="5ce99-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="5ce99-106">'SignalR' uid:</span></span> 

---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="5ce99-107">在 ASP.NET Core 中使用變更權杖來偵測變更</span><span class="sxs-lookup"><span data-stu-id="5ce99-107">Detect changes with change tokens in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5ce99-108">「變更權杖」\*\* 是用來追蹤狀態變更的一般用途低階建置組塊。</span><span class="sxs-lookup"><span data-stu-id="5ce99-108">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="5ce99-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="5ce99-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="5ce99-110">IChangeToken 介面</span><span class="sxs-lookup"><span data-stu-id="5ce99-110">IChangeToken interface</span></span>

<span data-ttu-id="5ce99-111"><xref:Microsoft.Extensions.Primitives.IChangeToken> 會傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="5ce99-111"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="5ce99-112">`IChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。</span><span class="sxs-lookup"><span data-stu-id="5ce99-112">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="5ce99-113">會以隱含方式為 ASP.NET Core 應用程式提供了[Microsoft Extensions 原型](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="5ce99-113">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="5ce99-114">`IChangeToken` 有兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="5ce99-114">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="5ce99-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> 指出權杖是否主動引發回呼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="5ce99-116">如果 `ActiveChangedCallbacks` 設定為 `false`，則絕不會呼叫回呼，而且應用程式必須輪詢 `HasChanged` 是否有變更。</span><span class="sxs-lookup"><span data-stu-id="5ce99-116">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="5ce99-117">如果未發生任何變更，或基礎變更接聽程式已遭處置或停用，權杖也可能永遠不會被取消。</span><span class="sxs-lookup"><span data-stu-id="5ce99-117">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="5ce99-118"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> 會接收指出是否已發生變更的值。</span><span class="sxs-lookup"><span data-stu-id="5ce99-118"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="5ce99-119">`IChangeToken`介面包含[RegisterChangeCallback （Action \<Object> ，Object）](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*)方法，它會註冊在權杖變更時所叫用的回呼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-119">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="5ce99-120">`HasChanged` 必須在叫用回呼之前設定。</span><span class="sxs-lookup"><span data-stu-id="5ce99-120">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="5ce99-121">ChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="5ce99-121">ChangeToken class</span></span>

<span data-ttu-id="5ce99-122"><xref:Microsoft.Extensions.Primitives.ChangeToken> 靜態類別用來傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="5ce99-122"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="5ce99-123">`ChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。</span><span class="sxs-lookup"><span data-stu-id="5ce99-123">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="5ce99-124">會以隱含方式為 ASP.NET Core 應用程式提供了[Microsoft Extensions 原型](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="5ce99-124">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="5ce99-125">[ChangeToken （Func \<IChangeToken> ，Action）](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*)方法會在 `Action` 權杖變更時，註冊以進行呼叫：</span><span class="sxs-lookup"><span data-stu-id="5ce99-125">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="5ce99-126">`Func<IChangeToken>` 會產生權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-126">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="5ce99-127">在權杖變更時呼叫 `Action`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-127">`Action` is called when the token changes.</span></span>

<span data-ttu-id="5ce99-128">[ChangeToken \<TState> （Func \<IChangeToken> ，Action \<TState> ，TState）](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*)多載會採用 `TState` 傳入權杖取用者的額外參數 `Action` 。</span><span class="sxs-lookup"><span data-stu-id="5ce99-128">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="5ce99-129">`OnChange` 會傳回 <xref:System.IDisposable>。</span><span class="sxs-lookup"><span data-stu-id="5ce99-129">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="5ce99-130">呼叫 <xref:System.IDisposable.Dispose*> 將阻止權杖接聽其他變更和釋出權杖的資源。</span><span class="sxs-lookup"><span data-stu-id="5ce99-130">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="5ce99-131">ASP.NET Core 中變更權杖的範例用法</span><span class="sxs-lookup"><span data-stu-id="5ce99-131">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="5ce99-132">變更權杖用於 ASP.NET Core 監視物件變更的重要區域：</span><span class="sxs-lookup"><span data-stu-id="5ce99-132">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="5ce99-133">針對監視檔案變更，<xref:Microsoft.Extensions.FileProviders.IFileProvider> 的 <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> 方法會針對要監看的指定檔案或資料夾建立 `IChangeToken`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-133">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="5ce99-134">`IChangeToken` 權杖可新增至快取項目，以在變更時觸發快取收回。</span><span class="sxs-lookup"><span data-stu-id="5ce99-134">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="5ce99-135">針對 `TOptions` 變更，<xref:Microsoft.Extensions.Options.IOptionsMonitor`1> 的預設 <xref:Microsoft.Extensions.Options.OptionsMonitor`1> 實作具有會接受一或多個 <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> 執行個體的多載。</span><span class="sxs-lookup"><span data-stu-id="5ce99-135">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="5ce99-136">每個執行個體會傳回 `IChangeToken`，以登錄變更通知回呼來追蹤選項變更。</span><span class="sxs-lookup"><span data-stu-id="5ce99-136">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="5ce99-137">監視設定變更</span><span class="sxs-lookup"><span data-stu-id="5ce99-137">Monitor for configuration changes</span></span>

<span data-ttu-id="5ce99-138">ASP.NET Core 範本預設使用 [JSON 組態檔](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*、*appsettings.Development.json* 和 *appsettings.Production.json*) 來載入應用程式組態設定。</span><span class="sxs-lookup"><span data-stu-id="5ce99-138">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="5ce99-139">這些檔案是在接受 `reloadOnChange` 參數的 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 擴充方法上使用 [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) 擴充方法來設定的。</span><span class="sxs-lookup"><span data-stu-id="5ce99-139">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="5ce99-140">`reloadOnChange` 指出組態是否應該在檔案變更時重新載入。</span><span class="sxs-lookup"><span data-stu-id="5ce99-140">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="5ce99-141">此設定會出在 <xref:Microsoft.Extensions.Hosting.Host> 便利方法 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 中：</span><span class="sxs-lookup"><span data-stu-id="5ce99-141">This setting appears in the <xref:Microsoft.Extensions.Hosting.Host> convenience method <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="5ce99-142">檔案型設定是以 <xref:Microsoft.Extensions.Configuration.FileConfigurationSource> 代表的。</span><span class="sxs-lookup"><span data-stu-id="5ce99-142">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="5ce99-143">`FileConfigurationSource` 使用 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 來監視檔案。</span><span class="sxs-lookup"><span data-stu-id="5ce99-143">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="5ce99-144">根據預設，`IFileMonitor` 由 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供，它會使用 <xref:System.IO.FileSystemWatcher> 來監視設定檔變更。</span><span class="sxs-lookup"><span data-stu-id="5ce99-144">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="5ce99-145">範例應用程式將示範監視組態變更的兩個實作。</span><span class="sxs-lookup"><span data-stu-id="5ce99-145">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="5ce99-146">如果任一 *appsettings* 檔案變更，兩個檔案監視實作都會執行自訂程式碼&mdash;範例應用程式會將訊息寫入到主控台。</span><span class="sxs-lookup"><span data-stu-id="5ce99-146">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="5ce99-147">組態檔的 `FileSystemWatcher` 可以觸發單一組態檔案變更的多個權杖回呼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-147">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="5ce99-148">為確定自訂程式碼只在觸發多個權杖回呼時執行一次，範例的實作會檢查檔案雜湊。</span><span class="sxs-lookup"><span data-stu-id="5ce99-148">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="5ce99-149">範例使用 SHA1 檔案雜湊處理。</span><span class="sxs-lookup"><span data-stu-id="5ce99-149">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="5ce99-150">重試將利用指數倒退來實作。</span><span class="sxs-lookup"><span data-stu-id="5ce99-150">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="5ce99-151">因為可能發生檔案鎖定而暫時無法對檔案計算新的雜湊，所以會進行重試。</span><span class="sxs-lookup"><span data-stu-id="5ce99-151">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="5ce99-152">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-152">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="5ce99-153">簡易啟動變更權杖</span><span class="sxs-lookup"><span data-stu-id="5ce99-153">Simple startup change token</span></span>

<span data-ttu-id="5ce99-154">將變更通知的權杖取用者 `Action` 回呼登錄到設定重新載入權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-154">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="5ce99-155">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="5ce99-155">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="5ce99-156">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-156">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="5ce99-157">回呼是 `InvokeChanged` 方法：</span><span class="sxs-lookup"><span data-stu-id="5ce99-157">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="5ce99-158">回呼的 `state` 是用來傳入 `IWebHostEnvironment`，它在指定要監視的 *appsettings* 設定檔時非常實用 (例如，開發環境中的 *appsettings.Development.json*)。</span><span class="sxs-lookup"><span data-stu-id="5ce99-158">The `state` of the callback is used to pass in the `IWebHostEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="5ce99-159">檔案雜湊用來防止 `WriteConsole` 陳述式在組態檔只變更過一次時，由於多個權杖回呼而執行多次。</span><span class="sxs-lookup"><span data-stu-id="5ce99-159">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="5ce99-160">只要應用程式在執行中，此系統就會執行，而且使用者無法停用此系統。</span><span class="sxs-lookup"><span data-stu-id="5ce99-160">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="5ce99-161">以服務方式監視設定變更</span><span class="sxs-lookup"><span data-stu-id="5ce99-161">Monitor configuration changes as a service</span></span>

<span data-ttu-id="5ce99-162">此範例將實作：</span><span class="sxs-lookup"><span data-stu-id="5ce99-162">The sample implements:</span></span>

* <span data-ttu-id="5ce99-163">基本啟動權杖監視。</span><span class="sxs-lookup"><span data-stu-id="5ce99-163">Basic startup token monitoring.</span></span>
* <span data-ttu-id="5ce99-164">以服務方式監視。</span><span class="sxs-lookup"><span data-stu-id="5ce99-164">Monitoring as a service.</span></span>
* <span data-ttu-id="5ce99-165">啟用和停用監視的機制。</span><span class="sxs-lookup"><span data-stu-id="5ce99-165">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="5ce99-166">範例會建立 `IConfigurationMonitor` 介面。</span><span class="sxs-lookup"><span data-stu-id="5ce99-166">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="5ce99-167">*Extensions/ConfigurationMonitor.cs*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-167">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="5ce99-168">已實作類別的建構函式 `ConfigurationMonitor` 登錄變更通知的回呼：</span><span class="sxs-lookup"><span data-stu-id="5ce99-168">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="5ce99-169">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-169">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="5ce99-170">`InvokeChanged` 是回呼方法。</span><span class="sxs-lookup"><span data-stu-id="5ce99-170">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="5ce99-171">此執行個體中的 `state` 是用來存取監視狀態的 `IConfigurationMonitor` 執行個體參考。</span><span class="sxs-lookup"><span data-stu-id="5ce99-171">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="5ce99-172">會使用兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="5ce99-172">Two properties are used:</span></span>

* <span data-ttu-id="5ce99-173">`MonitoringEnabled`：指出回呼是否應執行其自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-173">`MonitoringEnabled`: Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="5ce99-174">`CurrentState`：描述目前在 UI 中使用的監視狀態。</span><span class="sxs-lookup"><span data-stu-id="5ce99-174">`CurrentState`: Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="5ce99-175">`InvokeChanged` 方法類似於之前的方法，不同之處在於：</span><span class="sxs-lookup"><span data-stu-id="5ce99-175">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="5ce99-176">不會執行其程式碼，除非 `MonitoringEnabled` 是 `true`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-176">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="5ce99-177">在其 `WriteConsole` 輸出中輸出目前的 `state`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-177">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="5ce99-178">執行個體 `ConfigurationMonitor` 會在 `Startup.ConfigureServices` 中登錄為服務：</span><span class="sxs-lookup"><span data-stu-id="5ce99-178">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="5ce99-179">[索引] 頁面提供對組態監視的使用者控制。</span><span class="sxs-lookup"><span data-stu-id="5ce99-179">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="5ce99-180">`IConfigurationMonitor` 的執行個體會插入到 `IndexModel` 中。</span><span class="sxs-lookup"><span data-stu-id="5ce99-180">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="5ce99-181">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-181">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="5ce99-182">設定監視器 (`_monitor`) 是用來啟用或停用監視並設定 UI 回饋的目前狀態：</span><span class="sxs-lookup"><span data-stu-id="5ce99-182">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="5ce99-183">當觸發 `OnPostStartMonitoring` 時，會啟用監視並清除目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="5ce99-183">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="5ce99-184">當觸發 `OnPostStopMonitoring` 時，則會停用監視，且狀態會設定為反映未進行監視。</span><span class="sxs-lookup"><span data-stu-id="5ce99-184">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="5ce99-185">UI 啟用和停用監視中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="5ce99-185">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="5ce99-186">*Pages/Index. cshtml*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-186">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="5ce99-187">監視快取的檔案變更</span><span class="sxs-lookup"><span data-stu-id="5ce99-187">Monitor cached file changes</span></span>

<span data-ttu-id="5ce99-188">檔案內容可以使用 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 在記憶體內部快取。</span><span class="sxs-lookup"><span data-stu-id="5ce99-188">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="5ce99-189">記憶體內部快取將在[記憶體內部快取](xref:performance/caching/memory)主題中描述。</span><span class="sxs-lookup"><span data-stu-id="5ce99-189">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="5ce99-190">若不採取額外的步驟 (例如下述實作)，當來源資料變更時，將會從快取傳回「過時」\*\*(過期) 資料。</span><span class="sxs-lookup"><span data-stu-id="5ce99-190">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="5ce99-191">例如，更新[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)時，若不考慮快取原始程式檔的狀態，將導致過時的快取檔案資料。</span><span class="sxs-lookup"><span data-stu-id="5ce99-191">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="5ce99-192">資料的每個要求都會更新滑動期限，但檔案永遠不會重新載入至快取。</span><span class="sxs-lookup"><span data-stu-id="5ce99-192">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="5ce99-193">使用檔案快取內容的任何應用程式功能都可能會收到過時的內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-193">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="5ce99-194">在檔案快取案例中使用變更權杖可防止快取中出現過時的檔案內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-194">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="5ce99-195">範例應用程式將示範該方法的實作。</span><span class="sxs-lookup"><span data-stu-id="5ce99-195">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="5ce99-196">此範例會使用 `GetFileContent` 來：</span><span class="sxs-lookup"><span data-stu-id="5ce99-196">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="5ce99-197">傳回檔案內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-197">Return file content.</span></span>
* <span data-ttu-id="5ce99-198">實作使用指數倒退的重試演算法，以涵蓋檔案鎖定暫時阻止讀取檔案的情況。</span><span class="sxs-lookup"><span data-stu-id="5ce99-198">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="5ce99-199">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-199">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="5ce99-200">`FileService` 是建立來處理快取的檔案查閱。</span><span class="sxs-lookup"><span data-stu-id="5ce99-200">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="5ce99-201">服務的 `GetFileContent` 方法呼叫會嘗試從記憶體內部快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。</span><span class="sxs-lookup"><span data-stu-id="5ce99-201">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="5ce99-202">如果使用快取索引鍵找不到快取的內容，則會採取下列動作：</span><span class="sxs-lookup"><span data-stu-id="5ce99-202">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="5ce99-203">使用 `GetFileContent` 取得檔案內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-203">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="5ce99-204">使用 [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 從檔案提供者取得變更權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-204">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="5ce99-205">修改檔案時，就會觸發權杖的回呼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-205">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="5ce99-206">使用[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)快取檔案內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-206">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="5ce99-207">變更權杖附有 [MemoryCacheEntryExtensions.AddExpirationToke](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*)，可在快取的檔案變更時收回快取項目。</span><span class="sxs-lookup"><span data-stu-id="5ce99-207">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="5ce99-208">在下列範例中，檔案會儲存在應用程式的[內容根目錄](xref:fundamentals/index#content-root)中。</span><span class="sxs-lookup"><span data-stu-id="5ce99-208">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="5ce99-209">`IWebHostEnvironment.ContentRootFileProvider`是用來取得 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 指向應用程式的 `IWebHostEnvironment.ContentRootPath` 。</span><span class="sxs-lookup"><span data-stu-id="5ce99-209">`IWebHostEnvironment.ContentRootFileProvider` is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's `IWebHostEnvironment.ContentRootPath`.</span></span> <span data-ttu-id="5ce99-210">`filePath` 是使用 [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath) 取得的。</span><span class="sxs-lookup"><span data-stu-id="5ce99-210">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="5ce99-211">`FileService` 是在服務容器以及記憶體快取服務中登錄的。</span><span class="sxs-lookup"><span data-stu-id="5ce99-211">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="5ce99-212">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="5ce99-212">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="5ce99-213">頁面模型會使用服務載入檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-213">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="5ce99-214">在索引頁面的 `OnGet` 方法 (*Pages/Index.cshtml.cs*) 中：</span><span class="sxs-lookup"><span data-stu-id="5ce99-214">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="5ce99-215">CompositeChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="5ce99-215">CompositeChangeToken class</span></span>

<span data-ttu-id="5ce99-216">為了表示單一物件中的一或多個 `IChangeToken` 執行個體，請使用 <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> 類別。</span><span class="sxs-lookup"><span data-stu-id="5ce99-216">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="5ce99-217">如果任何表示的權杖 `HasChanged` 是 `true`，複合權杖上的 `HasChanged` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-217">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="5ce99-218">如果任何表示的權杖 `ActiveChangeCallbacks` 是 `true`，複合權杖上的 `ActiveChangeCallbacks` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-218">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="5ce99-219">如果發生多個同時變更事件，會叫用複合變更回呼一次。</span><span class="sxs-lookup"><span data-stu-id="5ce99-219">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5ce99-220">「變更權杖」\*\* 是用來追蹤狀態變更的一般用途低階建置組塊。</span><span class="sxs-lookup"><span data-stu-id="5ce99-220">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="5ce99-221">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="5ce99-221">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="5ce99-222">IChangeToken 介面</span><span class="sxs-lookup"><span data-stu-id="5ce99-222">IChangeToken interface</span></span>

<span data-ttu-id="5ce99-223"><xref:Microsoft.Extensions.Primitives.IChangeToken> 會傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="5ce99-223"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="5ce99-224">`IChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。</span><span class="sxs-lookup"><span data-stu-id="5ce99-224">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="5ce99-225">如需不使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)的應用程式，請建立 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="5ce99-225">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="5ce99-226">`IChangeToken` 有兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="5ce99-226">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="5ce99-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> 指出權杖是否主動引發回呼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="5ce99-228">如果 `ActiveChangedCallbacks` 設定為 `false`，則絕不會呼叫回呼，而且應用程式必須輪詢 `HasChanged` 是否有變更。</span><span class="sxs-lookup"><span data-stu-id="5ce99-228">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="5ce99-229">如果未發生任何變更，或基礎變更接聽程式已遭處置或停用，權杖也可能永遠不會被取消。</span><span class="sxs-lookup"><span data-stu-id="5ce99-229">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="5ce99-230"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> 會接收指出是否已發生變更的值。</span><span class="sxs-lookup"><span data-stu-id="5ce99-230"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="5ce99-231">`IChangeToken`介面包含[RegisterChangeCallback （Action \<Object> ，Object）](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*)方法，它會註冊在權杖變更時所叫用的回呼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-231">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="5ce99-232">`HasChanged` 必須在叫用回呼之前設定。</span><span class="sxs-lookup"><span data-stu-id="5ce99-232">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="5ce99-233">ChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="5ce99-233">ChangeToken class</span></span>

<span data-ttu-id="5ce99-234"><xref:Microsoft.Extensions.Primitives.ChangeToken> 靜態類別用來傳播已發生變更的通知。</span><span class="sxs-lookup"><span data-stu-id="5ce99-234"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="5ce99-235">`ChangeToken` 位於 <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> 命名空間內。</span><span class="sxs-lookup"><span data-stu-id="5ce99-235">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="5ce99-236">如需不使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)的應用程式，請建立 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="5ce99-236">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="5ce99-237">[ChangeToken （Func \<IChangeToken> ，Action）](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*)方法會在 `Action` 權杖變更時，註冊以進行呼叫：</span><span class="sxs-lookup"><span data-stu-id="5ce99-237">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="5ce99-238">`Func<IChangeToken>` 會產生權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-238">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="5ce99-239">在權杖變更時呼叫 `Action`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-239">`Action` is called when the token changes.</span></span>

<span data-ttu-id="5ce99-240">[ChangeToken \<TState> （Func \<IChangeToken> ，Action \<TState> ，TState）](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*)多載會採用 `TState` 傳入權杖取用者的額外參數 `Action` 。</span><span class="sxs-lookup"><span data-stu-id="5ce99-240">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="5ce99-241">`OnChange` 會傳回 <xref:System.IDisposable>。</span><span class="sxs-lookup"><span data-stu-id="5ce99-241">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="5ce99-242">呼叫 <xref:System.IDisposable.Dispose*> 將阻止權杖接聽其他變更和釋出權杖的資源。</span><span class="sxs-lookup"><span data-stu-id="5ce99-242">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="5ce99-243">ASP.NET Core 中變更權杖的範例用法</span><span class="sxs-lookup"><span data-stu-id="5ce99-243">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="5ce99-244">變更權杖用於 ASP.NET Core 監視物件變更的重要區域：</span><span class="sxs-lookup"><span data-stu-id="5ce99-244">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="5ce99-245">針對監視檔案變更，<xref:Microsoft.Extensions.FileProviders.IFileProvider> 的 <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> 方法會針對要監看的指定檔案或資料夾建立 `IChangeToken`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-245">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="5ce99-246">`IChangeToken` 權杖可新增至快取項目，以在變更時觸發快取收回。</span><span class="sxs-lookup"><span data-stu-id="5ce99-246">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="5ce99-247">針對 `TOptions` 變更，<xref:Microsoft.Extensions.Options.IOptionsMonitor`1> 的預設 <xref:Microsoft.Extensions.Options.OptionsMonitor`1> 實作具有會接受一或多個 <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> 執行個體的多載。</span><span class="sxs-lookup"><span data-stu-id="5ce99-247">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="5ce99-248">每個執行個體會傳回 `IChangeToken`，以登錄變更通知回呼來追蹤選項變更。</span><span class="sxs-lookup"><span data-stu-id="5ce99-248">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="5ce99-249">監視設定變更</span><span class="sxs-lookup"><span data-stu-id="5ce99-249">Monitor for configuration changes</span></span>

<span data-ttu-id="5ce99-250">ASP.NET Core 範本預設使用 [JSON 組態檔](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*、*appsettings.Development.json* 和 *appsettings.Production.json*) 來載入應用程式組態設定。</span><span class="sxs-lookup"><span data-stu-id="5ce99-250">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="5ce99-251">這些檔案是在接受 `reloadOnChange` 參數的 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 擴充方法上使用 [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) 擴充方法來設定的。</span><span class="sxs-lookup"><span data-stu-id="5ce99-251">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="5ce99-252">`reloadOnChange` 指出組態是否應該在檔案變更時重新載入。</span><span class="sxs-lookup"><span data-stu-id="5ce99-252">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="5ce99-253">此設定會出在 <xref:Microsoft.AspNetCore.WebHost> 便利方法 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 中：</span><span class="sxs-lookup"><span data-stu-id="5ce99-253">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="5ce99-254">檔案型設定是以 <xref:Microsoft.Extensions.Configuration.FileConfigurationSource> 代表的。</span><span class="sxs-lookup"><span data-stu-id="5ce99-254">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="5ce99-255">`FileConfigurationSource` 使用 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 來監視檔案。</span><span class="sxs-lookup"><span data-stu-id="5ce99-255">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="5ce99-256">根據預設，`IFileMonitor` 由 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供，它會使用 <xref:System.IO.FileSystemWatcher> 來監視設定檔變更。</span><span class="sxs-lookup"><span data-stu-id="5ce99-256">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="5ce99-257">範例應用程式將示範監視組態變更的兩個實作。</span><span class="sxs-lookup"><span data-stu-id="5ce99-257">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="5ce99-258">如果任一 *appsettings* 檔案變更，兩個檔案監視實作都會執行自訂程式碼&mdash;範例應用程式會將訊息寫入到主控台。</span><span class="sxs-lookup"><span data-stu-id="5ce99-258">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="5ce99-259">組態檔的 `FileSystemWatcher` 可以觸發單一組態檔案變更的多個權杖回呼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-259">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="5ce99-260">為確定自訂程式碼只在觸發多個權杖回呼時執行一次，範例的實作會檢查檔案雜湊。</span><span class="sxs-lookup"><span data-stu-id="5ce99-260">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="5ce99-261">範例使用 SHA1 檔案雜湊處理。</span><span class="sxs-lookup"><span data-stu-id="5ce99-261">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="5ce99-262">重試將利用指數倒退來實作。</span><span class="sxs-lookup"><span data-stu-id="5ce99-262">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="5ce99-263">因為可能發生檔案鎖定而暫時無法對檔案計算新的雜湊，所以會進行重試。</span><span class="sxs-lookup"><span data-stu-id="5ce99-263">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="5ce99-264">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-264">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="5ce99-265">簡易啟動變更權杖</span><span class="sxs-lookup"><span data-stu-id="5ce99-265">Simple startup change token</span></span>

<span data-ttu-id="5ce99-266">將變更通知的權杖取用者 `Action` 回呼登錄到設定重新載入權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-266">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="5ce99-267">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="5ce99-267">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="5ce99-268">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-268">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="5ce99-269">回呼是 `InvokeChanged` 方法：</span><span class="sxs-lookup"><span data-stu-id="5ce99-269">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="5ce99-270">回呼的 `state` 是用來傳入 `IHostingEnvironment`，它在指定要監視的 *appsettings* 設定檔時非常實用 (例如，開發環境中的 *appsettings.Development.json*)。</span><span class="sxs-lookup"><span data-stu-id="5ce99-270">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="5ce99-271">檔案雜湊用來防止 `WriteConsole` 陳述式在組態檔只變更過一次時，由於多個權杖回呼而執行多次。</span><span class="sxs-lookup"><span data-stu-id="5ce99-271">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="5ce99-272">只要應用程式在執行中，此系統就會執行，而且使用者無法停用此系統。</span><span class="sxs-lookup"><span data-stu-id="5ce99-272">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="5ce99-273">以服務方式監視設定變更</span><span class="sxs-lookup"><span data-stu-id="5ce99-273">Monitor configuration changes as a service</span></span>

<span data-ttu-id="5ce99-274">此範例將實作：</span><span class="sxs-lookup"><span data-stu-id="5ce99-274">The sample implements:</span></span>

* <span data-ttu-id="5ce99-275">基本啟動權杖監視。</span><span class="sxs-lookup"><span data-stu-id="5ce99-275">Basic startup token monitoring.</span></span>
* <span data-ttu-id="5ce99-276">以服務方式監視。</span><span class="sxs-lookup"><span data-stu-id="5ce99-276">Monitoring as a service.</span></span>
* <span data-ttu-id="5ce99-277">啟用和停用監視的機制。</span><span class="sxs-lookup"><span data-stu-id="5ce99-277">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="5ce99-278">範例會建立 `IConfigurationMonitor` 介面。</span><span class="sxs-lookup"><span data-stu-id="5ce99-278">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="5ce99-279">*Extensions/ConfigurationMonitor.cs*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-279">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="5ce99-280">已實作類別的建構函式 `ConfigurationMonitor` 登錄變更通知的回呼：</span><span class="sxs-lookup"><span data-stu-id="5ce99-280">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="5ce99-281">`config.GetReloadToken()` 提供此權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-281">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="5ce99-282">`InvokeChanged` 是回呼方法。</span><span class="sxs-lookup"><span data-stu-id="5ce99-282">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="5ce99-283">此執行個體中的 `state` 是用來存取監視狀態的 `IConfigurationMonitor` 執行個體參考。</span><span class="sxs-lookup"><span data-stu-id="5ce99-283">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="5ce99-284">會使用兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="5ce99-284">Two properties are used:</span></span>

* <span data-ttu-id="5ce99-285">`MonitoringEnabled`：指出回呼是否應執行其自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-285">`MonitoringEnabled`: Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="5ce99-286">`CurrentState`：描述目前在 UI 中使用的監視狀態。</span><span class="sxs-lookup"><span data-stu-id="5ce99-286">`CurrentState`: Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="5ce99-287">`InvokeChanged` 方法類似於之前的方法，不同之處在於：</span><span class="sxs-lookup"><span data-stu-id="5ce99-287">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="5ce99-288">不會執行其程式碼，除非 `MonitoringEnabled` 是 `true`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-288">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="5ce99-289">在其 `WriteConsole` 輸出中輸出目前的 `state`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-289">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="5ce99-290">執行個體 `ConfigurationMonitor` 會在 `Startup.ConfigureServices` 中登錄為服務：</span><span class="sxs-lookup"><span data-stu-id="5ce99-290">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="5ce99-291">[索引] 頁面提供對組態監視的使用者控制。</span><span class="sxs-lookup"><span data-stu-id="5ce99-291">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="5ce99-292">`IConfigurationMonitor` 的執行個體會插入到 `IndexModel` 中。</span><span class="sxs-lookup"><span data-stu-id="5ce99-292">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="5ce99-293">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-293">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="5ce99-294">設定監視器 (`_monitor`) 是用來啟用或停用監視並設定 UI 回饋的目前狀態：</span><span class="sxs-lookup"><span data-stu-id="5ce99-294">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="5ce99-295">當觸發 `OnPostStartMonitoring` 時，會啟用監視並清除目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="5ce99-295">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="5ce99-296">當觸發 `OnPostStopMonitoring` 時，則會停用監視，且狀態會設定為反映未進行監視。</span><span class="sxs-lookup"><span data-stu-id="5ce99-296">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="5ce99-297">UI 啟用和停用監視中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="5ce99-297">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="5ce99-298">*Pages/Index. cshtml*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-298">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="5ce99-299">監視快取的檔案變更</span><span class="sxs-lookup"><span data-stu-id="5ce99-299">Monitor cached file changes</span></span>

<span data-ttu-id="5ce99-300">檔案內容可以使用 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 在記憶體內部快取。</span><span class="sxs-lookup"><span data-stu-id="5ce99-300">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="5ce99-301">記憶體內部快取將在[記憶體內部快取](xref:performance/caching/memory)主題中描述。</span><span class="sxs-lookup"><span data-stu-id="5ce99-301">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="5ce99-302">若不採取額外的步驟 (例如下述實作)，當來源資料變更時，將會從快取傳回「過時」\*\*(過期) 資料。</span><span class="sxs-lookup"><span data-stu-id="5ce99-302">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="5ce99-303">例如，更新[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)時，若不考慮快取原始程式檔的狀態，將導致過時的快取檔案資料。</span><span class="sxs-lookup"><span data-stu-id="5ce99-303">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="5ce99-304">資料的每個要求都會更新滑動期限，但檔案永遠不會重新載入至快取。</span><span class="sxs-lookup"><span data-stu-id="5ce99-304">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="5ce99-305">使用檔案快取內容的任何應用程式功能都可能會收到過時的內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-305">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="5ce99-306">在檔案快取案例中使用變更權杖可防止快取中出現過時的檔案內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-306">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="5ce99-307">範例應用程式將示範該方法的實作。</span><span class="sxs-lookup"><span data-stu-id="5ce99-307">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="5ce99-308">此範例會使用 `GetFileContent` 來：</span><span class="sxs-lookup"><span data-stu-id="5ce99-308">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="5ce99-309">傳回檔案內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-309">Return file content.</span></span>
* <span data-ttu-id="5ce99-310">實作使用指數倒退的重試演算法，以涵蓋檔案鎖定暫時阻止讀取檔案的情況。</span><span class="sxs-lookup"><span data-stu-id="5ce99-310">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="5ce99-311">*Utilities/Utilities.cs*：</span><span class="sxs-lookup"><span data-stu-id="5ce99-311">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="5ce99-312">`FileService` 是建立來處理快取的檔案查閱。</span><span class="sxs-lookup"><span data-stu-id="5ce99-312">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="5ce99-313">服務的 `GetFileContent` 方法呼叫會嘗試從記憶體內部快取取得檔案內容，並將它傳回給呼叫端 (*Services/FileService.cs*)。</span><span class="sxs-lookup"><span data-stu-id="5ce99-313">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="5ce99-314">如果使用快取索引鍵找不到快取的內容，則會採取下列動作：</span><span class="sxs-lookup"><span data-stu-id="5ce99-314">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="5ce99-315">使用 `GetFileContent` 取得檔案內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-315">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="5ce99-316">使用 [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 從檔案提供者取得變更權杖。</span><span class="sxs-lookup"><span data-stu-id="5ce99-316">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="5ce99-317">修改檔案時，就會觸發權杖的回呼。</span><span class="sxs-lookup"><span data-stu-id="5ce99-317">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="5ce99-318">使用[滑動期限](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration)快取檔案內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-318">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="5ce99-319">變更權杖附有 [MemoryCacheEntryExtensions.AddExpirationToke](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*)，可在快取的檔案變更時收回快取項目。</span><span class="sxs-lookup"><span data-stu-id="5ce99-319">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="5ce99-320">在下列範例中，檔案會儲存在應用程式的[內容根目錄](xref:fundamentals/index#content-root)中。</span><span class="sxs-lookup"><span data-stu-id="5ce99-320">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="5ce99-321">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) 是用來取得指向應用程式 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath> 的 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="5ce99-321">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="5ce99-322">`filePath` 是使用 [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath) 取得的。</span><span class="sxs-lookup"><span data-stu-id="5ce99-322">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="5ce99-323">`FileService` 是在服務容器以及記憶體快取服務中登錄的。</span><span class="sxs-lookup"><span data-stu-id="5ce99-323">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="5ce99-324">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="5ce99-324">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="5ce99-325">頁面模型會使用服務載入檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="5ce99-325">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="5ce99-326">在索引頁面的 `OnGet` 方法 (*Pages/Index.cshtml.cs*) 中：</span><span class="sxs-lookup"><span data-stu-id="5ce99-326">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="5ce99-327">CompositeChangeToken 類別</span><span class="sxs-lookup"><span data-stu-id="5ce99-327">CompositeChangeToken class</span></span>

<span data-ttu-id="5ce99-328">為了表示單一物件中的一或多個 `IChangeToken` 執行個體，請使用 <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> 類別。</span><span class="sxs-lookup"><span data-stu-id="5ce99-328">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="5ce99-329">如果任何表示的權杖 `HasChanged` 是 `true`，複合權杖上的 `HasChanged` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-329">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="5ce99-330">如果任何表示的權杖 `ActiveChangeCallbacks` 是 `true`，複合權杖上的 `ActiveChangeCallbacks` 會報告 `true`。</span><span class="sxs-lookup"><span data-stu-id="5ce99-330">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="5ce99-331">如果發生多個同時變更事件，會叫用複合變更回呼一次。</span><span class="sxs-lookup"><span data-stu-id="5ce99-331">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5ce99-332">其他資源</span><span class="sxs-lookup"><span data-stu-id="5ce99-332">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
