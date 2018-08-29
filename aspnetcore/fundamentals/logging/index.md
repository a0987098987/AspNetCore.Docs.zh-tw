---
title: ASP.NET Core 中的記錄
author: ardalis
description: 了解 ASP.NET Core 中的記錄架構。 探索內建記錄提供者，並深入了解熱門協力廠商提供者。
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/27/2018
uid: fundamentals/logging/index
ms.openlocfilehash: c6e9aae06df6ebec373b1296f86e37380bf08b15
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055757"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="d3b3c-104">ASP.NET Core 中的記錄</span><span class="sxs-lookup"><span data-stu-id="d3b3c-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="d3b3c-105">作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d3b3c-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d3b3c-106">ASP.NET Core 支援可搭配各種記錄提供者的記錄 API。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="d3b3c-107">內建提供者可讓您將記錄傳送至一或多個目的地，而且您可以外掛協力廠商記錄架構。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="d3b3c-108">本文說明如何在程式碼中使用內建記錄 API 和提供者。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

<span data-ttu-id="d3b3c-109">如需使用 IIS 裝載時的 stdout 記錄資訊，請參閱<xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-109">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="d3b3c-110">如需使用 Azure App Service 的 stdout 記錄資訊，請參閱 <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-110">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

<span data-ttu-id="d3b3c-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3b3c-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="d3b3c-112">如何建立記錄</span><span class="sxs-lookup"><span data-stu-id="d3b3c-112">How to create logs</span></span>

<span data-ttu-id="d3b3c-113">若要建立記錄，請從[相依性插入](xref:fundamentals/dependency-injection)容器實作 [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) 物件：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-113">To create logs, implement an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="d3b3c-114">然後在該記錄器物件上呼叫記錄方法：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-114">Then call logging methods on that logger object:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="d3b3c-115">此範例會建立以 `TodoController` 為「類別」的記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-115">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="d3b3c-116">[本文稍後](#log-category)將說明類別。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-116">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="d3b3c-117">ASP.NET Core 不會提供非同步記錄器方法，因為記錄應該相當快速，因此不值得使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-117">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="d3b3c-118">如果發生與其相反的情況，請考慮變更您的記錄方式。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-118">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="d3b3c-119">如果您的資料存放區很慢，請先將記錄訊息寫入至快速存放區，稍後再移至慢速存放區。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-119">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="d3b3c-120">例如，記錄至訊息佇列以供其他處理序讀取及保存至慢速儲存體。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-120">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="d3b3c-121">如何新增提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-121">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d3b3c-122">記錄提供者會擷取您使用 `ILogger` 物件建立的訊息，並加以顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-122">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="d3b3c-123">例如，主控台提供者可在主控台中顯示訊息，而 Azure App Service 提供者則可將其儲存在 Azure Blob 儲存體中。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-123">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="d3b3c-124">若要使用提供者，請在 *Program.cs* 中呼叫提供者的 `Add<ProviderName>` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-124">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="d3b3c-125">預設專案範本可讓主控台與偵錯記錄提供者呼叫 *Program.cs* 中的 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-125">The default project template enables the Console and Debug logging providers with a call to the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d3b3c-126">記錄提供者會擷取您使用 `ILogger` 物件建立的訊息，並加以顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-126">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="d3b3c-127">例如，主控台提供者可在主控台中顯示訊息，而 Azure App Service 提供者則可將其儲存在 Azure Blob 儲存體中。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-127">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="d3b3c-128">若要使用提供者，請安裝其 NuGet 套件，並在 [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) 的執行個體上呼叫該提供者的擴充方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-128">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="d3b3c-129">ASP.NET Core [相依性插入](xref:fundamentals/dependency-injection) (DI) 提供 `ILoggerFactory` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-129">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="d3b3c-130">`AddConsole` 和 `AddDebug` 擴充方法定義於 [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) 和 [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) 套件中。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-130">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="d3b3c-131">每個擴充方法會呼叫 `ILoggerFactory.AddProvider` 方法，並傳入提供者的執行個體。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-131">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="d3b3c-132">[1.x 範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x)會在 `Startup.Configure` 方法中新增記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-132">The [1.x sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="d3b3c-133">如果您想要從先前執行的程式碼取得記錄輸出，請在 `Startup` 類別建構函式中新增記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-133">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="d3b3c-134">您會在本文深入了解[內建記錄提供者](#built-in-logging-providers)，並找到[協力廠商記錄提供者](#third-party-logging-providers)的連結。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-134">Learn more about the [built-in logging providers](#built-in-logging-providers) and find links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="configuration"></a><span data-ttu-id="d3b3c-135">Configuration</span><span class="sxs-lookup"><span data-stu-id="d3b3c-135">Configuration</span></span>

<span data-ttu-id="d3b3c-136">記錄提供者設定是由一或多個記錄提供者提供：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-136">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="d3b3c-137">檔案格式 (INI、JSON 及 XML)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-137">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="d3b3c-138">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-138">Command-line arguments.</span></span>
* <span data-ttu-id="d3b3c-139">環境變數。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-139">Environment variables.</span></span>
* <span data-ttu-id="d3b3c-140">記憶體內部 .NET 物件。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-140">In-memory .NET objects.</span></span>
* <span data-ttu-id="d3b3c-141">未加密的[祕密管理員](xref:security/app-secrets)儲存體。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-141">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="d3b3c-142">類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-142">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="d3b3c-143">自訂提供者 (已安裝或已建立)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-143">Custom providers (installed or created).</span></span>

<span data-ttu-id="d3b3c-144">例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-144">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="d3b3c-145">下列範例顯示一般 *appsettings.Development.json* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-145">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": "true"
    }
  }
}
```

<span data-ttu-id="d3b3c-146">`LogLevel` 索引鍵代表記錄名稱。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-146">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="d3b3c-147">`Default` 索引鍵會套用到未明確列出的記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-147">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="d3b3c-148">此值代表套用到指定記錄的[記錄層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-148">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="d3b3c-149">設定 `IncludeScopes` 的記錄索引鍵 (在範例中為 `Console`) 會指定是否為指明的記錄啟用[記錄範圍](#log-scopes)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-149">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

<span data-ttu-id="d3b3c-150">`LogLevel` 索引鍵代表記錄名稱。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-150">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="d3b3c-151">`Default` 索引鍵會套用到未明確列出的記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-151">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="d3b3c-152">此值代表套用到指定記錄的[記錄層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-152">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="d3b3c-153">如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-153">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="d3b3c-154">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="d3b3c-154">Sample logging output</span></span>

<span data-ttu-id="d3b3c-155">當您從命令列執行上一節中所示的範例程式碼時，您會在主控台中看到記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-155">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="d3b3c-156">主控台輸出範例如下：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-156">Here's an example of console output:</span></span>

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

<span data-ttu-id="d3b3c-157">這些記錄是透過前往 `http://localhost:5000/api/todo/0` 來建立，這會觸發上一節中所示之兩個 `ILogger` 呼叫的執行。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-157">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="d3b3c-158">以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-158">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="d3b3c-159">這些透過上一節中所示的 `ILogger` 呼叫建立的記錄是以 "TodoApi.Controllers.TodoController" 為開頭。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-159">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="d3b3c-160">開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-160">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="d3b3c-161">ASP.NET Core 本身和您的應用程式程式碼會使用相同的記錄 API 和相同的記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-161">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="d3b3c-162">本文的其餘部分將說明記錄的一些詳細資料和選項。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-162">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="d3b3c-163">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="d3b3c-163">NuGet packages</span></span>

