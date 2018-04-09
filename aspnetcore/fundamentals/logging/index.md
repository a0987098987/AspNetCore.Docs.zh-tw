---
title: ASP.NET Core 中的記錄
author: ardalis
description: 了解 ASP.NET Core 中的記錄架構。 探索內建記錄提供者，並深入了解熱門協力廠商提供者。
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: 4cb2cf5b22ed9f5b84638b5f8c4b07d99a17ce1c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="df73b-104">ASP.NET Core 中的記錄</span><span class="sxs-lookup"><span data-stu-id="df73b-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="df73b-105">作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="df73b-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="df73b-106">ASP.NET Core 支援可搭配各種記錄提供者的記錄 API。</span><span class="sxs-lookup"><span data-stu-id="df73b-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="df73b-107">內建提供者可讓您將記錄傳送至一或多個目的地，而且您可以外掛協力廠商記錄架構。</span><span class="sxs-lookup"><span data-stu-id="df73b-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="df73b-108">本文說明如何在程式碼中使用內建記錄 API 和提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="df73b-110">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="df73b-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="df73b-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="df73b-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="df73b-113">如何建立記錄</span><span class="sxs-lookup"><span data-stu-id="df73b-113">How to create logs</span></span>

<span data-ttu-id="df73b-114">若要建立記錄，請取得[相依性插入](xref:fundamentals/dependency-injection)容器中的 `ILogger` 物件：</span><span class="sxs-lookup"><span data-stu-id="df73b-114">To create logs, get an `ILogger` object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="df73b-115">然後在該記錄器物件上呼叫記錄方法：</span><span class="sxs-lookup"><span data-stu-id="df73b-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="df73b-116">此範例會建立以 `TodoController` 為「類別」的記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="df73b-117">[本文稍後](#log-category)將說明類別。</span><span class="sxs-lookup"><span data-stu-id="df73b-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="df73b-118">ASP.NET Core 不會提供非同步記錄器方法，因為記錄應該相當快速，因此不值得使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="df73b-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="df73b-119">如果發生與其相反的情況，請考慮變更您的記錄方式。</span><span class="sxs-lookup"><span data-stu-id="df73b-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="df73b-120">如果您的資料存放區很慢，請先將記錄訊息寫入至快速存放區，稍後再移至慢速存放區。</span><span class="sxs-lookup"><span data-stu-id="df73b-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="df73b-121">例如，記錄至訊息佇列以供其他處理序讀取及保存至慢速儲存體。</span><span class="sxs-lookup"><span data-stu-id="df73b-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="df73b-122">如何新增提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-122">How to add providers</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="df73b-124">記錄提供者會擷取您使用 `ILogger` 物件建立的訊息，並加以顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="df73b-124">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="df73b-125">例如，主控台提供者可在主控台中顯示訊息，而 Azure App Service 提供者則可將其儲存在 Azure Blob 儲存體中。</span><span class="sxs-lookup"><span data-stu-id="df73b-125">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="df73b-126">若要使用提供者，請在 *Program.cs* 中呼叫提供者的 `Add<ProviderName>` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="df73b-126">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="df73b-127">預設專案範本可使用 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) 方法來記錄：</span><span class="sxs-lookup"><span data-stu-id="df73b-127">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="df73b-129">記錄提供者會擷取您使用 `ILogger` 物件建立的訊息，並加以顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="df73b-129">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="df73b-130">例如，主控台提供者可在主控台中顯示訊息，而 Azure App Service 提供者則可將其儲存在 Azure Blob 儲存體中。</span><span class="sxs-lookup"><span data-stu-id="df73b-130">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="df73b-131">若要使用提供者，請安裝其 NuGet 套件，並在 `ILoggerFactory` 的執行個體上呼叫該提供者的擴充方法，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="df73b-131">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="df73b-132">ASP.NET Core [相依性插入](xref:fundamentals/dependency-injection) (DI) 提供 `ILoggerFactory` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="df73b-132">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="df73b-133">`AddConsole` 和 `AddDebug` 擴充方法定義於 [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) 和 [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) 套件中。</span><span class="sxs-lookup"><span data-stu-id="df73b-133">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="df73b-134">每個擴充方法會呼叫 `ILoggerFactory.AddProvider` 方法，並傳入提供者的執行個體。</span><span class="sxs-lookup"><span data-stu-id="df73b-134">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="df73b-135">本文的範例應用程式會在 `Startup` 類別的 `Configure` 方法中新增記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-135">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="df73b-136">如果您想要從先前稍早執行的程式碼取得記錄輸出，請改為在 `Startup` 類別建構函式中新增記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-136">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

