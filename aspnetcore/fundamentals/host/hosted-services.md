---
<span data-ttu-id="d4c87-101">標題： ASP.NET Core 作者中具有託管服務的背景工作： rick-anderson 描述：瞭解如何在 ASP.NET Core 中使用託管服務來執行背景工作。</span><span class="sxs-lookup"><span data-stu-id="d4c87-101">title: Background tasks with hosted services in ASP.NET Core author: rick-anderson description: Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>
<span data-ttu-id="d4c87-102">monikerRange： ' >= aspnetcore-2.1 ' ms-chap： riande ms. custom： mvc ms. date： 02/10/2020 no-loc：</span><span class="sxs-lookup"><span data-stu-id="d4c87-102">monikerRange: '>= aspnetcore-2.1' ms.author: riande ms.custom: mvc ms.date: 02/10/2020 no-loc:</span></span>
- <span data-ttu-id="d4c87-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d4c87-103">'Blazor'</span></span>
- <span data-ttu-id="d4c87-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d4c87-104">'Identity'</span></span>
- <span data-ttu-id="d4c87-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d4c87-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="d4c87-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d4c87-106">'Razor'</span></span>
- <span data-ttu-id="d4c87-107">' SignalR ' uid：基本/主機/託管服務</span><span class="sxs-lookup"><span data-stu-id="d4c87-107">'SignalR' uid: fundamentals/host/hosted-services</span></span>