<span data-ttu-id="d3b3c-164">`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-164">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="d3b3c-165">記錄類別</span><span class="sxs-lookup"><span data-stu-id="d3b3c-165">Log category</span></span>

<span data-ttu-id="d3b3c-166">您建立的每個記錄都會包含一個「類別」。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-166">A *category* is included with each log that you create.</span></span> <span data-ttu-id="d3b3c-167">當您建立 `ILogger` 物件時會指定類別。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-167">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="d3b3c-168">類別可以是任何字串，但通常會使用寫入記錄之來源類別的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-168">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="d3b3c-169">例如："TodoApi.Controllers.TodoController"。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-169">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="d3b3c-170">您可以將類別指定為字串，或使用擴充方法從類型衍生類別。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-170">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="d3b3c-171">若要將類別指定為字串，請在 `ILoggerFactory` 執行個體上呼叫 `CreateLogger`，如下所示。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-171">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="d3b3c-172">在大多時候，使用 `ILogger<T>` 會更容易，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-172">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="d3b3c-173">這相當於使用 `T` 的完整類型名稱呼叫 `CreateLogger`。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-173">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="d3b3c-174">記錄層級</span><span class="sxs-lookup"><span data-stu-id="d3b3c-174">Log level</span></span>

<span data-ttu-id="d3b3c-175">每次寫入記錄，您都會指定其 [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-175">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="d3b3c-176">記錄層級表示嚴重或重要程度。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-176">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="d3b3c-177">例如，您可以在某個方法正常結束時寫入 `Information` 記錄，在某個方法傳回 404 傳回碼時寫入 `Warning` 記錄，並在攔截到未預期的例外狀況時寫入 `Error` 記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-177">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="d3b3c-178">在下列程式碼範例中，方法的名稱 (例如 `LogWarning`) 會指定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-178">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="d3b3c-179">第一個參數是[記錄事件識別碼](#log-event-id)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-179">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="d3b3c-180">第二個參數是[訊息範本](#log-message-template)，其中的預留位置會置入其餘方法參數所提供的引數值。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-180">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="d3b3c-181">本文稍後將更詳細說明方法參數。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-181">The method parameters are explained in more detail later in this article.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="d3b3c-182">在方法名稱中包含層級的記錄方法是 [ILogger 的擴充方法](/dotnet/api/microsoft.extensions.logging.loggerextensions)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-182">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="d3b3c-183">這些方法會在幕後呼叫接受 `LogLevel` 參數的 `Log` 方法。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-183">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="d3b3c-184">您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-184">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="d3b3c-185">如需詳細資訊，請參閱 [ILogger Interface](/dotnet/api/microsoft.extensions.logging.ilogger) (ILogger 介面) 和[記錄器延伸模組的原始程式碼](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-185">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="d3b3c-186">ASP.NET Core 定義下列[記錄層級](/dotnet/api/microsoft.extensions.logging.loglevel)，並從最低嚴重性排列到最高嚴重性。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-186">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="d3b3c-187">追蹤 = 0</span><span class="sxs-lookup"><span data-stu-id="d3b3c-187">Trace = 0</span></span>

  <span data-ttu-id="d3b3c-188">只對偵錯問題的開發人員重要的資訊。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-188">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="d3b3c-189">這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-189">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="d3b3c-190">預設為停用。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-190">*Disabled by default.*</span></span> <span data-ttu-id="d3b3c-191">範例：`Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="d3b3c-191">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="d3b3c-192">偵錯 = 1</span><span class="sxs-lookup"><span data-stu-id="d3b3c-192">Debug = 1</span></span>

  <span data-ttu-id="d3b3c-193">在開發和偵錯期間短暫有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-193">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="d3b3c-194">範例：`Entering method Configure with flag set to true.`由於 `Debug` 層級記錄的數量很大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用此記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-194">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="d3b3c-195">資訊 = 2</span><span class="sxs-lookup"><span data-stu-id="d3b3c-195">Information = 2</span></span>

  <span data-ttu-id="d3b3c-196">用於追蹤應用程式的一般流程。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-196">For tracking the general flow of the application.</span></span> <span data-ttu-id="d3b3c-197">這些記錄通常有一些長期值。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-197">These logs typically have some long-term value.</span></span> <span data-ttu-id="d3b3c-198">範例：`Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="d3b3c-198">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="d3b3c-199">警告 = 3</span><span class="sxs-lookup"><span data-stu-id="d3b3c-199">Warning = 3</span></span>

  <span data-ttu-id="d3b3c-200">在應用程式流程中發生異常或意外事件。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-200">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="d3b3c-201">這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-201">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="d3b3c-202">已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-202">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="d3b3c-203">範例：`FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="d3b3c-203">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="d3b3c-204">錯誤 = 4</span><span class="sxs-lookup"><span data-stu-id="d3b3c-204">Error = 4</span></span>

  <span data-ttu-id="d3b3c-205">發生無法處理的錯誤和例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-205">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="d3b3c-206">這些訊息會指出目前的活動或作業 (例如目前的 HTTP 要求） 中的失敗，而不是整個應用程式的失敗。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-206">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="d3b3c-207">範例記錄訊息：`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="d3b3c-207">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="d3b3c-208">重大 = 5</span><span class="sxs-lookup"><span data-stu-id="d3b3c-208">Critical = 5</span></span>

  <span data-ttu-id="d3b3c-209">發生需要立即注意的失敗。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-209">For failures that require immediate attention.</span></span> <span data-ttu-id="d3b3c-210">範例：資料遺失情況、磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-210">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="d3b3c-211">您可以使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-211">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="d3b3c-212">例如，您可能想要在生產環境中，將 `Information` 和較低層級的所有記錄移至磁碟區資料存放區，並將 `Warning` 和更高層級的所有記錄移至數值資料存放區。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-212">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="d3b3c-213">在開發期間，您可以如往常般將 `Warning` 或更高嚴重性的記錄傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-213">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="d3b3c-214">然後當您需要進行疑難排解時，您可以新增 `Debug` 層級。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-214">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="d3b3c-215">本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-215">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="d3b3c-216">ASP.NET Core 架構會寫入架構事件的 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-216">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="d3b3c-217">本文稍早的記錄範例已排除 `Information` 層級以下的記錄，因此不會顯示任何 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-217">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="d3b3c-218">如果您執行的範例應用程式已設定為顯示主控台提供者的 `Debug` 和更高層級記錄，其主控台記錄範例如下。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-218">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="d3b3c-219">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="d3b3c-219">Log event ID</span></span>

<span data-ttu-id="d3b3c-220">每次寫入記錄，您都會指定一個「事件識別碼」。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-220">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="d3b3c-221">範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行這項作業：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-221">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="d3b3c-222">事件識別碼是您可用來將兩組記錄的事件建立關聯的整數值。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-222">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="d3b3c-223">例如，將某個項目新增至購物車的記錄可以是事件識別碼 1000，而完成購買的記錄可以是事件識別碼 1001。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-223">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="d3b3c-224">在記錄輸出中，視提供者而定，事件識別碼可能會儲存在欄位中或包含在文字訊息中。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-224">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="d3b3c-225">偵錯提供者不會顯示事件識別碼，但主控台提供者會在類別後面的括弧中顯示事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-225">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="d3b3c-226">記錄訊息範本</span><span class="sxs-lookup"><span data-stu-id="d3b3c-226">Log message template</span></span>

<span data-ttu-id="d3b3c-227">每次寫入記錄訊息，您都會提供一個訊息範本。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-227">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="d3b3c-228">訊息範本可以是字串，也可以包含要置入引數值的具名預留位置。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-228">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="d3b3c-229">範本不是格式字串，而且預留位置必須命名而不是編號。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-229">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="d3b3c-230">預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-230">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="d3b3c-231">如果您有下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-231">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="d3b3c-232">產生的記錄訊息看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-232">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="d3b3c-233">記錄架構會以此方式來格式化訊息，以便記錄提供者能夠實作[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-233">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="d3b3c-234">由於引數本身會傳遞給記錄系統，而不只是格式化的訊息範本，因此除了訊息範本以外，記錄提供者還可以將參數值儲存為欄位。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-234">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="d3b3c-235">如果您要將記錄輸出導向至 Azure 資料表儲存體，您的記錄器方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-235">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="d3b3c-236">每個 Azure 資料表實體可以有 `ID` 和 `RequestTime` 屬性，以簡化記錄資料的查詢。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-236">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="d3b3c-237">您可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要剖析文字訊息中的時間。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-237">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="d3b3c-238">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="d3b3c-238">Logging exceptions</span></span>

<span data-ttu-id="d3b3c-239">記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-239">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="d3b3c-240">不同提供者處理例外狀況資訊的方式會不同。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-240">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="d3b3c-241">以下是來自上述程式碼中的偵錯提供者輸出範例。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-241">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="d3b3c-242">記錄篩選</span><span class="sxs-lookup"><span data-stu-id="d3b3c-242">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d3b3c-243">您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-243">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="d3b3c-244">最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-244">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="d3b3c-245">如果您想要隱藏所有記錄，您可以指定 `LogLevel.None` 作為最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-245">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="d3b3c-246">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-246">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="d3b3c-247">在組態中建立篩選規則</span><span class="sxs-lookup"><span data-stu-id="d3b3c-247">Create filter rules in configuration</span></span>

<span data-ttu-id="d3b3c-248">這些專案範本會建立程式碼，以呼叫 `CreateDefaultBuilder` 設定主控台和偵錯提供者的記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-248">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="d3b3c-249">`CreateDefaultBuilder` 方法也會使用如下所示的程式碼，將記錄設定為尋找 `Logging` 區段中的組態：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-249">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="d3b3c-250">組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-250">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="d3b3c-251">此 JSON 會建立六項篩選規則，一項適用於偵錯提供者、四項適用於主控台提供者，還有一項適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-251">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="d3b3c-252">您稍後會看到在建立 `ILogger` 物件時，如何為每個提供者只選擇其中一項規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-252">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="d3b3c-253">程式碼中的篩選規則</span><span class="sxs-lookup"><span data-stu-id="d3b3c-253">Filter rules in code</span></span>

<span data-ttu-id="d3b3c-254">您可以在程式碼中註冊篩選規則，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-254">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="d3b3c-255">第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-255">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="d3b3c-256">第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-256">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="d3b3c-257">如何套用篩選規則</span><span class="sxs-lookup"><span data-stu-id="d3b3c-257">How filtering rules are applied</span></span>

<span data-ttu-id="d3b3c-258">組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-258">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="d3b3c-259">前六項來自組態範例，最後兩項來自程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-259">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="d3b3c-260">number</span><span class="sxs-lookup"><span data-stu-id="d3b3c-260">Number</span></span> | <span data-ttu-id="d3b3c-261">提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-261">Provider</span></span>      | <span data-ttu-id="d3b3c-262">開頭如下的類別...</span><span class="sxs-lookup"><span data-stu-id="d3b3c-262">Categories that begin with ...</span></span>          | <span data-ttu-id="d3b3c-263">最低記錄層級</span><span class="sxs-lookup"><span data-stu-id="d3b3c-263">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="d3b3c-264">1</span><span class="sxs-lookup"><span data-stu-id="d3b3c-264">1</span></span>      | <span data-ttu-id="d3b3c-265">偵錯</span><span class="sxs-lookup"><span data-stu-id="d3b3c-265">Debug</span></span>         | <span data-ttu-id="d3b3c-266">所有類別</span><span class="sxs-lookup"><span data-stu-id="d3b3c-266">All categories</span></span>                          | <span data-ttu-id="d3b3c-267">資訊</span><span class="sxs-lookup"><span data-stu-id="d3b3c-267">Information</span></span>       |
| <span data-ttu-id="d3b3c-268">2</span><span class="sxs-lookup"><span data-stu-id="d3b3c-268">2</span></span>      | <span data-ttu-id="d3b3c-269">主控台</span><span class="sxs-lookup"><span data-stu-id="d3b3c-269">Console</span></span>       | <span data-ttu-id="d3b3c-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="d3b3c-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="d3b3c-271">警告</span><span class="sxs-lookup"><span data-stu-id="d3b3c-271">Warning</span></span>           |
| <span data-ttu-id="d3b3c-272">3</span><span class="sxs-lookup"><span data-stu-id="d3b3c-272">3</span></span>      | <span data-ttu-id="d3b3c-273">主控台</span><span class="sxs-lookup"><span data-stu-id="d3b3c-273">Console</span></span>       | <span data-ttu-id="d3b3c-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="d3b3c-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="d3b3c-275">偵錯</span><span class="sxs-lookup"><span data-stu-id="d3b3c-275">Debug</span></span>             |
| <span data-ttu-id="d3b3c-276">4</span><span class="sxs-lookup"><span data-stu-id="d3b3c-276">4</span></span>      | <span data-ttu-id="d3b3c-277">主控台</span><span class="sxs-lookup"><span data-stu-id="d3b3c-277">Console</span></span>       | <span data-ttu-id="d3b3c-278">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="d3b3c-278">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="d3b3c-279">錯誤</span><span class="sxs-lookup"><span data-stu-id="d3b3c-279">Error</span></span>             |
| <span data-ttu-id="d3b3c-280">5</span><span class="sxs-lookup"><span data-stu-id="d3b3c-280">5</span></span>      | <span data-ttu-id="d3b3c-281">主控台</span><span class="sxs-lookup"><span data-stu-id="d3b3c-281">Console</span></span>       | <span data-ttu-id="d3b3c-282">所有類別</span><span class="sxs-lookup"><span data-stu-id="d3b3c-282">All categories</span></span>                          | <span data-ttu-id="d3b3c-283">資訊</span><span class="sxs-lookup"><span data-stu-id="d3b3c-283">Information</span></span>       |
| <span data-ttu-id="d3b3c-284">6</span><span class="sxs-lookup"><span data-stu-id="d3b3c-284">6</span></span>      | <span data-ttu-id="d3b3c-285">所有提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-285">All providers</span></span> | <span data-ttu-id="d3b3c-286">所有類別</span><span class="sxs-lookup"><span data-stu-id="d3b3c-286">All categories</span></span>                          | <span data-ttu-id="d3b3c-287">偵錯</span><span class="sxs-lookup"><span data-stu-id="d3b3c-287">Debug</span></span>             |
| <span data-ttu-id="d3b3c-288">7</span><span class="sxs-lookup"><span data-stu-id="d3b3c-288">7</span></span>      | <span data-ttu-id="d3b3c-289">所有提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-289">All providers</span></span> | <span data-ttu-id="d3b3c-290">系統</span><span class="sxs-lookup"><span data-stu-id="d3b3c-290">System</span></span>                                  | <span data-ttu-id="d3b3c-291">偵錯</span><span class="sxs-lookup"><span data-stu-id="d3b3c-291">Debug</span></span>             |
| <span data-ttu-id="d3b3c-292">8</span><span class="sxs-lookup"><span data-stu-id="d3b3c-292">8</span></span>      | <span data-ttu-id="d3b3c-293">偵錯</span><span class="sxs-lookup"><span data-stu-id="d3b3c-293">Debug</span></span>         | <span data-ttu-id="d3b3c-294">Microsoft</span><span class="sxs-lookup"><span data-stu-id="d3b3c-294">Microsoft</span></span>                               | <span data-ttu-id="d3b3c-295">追蹤</span><span class="sxs-lookup"><span data-stu-id="d3b3c-295">Trace</span></span>             |

<span data-ttu-id="d3b3c-296">當您建立要用來寫入記錄的 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一項規則來套用至該記錄器。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-296">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="d3b3c-297">由該 `ILogger` 寫入的所有訊息都會根據選取的規則進行篩選。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-297">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="d3b3c-298">系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-298">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="d3b3c-299">當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-299">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="d3b3c-300">選取所有符合提供者或其別名的規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-300">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="d3b3c-301">如果找不到，請選擇提供者為空白的所有規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-301">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="d3b3c-302">從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-302">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="d3b3c-303">如果找不到，請選取未指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-303">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="d3b3c-304">如果選取了多項規則，請使用**最後**一項。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-304">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="d3b3c-305">如果未選取任何規則，請使用 `MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-305">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="d3b3c-306">例如，假設您有上述規則清單，並建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-306">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="d3b3c-307">針對偵錯提供者，規則 1、6 和 8 均適用。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-307">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="d3b3c-308">規則 8 最明確，因此會選取此規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-308">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="d3b3c-309">針對主控台提供者，規則 3、4、5 和 6 均適用。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-309">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="d3b3c-310">規則 3 最明確。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-310">Rule 3 is most specific.</span></span>