* * *
<span data-ttu-id="df73b-137">您會在本文稍後找到每個[內建記錄提供者](#built-in-logging-providers)的相關資訊，以及[協力廠商記錄提供者](#third-party-logging-providers)的連結。</span><span class="sxs-lookup"><span data-stu-id="df73b-137">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="df73b-138">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="df73b-138">Sample logging output</span></span>

<span data-ttu-id="df73b-139">當您從命令列執行上一節中所示的範例程式碼時，您會在主控台中看到記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-139">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="df73b-140">主控台輸出範例如下：</span><span class="sxs-lookup"><span data-stu-id="df73b-140">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

<span data-ttu-id="df73b-141">這些記錄是透過前往 `http://localhost:5000/api/todo/0` 來建立，這會觸發上一節中所示之兩個 `ILogger` 呼叫的執行。</span><span class="sxs-lookup"><span data-stu-id="df73b-141">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="df73b-142">以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：</span><span class="sxs-lookup"><span data-stu-id="df73b-142">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="df73b-143">這些透過上一節中所示的 `ILogger` 呼叫建立的記錄是以 "TodoApi.Controllers.TodoController" 為開頭。</span><span class="sxs-lookup"><span data-stu-id="df73b-143">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="df73b-144">開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="df73b-144">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="df73b-145">ASP.NET Core 本身和您的應用程式程式碼會使用相同的記錄 API 和相同的記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-145">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="df73b-146">本文的其餘部分將說明記錄的一些詳細資料和選項。</span><span class="sxs-lookup"><span data-stu-id="df73b-146">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="df73b-147">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="df73b-147">NuGet packages</span></span>

<span data-ttu-id="df73b-148">`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。</span><span class="sxs-lookup"><span data-stu-id="df73b-148">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="df73b-149">記錄類別</span><span class="sxs-lookup"><span data-stu-id="df73b-149">Log category</span></span>

<span data-ttu-id="df73b-150">您建立的每個記錄都會包含一個「類別」。</span><span class="sxs-lookup"><span data-stu-id="df73b-150">A *category* is included with each log that you create.</span></span> <span data-ttu-id="df73b-151">當您建立 `ILogger` 物件時會指定類別。</span><span class="sxs-lookup"><span data-stu-id="df73b-151">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="df73b-152">類別可以是任何字串，但通常會使用寫入記錄之來源類別的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="df73b-152">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="df73b-153">例如："TodoApi.Controllers.TodoController"。</span><span class="sxs-lookup"><span data-stu-id="df73b-153">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="df73b-154">您可以將類別指定為字串，或使用擴充方法從類型衍生類別。</span><span class="sxs-lookup"><span data-stu-id="df73b-154">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="df73b-155">若要將類別指定為字串，請在 `ILoggerFactory` 執行個體上呼叫 `CreateLogger`，如下所示。</span><span class="sxs-lookup"><span data-stu-id="df73b-155">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="df73b-156">在大多時候，使用 `ILogger<T>` 會更容易，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="df73b-156">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="df73b-157">這相當於使用 `T` 的完整類型名稱呼叫 `CreateLogger`。</span><span class="sxs-lookup"><span data-stu-id="df73b-157">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="df73b-158">記錄層級</span><span class="sxs-lookup"><span data-stu-id="df73b-158">Log level</span></span>

<span data-ttu-id="df73b-159">每次寫入記錄，您都會指定其 [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel)。</span><span class="sxs-lookup"><span data-stu-id="df73b-159">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="df73b-160">記錄層級表示嚴重或重要程度。</span><span class="sxs-lookup"><span data-stu-id="df73b-160">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="df73b-161">例如，您可以在某個方法正常結束時寫入 `Information` 記錄，在某個方法傳回 404 傳回碼時寫入 `Warning` 記錄，並在攔截到未預期的例外狀況時寫入 `Error` 記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-161">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="df73b-162">在下列程式碼範例中，方法的名稱 (例如 `LogWarning`) 會指定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="df73b-162">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="df73b-163">第一個參數是[記錄事件識別碼](#log-event-id)。</span><span class="sxs-lookup"><span data-stu-id="df73b-163">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="df73b-164">第二個參數是[訊息範本](#log-message-template)，其中的預留位置會置入其餘方法參數所提供的引數值。</span><span class="sxs-lookup"><span data-stu-id="df73b-164">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="df73b-165">本文稍後將更詳細說明方法參數。</span><span class="sxs-lookup"><span data-stu-id="df73b-165">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="df73b-166">在方法名稱中包含層級的記錄方法是 [ILogger 的擴充方法](/dotnet/api/microsoft.extensions.logging.loggerextensions)。</span><span class="sxs-lookup"><span data-stu-id="df73b-166">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="df73b-167">這些方法會在幕後呼叫接受 `LogLevel` 參數的 `Log` 方法。</span><span class="sxs-lookup"><span data-stu-id="df73b-167">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="df73b-168">您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。</span><span class="sxs-lookup"><span data-stu-id="df73b-168">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="df73b-169">如需詳細資訊，請參閱 [ILogger Interface](/dotnet/api/microsoft.extensions.logging.ilogger) (ILogger 介面) 和[記錄器延伸模組的原始程式碼](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="df73b-169">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="df73b-170">ASP.NET Core 定義下列[記錄層級](/dotnet/api/microsoft.extensions.logging.loglevel)，並從最低嚴重性排列到最高嚴重性。</span><span class="sxs-lookup"><span data-stu-id="df73b-170">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="df73b-171">追蹤 = 0</span><span class="sxs-lookup"><span data-stu-id="df73b-171">Trace = 0</span></span>

  <span data-ttu-id="df73b-172">只對偵錯問題的開發人員重要的資訊。</span><span class="sxs-lookup"><span data-stu-id="df73b-172">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="df73b-173">這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="df73b-173">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="df73b-174">預設為停用。</span><span class="sxs-lookup"><span data-stu-id="df73b-174">*Disabled by default.*</span></span> <span data-ttu-id="df73b-175">範例：`Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="df73b-175">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="df73b-176">偵錯 = 1</span><span class="sxs-lookup"><span data-stu-id="df73b-176">Debug = 1</span></span>

  <span data-ttu-id="df73b-177">在開發和偵錯期間短暫有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="df73b-177">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="df73b-178">範例：`Entering method Configure with flag set to true.`由於 `Debug` 層級記錄的數量很大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用此記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-178">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="df73b-179">資訊 = 2</span><span class="sxs-lookup"><span data-stu-id="df73b-179">Information = 2</span></span>

  <span data-ttu-id="df73b-180">用於追蹤應用程式的一般流程。</span><span class="sxs-lookup"><span data-stu-id="df73b-180">For tracking the general flow of the application.</span></span> <span data-ttu-id="df73b-181">這些記錄通常有一些長期值。</span><span class="sxs-lookup"><span data-stu-id="df73b-181">These logs typically have some long-term value.</span></span> <span data-ttu-id="df73b-182">範例：`Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="df73b-182">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="df73b-183">警告 = 3</span><span class="sxs-lookup"><span data-stu-id="df73b-183">Warning = 3</span></span>

  <span data-ttu-id="df73b-184">在應用程式流程中發生異常或意外事件。</span><span class="sxs-lookup"><span data-stu-id="df73b-184">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="df73b-185">這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。</span><span class="sxs-lookup"><span data-stu-id="df73b-185">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="df73b-186">已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。</span><span class="sxs-lookup"><span data-stu-id="df73b-186">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="df73b-187">範例：`FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="df73b-187">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="df73b-188">錯誤 = 4</span><span class="sxs-lookup"><span data-stu-id="df73b-188">Error = 4</span></span>

  <span data-ttu-id="df73b-189">發生無法處理的錯誤和例外狀況。</span><span class="sxs-lookup"><span data-stu-id="df73b-189">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="df73b-190">這些訊息會指出目前的活動或作業 (例如目前的 HTTP 要求） 中的失敗，而不是整個應用程式的失敗。</span><span class="sxs-lookup"><span data-stu-id="df73b-190">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="df73b-191">範例記錄訊息：`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="df73b-191">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="df73b-192">重大 = 5</span><span class="sxs-lookup"><span data-stu-id="df73b-192">Critical = 5</span></span>

  <span data-ttu-id="df73b-193">發生需要立即注意的失敗。</span><span class="sxs-lookup"><span data-stu-id="df73b-193">For failures that require immediate attention.</span></span> <span data-ttu-id="df73b-194">範例：資料遺失情況、磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="df73b-194">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="df73b-195">您可以使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。</span><span class="sxs-lookup"><span data-stu-id="df73b-195">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="df73b-196">例如，您可能想要在生產環境中，將 `Information` 和較低層級的所有記錄移至磁碟區資料存放區，並將 `Warning` 和更高層級的所有記錄移至數值資料存放區。</span><span class="sxs-lookup"><span data-stu-id="df73b-196">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="df73b-197">在開發期間，您可以如往常般將 `Warning` 或更高嚴重性的記錄傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="df73b-197">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="df73b-198">然後當您需要進行疑難排解時，您可以新增 `Debug` 層級。</span><span class="sxs-lookup"><span data-stu-id="df73b-198">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="df73b-199">本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="df73b-199">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="df73b-200">ASP.NET Core 架構會寫入架構事件的 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-200">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="df73b-201">本文稍早的記錄範例已排除 `Information` 層級以下的記錄，因此不會顯示任何 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-201">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="df73b-202">如果您執行的範例應用程式已設定為顯示主控台提供者的 `Debug` 和更高層級記錄，其主控台記錄範例如下。</span><span class="sxs-lookup"><span data-stu-id="df73b-202">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="df73b-203">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="df73b-203">Log event ID</span></span>

<span data-ttu-id="df73b-204">每次寫入記錄，您都會指定一個「事件識別碼」。</span><span class="sxs-lookup"><span data-stu-id="df73b-204">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="df73b-205">範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行這項作業：</span><span class="sxs-lookup"><span data-stu-id="df73b-205">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="df73b-206">事件識別碼是您可用來將兩組記錄的事件建立關聯的整數值。</span><span class="sxs-lookup"><span data-stu-id="df73b-206">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="df73b-207">例如，將某個項目新增至購物車的記錄可以是事件識別碼 1000，而完成購買的記錄可以是事件識別碼 1001。</span><span class="sxs-lookup"><span data-stu-id="df73b-207">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="df73b-208">在記錄輸出中，視提供者而定，事件識別碼可能會儲存在欄位中或包含在文字訊息中。</span><span class="sxs-lookup"><span data-stu-id="df73b-208">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="df73b-209">偵錯提供者不會顯示事件識別碼，但主控台提供者會在類別後面的括弧中顯示事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="df73b-209">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="df73b-210">記錄訊息範本</span><span class="sxs-lookup"><span data-stu-id="df73b-210">Log message template</span></span>

<span data-ttu-id="df73b-211">每次寫入記錄訊息，您都會提供一個訊息範本。</span><span class="sxs-lookup"><span data-stu-id="df73b-211">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="df73b-212">訊息範本可以是字串，也可以包含要置入引數值的具名預留位置。</span><span class="sxs-lookup"><span data-stu-id="df73b-212">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="df73b-213">範本不是格式字串，而且預留位置必須命名而不是編號。</span><span class="sxs-lookup"><span data-stu-id="df73b-213">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="df73b-214">預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。</span><span class="sxs-lookup"><span data-stu-id="df73b-214">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="df73b-215">如果您有下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="df73b-215">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="df73b-216">產生的記錄訊息看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="df73b-216">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="df73b-217">記錄架構會以此方式來格式化訊息，以便記錄提供者能夠實作[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="df73b-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="df73b-218">由於引數本身會傳遞給記錄系統，而不只是格式化的訊息範本，因此除了訊息範本以外，記錄提供者還可以將參數值儲存為欄位。</span><span class="sxs-lookup"><span data-stu-id="df73b-218">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="df73b-219">如果您要將記錄輸出導向至 Azure 資料表儲存體，您的記錄器方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="df73b-219">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="df73b-220">每個 Azure 資料表實體可以有 `ID` 和 `RequestTime` 屬性，以簡化記錄資料的查詢。</span><span class="sxs-lookup"><span data-stu-id="df73b-220">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="df73b-221">您可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要剖析文字訊息中的時間。</span><span class="sxs-lookup"><span data-stu-id="df73b-221">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="df73b-222">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="df73b-222">Logging exceptions</span></span>

<span data-ttu-id="df73b-223">記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="df73b-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="df73b-224">不同提供者處理例外狀況資訊的方式會不同。</span><span class="sxs-lookup"><span data-stu-id="df73b-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="df73b-225">以下是來自上述程式碼中的偵錯提供者輸出範例。</span><span class="sxs-lookup"><span data-stu-id="df73b-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="df73b-226">記錄篩選</span><span class="sxs-lookup"><span data-stu-id="df73b-226">Log filtering</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="df73b-228">您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="df73b-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="df73b-229">最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="df73b-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="df73b-230">如果您想要隱藏所有記錄，您可以指定 `LogLevel.None` 作為最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="df73b-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="df73b-231">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="df73b-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="df73b-232">**在組態中建立篩選規則**</span><span class="sxs-lookup"><span data-stu-id="df73b-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="df73b-233">這些專案範本會建立程式碼，以呼叫 `CreateDefaultBuilder` 設定主控台和偵錯提供者的記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="df73b-234">`CreateDefaultBuilder` 方法也會使用如下所示的程式碼，將記錄設定為尋找 `Logging` 區段中的組態：</span><span class="sxs-lookup"><span data-stu-id="df73b-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="df73b-235">組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="df73b-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="df73b-236">此 JSON 會建立六項篩選規則，一項適用於偵錯提供者、四項適用於主控台提供者，還有一項適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="df73b-237">您稍後會看到在建立 `ILogger` 物件時，如何為每個提供者只選擇其中一項規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="df73b-238">**程式碼中的篩選規則**</span><span class="sxs-lookup"><span data-stu-id="df73b-238">**Filter rules in code**</span></span>

<span data-ttu-id="df73b-239">您可以在程式碼中註冊篩選規則，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="df73b-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="df73b-240">第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="df73b-241">第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="df73b-242">**如何套用篩選規則**</span><span class="sxs-lookup"><span data-stu-id="df73b-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="df73b-243">組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="df73b-244">前六項來自組態範例，最後兩項來自程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="df73b-244">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="df73b-245">number</span><span class="sxs-lookup"><span data-stu-id="df73b-245">Number</span></span> | <span data-ttu-id="df73b-246">提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-246">Provider</span></span>      | <span data-ttu-id="df73b-247">開頭如下的類別...</span><span class="sxs-lookup"><span data-stu-id="df73b-247">Categories that begin with ...</span></span>          | <span data-ttu-id="df73b-248">最低記錄層級</span><span class="sxs-lookup"><span data-stu-id="df73b-248">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="df73b-249">1</span><span class="sxs-lookup"><span data-stu-id="df73b-249">1</span></span>      | <span data-ttu-id="df73b-250">偵錯</span><span class="sxs-lookup"><span data-stu-id="df73b-250">Debug</span></span>         | <span data-ttu-id="df73b-251">所有類別</span><span class="sxs-lookup"><span data-stu-id="df73b-251">All categories</span></span>                          | <span data-ttu-id="df73b-252">資訊</span><span class="sxs-lookup"><span data-stu-id="df73b-252">Information</span></span>       |
| <span data-ttu-id="df73b-253">2</span><span class="sxs-lookup"><span data-stu-id="df73b-253">2</span></span>      | <span data-ttu-id="df73b-254">主控台</span><span class="sxs-lookup"><span data-stu-id="df73b-254">Console</span></span>       | <span data-ttu-id="df73b-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="df73b-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="df73b-256">警告</span><span class="sxs-lookup"><span data-stu-id="df73b-256">Warning</span></span>           |
| <span data-ttu-id="df73b-257">3</span><span class="sxs-lookup"><span data-stu-id="df73b-257">3</span></span>      | <span data-ttu-id="df73b-258">主控台</span><span class="sxs-lookup"><span data-stu-id="df73b-258">Console</span></span>       | <span data-ttu-id="df73b-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="df73b-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="df73b-260">偵錯</span><span class="sxs-lookup"><span data-stu-id="df73b-260">Debug</span></span>             |
| <span data-ttu-id="df73b-261">4</span><span class="sxs-lookup"><span data-stu-id="df73b-261">4</span></span>      | <span data-ttu-id="df73b-262">主控台</span><span class="sxs-lookup"><span data-stu-id="df73b-262">Console</span></span>       | <span data-ttu-id="df73b-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="df73b-263">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="df73b-264">錯誤</span><span class="sxs-lookup"><span data-stu-id="df73b-264">Error</span></span>             |
| <span data-ttu-id="df73b-265">5</span><span class="sxs-lookup"><span data-stu-id="df73b-265">5</span></span>      | <span data-ttu-id="df73b-266">主控台</span><span class="sxs-lookup"><span data-stu-id="df73b-266">Console</span></span>       | <span data-ttu-id="df73b-267">所有類別</span><span class="sxs-lookup"><span data-stu-id="df73b-267">All categories</span></span>                          | <span data-ttu-id="df73b-268">資訊</span><span class="sxs-lookup"><span data-stu-id="df73b-268">Information</span></span>       |
| <span data-ttu-id="df73b-269">6</span><span class="sxs-lookup"><span data-stu-id="df73b-269">6</span></span>      | <span data-ttu-id="df73b-270">所有提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-270">All providers</span></span> | <span data-ttu-id="df73b-271">所有類別</span><span class="sxs-lookup"><span data-stu-id="df73b-271">All categories</span></span>                          | <span data-ttu-id="df73b-272">偵錯</span><span class="sxs-lookup"><span data-stu-id="df73b-272">Debug</span></span>             |
| <span data-ttu-id="df73b-273">7</span><span class="sxs-lookup"><span data-stu-id="df73b-273">7</span></span>      | <span data-ttu-id="df73b-274">所有提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-274">All providers</span></span> | <span data-ttu-id="df73b-275">系統</span><span class="sxs-lookup"><span data-stu-id="df73b-275">System</span></span>                                  | <span data-ttu-id="df73b-276">偵錯</span><span class="sxs-lookup"><span data-stu-id="df73b-276">Debug</span></span>             |
| <span data-ttu-id="df73b-277">8</span><span class="sxs-lookup"><span data-stu-id="df73b-277">8</span></span>      | <span data-ttu-id="df73b-278">偵錯</span><span class="sxs-lookup"><span data-stu-id="df73b-278">Debug</span></span>         | <span data-ttu-id="df73b-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="df73b-279">Microsoft</span></span>                               | <span data-ttu-id="df73b-280">追蹤</span><span class="sxs-lookup"><span data-stu-id="df73b-280">Trace</span></span>             |

<span data-ttu-id="df73b-281">當您建立要用來寫入記錄的 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一項規則來套用至該記錄器。</span><span class="sxs-lookup"><span data-stu-id="df73b-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="df73b-282">由該 `ILogger` 寫入的所有訊息都會根據選取的規則進行篩選。</span><span class="sxs-lookup"><span data-stu-id="df73b-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="df73b-283">系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="df73b-284">當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：</span><span class="sxs-lookup"><span data-stu-id="df73b-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="df73b-285">選取所有符合提供者或其別名的規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-285">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="df73b-286">如果找不到，請選擇提供者為空白的所有規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="df73b-287">從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="df73b-288">如果找不到，請選取未指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="df73b-289">如果選取了多項規則，請使用**最後**一項。</span><span class="sxs-lookup"><span data-stu-id="df73b-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="df73b-290">如果未選取任何規則，請使用 `MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="df73b-290">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="df73b-291">例如，假設您有上述規則清單，並建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：</span><span class="sxs-lookup"><span data-stu-id="df73b-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="df73b-292">針對偵錯提供者，規則 1、6 和 8 均適用。</span><span class="sxs-lookup"><span data-stu-id="df73b-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="df73b-293">規則 8 最明確，因此會選取此規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="df73b-294">針對主控台提供者，規則 3、4、5 和 6 均適用。</span><span class="sxs-lookup"><span data-stu-id="df73b-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="df73b-295">規則 3 最明確。</span><span class="sxs-lookup"><span data-stu-id="df73b-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="df73b-296">當您使用 `ILogger` 建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的記錄時，`Trace` 和更高層級的記錄會移至偵錯提供者，而 `Debug` 和更高層級的記錄則會移至主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="df73b-297">**提供者別名**</span><span class="sxs-lookup"><span data-stu-id="df73b-297">**Provider aliases**</span></span>

<span data-ttu-id="df73b-298">您可以在組態中使用類型名稱來指定提供者，但每個提供者定義了方便您使用的更短「別名」。</span><span class="sxs-lookup"><span data-stu-id="df73b-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="df73b-299">針對內建提供者，請使用下列別名：</span><span class="sxs-lookup"><span data-stu-id="df73b-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="df73b-300">主控台</span><span class="sxs-lookup"><span data-stu-id="df73b-300">Console</span></span>
- <span data-ttu-id="df73b-301">偵錯</span><span class="sxs-lookup"><span data-stu-id="df73b-301">Debug</span></span>
- <span data-ttu-id="df73b-302">EventLog</span><span class="sxs-lookup"><span data-stu-id="df73b-302">EventLog</span></span>
- <span data-ttu-id="df73b-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="df73b-303">AzureAppServices</span></span>
- <span data-ttu-id="df73b-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="df73b-304">TraceSource</span></span>
- <span data-ttu-id="df73b-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="df73b-305">EventSource</span></span>

<span data-ttu-id="df73b-306">**預設最低層級**</span><span class="sxs-lookup"><span data-stu-id="df73b-306">**Default minimum level**</span></span>

<span data-ttu-id="df73b-307">只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。</span><span class="sxs-lookup"><span data-stu-id="df73b-307">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="df73b-308">下列範例示範如何設定最低層級：</span><span class="sxs-lookup"><span data-stu-id="df73b-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="df73b-309">如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-309">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="df73b-310">**篩選函式**</span><span class="sxs-lookup"><span data-stu-id="df73b-310">**Filter functions**</span></span>

<span data-ttu-id="df73b-311">您可以在篩選函式中撰寫程式碼來套用篩選規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="df73b-312">針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。</span><span class="sxs-lookup"><span data-stu-id="df73b-312">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="df73b-313">函式中的程式碼可存取提供者類型、類別和記錄層級，以判斷是否應該記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="df73b-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="df73b-314">例如: </span><span class="sxs-lookup"><span data-stu-id="df73b-314">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="df73b-316">某些記錄提供者可讓您指定何時應該將記錄寫入儲存媒體，何時應該根據記錄層級和類別加以略過。</span><span class="sxs-lookup"><span data-stu-id="df73b-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="df73b-317">`AddConsole` 和 `AddDebug` 擴充方法提供多載，可讓您傳入篩選條件。</span><span class="sxs-lookup"><span data-stu-id="df73b-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="df73b-318">下列範例程式碼會使主控台提供者略過 `Warning` 層級以下的記錄，而偵錯提供者則會略過架構所建立的記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="df73b-319">`AddEventLog` 方法具有接受 `EventLogSettings` 執行個體的多載，其 `Filter` 屬性中可能包含篩選函式。</span><span class="sxs-lookup"><span data-stu-id="df73b-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="df73b-320">TraceSource 提供者未提供上述任何多載，因為其記錄層級和其他參數會根據其所使用的 `SourceSwitch` 和 `TraceListener`。</span><span class="sxs-lookup"><span data-stu-id="df73b-320">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="df73b-321">您可以使用 `WithFilter` 擴充方法，為 `ILoggerFactory` 執行個體中註冊的所有提供者設定篩選規則。</span><span class="sxs-lookup"><span data-stu-id="df73b-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="df73b-322">下列範例會將架構記錄 (開頭為 "Microsoft" 或 "System" 的類別) 限制為警告，同時讓應用程式在偵錯層級記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="df73b-323">如果您想要使用篩選來防止所有記錄針對特定類別寫入，您可以指定 `LogLevel.None` 作為該類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="df73b-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="df73b-324">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="df73b-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="df73b-325">`WithFilter` 擴充方法是由 [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet 套件提供。</span><span class="sxs-lookup"><span data-stu-id="df73b-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="df73b-326">此方法會傳回新的 `ILoggerFactory` 執行個體，這會篩選傳遞至用它來註冊之所有記錄器提供者的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="df73b-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="df73b-327">它不會影響任何其他 `ILoggerFactory` 執行個體，包括原始 `ILoggerFactory` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="df73b-327">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

* * *
## <a name="log-scopes"></a><span data-ttu-id="df73b-328">記錄範圍</span><span class="sxs-lookup"><span data-stu-id="df73b-328">Log scopes</span></span>

<span data-ttu-id="df73b-329">您可以將某個「範圍」內的一組邏輯作業群組在一起，以便將相同的資料附加至作為該集合一部分建立的每個記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="df73b-330">例如，您可能想要將每個記錄建立為處理交易的一部分，以包含交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="df73b-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="df73b-331">範圍是 `ILogger.BeginScope<TState>` 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。</span><span class="sxs-lookup"><span data-stu-id="df73b-331">A scope is an `IDisposable` type that's returned by the `ILogger.BeginScope<TState>` method and lasts until it's disposed.</span></span> <span data-ttu-id="df73b-332">您可以透過將記錄器呼叫包裝在 `using` 區塊中來使用範圍，如下所示：</span><span class="sxs-lookup"><span data-stu-id="df73b-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="df73b-333">下列程式碼會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="df73b-333">The following code enables scopes for the console provider:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="df73b-335">在 *Program.cs* 中：</span><span class="sxs-lookup"><span data-stu-id="df73b-335">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="df73b-336">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="df73b-336">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="df73b-337">ASP.NET Core 2.1 版將可使用 *appsettings* 組態檔來設定 `IncludeScopes`。</span><span class="sxs-lookup"><span data-stu-id="df73b-337">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-338">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-338">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="df73b-339">在 *Startup.cs* 中：</span><span class="sxs-lookup"><span data-stu-id="df73b-339">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

* * *
<span data-ttu-id="df73b-340">每個記錄訊息包含範圍資訊：</span><span class="sxs-lookup"><span data-stu-id="df73b-340">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="df73b-341">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-341">Built-in logging providers</span></span>

<span data-ttu-id="df73b-342">ASP.NET Core 隨附下列提供者：</span><span class="sxs-lookup"><span data-stu-id="df73b-342">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="df73b-343">Console</span><span class="sxs-lookup"><span data-stu-id="df73b-343">Console</span></span>](#console)
* [<span data-ttu-id="df73b-344">偵錯</span><span class="sxs-lookup"><span data-stu-id="df73b-344">Debug</span></span>](#debug)
* [<span data-ttu-id="df73b-345">EventSource</span><span class="sxs-lookup"><span data-stu-id="df73b-345">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="df73b-346">EventLog</span><span class="sxs-lookup"><span data-stu-id="df73b-346">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="df73b-347">TraceSource</span><span class="sxs-lookup"><span data-stu-id="df73b-347">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="df73b-348">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="df73b-348">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="df73b-349">主控台提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-349">The console provider</span></span>

<span data-ttu-id="df73b-350">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="df73b-350">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-351">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-351">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
```csharp
logging.AddConsole()
```

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="df73b-353">[AddConsole 多載](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions)可讓您傳入最低記錄層級、篩選函式，以及指出是否支援範圍的布林值。</span><span class="sxs-lookup"><span data-stu-id="df73b-353">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="df73b-354">另一個選項是傳入可指定範圍支援和記錄層級的 `IConfiguration` 物件。</span><span class="sxs-lookup"><span data-stu-id="df73b-354">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="df73b-355">如果您考慮在生產環境中使用主控台提供者，請注意它對效能有重大影響。</span><span class="sxs-lookup"><span data-stu-id="df73b-355">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="df73b-356">當您在 Visual Studio 中建立新的專案時，`AddConsole` 方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="df73b-356">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="df73b-357">此程式碼會參考 *appSettings.json* 檔案的 `Logging` 區段：</span><span class="sxs-lookup"><span data-stu-id="df73b-357">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="df73b-358">上述設定會將架構記錄限制為警告，同時讓應用程式在偵錯層級記錄，如[記錄篩選](#log-filtering)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="df73b-358">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="df73b-359">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="df73b-359">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

* * *
<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="df73b-360">偵錯提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-360">The Debug provider</span></span>

<span data-ttu-id="df73b-361">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="df73b-361">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="df73b-362">在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。</span><span class="sxs-lookup"><span data-stu-id="df73b-362">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="df73b-365">[AddDebug 多載](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions)可讓您傳入最低記錄層級或篩選函式。</span><span class="sxs-lookup"><span data-stu-id="df73b-365">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="df73b-366">EventSource 提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-366">The EventSource provider</span></span>

<span data-ttu-id="df73b-367">針對以 ASP.NET Core 1.1.0 或更高版本為目標的應用程式，[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) 提供者套件可以實作事件追蹤。</span><span class="sxs-lookup"><span data-stu-id="df73b-367">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="df73b-368">在 Windows 上，它會使用 [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="df73b-368">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="df73b-369">提供者可以跨平台，但目前沒有適用於 Linux 或 macOS 的事件收集和顯示工具。</span><span class="sxs-lookup"><span data-stu-id="df73b-369">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="df73b-372">收集及檢視記錄的一個好方法是使用 [PerfView 公用程式](https://www.microsoft.com/download/details.aspx?id=28567)。</span><span class="sxs-lookup"><span data-stu-id="df73b-372">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="df73b-373">此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET 所發出之 ETW 事件的最佳體驗。</span><span class="sxs-lookup"><span data-stu-id="df73b-373">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="df73b-374">若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者] 清單</span><span class="sxs-lookup"><span data-stu-id="df73b-374">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="df73b-375">(請勿遺漏字串開頭的星號)。</span><span class="sxs-lookup"><span data-stu-id="df73b-375">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

<span data-ttu-id="df73b-377">在 Nano 伺服器上擷取事件需要一些額外的設定：</span><span class="sxs-lookup"><span data-stu-id="df73b-377">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="df73b-378">將 PowerShell 遠端連線到 Nano 伺服器：</span><span class="sxs-lookup"><span data-stu-id="df73b-378">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="df73b-379">建立 ETW 工作階段：</span><span class="sxs-lookup"><span data-stu-id="df73b-379">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="df73b-380">視需要為 [CLR](/dotnet/framework/performance/clr-etw-providers)、ASP.NET Core 等新增 ETW 提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-380">Add ETW providers for [CLR](/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="df73b-381">ASP.NET Core 提供者 GUID 為 `3ac73b97-af73-50e9-0822-5da4367920d0`。</span><span class="sxs-lookup"><span data-stu-id="df73b-381">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="df73b-382">執行網站並進行需要追蹤資訊的任何動作。</span><span class="sxs-lookup"><span data-stu-id="df73b-382">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="df73b-383">完成時停止追蹤工作階段：</span><span class="sxs-lookup"><span data-stu-id="df73b-383">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="df73b-384">產生的 *C:\trace.etl* 檔案可以在其他版本的 Windows 上使用 PerfView 進行分析。</span><span class="sxs-lookup"><span data-stu-id="df73b-384">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="df73b-385">Windows EventLog 提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-385">The Windows EventLog provider</span></span>

<span data-ttu-id="df73b-386">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="df73b-386">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-387">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-387">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-388">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-388">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="df73b-389">[AddEventLog 多載](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions)可讓您傳入 `EventLogSettings` 或最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="df73b-389">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="df73b-390">TraceSource 提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-390">The TraceSource provider</span></span>

<span data-ttu-id="df73b-391">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) 程式庫和提供者。</span><span class="sxs-lookup"><span data-stu-id="df73b-391">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-392">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-392">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-393">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-393">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="df73b-394">[AddTraceSource 多載](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions)可讓您傳入來源參數和追蹤接聽項。</span><span class="sxs-lookup"><span data-stu-id="df73b-394">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="df73b-395">若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。</span><span class="sxs-lookup"><span data-stu-id="df73b-395">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="df73b-396">此提供者可讓您將訊息路由傳送至各種不同的[接聽項](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr)。</span><span class="sxs-lookup"><span data-stu-id="df73b-396">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="df73b-397">下列範例會設定 `TraceSource` 提供者，將 `Warning` 和更高層級訊息記錄至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="df73b-397">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="df73b-398">Azure App Service 提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-398">The Azure App Service provider</span></span>

<span data-ttu-id="df73b-399">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="df73b-399">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="df73b-400">此提供者僅適用於以 ASP.NET Core 1.1.0 或更高版本為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="df73b-400">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df73b-401">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df73b-401">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="df73b-402">若是以 .NET Core 作為目標，即無須安裝提供者套件或明確地呼叫 `AddAzureWebAppDiagnostics`。</span><span class="sxs-lookup"><span data-stu-id="df73b-402">If targeting .NET Core, you don't have to install the provider package or explicitly call `AddAzureWebAppDiagnostics`.</span></span> <span data-ttu-id="df73b-403">當您將應用程式部署至 Azure App Service 時，此提供者就會自動提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="df73b-403">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

<span data-ttu-id="df73b-404">若是以 .NET Core 作為目標，請將提供者套件新增至專案，然後叫用 `AddAzureWebAppDiagnostics`：</span><span class="sxs-lookup"><span data-stu-id="df73b-404">If targeting .NET Framework, add the provider package to your project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df73b-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df73b-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="df73b-406">`AddAzureWebAppDiagnostics` 多載可讓您傳入 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)，您可以用它來覆寫預設值，例如記錄輸出範本、Blob 名稱和檔案大小限制</span><span class="sxs-lookup"><span data-stu-id="df73b-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="df73b-407">(「輸出範本」是套用至呼叫 `ILogger` 方法時所提供記錄上方之所有記錄的訊息範本)。</span><span class="sxs-lookup"><span data-stu-id="df73b-407">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="df73b-408">當您部署至 App Service 應用程式時，您的應用程式會接受 Azure 入口網站的 [App Service] 頁面之[診斷記錄](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag)區段中的設定。</span><span class="sxs-lookup"><span data-stu-id="df73b-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="df73b-409">當您變更這些設定時，所做的變更會立即生效，而不需要您重新啟動應用程式或對其重新部署程式碼。</span><span class="sxs-lookup"><span data-stu-id="df73b-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Azure 記錄設定](index/_static/azure-logging-settings.png)

<span data-ttu-id="df73b-411">記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="df73b-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="df73b-412">預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="df73b-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="df73b-413">預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="df73b-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="df73b-414">如需預設行為的詳細資訊，請參閱 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)。</span><span class="sxs-lookup"><span data-stu-id="df73b-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="df73b-415">此提供者僅適用於您的專案在 Azure 環境中執行的情況。</span><span class="sxs-lookup"><span data-stu-id="df73b-415">The provider only works when your project runs in the Azure environment.</span></span> <span data-ttu-id="df73b-416">當您在本機執行時，不會有任何作用 &mdash; 它不會寫入本機檔案或本機開發 Blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="df73b-416">It has no effect when you run locally &mdash; it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="df73b-417">協力廠商記錄提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-417">Third-party logging providers</span></span>

<span data-ttu-id="df73b-418">以下是可搭配 ASP.NET Core 使用的一些協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="df73b-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="df73b-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - Elmah.Io 服務的提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="df73b-420">[JSNLog](http://jsnlog.com) - 在您的伺服器端記錄中記錄 JavaScript 例外狀況和其他用戶端事件。</span><span class="sxs-lookup"><span data-stu-id="df73b-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="df73b-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - Loggr 服務的提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="df73b-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - NLog 程式庫的提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="df73b-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) - Serilog 程式庫的提供者</span><span class="sxs-lookup"><span data-stu-id="df73b-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="df73b-424">某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="df73b-424">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="df73b-425">使用協力廠商架構類似於使用其中一個內建提供者：將 NuGet 套件新增至您的專案，然後在 `ILoggerFactory` 上呼叫擴充方法。</span><span class="sxs-lookup"><span data-stu-id="df73b-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="df73b-426">如需詳細資訊，請參閱每個架構的文件。</span><span class="sxs-lookup"><span data-stu-id="df73b-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="df73b-427">您也可以建立自己的自訂提供者，以支援其他記錄架構或您自己的記錄需求。</span><span class="sxs-lookup"><span data-stu-id="df73b-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="df73b-428">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="df73b-428">Azure log streaming</span></span>

<span data-ttu-id="df73b-429">Azure 記錄資料流可讓您即時檢視來自下列位置的記錄活動：</span><span class="sxs-lookup"><span data-stu-id="df73b-429">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="df73b-430">應用程式伺服器</span><span class="sxs-lookup"><span data-stu-id="df73b-430">The application server</span></span> 
* <span data-ttu-id="df73b-431">網頁伺服器</span><span class="sxs-lookup"><span data-stu-id="df73b-431">The web server</span></span>
* <span data-ttu-id="df73b-432">失敗的要求追蹤</span><span class="sxs-lookup"><span data-stu-id="df73b-432">Failed request tracing</span></span> 

<span data-ttu-id="df73b-433">若要設定 Azure 記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="df73b-433">To configure Azure log streaming:</span></span> 

* <span data-ttu-id="df73b-434">從您應用程式的入口網站頁面巡覽至 [診斷記錄]</span><span class="sxs-lookup"><span data-stu-id="df73b-434">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="df73b-435">將 [應用程式記錄 (檔案系統)] 設定為 [開啟]。</span><span class="sxs-lookup"><span data-stu-id="df73b-435">Set **Application Logging (Filesystem)** to on.</span></span> 

![Azure 入口網站的 [診斷記錄] 頁面](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="df73b-437">巡覽至 [記錄資料流] 頁面以檢視應用程式訊息。</span><span class="sxs-lookup"><span data-stu-id="df73b-437">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="df73b-438">這些是應用程式透過 `ILogger` 介面記錄的訊息。</span><span class="sxs-lookup"><span data-stu-id="df73b-438">They're logged by application through the `ILogger` interface.</span></span> 

![Azure 入口網站應用程式的 [記錄資料流]](index/_static/azure-log-streaming.png)


## <a name="see-also"></a><span data-ttu-id="df73b-440">另請參閱</span><span class="sxs-lookup"><span data-stu-id="df73b-440">See also</span></span>

[<span data-ttu-id="df73b-441">使用 LoggerMessage 進行高效能記錄</span><span class="sxs-lookup"><span data-stu-id="df73b-441">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