---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="d4c87-108">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="d4c87-108">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="d4c87-109">[Jeow Li Huan](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="d4c87-109">By [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d4c87-110">在 ASP.NET Core 中，背景工作可實作為「託管服務」\*\*。</span><span class="sxs-lookup"><span data-stu-id="d4c87-110">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="d4c87-111">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="d4c87-111">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="d4c87-112">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="d4c87-112">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="d4c87-113">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="d4c87-113">Background task that runs on a timer.</span></span>
* <span data-ttu-id="d4c87-114">啟動已設定範圍之[服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="d4c87-114">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="d4c87-115">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="d4c87-115">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="d4c87-116">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="d4c87-116">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="d4c87-117">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="d4c87-117">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="d4c87-118">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="d4c87-118">Worker Service template</span></span>

<span data-ttu-id="d4c87-119">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="d4c87-119">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="d4c87-120">從背景工作角色服務範本建立的應用程式會在其專案檔中指定背景工作角色 SDK：</span><span class="sxs-lookup"><span data-stu-id="d4c87-120">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="d4c87-121">使用範本作為裝載服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="d4c87-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="d4c87-122">套件</span><span class="sxs-lookup"><span data-stu-id="d4c87-122">Package</span></span>

<span data-ttu-id="d4c87-123">以背景工作角色服務範本為基礎的應用程式會使用 `Microsoft.NET.Sdk.Worker` SDK，並具有對[Microsoft Extensions. 裝載](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)封裝的明確套件參考。</span><span class="sxs-lookup"><span data-stu-id="d4c87-123">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="d4c87-124">例如，請參閱範例應用程式的專案檔（*BackgroundTasksSample .csproj*）。</span><span class="sxs-lookup"><span data-stu-id="d4c87-124">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="d4c87-125">若是使用 SDK 的 web 應用程式 `Microsoft.NET.Sdk.Web` ，則會隱含地從共用架構參考[Microsoft Extensions. 裝載](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)套件。</span><span class="sxs-lookup"><span data-stu-id="d4c87-125">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="d4c87-126">應用程式的專案檔中不需要明確的套件參考。</span><span class="sxs-lookup"><span data-stu-id="d4c87-126">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="d4c87-127">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="d4c87-127">IHostedService interface</span></span>

<span data-ttu-id="d4c87-128"><xref:Microsoft.Extensions.Hosting.IHostedService>介面會針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="d4c87-128">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="d4c87-129">[StartAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*)： `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d4c87-129">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*): `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="d4c87-130">`StartAsync`會*在之前*呼叫：</span><span class="sxs-lookup"><span data-stu-id="d4c87-130">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="d4c87-131">應用程式的要求處理管線已設定（ `Startup.Configure` ）。</span><span class="sxs-lookup"><span data-stu-id="d4c87-131">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="d4c87-132">伺服器已啟動並[IApplicationLifetime。 ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)會觸發。</span><span class="sxs-lookup"><span data-stu-id="d4c87-132">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="d4c87-133">您可以變更預設行為，以便在 `StartAsync` 應用程式的管線已設定且呼叫之後，託管服務才會 `ApplicationStarted` 執行。</span><span class="sxs-lookup"><span data-stu-id="d4c87-133">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="d4c87-134">若要變更預設行為，請在呼叫之後新增託管服務（ `VideosWatcher` 在下列範例中） `ConfigureWebHostDefaults` ：</span><span class="sxs-lookup"><span data-stu-id="d4c87-134">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.DependencyInjection;
  using Microsoft.Extensions.Hosting;

  public class Program
  {
      public static void Main(string[] args)
      {
          CreateHostBuilder(args).Build().Run();
      }

      public static IHostBuilder CreateHostBuilder(string[] args) =>
          Host.CreateDefaultBuilder(args)
              .ConfigureWebHostDefaults(webBuilder =>
              {
                  webBuilder.UseStartup<Startup>();
              })
              .ConfigureServices(services =>
              {
                  services.AddHostedService<VideosWatcher>();
              });
  }
  ```

* <span data-ttu-id="d4c87-135">[StopAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)：當主機執行正常關機時觸發。</span><span class="sxs-lookup"><span data-stu-id="d4c87-135">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="d4c87-136">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d4c87-136">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="d4c87-137">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="d4c87-137">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="d4c87-138">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="d4c87-138">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="d4c87-139">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="d4c87-139">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="d4c87-140">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="d4c87-140">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="d4c87-141">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="d4c87-141">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="d4c87-142">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="d4c87-142">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="d4c87-143">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="d4c87-143">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="d4c87-144">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="d4c87-144">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="d4c87-145">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="d4c87-145">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="d4c87-146"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="d4c87-146"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="d4c87-147">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="d4c87-147">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="d4c87-148">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="d4c87-148">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="d4c87-149">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="d4c87-149">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="d4c87-150">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="d4c87-150">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="d4c87-151">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="d4c87-151">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice-base-class"></a><span data-ttu-id="d4c87-152">BackgroundService 基類</span><span class="sxs-lookup"><span data-stu-id="d4c87-152">BackgroundService base class</span></span>

<span data-ttu-id="d4c87-153"><xref:Microsoft.Extensions.Hosting.BackgroundService>是用來進行長時間執行的基類 <xref:Microsoft.Extensions.Hosting.IHostedService> 。</span><span class="sxs-lookup"><span data-stu-id="d4c87-153"><xref:Microsoft.Extensions.Hosting.BackgroundService> is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span>

<span data-ttu-id="d4c87-154">呼叫[ExecuteAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*)以執行背景服務。</span><span class="sxs-lookup"><span data-stu-id="d4c87-154">[ExecuteAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) is called to run the background service.</span></span> <span data-ttu-id="d4c87-155">此實作為傳回 <xref:System.Threading.Tasks.Task> ，代表背景服務的整個存留期。</span><span class="sxs-lookup"><span data-stu-id="d4c87-155">The implementation returns a <xref:System.Threading.Tasks.Task> that represents the entire lifetime of the background service.</span></span> <span data-ttu-id="d4c87-156">在[ExecuteAsync 變成非同步](https://github.com/dotnet/extensions/issues/2149)（例如呼叫）之前，不會再啟動任何進一步的服務 `await` 。</span><span class="sxs-lookup"><span data-stu-id="d4c87-156">No further services are started until [ExecuteAsync becomes asynchronous](https://github.com/dotnet/extensions/issues/2149), such as by calling `await`.</span></span> <span data-ttu-id="d4c87-157">避免執行長時間的封鎖初始化工作 `ExecuteAsync` 。</span><span class="sxs-lookup"><span data-stu-id="d4c87-157">Avoid performing long, blocking initialization work in `ExecuteAsync`.</span></span> <span data-ttu-id="d4c87-158">[StopAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*)中等候完成的主機區塊 `ExecuteAsync` 。</span><span class="sxs-lookup"><span data-stu-id="d4c87-158">The host blocks in [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) waiting for `ExecuteAsync` to complete.</span></span>

<span data-ttu-id="d4c87-159">呼叫[IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)時，會觸發解除標記。</span><span class="sxs-lookup"><span data-stu-id="d4c87-159">The cancellation token is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="d4c87-160">當解除標記引發時，您的執行 `ExecuteAsync` 應該會立即完成，以便正常地關閉服務。</span><span class="sxs-lookup"><span data-stu-id="d4c87-160">Your implementation of `ExecuteAsync` should finish promptly when the cancellation token is fired in order to gracefully shut down the service.</span></span> <span data-ttu-id="d4c87-161">否則，服務強制會在關機時間關閉。</span><span class="sxs-lookup"><span data-stu-id="d4c87-161">Otherwise, the service ungracefully shuts down at the shutdown timeout.</span></span> <span data-ttu-id="d4c87-162">如需詳細資訊，請參閱[IHostedService 介面](#ihostedservice-interface)一節。</span><span class="sxs-lookup"><span data-stu-id="d4c87-162">For more information, see the [IHostedService interface](#ihostedservice-interface) section.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="d4c87-163">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="d4c87-163">Timed background tasks</span></span>

<span data-ttu-id="d4c87-164">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="d4c87-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="d4c87-165">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="d4c87-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="d4c87-166">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="d4c87-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-17,34,41)]

<span data-ttu-id="d4c87-167"><xref:System.Threading.Timer>不會等待先前的執行 `DoWork` 完成，因此所顯示的方法可能不適用於每個案例。</span><span class="sxs-lookup"><span data-stu-id="d4c87-167">The <xref:System.Threading.Timer> doesn't wait for previous executions of `DoWork` to finish, so the approach shown might not be suitable for every scenario.</span></span> <span data-ttu-id="d4c87-168">[連鎖：遞增](xref:System.Threading.Interlocked.Increment*)是用來將執行計數器遞增為不可部分完成的作業，這可確保多個執行緒不會 `executionCount` 同時更新。</span><span class="sxs-lookup"><span data-stu-id="d4c87-168">[Interlocked.Increment](xref:System.Threading.Interlocked.Increment*) is used to increment the execution counter as an atomic operation, which ensures that multiple threads don't update `executionCount` concurrently.</span></span>

<span data-ttu-id="d4c87-169">服務會在 `IHostBuilder.ConfigureServices` （*Program.cs*）中以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="d4c87-169">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="d4c87-170">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="d4c87-170">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="d4c87-171">若要使用[BackgroundService](#backgroundservice-base-class)內的[範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立範圍。</span><span class="sxs-lookup"><span data-stu-id="d4c87-171">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a [BackgroundService](#backgroundservice-base-class), create a scope.</span></span> <span data-ttu-id="d4c87-172">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="d4c87-172">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="d4c87-173">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d4c87-173">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="d4c87-174">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="d4c87-174">In the following example:</span></span>

* <span data-ttu-id="d4c87-175">服務是非同步。</span><span class="sxs-lookup"><span data-stu-id="d4c87-175">The service is asynchronous.</span></span> <span data-ttu-id="d4c87-176">`DoWork` 方法會傳回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="d4c87-176">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="d4c87-177">基於示範目的，會在方法中等待10秒的延遲 `DoWork` 。</span><span class="sxs-lookup"><span data-stu-id="d4c87-177">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="d4c87-178"><xref:Microsoft.Extensions.Logging.ILogger>會插入服務中。</span><span class="sxs-lookup"><span data-stu-id="d4c87-178">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="d4c87-179">託管服務會建立範圍來解析已設定範圍的背景工作服務，以呼叫其 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="d4c87-179">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="d4c87-180">`DoWork`傳回 `Task` ，其等候于 `ExecuteAsync` ：</span><span class="sxs-lookup"><span data-stu-id="d4c87-180">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="d4c87-181">服務會在 `IHostBuilder.ConfigureServices` （*Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="d4c87-181">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="d4c87-182">託管服務會向 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="d4c87-182">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="d4c87-183">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="d4c87-183">Queued background tasks</span></span>

<span data-ttu-id="d4c87-184">背景工作佇列是以 .NET 4.x 為基礎 <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ：</span><span class="sxs-lookup"><span data-stu-id="d4c87-184">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*>:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="d4c87-185">在下列 `QueueHostedService` 範例中：</span><span class="sxs-lookup"><span data-stu-id="d4c87-185">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="d4c87-186">`BackgroundProcessing`方法 `Task` 會傳回，它會在中等待 `ExecuteAsync` 。</span><span class="sxs-lookup"><span data-stu-id="d4c87-186">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="d4c87-187">佇列中的背景工作會在中進行清除並執行 `BackgroundProcessing` 。</span><span class="sxs-lookup"><span data-stu-id="d4c87-187">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="d4c87-188">在服務停止之前，會等待工作專案 `StopAsync` 。</span><span class="sxs-lookup"><span data-stu-id="d4c87-188">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="d4c87-189">`MonitorLoop`每當在 `w` 輸入裝置上選取索引鍵時，服務就會處理託管服務的佇列工作：</span><span class="sxs-lookup"><span data-stu-id="d4c87-189">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="d4c87-190">`IBackgroundTaskQueue`會插入 `MonitorLoop` 服務中。</span><span class="sxs-lookup"><span data-stu-id="d4c87-190">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="d4c87-191">`IBackgroundTaskQueue.QueueBackgroundWorkItem`呼叫以將工作專案排入佇列。</span><span class="sxs-lookup"><span data-stu-id="d4c87-191">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="d4c87-192">工作專案會模擬長時間執行的背景工作：</span><span class="sxs-lookup"><span data-stu-id="d4c87-192">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="d4c87-193">會執行三個5秒的延遲（ `Task.Delay` ）。</span><span class="sxs-lookup"><span data-stu-id="d4c87-193">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="d4c87-194">`try-catch` <xref:System.OperationCanceledException> 如果工作已取消，則語句會補漏白。</span><span class="sxs-lookup"><span data-stu-id="d4c87-194">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="d4c87-195">服務會在 `IHostBuilder.ConfigureServices` （*Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="d4c87-195">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="d4c87-196">託管服務會向 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="d4c87-196">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

<span data-ttu-id="d4c87-197">`MonitorLoop`開始于 `Program.Main` ：</span><span class="sxs-lookup"><span data-stu-id="d4c87-197">`MonitorLoop` is started in `Program.Main`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d4c87-198">在 ASP.NET Core 中，背景工作可實作為「託管服務」\*\*。</span><span class="sxs-lookup"><span data-stu-id="d4c87-198">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="d4c87-199">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="d4c87-199">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="d4c87-200">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="d4c87-200">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="d4c87-201">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="d4c87-201">Background task that runs on a timer.</span></span>
* <span data-ttu-id="d4c87-202">啟動已設定範圍之[服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="d4c87-202">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="d4c87-203">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="d4c87-203">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="d4c87-204">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="d4c87-204">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="d4c87-205">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="d4c87-205">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="d4c87-206">套件</span><span class="sxs-lookup"><span data-stu-id="d4c87-206">Package</span></span>

<span data-ttu-id="d4c87-207">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="d4c87-207">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="d4c87-208">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="d4c87-208">IHostedService interface</span></span>

<span data-ttu-id="d4c87-209">託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="d4c87-209">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="d4c87-210">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="d4c87-210">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="d4c87-211">[StartAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*)： `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d4c87-211">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*): `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="d4c87-212">使用[Web 主機](xref:fundamentals/host/web-host)時， `StartAsync` 會在伺服器啟動且 IApplicationLifetime 之後呼叫[。 ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)會觸發。</span><span class="sxs-lookup"><span data-stu-id="d4c87-212">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="d4c87-213">使用[泛型主機](xref:fundamentals/host/generic-host)時， `StartAsync` 會在觸發之前呼叫 `ApplicationStarted` 。</span><span class="sxs-lookup"><span data-stu-id="d4c87-213">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="d4c87-214">[StopAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)：當主機執行正常關機時觸發。</span><span class="sxs-lookup"><span data-stu-id="d4c87-214">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="d4c87-215">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d4c87-215">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="d4c87-216">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="d4c87-216">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="d4c87-217">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="d4c87-217">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="d4c87-218">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="d4c87-218">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="d4c87-219">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="d4c87-219">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="d4c87-220">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="d4c87-220">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="d4c87-221">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="d4c87-221">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="d4c87-222">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="d4c87-222">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="d4c87-223">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="d4c87-223">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="d4c87-224">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="d4c87-224">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="d4c87-225"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="d4c87-225"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="d4c87-226">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="d4c87-226">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="d4c87-227">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="d4c87-227">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="d4c87-228">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="d4c87-228">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="d4c87-229">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="d4c87-229">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="d4c87-230">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="d4c87-230">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="d4c87-231">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="d4c87-231">Timed background tasks</span></span>

<span data-ttu-id="d4c87-232">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="d4c87-232">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="d4c87-233">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="d4c87-233">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="d4c87-234">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="d4c87-234">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="d4c87-235"><xref:System.Threading.Timer>不會等待先前的執行 `DoWork` 完成，因此所顯示的方法可能不適用於每個案例。</span><span class="sxs-lookup"><span data-stu-id="d4c87-235">The <xref:System.Threading.Timer> doesn't wait for previous executions of `DoWork` to finish, so the approach shown might not be suitable for every scenario.</span></span>

<span data-ttu-id="d4c87-236">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="d4c87-236">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="d4c87-237">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="d4c87-237">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="d4c87-238">若要在中使用[範圍服務](xref:fundamentals/dependency-injection#service-lifetimes) `IHostedService` ，請建立範圍。</span><span class="sxs-lookup"><span data-stu-id="d4c87-238">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="d4c87-239">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="d4c87-239">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="d4c87-240">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d4c87-240">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="d4c87-241">在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="d4c87-241">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="d4c87-242">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="d4c87-242">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="d4c87-243">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="d4c87-243">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d4c87-244">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="d4c87-244">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="d4c87-245">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="d4c87-245">Queued background tasks</span></span>

<span data-ttu-id="d4c87-246">背景工作佇列是以 .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：</span><span class="sxs-lookup"><span data-stu-id="d4c87-246">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="d4c87-247">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 [BackgroundService](#backgroundservice-base-class) 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="d4c87-247">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a [BackgroundService](#backgroundservice-base-class), which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="d4c87-248">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="d4c87-248">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d4c87-249">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="d4c87-249">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="d4c87-250">在索引頁面模型類別中：</span><span class="sxs-lookup"><span data-stu-id="d4c87-250">In the Index page model class:</span></span>

* <span data-ttu-id="d4c87-251">將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。</span><span class="sxs-lookup"><span data-stu-id="d4c87-251">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="d4c87-252">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。</span><span class="sxs-lookup"><span data-stu-id="d4c87-252">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="d4c87-253">處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。</span><span class="sxs-lookup"><span data-stu-id="d4c87-253">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="d4c87-254">建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="d4c87-254">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="d4c87-255">在索引頁面上選取 [新增工作]\*\*\*\* 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="d4c87-255">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="d4c87-256">`QueueBackgroundWorkItem`呼叫以將工作專案排入佇列：</span><span class="sxs-lookup"><span data-stu-id="d4c87-256">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d4c87-257">其他資源</span><span class="sxs-lookup"><span data-stu-id="d4c87-257">Additional resources</span></span>

* [<span data-ttu-id="d4c87-258">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="d4c87-258">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="d4c87-259">在 Azure App Service 中使用 Webjob 執行背景工作</span><span class="sxs-lookup"><span data-stu-id="d4c87-259">Run background tasks with WebJobs in Azure App Service</span></span>](/azure/app-service/webjobs-create)
* <xref:System.Threading.Timer>