<span data-ttu-id="d3b3c-311">當您使用 `ILogger` 建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的記錄時，`Trace` 和更高層級的記錄會移至偵錯提供者，而 `Debug` 和更高層級的記錄則會移至主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-311">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="d3b3c-312">提供者別名</span><span class="sxs-lookup"><span data-stu-id="d3b3c-312">Provider aliases</span></span>

<span data-ttu-id="d3b3c-313">您可以在組態中使用類型名稱來指定提供者，但每個提供者定義了方便您使用的更短「別名」。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-313">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="d3b3c-314">針對內建提供者，請使用下列別名：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-314">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="d3b3c-315">主控台</span><span class="sxs-lookup"><span data-stu-id="d3b3c-315">Console</span></span>
* <span data-ttu-id="d3b3c-316">偵錯</span><span class="sxs-lookup"><span data-stu-id="d3b3c-316">Debug</span></span>
* <span data-ttu-id="d3b3c-317">EventLog</span><span class="sxs-lookup"><span data-stu-id="d3b3c-317">EventLog</span></span>
* <span data-ttu-id="d3b3c-318">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="d3b3c-318">AzureAppServices</span></span>
* <span data-ttu-id="d3b3c-319">TraceSource</span><span class="sxs-lookup"><span data-stu-id="d3b3c-319">TraceSource</span></span>
* <span data-ttu-id="d3b3c-320">EventSource</span><span class="sxs-lookup"><span data-stu-id="d3b3c-320">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="d3b3c-321">預設最低層級</span><span class="sxs-lookup"><span data-stu-id="d3b3c-321">Default minimum level</span></span>

