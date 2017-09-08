---
title: "在 ASP.NET Core 記錄"
author: ardalis
description: "導入了 ASP.NET Core 的記錄架構。 每個內建的記錄提供者和一些受歡迎的協力廠商提供者的連結，包括區段。"
keywords: "ASP.NET Core 記錄、 記錄提供者、 Microsoft.Extensions.Logging、 ILogger、 ILoggerFactory、 LogLevel、 WithFilter、 TraceSource、 事件記錄檔、 EventSource，範圍"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 15abe93d881aed3b6950a859dc9445ec50ee9bb5
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="9684d-105">若要登入 ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="9684d-105">Introduction to Logging in ASP.NET Core</span></span>

<span data-ttu-id="9684d-106">由[Steve Smith](http://ardalis.com)和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9684d-106">By [Steve Smith](http://ardalis.com) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="9684d-107">ASP.NET Core 支援的記錄 API，可搭配各種記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="9684d-108">內建提供者可讓您將記錄檔傳送至一個或多個目的地，而您可以插入的第三方記錄架構。</span><span class="sxs-lookup"><span data-stu-id="9684d-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="9684d-109">本文示範如何在程式碼中使用的內建的記錄 API 和提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[<span data-ttu-id="9684d-111">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="9684d-111">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[<span data-ttu-id="9684d-113">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="9684d-113">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a><span data-ttu-id="9684d-114">如何建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="9684d-114">How to create logs</span></span>

<span data-ttu-id="9684d-115">若要建立記錄檔，取得`ILogger`物件從[相依性插入](dependency-injection.md)容器：</span><span class="sxs-lookup"><span data-stu-id="9684d-115">To create logs, get an `ILogger` object from the [dependency injection](dependency-injection.md) container:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="9684d-116">然後呼叫該記錄器物件記錄方法：</span><span class="sxs-lookup"><span data-stu-id="9684d-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9684d-117">這個範例會建立記錄檔與`TodoController`類別做為*類別*。</span><span class="sxs-lookup"><span data-stu-id="9684d-117">This example creates logs with the `TodoController` class as the *category*.</span></span>  <span data-ttu-id="9684d-118">類別會說明[本文稍後](#log-category)。</span><span class="sxs-lookup"><span data-stu-id="9684d-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="9684d-119">ASP.NET Core 不會因為不提供非同步記錄器方法應該快速，因此並不需要使用非同步的成本，記錄。</span><span class="sxs-lookup"><span data-stu-id="9684d-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="9684d-120">如果您在其中，則不成立的情況下，請考慮變更您登入的方式。</span><span class="sxs-lookup"><span data-stu-id="9684d-120">If you're in a situation where that's not true, consider changing the way you log.</span></span>  <span data-ttu-id="9684d-121">如果您的資料存放區緩慢時，首先，快速存放區寫入記錄檔訊息，然後將它們移至低速的存放區。</span><span class="sxs-lookup"><span data-stu-id="9684d-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="9684d-122">例如，記錄至訊息佇列讀取及保存到慢的儲存體，另一個處理序。</span><span class="sxs-lookup"><span data-stu-id="9684d-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="9684d-123">如何新增提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9684d-125">記錄提供者會使用您建立的訊息`ILogger`物件，並顯示或將其儲存。</span><span class="sxs-lookup"><span data-stu-id="9684d-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="9684d-126">例如，主控台提供者會在主控台中，顯示訊息，而 Azure 應用程式服務提供者可以將它們儲存在 Azure blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="9684d-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="9684d-127">若要使用的提供者，呼叫者`Add<ProviderName>`中的擴充方法*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9684d-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="9684d-128">記錄的方式，請參閱上述的程式碼中設定的預設專案範本，但`ConfigureLogging`呼叫由`CreateDefaultBuilder`方法。</span><span class="sxs-lookup"><span data-stu-id="9684d-128">The default project template sets up logging the way you see it in the preceding code, but the `ConfigureLogging` call is done by the `CreateDefaultBuilder` method.</span></span> <span data-ttu-id="9684d-129">下列程式碼*Program.cs* ，它由專案範本：</span><span class="sxs-lookup"><span data-stu-id="9684d-129">Here's the code in *Program.cs* that is created by project templates:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9684d-131">記錄提供者會使用您建立的訊息`ILogger`物件，並顯示或將其儲存。</span><span class="sxs-lookup"><span data-stu-id="9684d-131">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="9684d-132">例如，主控台提供者會在主控台中，顯示訊息，而 Azure 應用程式服務提供者可以將它們儲存在 Azure blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="9684d-132">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="9684d-133">若要使用的提供者，安裝 NuGet 封裝和提供者的擴充方法呼叫的執行個體上`ILoggerFactory`，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="9684d-133">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="9684d-134">ASP.NET Core[相依性插入](dependency-injection.md)(DI) 提供`ILoggerFactory`執行個體。</span><span class="sxs-lookup"><span data-stu-id="9684d-134">ASP.NET Core [dependency injection](dependency-injection.md) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="9684d-135">`AddConsole`和`AddDebug`擴充方法定義在[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)和[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/)封裝。</span><span class="sxs-lookup"><span data-stu-id="9684d-135">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="9684d-136">每個擴充方法呼叫`ILoggerFactory.AddProvider`方法，傳入的提供者執行個體。</span><span class="sxs-lookup"><span data-stu-id="9684d-136">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="9684d-137">這個發行項的範例應用程式將記錄提供者中的`Configure`方法`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="9684d-137">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="9684d-138">如果您想要從先前執行的程式碼中取得記錄檔輸出，加入記錄提供者中的`Startup`類別建構函式。</span><span class="sxs-lookup"><span data-stu-id="9684d-138">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="9684d-139">您會發現每個資訊[內建的記錄提供者](#built-in-logging-providers)並連結至[協力廠商記錄提供者](#third-party-logging-providers)稍後的發行項。</span><span class="sxs-lookup"><span data-stu-id="9684d-139">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="9684d-140">記錄輸出範例</span><span class="sxs-lookup"><span data-stu-id="9684d-140">Sample logging output</span></span>

<span data-ttu-id="9684d-141">上一節中所示的範例程式碼，您會看到主控台中的記錄檔，當您從命令列執行。</span><span class="sxs-lookup"><span data-stu-id="9684d-141">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="9684d-142">主控台輸出的範例如下：</span><span class="sxs-lookup"><span data-stu-id="9684d-142">Here's an example of console output:</span></span>

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
 
<span data-ttu-id="9684d-143">這些記錄檔所建立，請前往`http://localhost:5000/api/todo/0`，此觸發程序執行兩個`ILogger`呼叫上一節中所示。</span><span class="sxs-lookup"><span data-stu-id="9684d-143">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="9684d-144">當您執行 Visual Studio 中的範例應用程式時出現在 偵錯視窗中，以下是相同的記錄檔的範例：</span><span class="sxs-lookup"><span data-stu-id="9684d-144">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="9684d-145">所建立的記錄檔`ILogger`以"TodoApi.Controllers.TodoController"開頭的呼叫上一節中所示。</span><span class="sxs-lookup"><span data-stu-id="9684d-145">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="9684d-146">開頭為"Microsoft"類別的記錄檔是從 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="9684d-146">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="9684d-147">ASP.NET Core 本身和您的應用程式程式碼會使用相同的記錄 API 以及相同的記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-147">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="9684d-148">這篇文章的其餘部分將說明一些詳細資料和記錄選項。</span><span class="sxs-lookup"><span data-stu-id="9684d-148">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="9684d-149">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="9684d-149">NuGet packages</span></span>

<span data-ttu-id="9684d-150">`ILogger`和`ILoggerFactory`介面位於[Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)，並為其預設實作是在[Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/)。</span><span class="sxs-lookup"><span data-stu-id="9684d-150">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="9684d-151">記錄檔類別</span><span class="sxs-lookup"><span data-stu-id="9684d-151">Log category</span></span>

<span data-ttu-id="9684d-152">A*類別*隨附於您所建立的每個記錄檔。</span><span class="sxs-lookup"><span data-stu-id="9684d-152">A *category* is included with each log that you create.</span></span>  <span data-ttu-id="9684d-153">您指定的類別，當您建立`ILogger`物件。</span><span class="sxs-lookup"><span data-stu-id="9684d-153">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="9684d-154">類別可能是任何字串，但慣例是使用的類別從中寫入記錄檔的完整的名稱。</span><span class="sxs-lookup"><span data-stu-id="9684d-154">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span>  <span data-ttu-id="9684d-155">例如:"TodoApi.Controllers.TodoController"。</span><span class="sxs-lookup"><span data-stu-id="9684d-155">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="9684d-156">您可以指定為字串的類別或類別衍生自類型的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="9684d-156">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="9684d-157">若要指定類別目錄做為字串，呼叫`CreateLogger`上`ILoggerFactory`執行個體，如下所示。</span><span class="sxs-lookup"><span data-stu-id="9684d-157">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="9684d-158">大部分的情況下，會很容易使用`ILogger<T>`，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="9684d-158">Most of the time, it will be easier to use  `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="9684d-159">這就相當於呼叫`CreateLogger`的完整限定的類型名稱`T`。</span><span class="sxs-lookup"><span data-stu-id="9684d-159">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="9684d-160">記錄層級</span><span class="sxs-lookup"><span data-stu-id="9684d-160">Log level</span></span>

<span data-ttu-id="9684d-161">每次您將寫入記錄檔，您指定其[LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)。</span><span class="sxs-lookup"><span data-stu-id="9684d-161">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="9684d-162">記錄層級表示嚴重性或重要性的程度。</span><span class="sxs-lookup"><span data-stu-id="9684d-162">The log level indicates the degree of severity or importance.</span></span>  <span data-ttu-id="9684d-163">例如，您可以撰寫`Information`方法通常結束時，記錄`Warning`記錄方法會傳回 404 傳回碼，和一個`Error`時攔截預期的例外狀況記錄。</span><span class="sxs-lookup"><span data-stu-id="9684d-163">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="9684d-164">下列程式碼範例，方法的名稱 (例如， `LogWarning`) 指定的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="9684d-164">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span>  <span data-ttu-id="9684d-165">第一個參數是[記錄事件識別碼](#log-event-id)（本文稍後說明）。</span><span class="sxs-lookup"><span data-stu-id="9684d-165">The first parameter is the [Log event ID](#log-event-id) (explained later in this article).</span></span>  <span data-ttu-id="9684d-166">其餘的參數來建構記錄訊息字串。</span><span class="sxs-lookup"><span data-stu-id="9684d-166">The remaining parameters construct a log message string.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9684d-167">方法名稱中包含的層級的記錄方法[的 ILogger 擴充方法](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions)。</span><span class="sxs-lookup"><span data-stu-id="9684d-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="9684d-168">在幕後呼叫這些方法`Log`採用方法`LogLevel`參數。</span><span class="sxs-lookup"><span data-stu-id="9684d-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="9684d-169">您可以呼叫`Log`直接方法，而不是其中一個這些擴充方法，但語法是相當複雜。</span><span class="sxs-lookup"><span data-stu-id="9684d-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="9684d-170">如需詳細資訊，請參閱[ILogger 介面](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)和[記錄器延伸模組的原始程式碼](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="9684d-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="9684d-171">ASP.NET Core 會定義下列[記錄層級](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)，從最低到最高嚴重性此處的順序排列。</span><span class="sxs-lookup"><span data-stu-id="9684d-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="9684d-172">追蹤 = 0</span><span class="sxs-lookup"><span data-stu-id="9684d-172">Trace = 0</span></span>

  <span data-ttu-id="9684d-173">只對問題進行偵錯的開發人員有價值的資訊。</span><span class="sxs-lookup"><span data-stu-id="9684d-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="9684d-174">這些訊息可能包含機密的應用程式資料，因此不應啟用實際執行環境中。</span><span class="sxs-lookup"><span data-stu-id="9684d-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="9684d-175">*預設為停用。*</span><span class="sxs-lookup"><span data-stu-id="9684d-175">*Disabled by default.*</span></span> <span data-ttu-id="9684d-176">範例：`Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="9684d-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="9684d-177">偵錯 = 1</span><span class="sxs-lookup"><span data-stu-id="9684d-177">Debug = 1</span></span>

  <span data-ttu-id="9684d-178">在開發和偵錯期間有短期用途的資訊。</span><span class="sxs-lookup"><span data-stu-id="9684d-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="9684d-179">範例：`Entering method Configure with flag set to true.`您通常不會讓`Debug`層級會記錄在生產環境中，除非您正在進行疑難排解，因為大量記錄檔。</span><span class="sxs-lookup"><span data-stu-id="9684d-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="9684d-180">資訊 = 2</span><span class="sxs-lookup"><span data-stu-id="9684d-180">Information = 2</span></span>

  <span data-ttu-id="9684d-181">以追蹤應用程式的一般流程。</span><span class="sxs-lookup"><span data-stu-id="9684d-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="9684d-182">這些記錄檔通常會有一些長期的值。</span><span class="sxs-lookup"><span data-stu-id="9684d-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="9684d-183">範例：`Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="9684d-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="9684d-184">警告 = 3</span><span class="sxs-lookup"><span data-stu-id="9684d-184">Warning = 3</span></span>

  <span data-ttu-id="9684d-185">適用於應用程式流程中的異常或非預期事件。</span><span class="sxs-lookup"><span data-stu-id="9684d-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="9684d-186">這些可能包含錯誤或其他條件，不會造成應用程式停止，但這可能需要進行調查。</span><span class="sxs-lookup"><span data-stu-id="9684d-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="9684d-187">處理的例外狀況所使用的通用位置`Warning`記錄層級。</span><span class="sxs-lookup"><span data-stu-id="9684d-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="9684d-188">範例：`FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="9684d-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="9684d-189">錯誤 = 4</span><span class="sxs-lookup"><span data-stu-id="9684d-189">Error = 4</span></span>

  <span data-ttu-id="9684d-190">錯誤和無法處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9684d-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="9684d-191">這些訊息會指出目前的活動或作業 （例如目前的 HTTP 要求） 中的失敗，不是整個應用程式失敗。</span><span class="sxs-lookup"><span data-stu-id="9684d-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="9684d-192">範例記錄檔訊息：`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="9684d-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="9684d-193">重大 = 5</span><span class="sxs-lookup"><span data-stu-id="9684d-193">Critical = 5</span></span>

  <span data-ttu-id="9684d-194">需要立即關注的失敗。</span><span class="sxs-lookup"><span data-stu-id="9684d-194">For failures that require immediate attention.</span></span> <span data-ttu-id="9684d-195">範例： 資料遺失情況下，磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="9684d-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="9684d-196">您可以使用記錄層級來控制多少記錄輸出寫入到特定的儲存媒體，或顯示視窗。</span><span class="sxs-lookup"><span data-stu-id="9684d-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="9684d-197">比方說，在生產環境中您可能想要的所有記錄檔`Information`層級和較低的所有記錄檔的磁碟區資料存放區中，並移至`Warning`層級和更新版本移至 值資料存放區。</span><span class="sxs-lookup"><span data-stu-id="9684d-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="9684d-198">在開發期間，您可能會正常傳送記錄檔的`Warning`或更高嚴重性至主控台。</span><span class="sxs-lookup"><span data-stu-id="9684d-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="9684d-199">然後當您需要進行疑難排解，您可以加入`Debug`層級。</span><span class="sxs-lookup"><span data-stu-id="9684d-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="9684d-200">[記錄檔篩選](#log-filtering)本文中稍後的章節將說明如何控制提供者會處理的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="9684d-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="9684d-201">ASP.NET Core framework 寫入`Debug`層級記錄檔中的 framework 事件。</span><span class="sxs-lookup"><span data-stu-id="9684d-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="9684d-202">記錄範例稍早在本文中排除下列記錄檔`Information`層級，所以沒有`Debug`層級的記錄檔已顯示。</span><span class="sxs-lookup"><span data-stu-id="9684d-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="9684d-203">以下是主控台記錄檔的範例，如果您執行範例應用程式設定為顯示`Debug`和更高版本的記錄檔，主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="9684d-204">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="9684d-204">Log event ID</span></span>

<span data-ttu-id="9684d-205">每次您將寫入記錄檔，您可以指定*事件識別碼*。</span><span class="sxs-lookup"><span data-stu-id="9684d-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="9684d-206">範例應用程式會使用本機定義`LoggingEvents`類別：</span><span class="sxs-lookup"><span data-stu-id="9684d-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="9684d-207">事件識別碼是整數值，可用來將一組記錄的事件與彼此產生關聯。</span><span class="sxs-lookup"><span data-stu-id="9684d-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="9684d-208">比方說，記錄檔的新增至購物車的項目可以是事件識別碼 1000年，而且完成購買的記錄檔可能是事件識別碼 1001年。</span><span class="sxs-lookup"><span data-stu-id="9684d-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="9684d-209">在記錄輸出可能儲存在欄位或文字訊息，根據提供者中包含的事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="9684d-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span>  <span data-ttu-id="9684d-210">偵錯提供者不會顯示事件識別碼，但主控台提供者中會顯示這些方括號之後類別：</span><span class="sxs-lookup"><span data-stu-id="9684d-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a><span data-ttu-id="9684d-211">記錄檔訊息的格式字串</span><span class="sxs-lookup"><span data-stu-id="9684d-211">Log message format string</span></span>

<span data-ttu-id="9684d-212">每次寫入記錄檔，您提供的文字訊息。</span><span class="sxs-lookup"><span data-stu-id="9684d-212">Each time you write a log, you provide a text message.</span></span> <span data-ttu-id="9684d-213">訊息字串可以包含具名的預留位置的引數的值會放置在如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="9684d-213">The message string can contain named placeholders into which argument values are placed, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9684d-214">預留位置，其名稱則不順序會決定哪一個參數用它們。</span><span class="sxs-lookup"><span data-stu-id="9684d-214">The order of placeholders, not their names, determines which parameters are used for them.</span></span> <span data-ttu-id="9684d-215">例如，如果您有下列的程式碼：</span><span class="sxs-lookup"><span data-stu-id="9684d-215">For example, if you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="9684d-216">產生的記錄檔訊息看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="9684d-216">The resulting log message would look like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="9684d-217">記錄架構執行訊息格式化以使記錄提供者來實作這種方式[語意的記錄，也稱為結構化記錄](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="9684d-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="9684d-218">因為本身的引數會傳遞給記錄系統，而不只是格式化的訊息字串中，記錄提供者可以儲存的參數值做為欄位除了訊息字串。</span><span class="sxs-lookup"><span data-stu-id="9684d-218">Because the arguments themselves are passed to the logging system, not just the formatted message string, logging providers can store the parameter values as fields in addition to the message string.</span></span> <span data-ttu-id="9684d-219">例如，如果導向您的記錄檔輸出到 Azure 資料表儲存體，而記錄器方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="9684d-219">For example, if you are directing your log output to Azure Table Storage, and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="9684d-220">Azure 資料表的每個實體可能會有`ID`和`RequestTime`會簡化查詢記錄資料的屬性。</span><span class="sxs-lookup"><span data-stu-id="9684d-220">Each Azure Table entity could have `ID` and `RequestTime` properties, which would simplify queries on log data.</span></span> <span data-ttu-id="9684d-221">您可能會發現所有記錄檔中特定`RequestTime`範圍，而無需剖析時間超出文字訊息。</span><span class="sxs-lookup"><span data-stu-id="9684d-221">You could find all logs within a particular `RequestTime` range, without having to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="9684d-222">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="9684d-222">Logging exceptions</span></span>

<span data-ttu-id="9684d-223">記錄器方法具有多載，可讓您傳遞例外狀況，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="9684d-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="9684d-224">不同的提供者以不同方式處理的例外狀況資訊。</span><span class="sxs-lookup"><span data-stu-id="9684d-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="9684d-225">以下是從上述的程式碼的偵錯提供者輸出範例。</span><span class="sxs-lookup"><span data-stu-id="9684d-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="9684d-226">記錄檔篩選</span><span class="sxs-lookup"><span data-stu-id="9684d-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9684d-228">您可以指定最小的記錄層級為特定的提供者、 類別目錄或所有的提供者或所有類別目錄。</span><span class="sxs-lookup"><span data-stu-id="9684d-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span>  <span data-ttu-id="9684d-229">下列的最低層級的任何記錄檔不被傳遞該提供者，使它們不是您取得顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="9684d-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="9684d-230">如果您想要隱藏所有記錄檔，您可以指定`LogLevel.None`為最小的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="9684d-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="9684d-231">整數值`LogLevel.None`是 6，這是高於`LogLevel.Critical`(5)。</span><span class="sxs-lookup"><span data-stu-id="9684d-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="9684d-232">**在組態中建立篩選器規則**</span><span class="sxs-lookup"><span data-stu-id="9684d-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="9684d-233">專案範本建立程式碼，呼叫`CreateDefaultBuilder`設定主控台和偵錯提供者的記錄。</span><span class="sxs-lookup"><span data-stu-id="9684d-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="9684d-234">`CreateDefaultBuilder`方法也可以設定記錄設定中尋找`Logging`區段中，使用程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="9684d-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="9684d-235">設定資料指定最小的記錄層級提供者及類別，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="9684d-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](logging/sample2/appsettings.json)]

<span data-ttu-id="9684d-236">此 JSON 建立六個篩選規則，一個用於偵錯提供者，四個主控台提供者，以及適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="9684d-237">您會看到如何只是其中一個這些規則會選擇每個提供者時`ILogger`建立物件。</span><span class="sxs-lookup"><span data-stu-id="9684d-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="9684d-238">**在程式碼中的篩選規則**</span><span class="sxs-lookup"><span data-stu-id="9684d-238">**Filter rules in code**</span></span>

<span data-ttu-id="9684d-239">下列範例所示，您可以在程式碼，註冊篩選規則：</span><span class="sxs-lookup"><span data-stu-id="9684d-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="9684d-240">第二個`AddFilter`使用型別名稱來指定偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="9684d-241">第一個`AddFilter`適用於所有提供者，因為它不會指定提供者類型。</span><span class="sxs-lookup"><span data-stu-id="9684d-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="9684d-242">**篩選規則的方式套用**</span><span class="sxs-lookup"><span data-stu-id="9684d-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="9684d-243">設定資料和`AddFilter`上述範例所示的程式碼會建立下表中所顯示的規則。</span><span class="sxs-lookup"><span data-stu-id="9684d-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="9684d-244">前六個來自組態範例，而且最後兩個會從程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="9684d-244">The first six come from the configuration example and the last two come from the code example.</span></span>

<span data-ttu-id="9684d-245">數字</span><span class="sxs-lookup"><span data-stu-id="9684d-245">Number</span></span>|<span data-ttu-id="9684d-246">提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-246">Provider</span></span>|<span data-ttu-id="9684d-247">開頭的類別</span><span class="sxs-lookup"><span data-stu-id="9684d-247">Categories that begin with</span></span>|<span data-ttu-id="9684d-248">最小的記錄層級</span><span class="sxs-lookup"><span data-stu-id="9684d-248">Minimum log level</span></span>|
------|--------|--------------------------|-----------------|
<span data-ttu-id="9684d-249">1</span><span class="sxs-lookup"><span data-stu-id="9684d-249">1</span></span>|<span data-ttu-id="9684d-250">偵錯</span><span class="sxs-lookup"><span data-stu-id="9684d-250">Debug</span></span>|<span data-ttu-id="9684d-251">所有類別</span><span class="sxs-lookup"><span data-stu-id="9684d-251">All categories</span></span>|<span data-ttu-id="9684d-252">資訊</span><span class="sxs-lookup"><span data-stu-id="9684d-252">Information</span></span>|
<span data-ttu-id="9684d-253">2</span><span class="sxs-lookup"><span data-stu-id="9684d-253">2</span></span>|<span data-ttu-id="9684d-254">主控台</span><span class="sxs-lookup"><span data-stu-id="9684d-254">Console</span></span>|<span data-ttu-id="9684d-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="9684d-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span>|<span data-ttu-id="9684d-256">警告</span><span class="sxs-lookup"><span data-stu-id="9684d-256">Warning</span></span>|
<span data-ttu-id="9684d-257">3</span><span class="sxs-lookup"><span data-stu-id="9684d-257">3</span></span>|<span data-ttu-id="9684d-258">主控台</span><span class="sxs-lookup"><span data-stu-id="9684d-258">Console</span></span>|<span data-ttu-id="9684d-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="9684d-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>|<span data-ttu-id="9684d-260">偵錯</span><span class="sxs-lookup"><span data-stu-id="9684d-260">Debug</span></span>|
<span data-ttu-id="9684d-261">4</span><span class="sxs-lookup"><span data-stu-id="9684d-261">4</span></span>|<span data-ttu-id="9684d-262">主控台</span><span class="sxs-lookup"><span data-stu-id="9684d-262">Console</span></span>|<span data-ttu-id="9684d-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="9684d-263">Microsoft.AspNetCore.Mvc.Razor</span></span>|<span data-ttu-id="9684d-264">錯誤</span><span class="sxs-lookup"><span data-stu-id="9684d-264">Error</span></span>|
<span data-ttu-id="9684d-265">5</span><span class="sxs-lookup"><span data-stu-id="9684d-265">5</span></span>|<span data-ttu-id="9684d-266">主控台</span><span class="sxs-lookup"><span data-stu-id="9684d-266">Console</span></span>|<span data-ttu-id="9684d-267">所有類別</span><span class="sxs-lookup"><span data-stu-id="9684d-267">All categories</span></span>|<span data-ttu-id="9684d-268">資訊</span><span class="sxs-lookup"><span data-stu-id="9684d-268">Information</span></span>|
<span data-ttu-id="9684d-269">6</span><span class="sxs-lookup"><span data-stu-id="9684d-269">6</span></span>|<span data-ttu-id="9684d-270">所有提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-270">All providers</span></span>|<span data-ttu-id="9684d-271">所有類別</span><span class="sxs-lookup"><span data-stu-id="9684d-271">All categories</span></span>|<span data-ttu-id="9684d-272">偵錯</span><span class="sxs-lookup"><span data-stu-id="9684d-272">Debug</span></span>
<span data-ttu-id="9684d-273">7</span><span class="sxs-lookup"><span data-stu-id="9684d-273">7</span></span>|<span data-ttu-id="9684d-274">所有提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-274">All providers</span></span>|<span data-ttu-id="9684d-275">系統</span><span class="sxs-lookup"><span data-stu-id="9684d-275">System</span></span>|<span data-ttu-id="9684d-276">偵錯</span><span class="sxs-lookup"><span data-stu-id="9684d-276">Debug</span></span>
<span data-ttu-id="9684d-277">8</span><span class="sxs-lookup"><span data-stu-id="9684d-277">8</span></span>|<span data-ttu-id="9684d-278">偵錯</span><span class="sxs-lookup"><span data-stu-id="9684d-278">Debug</span></span>|<span data-ttu-id="9684d-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="9684d-279">Microsoft</span></span>|<span data-ttu-id="9684d-280">追蹤</span><span class="sxs-lookup"><span data-stu-id="9684d-280">Trace</span></span>

<span data-ttu-id="9684d-281">當您建立`ILogger`物件寫入記錄檔，`ILoggerFactory`物件會選取每個提供者来套用至該記錄器的單一規則。</span><span class="sxs-lookup"><span data-stu-id="9684d-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="9684d-282">所有訊息的寫入`ILogger`會根據選取的規則篩選物件。</span><span class="sxs-lookup"><span data-stu-id="9684d-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="9684d-283">從可用的規則，會選取每個提供者] 和 [類別目錄配對可能最適合的規則。</span><span class="sxs-lookup"><span data-stu-id="9684d-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="9684d-284">每個提供者會使用下列演算法時`ILogger`建立指定類別目錄：</span><span class="sxs-lookup"><span data-stu-id="9684d-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="9684d-285">選取符合提供者或其別名的所有規則。</span><span class="sxs-lookup"><span data-stu-id="9684d-285">Select all rules that match the provider or its alias.</span></span>  <span data-ttu-id="9684d-286">如果找不到，請以空的提供者選取所有規則。</span><span class="sxs-lookup"><span data-stu-id="9684d-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="9684d-287">從上一個步驟的結果，具有最長的選取規則比對類別前置詞。</span><span class="sxs-lookup"><span data-stu-id="9684d-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="9684d-288">如果找不到，請選取 不指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="9684d-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="9684d-289">如果選取多個規則採取**最後**其中一個。</span><span class="sxs-lookup"><span data-stu-id="9684d-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="9684d-290">如果未不選取任何規則，則使用`MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="9684d-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="9684d-291">例如，假設您有上述規則的清單，而且您建立`ILogger`"Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine"類別的物件：</span><span class="sxs-lookup"><span data-stu-id="9684d-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="9684d-292">偵錯提供者，將套用規則 1、 6 和 8。</span><span class="sxs-lookup"><span data-stu-id="9684d-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="9684d-293">規則 8 是最特定的因此這是所選取。</span><span class="sxs-lookup"><span data-stu-id="9684d-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="9684d-294">主控台提供者，將套用規則 3、 4、 5 和 6。</span><span class="sxs-lookup"><span data-stu-id="9684d-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="9684d-295">規則 3 是最適合。</span><span class="sxs-lookup"><span data-stu-id="9684d-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="9684d-296">當您建立的記錄檔`ILogger`為類別目錄 」 Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine 」 記錄的`Trace`層級和更新版本將會移至的偵錯提供者和記錄檔`Debug`層級和更新版本將會移至主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="9684d-297">**別名提供者**</span><span class="sxs-lookup"><span data-stu-id="9684d-297">**Provider aliases**</span></span>

<span data-ttu-id="9684d-298">您可以在組態中，指定提供者使用的型別名稱，但每個提供者會定義短*別名*，比較容易使用。</span><span class="sxs-lookup"><span data-stu-id="9684d-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="9684d-299">內建的提供者，使用下列別名：</span><span class="sxs-lookup"><span data-stu-id="9684d-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="9684d-300">主控台</span><span class="sxs-lookup"><span data-stu-id="9684d-300">Console</span></span>
- <span data-ttu-id="9684d-301">偵錯</span><span class="sxs-lookup"><span data-stu-id="9684d-301">Debug</span></span>
- <span data-ttu-id="9684d-302">事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="9684d-302">EventLog</span></span>
- <span data-ttu-id="9684d-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="9684d-303">AzureAppServices</span></span>
- <span data-ttu-id="9684d-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9684d-304">TraceSource</span></span>
- <span data-ttu-id="9684d-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="9684d-305">EventSource</span></span>

<span data-ttu-id="9684d-306">**預設最小層級**</span><span class="sxs-lookup"><span data-stu-id="9684d-306">**Default minimum level**</span></span>

<span data-ttu-id="9684d-307">沒有任何組態或程式碼的規則適用於給定的提供者和類別目錄時，才會生效的最小層級設定。</span><span class="sxs-lookup"><span data-stu-id="9684d-307">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="9684d-308">下列範例會示範如何設定的最低層級：</span><span class="sxs-lookup"><span data-stu-id="9684d-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="9684d-309">如果您未明確設定的最低層級，預設值是`Information`，這表示`Trace`和`Debug`記錄檔會被忽略。</span><span class="sxs-lookup"><span data-stu-id="9684d-309">IF you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="9684d-310">**篩選函數**</span><span class="sxs-lookup"><span data-stu-id="9684d-310">**Filter functions**</span></span>

<span data-ttu-id="9684d-311">您可以撰寫程式碼中套用的篩選規則的篩選函數。</span><span class="sxs-lookup"><span data-stu-id="9684d-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="9684d-312">Filter 函式會叫用的所有提供者和沒有規則指派它們的組態或程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="9684d-312">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="9684d-313">函式中的程式碼可存取提供者類型、 類別和記錄層級，決定應該記錄的訊息。</span><span class="sxs-lookup"><span data-stu-id="9684d-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="9684d-314">例如: </span><span class="sxs-lookup"><span data-stu-id="9684d-314">For example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9684d-316">某些記錄提供者可讓您指定記錄檔時應該寫入儲存媒體或忽略根據記錄層級和類別目錄。</span><span class="sxs-lookup"><span data-stu-id="9684d-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="9684d-317">`AddConsole`和`AddDebug`擴充方法提供多載，可讓您在篩選條件中傳遞。</span><span class="sxs-lookup"><span data-stu-id="9684d-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="9684d-318">下列範例程式碼會導致略過下列記錄檔的主控台提供者`Warning`層級，而偵錯提供者會忽略 framework 建立的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="9684d-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="9684d-319">`AddEventLog`方法具有多載採用`EventLogSettings`執行個體，其中可能包含篩選的函式，在其`Filter`屬性。</span><span class="sxs-lookup"><span data-stu-id="9684d-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="9684d-320">TraceSource 提供者未提供任何多載，因為其記錄層級和其他參數會根據`SourceSwitch`和`TraceListener`它會使用。</span><span class="sxs-lookup"><span data-stu-id="9684d-320">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the  `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="9684d-321">您可以設定所有的提供者註冊的篩選規則`ILoggerFactory`所使用的執行個體`WithFilter`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="9684d-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="9684d-322">下列範例限制 framework 記錄檔 （類別開頭為 「 Microsoft 」 或 「 系統 」），以警告時讓偵錯層級的應用程式記錄檔。</span><span class="sxs-lookup"><span data-stu-id="9684d-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="9684d-323">如果您想要使用篩選來防止所有記錄檔寫入特定分類，您可以指定`LogLevel.None`為該分類的最小的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="9684d-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="9684d-324">整數值`LogLevel.None`是 6，這是高於`LogLevel.Critical`(5)。</span><span class="sxs-lookup"><span data-stu-id="9684d-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="9684d-325">`WithFilter`擴充方法由[Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="9684d-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="9684d-326">方法會傳回新`ILoggerFactory`會篩選傳遞記錄器的所有提供者的記錄檔訊息的執行個體已向它註冊。</span><span class="sxs-lookup"><span data-stu-id="9684d-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="9684d-327">不會影響任何其他`ILoggerFactory`執行個體，包括原始`ILoggerFactory`執行個體。</span><span class="sxs-lookup"><span data-stu-id="9684d-327">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="9684d-328">記錄檔的範圍</span><span class="sxs-lookup"><span data-stu-id="9684d-328">Log scopes</span></span>

<span data-ttu-id="9684d-329">您可以分組一組內的邏輯作業*範圍*以相同的資料附加到建立該集一部分的每個記錄檔。</span><span class="sxs-lookup"><span data-stu-id="9684d-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span>  <span data-ttu-id="9684d-330">例如，您可能想要包含交易識別碼。 交易的處理過程中建立每個記錄檔</span><span class="sxs-lookup"><span data-stu-id="9684d-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="9684d-331">領域是`IDisposable`所傳回的型別`ILogger.BeginScope<TState>`方法，直到它已處置的持續時間。</span><span class="sxs-lookup"><span data-stu-id="9684d-331">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="9684d-332">您所包裝您記錄器呼叫中使用範圍`using`封鎖，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9684d-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="9684d-333">下列程式碼可讓主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="9684d-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9684d-335">在*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9684d-335">In *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-336">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-336">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9684d-337">在*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9684d-337">In *Startup.cs*:</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="9684d-338">每個記錄檔訊息包含已設定領域的資訊：</span><span class="sxs-lookup"><span data-stu-id="9684d-338">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="9684d-339">內建的記錄提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-339">Built-in logging providers</span></span>

<span data-ttu-id="9684d-340">ASP.NET Core 隨附下列提供者：</span><span class="sxs-lookup"><span data-stu-id="9684d-340">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="9684d-341">Console</span><span class="sxs-lookup"><span data-stu-id="9684d-341">Console</span></span>](#console)
* [<span data-ttu-id="9684d-342">偵錯</span><span class="sxs-lookup"><span data-stu-id="9684d-342">Debug</span></span>](#debug)
* [<span data-ttu-id="9684d-343">EventSource</span><span class="sxs-lookup"><span data-stu-id="9684d-343">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="9684d-344">EventLog</span><span class="sxs-lookup"><span data-stu-id="9684d-344">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="9684d-345">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9684d-345">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="9684d-346">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9684d-346">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="9684d-347">主控台提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-347">The console provider</span></span>

<span data-ttu-id="9684d-348">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console)封裝提供者會傳送記錄檔輸出到主控台。</span><span class="sxs-lookup"><span data-stu-id="9684d-348">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-350">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="9684d-351">[AddConsole 多載](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions)可讓您傳入的最小的記錄層級、 篩選函數和布林值，指出是否支援範圍。</span><span class="sxs-lookup"><span data-stu-id="9684d-351">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span>  <span data-ttu-id="9684d-352">另一個選項是傳入`IConfiguration`物件，可以指定範圍的支援和記錄層級。</span><span class="sxs-lookup"><span data-stu-id="9684d-352">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="9684d-353">如果您考慮在生產環境中使用的主控台提供者，請注意它對效能有重大影響。</span><span class="sxs-lookup"><span data-stu-id="9684d-353">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="9684d-354">當您在 Visual Studio 中，建立新的專案`AddConsole`方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="9684d-354">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="9684d-355">此程式碼是指`Logging`區段*appSettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="9684d-355">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](logging/sample//appsettings.json)]

<span data-ttu-id="9684d-356">顯示限制 framework 警告記錄檔，同時允許應用程式的設定，以中所述，在偵錯層級，記錄[記錄檔篩選](#log-filtering)> 一節。</span><span class="sxs-lookup"><span data-stu-id="9684d-356">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="9684d-357">如需詳細資訊，請參閱[組態](configuration.md)。</span><span class="sxs-lookup"><span data-stu-id="9684d-357">For more information, see [Configuration](configuration.md).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="9684d-358">偵錯提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-358">The Debug provider</span></span>

<span data-ttu-id="9684d-359">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug)封裝提供者會將記錄輸出寫入使用[System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug)類別 (`Debug.WriteLine`方法呼叫)。</span><span class="sxs-lookup"><span data-stu-id="9684d-359">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="9684d-360">On Linux，此提供者寫入記錄檔以*/var/log/message*。</span><span class="sxs-lookup"><span data-stu-id="9684d-360">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="9684d-363">[AddDebug 多載](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions)可讓您在最小的記錄層級或篩選函數中傳遞。</span><span class="sxs-lookup"><span data-stu-id="9684d-363">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="9684d-364">EventSource 的提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-364">The EventSource provider</span></span>

<span data-ttu-id="9684d-365">以目標 ASP.NET Core 1.1.0 應用程式或更新版本， [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)封裝提供者可以實作事件追蹤。</span><span class="sxs-lookup"><span data-stu-id="9684d-365">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="9684d-366">在 Windows 中，它會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="9684d-366">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="9684d-367">提供者是跨平台，但不有任何事件尚未 for Linux 或 macOS 收集及顯示的工具。</span><span class="sxs-lookup"><span data-stu-id="9684d-367">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-368">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-368">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-369">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-369">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="9684d-370">收集並檢視記錄檔的好方法是使用[PerfView 公用程式](https://www.microsoft.com/download/details.aspx?id=28567)。</span><span class="sxs-lookup"><span data-stu-id="9684d-370">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="9684d-371">有一些其他工具來檢視 ETW 記錄檔，但 PerfView 提供最佳的經驗，使用 asp.net 所發出的 ETW 事件。</span><span class="sxs-lookup"><span data-stu-id="9684d-371">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="9684d-372">若要設定 PerfView 收集此提供者所記錄的事件，將字串加入`*Microsoft-Extensions-Logging`至**額外的提供者**清單。</span><span class="sxs-lookup"><span data-stu-id="9684d-372">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="9684d-373">（不會錯過星號開頭的字串。）</span><span class="sxs-lookup"><span data-stu-id="9684d-373">(Don't miss the asterisk at the start of the string.)</span></span>

![Perfview 額外的提供者](logging/_static/perfview-additional-providers.png)

<span data-ttu-id="9684d-375">擷取在 Nano Server 上的事件需要一些額外的設定：</span><span class="sxs-lookup"><span data-stu-id="9684d-375">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="9684d-376">連接至 Nano Server 的 PowerShell 遠端執行功能：</span><span class="sxs-lookup"><span data-stu-id="9684d-376">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="9684d-377">建立 ETW 工作階段：</span><span class="sxs-lookup"><span data-stu-id="9684d-377">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="9684d-378">新增 ETW 提供者[CLR](https://msdn.microsoft.com/library/ff357718)，ASP.NET Core 和其他人所需。</span><span class="sxs-lookup"><span data-stu-id="9684d-378">Add ETW providers for [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="9684d-379">ASP.NET Core 提供者 GUID 是`3ac73b97-af73-50e9-0822-5da4367920d0`。</span><span class="sxs-lookup"><span data-stu-id="9684d-379">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="9684d-380">執行站台，並執行您要的追蹤資訊的任何動作。</span><span class="sxs-lookup"><span data-stu-id="9684d-380">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="9684d-381">停止追蹤工作階段完成時：</span><span class="sxs-lookup"><span data-stu-id="9684d-381">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="9684d-382">產生*C:\trace.etl*可以在其他版本的 Windows 上使用 PerfView 分析檔案。</span><span class="sxs-lookup"><span data-stu-id="9684d-382">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="9684d-383">Windows 事件記錄檔提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-383">The Windows EventLog provider</span></span>

<span data-ttu-id="9684d-384">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)封裝提供者記錄檔會將輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="9684d-384">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-385">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-385">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-386">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-386">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="9684d-387">[AddEventLog 多載](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions)可讓您傳入`EventLogSettings`或最小的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="9684d-387">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="9684d-388">TraceSource 提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-388">The TraceSource provider</span></span>

<span data-ttu-id="9684d-389">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource)封裝提供者使用[System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx)程式庫和提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-390">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-390">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-391">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-391">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="9684d-392">[AddTraceSource 多載](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions)可讓您傳入來源參數和追蹤接聽項。</span><span class="sxs-lookup"><span data-stu-id="9684d-392">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="9684d-393">若要使用此提供者，應用程式必須在.NET Framework （而非.NET Core） 上執行。</span><span class="sxs-lookup"><span data-stu-id="9684d-393">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="9684d-394">此提供者可讓您將訊息傳送至各種不同的[接聽程式](https://msdn.microsoft.com/library/4y5y10s7)，例如[TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener)範例應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="9684d-394">The provider lets you route messages to a variety of [listeners](https://msdn.microsoft.com/library/4y5y10s7), such as the [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) used in the sample application.</span></span>

<span data-ttu-id="9684d-395">下列範例會設定`TraceSource`記錄提供者`Warning`和更高的訊息至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="9684d-395">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="9684d-396">Azure 應用程式服務提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-396">The Azure App Service provider</span></span>

<span data-ttu-id="9684d-397">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices)封裝提供者會將記錄寫入 Azure App Service 應用程式的檔案系統中，文字檔[blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)Azure 儲存體帳戶中。</span><span class="sxs-lookup"><span data-stu-id="9684d-397">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="9684d-398">僅適用於 ASP.NET Core 1.1.0 為目標的應用程式或更高版本的提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-398">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9684d-399">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9684d-399">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

> [!NOTE]
> <span data-ttu-id="9684d-400">ASP.NET Core 2.0 處於預覽狀態。</span><span class="sxs-lookup"><span data-stu-id="9684d-400">ASP.NET Core 2.0 is in preview.</span></span>  <span data-ttu-id="9684d-401">使用最新預覽版本建立應用程式部署至 Azure 應用程式服務時，可能無法執行。</span><span class="sxs-lookup"><span data-stu-id="9684d-401">Apps created with the latest preview release may not run when deployed to Azure App Service.</span></span> <span data-ttu-id="9684d-402">Azure App Service ASP.NET Core 2.0 發行時，會執行 2.0 應用程式和 Azure 應用程式服務提供者將使用此處所示。</span><span class="sxs-lookup"><span data-stu-id="9684d-402">When ASP.NET Core 2.0 is released, Azure App Service will run 2.0 apps, and the Azure App Service provider will work as indicated here.</span></span>

<span data-ttu-id="9684d-403">您不需要安裝之提供者的封裝或呼叫`AddAzureWebAppDiagnostics`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="9684d-403">You don't have to install the provider package or call the `AddAzureWebAppDiagnostics` extension method.</span></span>  <span data-ttu-id="9684d-404">當您將應用程式部署至 Azure App Service 自動提供給您的應用程式提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9684d-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9684d-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="9684d-406">`AddAzureWebAppDiagnostics`多載可讓您傳入[AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)，與您可以覆寫預設設定，例如記錄輸出範本、 blob 名稱和檔案大小限制。</span><span class="sxs-lookup"><span data-stu-id="9684d-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="9684d-407">(*輸出範本*是訊息的格式字串套用至所有的記錄檔，在您提供當您呼叫的一個之上`ILogger`方法。)</span><span class="sxs-lookup"><span data-stu-id="9684d-407">(*Output template* is a message format string that is applied to all logs, on top of the one that you provide when you call an `ILogger` method.)</span></span>  

---

<span data-ttu-id="9684d-408">當您部署 App Service 應用程式時，您的應用程式會接受中的設定[診斷記錄檔](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag)區段**App Service**在 Azure 入口網站頁面。</span><span class="sxs-lookup"><span data-stu-id="9684d-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="9684d-409">當您變更這些設定時，所做的變更會立即生效而不需要您重新啟動應用程式，或重新部署它的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9684d-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Azure 記錄設定](logging/_static/azure-logging-settings.png)

<span data-ttu-id="9684d-411">記錄檔的預設位置是在*d:\\家用\\LogFiles\\應用程式*資料夾和預設檔案名稱是*診斷 yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="9684d-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="9684d-412">預設檔案大小限制為 10 MB，並預設的保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="9684d-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="9684d-413">預設的 blob 名稱是*{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="9684d-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="9684d-414">如需有關預設行為的詳細資訊，請參閱[AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)。</span><span class="sxs-lookup"><span data-stu-id="9684d-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="9684d-415">在 Azure 環境中執行您的專案時，只適用於此提供者。</span><span class="sxs-lookup"><span data-stu-id="9684d-415">The provider only works when your project runs in the Azure environment.</span></span>  <span data-ttu-id="9684d-416">它沒有任何作用，當您在本機執行&mdash;它不會寫入本機檔案或 blob 的本機開發儲存體。</span><span class="sxs-lookup"><span data-stu-id="9684d-416">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="9684d-417">第三方記錄提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-417">Third-party logging providers</span></span>

<span data-ttu-id="9684d-418">以下是使用 ASP.NET Core 某些協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="9684d-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="9684d-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -Elmah.Io 服務提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="9684d-420">[JSNLog](http://jsnlog.com) -您的伺服器端記錄檔中記錄的 JavaScript 例外狀況和其他用戶端事件。</span><span class="sxs-lookup"><span data-stu-id="9684d-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="9684d-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -Loggr 服務提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="9684d-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) -NLog 程式庫提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="9684d-423">[Serilog](https://github.com/serilog/serilog-framework-logging) -Serilog 程式庫提供者</span><span class="sxs-lookup"><span data-stu-id="9684d-423">[Serilog](https://github.com/serilog/serilog-framework-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="9684d-424">可以執行一些協力廠商架構[語意的記錄，也稱為結構化記錄](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="9684d-424">Some third-party frameworks can do [semantic logging, also known as structured logging](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="9684d-425">使用協力廠商架構，類似於使用其中一個內建的提供者： 將 NuGet 封裝加入至您的專案和擴充方法呼叫上`ILoggerFactory`。</span><span class="sxs-lookup"><span data-stu-id="9684d-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="9684d-426">如需詳細資訊，請參閱每個架構的文件。</span><span class="sxs-lookup"><span data-stu-id="9684d-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="9684d-427">您可以建立您自己的自訂提供者，以支援其他的記錄架構或您自己的記錄需求。</span><span class="sxs-lookup"><span data-stu-id="9684d-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>