<span data-ttu-id="d3b3c-322">只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-322">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="d3b3c-323">下列範例示範如何設定最低層級：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-323">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="d3b3c-324">如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-324">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="d3b3c-325">篩選函式</span><span class="sxs-lookup"><span data-stu-id="d3b3c-325">Filter functions</span></span>

<span data-ttu-id="d3b3c-326">您可以在篩選函式中撰寫程式碼來套用篩選規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-326">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="d3b3c-327">針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-327">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="d3b3c-328">函式中的程式碼可存取提供者類型、類別和記錄層級，以判斷是否應該記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-328">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="d3b3c-329">例如: </span><span class="sxs-lookup"><span data-stu-id="d3b3c-329">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d3b3c-330">某些記錄提供者可讓您指定何時應該將記錄寫入儲存媒體，何時應該根據記錄層級和類別加以略過。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-330">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="d3b3c-331">`AddConsole` 和 `AddDebug` 擴充方法提供多載，可讓您傳入篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-331">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="d3b3c-332">下列範例程式碼會使主控台提供者略過 `Warning` 層級以下的記錄，而偵錯提供者則會略過架構所建立的記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-332">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="d3b3c-333">`AddEventLog` 方法具有接受 `EventLogSettings` 執行個體的多載，其 `Filter` 屬性中可能包含篩選函式。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-333">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="d3b3c-334">TraceSource 提供者未提供上述任何多載，因為其記錄層級和其他參數會根據其所使用的 `SourceSwitch` 和 `TraceListener`。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-334">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="d3b3c-335">您可以使用 `WithFilter` 擴充方法，為 `ILoggerFactory` 執行個體中註冊的所有提供者設定篩選規則。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-335">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="d3b3c-336">下列範例會將架構記錄 (開頭為 "Microsoft" 或 "System" 的類別) 限制為警告，同時讓應用程式在偵錯層級記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-336">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="d3b3c-337">如果您想要使用篩選來防止所有記錄針對特定類別寫入，您可以指定 `LogLevel.None` 作為該類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-337">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="d3b3c-338">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-338">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="d3b3c-339">`WithFilter` 擴充方法是由 [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet 套件提供。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-339">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="d3b3c-340">此方法會傳回新的 `ILoggerFactory` 執行個體，這會篩選傳遞至用它來註冊之所有記錄器提供者的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-340">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="d3b3c-341">它不會影響任何其他 `ILoggerFactory` 執行個體，包括原始 `ILoggerFactory` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-341">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="d3b3c-342">記錄範圍</span><span class="sxs-lookup"><span data-stu-id="d3b3c-342">Log scopes</span></span>

<span data-ttu-id="d3b3c-343">您可以將某個「範圍」內的一組邏輯作業群組在一起，以便將相同的資料附加至作為該集合一部分建立的每個記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-343">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="d3b3c-344">例如，您可能想要將每個記錄建立為處理交易的一部分，以包含交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-344">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="d3b3c-345">範圍是 [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) 方法傳回的 `IDisposable` 類型，並會持續到被處置為止。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-345">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="d3b3c-346">您可以透過將記錄器呼叫包裝在 `using` 區塊中來使用範圍，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-346">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="d3b3c-347">下列程式碼會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-347">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="d3b3c-348">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-348">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="d3b3c-349">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-349">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="d3b3c-350">如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-350">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d3b3c-351">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-351">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="d3b3c-352">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-352">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d3b3c-353">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-353">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="d3b3c-354">每個記錄訊息包含範圍資訊：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-354">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="d3b3c-355">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-355">Built-in logging providers</span></span>

<span data-ttu-id="d3b3c-356">ASP.NET Core 隨附下列提供者：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-356">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="d3b3c-357">Console</span><span class="sxs-lookup"><span data-stu-id="d3b3c-357">Console</span></span>](#console-provider)
* [<span data-ttu-id="d3b3c-358">偵錯</span><span class="sxs-lookup"><span data-stu-id="d3b3c-358">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="d3b3c-359">EventSource</span><span class="sxs-lookup"><span data-stu-id="d3b3c-359">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="d3b3c-360">EventLog</span><span class="sxs-lookup"><span data-stu-id="d3b3c-360">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="d3b3c-361">TraceSource</span><span class="sxs-lookup"><span data-stu-id="d3b3c-361">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="d3b3c-362">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d3b3c-362">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="d3b3c-363">Console 提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-363">Console provider</span></span>

<span data-ttu-id="d3b3c-364">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-364">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="d3b3c-365">[AddConsole 多載](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions)可讓您傳入最低記錄層級、篩選函式，以及指出是否支援範圍的布林值。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-365">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="d3b3c-366">另一個選項是傳入可指定範圍支援和記錄層級的 `IConfiguration` 物件。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-366">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="d3b3c-367">如果您考慮在生產環境中使用主控台提供者，請注意它對效能有重大影響。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-367">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="d3b3c-368">當您在 Visual Studio 中建立新的專案時，`AddConsole` 方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-368">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="d3b3c-369">此程式碼會參考 *appSettings.json* 檔案的 `Logging` 區段：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-369">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="d3b3c-370">上述設定會將架構記錄限制為警告，同時讓應用程式在偵錯層級記錄，如[記錄篩選](#log-filtering)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-370">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="d3b3c-371">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-371">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="d3b3c-372">Debug 提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-372">Debug provider</span></span>

<span data-ttu-id="d3b3c-373">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-373">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="d3b3c-374">在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-374">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="d3b3c-375">[AddDebug 多載](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions)可讓您傳入最低記錄層級或篩選函式。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-375">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="d3b3c-376">EventSource 提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-376">EventSource provider</span></span>

<span data-ttu-id="d3b3c-377">針對以 ASP.NET Core 1.1.0 或更新版本為目標的應用程式，[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) 提供者套件可以實作事件追蹤。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-377">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="d3b3c-378">在 Windows 上，它會使用 [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-378">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="d3b3c-379">提供者可以跨平台，但目前沒有適用於 Linux 或 macOS 的事件收集和顯示工具。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-379">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

<span data-ttu-id="d3b3c-380">收集及檢視記錄的一個好方法是使用 [PerfView 公用程式](https://github.com/Microsoft/perfview)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-380">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="d3b3c-381">此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET 所發出之 ETW 事件的最佳體驗。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-381">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="d3b3c-382">若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者] 清單</span><span class="sxs-lookup"><span data-stu-id="d3b3c-382">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="d3b3c-383">(請勿遺漏字串開頭的星號)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-383">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="d3b3c-385">Windows EventLog 提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-385">Windows EventLog provider</span></span>

<span data-ttu-id="d3b3c-386">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-386">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="d3b3c-387">[AddEventLog 多載](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions)可讓您傳入 `EventLogSettings` 或最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-387">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="d3b3c-388">TraceSource 提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-388">TraceSource provider</span></span>

<span data-ttu-id="d3b3c-389">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) 程式庫和提供者。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

<span data-ttu-id="d3b3c-390">[AddTraceSource 多載](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions)可讓您傳入來源參數和追蹤接聽項。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-390">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="d3b3c-391">若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-391">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="d3b3c-392">此提供者可讓您將訊息路由傳送至各種不同的[接聽項](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-392">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="d3b3c-393">下列範例會設定 `TraceSource` 提供者，將 `Warning` 和更高層級訊息記錄至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-393">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="d3b3c-394">Azure App Service 提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-394">Azure App Service provider</span></span>

<span data-ttu-id="d3b3c-395">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-395">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="d3b3c-396">提供者套件適用於以 .NET Core 1.1 或更新版本為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-396">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d3b3c-397">若以 .NET Core 為目標，請注意下列幾點：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-397">If targeting .NET Core, note the following points:</span></span>

* <span data-ttu-id="d3b3c-398">ASP.NET Core [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)包含提供者套件，但 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)則否。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-398">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) but not in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="d3b3c-399">請勿明確呼叫 [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-399">Don't explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="d3b3c-400">當應用程式部署至 Azure App Service 時，就會自動提供此提供者給應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-400">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="d3b3c-401">若以 .NET Framework 為目標，或是參考 `Microsoft.AspNetCore.App` 中繼套件，請將提供者套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-401">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="d3b3c-402">在 [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory)執行個體上叫用 `AddAzureWebAppDiagnostics`：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-402">Invoke `AddAzureWebAppDiagnostics` on an [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) instance:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

<span data-ttu-id="d3b3c-403">[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) 多載可讓您傳入 [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings)，您可以用它來覆寫預設設定，例如記錄輸出範本、blob 名稱和檔案大小限制。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-403">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="d3b3c-404">(「輸出範本」是套用至呼叫 `ILogger` 方法時所提供記錄上方之所有記錄的訊息範本)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-404">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

<span data-ttu-id="d3b3c-405">當您部署至 App Service 應用程式時，應用程式會接受 Azure 入口網站之 [App Service] 頁面上 [[診斷記錄]](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) 區段中的設定。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-405">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="d3b3c-406">當更新這些設定時，變更會立即生效，而不需要重新啟動或重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-406">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Azure 記錄設定](index/_static/azure-logging-settings.png)

<span data-ttu-id="d3b3c-408">記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-408">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="d3b3c-409">預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-409">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="d3b3c-410">預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-410">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="d3b3c-411">如需預設行為的詳細資訊，請參閱 [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-411">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="d3b3c-412">此提供者僅適用於專案在 Azure 環境中執行的情況。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-412">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="d3b3c-413">若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-413">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="d3b3c-414">協力廠商記錄提供者</span><span class="sxs-lookup"><span data-stu-id="d3b3c-414">Third-party logging providers</span></span>

<span data-ttu-id="d3b3c-415">可搭配 ASP.NET Core 使用的協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-415">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="d3b3c-416">[elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="d3b3c-416">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="d3b3c-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="d3b3c-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="d3b3c-418">[JSNLog](http://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="d3b3c-418">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="d3b3c-419">[KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="d3b3c-419">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="d3b3c-420">[Loggr](http://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="d3b3c-420">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="d3b3c-421">[NLog](http://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="d3b3c-421">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="d3b3c-422">[Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="d3b3c-422">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="d3b3c-423">某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-423">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="d3b3c-424">使用協力廠商架構類似於使用內建的提供者之一：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-424">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="d3b3c-425">將 NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-425">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="d3b3c-426">呼叫 `ILoggerFactory` 上的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-426">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="d3b3c-427">如需詳細資訊，請參閱每個架構的文件。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-427">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="d3b3c-428">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="d3b3c-428">Azure log streaming</span></span>

<span data-ttu-id="d3b3c-429">Azure 記錄資料流可讓您即時檢視來自下列位置的記錄活動：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-429">Azure log streaming enables you to view log activity in real time from:</span></span>

* <span data-ttu-id="d3b3c-430">應用程式伺服器</span><span class="sxs-lookup"><span data-stu-id="d3b3c-430">The application server</span></span>
* <span data-ttu-id="d3b3c-431">網頁伺服器</span><span class="sxs-lookup"><span data-stu-id="d3b3c-431">The web server</span></span>
* <span data-ttu-id="d3b3c-432">失敗的要求追蹤</span><span class="sxs-lookup"><span data-stu-id="d3b3c-432">Failed request tracing</span></span>

<span data-ttu-id="d3b3c-433">若要設定 Azure 記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="d3b3c-433">To configure Azure log streaming:</span></span>

* <span data-ttu-id="d3b3c-434">從您應用程式的入口網站頁面巡覽至 [診斷記錄]</span><span class="sxs-lookup"><span data-stu-id="d3b3c-434">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="d3b3c-435">將 [應用程式記錄 (檔案系統)] 設定為 [開啟]。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-435">Set **Application Logging (Filesystem)** to on.</span></span>

![Azure 入口網站的 [診斷記錄] 頁面](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="d3b3c-437">巡覽至 [記錄資料流] 頁面以檢視應用程式訊息。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-437">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="d3b3c-438">這些是應用程式透過 `ILogger` 介面記錄的訊息。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-438">They're logged by application through the `ILogger` interface.</span></span>

![Azure 入口網站應用程式的 [記錄資料流]](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="d3b3c-440">Azure Application Insights 追蹤記錄</span><span class="sxs-lookup"><span data-stu-id="d3b3c-440">Azure Application Insights trace logging</span></span>

<span data-ttu-id="d3b3c-441">[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK 能夠從透過 ASP.NET Core 記錄基礎結構產生的記錄收集追蹤遙測。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-441">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="d3b3c-442">如需詳細資訊，請參閱 [Application Insights for ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) 及 [Microsoft/ApplicationInsights-aspnetcore Wiki：記錄](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging)。</span><span class="sxs-lookup"><span data-stu-id="d3b3c-442">For more information, see [Application Insights for ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) and [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3b3c-443">其他資源</span><span class="sxs-lookup"><span data-stu-id="d3b3c-443">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
