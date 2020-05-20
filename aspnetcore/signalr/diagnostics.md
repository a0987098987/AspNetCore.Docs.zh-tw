---
title: ASP.NET Core 中的記錄和診斷SignalR
author: anurse
description: 瞭解如何從您的 ASP.NET Core 應用程式收集診斷資訊 SignalR 。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 11/12/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/diagnostics
ms.openlocfilehash: 0dda4fb55b1e2275d9cdb2af0b55824b12121dee
ms.sourcegitcommit: 16b3abec1ed70f9a206f0cfa7cf6404eebaf693d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2020
ms.locfileid: "83444213"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="b89bb-103">ASP.NET Core SignalR 中的記錄和診斷</span><span class="sxs-lookup"><span data-stu-id="b89bb-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="b89bb-104">[Andrew Stanton-護士](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="b89bb-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="b89bb-105">本文提供從您的 ASP.NET Core SignalR 應用程式收集診斷資訊，以協助疑難排解問題的指引。</span><span class="sxs-lookup"><span data-stu-id="b89bb-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="b89bb-106">伺服器端記錄</span><span class="sxs-lookup"><span data-stu-id="b89bb-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="b89bb-107">伺服器端記錄可能包含來自您應用程式的機密資訊。</span><span class="sxs-lookup"><span data-stu-id="b89bb-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="b89bb-108">**絕對不要**將未經處理的記錄從生產應用程式張貼到 GitHub 之類的公用論壇。</span><span class="sxs-lookup"><span data-stu-id="b89bb-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="b89bb-109">由於 SignalR 是 ASP.NET Core 的一部分，因此它會使用 ASP.NET Core 記錄系統。</span><span class="sxs-lookup"><span data-stu-id="b89bb-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="b89bb-110">在預設設定中，SignalR 會記錄非常少的資訊，但這可加以設定。</span><span class="sxs-lookup"><span data-stu-id="b89bb-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="b89bb-111">如需設定 ASP.NET Core 記錄的詳細資訊，請參閱有關[ASP.NET Core 記錄](xref:fundamentals/logging/index#configuration)的檔。</span><span class="sxs-lookup"><span data-stu-id="b89bb-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="b89bb-112">SignalR 使用兩個記錄器類別：</span><span class="sxs-lookup"><span data-stu-id="b89bb-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="b89bb-113">`Microsoft.AspNetCore.SignalR`&ndash;適用于與中樞通訊協定相關的記錄、啟用中樞、叫用方法，以及其他中樞相關的活動。</span><span class="sxs-lookup"><span data-stu-id="b89bb-113">`Microsoft.AspNetCore.SignalR` &ndash; for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="b89bb-114">`Microsoft.AspNetCore.Http.Connections`&ndash;適用于傳輸的相關記錄，例如 websocket、長輪詢和伺服器傳送事件和低層級的 SignalR 基礎結構。</span><span class="sxs-lookup"><span data-stu-id="b89bb-114">`Microsoft.AspNetCore.Http.Connections` &ndash; for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="b89bb-115">若要從 SignalR 啟用詳細記錄，請將 `Debug` 下列專案加入至中的子區段，將上述前置詞設定為*appsettings*中的層級 `LogLevel` `Logging` ：</span><span class="sxs-lookup"><span data-stu-id="b89bb-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="b89bb-116">您也可以在方法的程式碼中進行這項設定 `CreateWebHostBuilder` ：</span><span class="sxs-lookup"><span data-stu-id="b89bb-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="b89bb-117">如果您不是使用以 JSON 為基礎的設定，請在您的配置系統中設定下列設定值：</span><span class="sxs-lookup"><span data-stu-id="b89bb-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="b89bb-118">請查看設定系統的檔，以判斷如何指定嵌套的設定值。</span><span class="sxs-lookup"><span data-stu-id="b89bb-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="b89bb-119">例如，使用環境變數時， `_` 會使用兩個字元，而不是 `:` （例如 `Logging__LogLevel__Microsoft.AspNetCore.SignalR` ）。</span><span class="sxs-lookup"><span data-stu-id="b89bb-119">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="b89bb-120">`Debug`針對您的應用程式收集更詳細的診斷資訊時，我們建議使用此層級。</span><span class="sxs-lookup"><span data-stu-id="b89bb-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="b89bb-121">`Trace`層級會產生非常低層級的診斷，而且很少需要診斷應用程式中的問題。</span><span class="sxs-lookup"><span data-stu-id="b89bb-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="b89bb-122">存取伺服器端記錄</span><span class="sxs-lookup"><span data-stu-id="b89bb-122">Access server-side logs</span></span>

<span data-ttu-id="b89bb-123">您存取伺服器端記錄的方式，取決於您執行的環境。</span><span class="sxs-lookup"><span data-stu-id="b89bb-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="b89bb-124">作為 IIS 外部的主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="b89bb-124">As a console app outside IIS</span></span>

<span data-ttu-id="b89bb-125">如果您是在主控台應用程式中執行，預設應該啟用[主控台記錄器](xref:fundamentals/logging/index#console)。</span><span class="sxs-lookup"><span data-stu-id="b89bb-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console) should be enabled by default.</span></span> <span data-ttu-id="b89bb-126">SignalR 記錄將會出現在主控台中。</span><span class="sxs-lookup"><span data-stu-id="b89bb-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="b89bb-127">從 Visual Studio 的 IIS Express 內</span><span class="sxs-lookup"><span data-stu-id="b89bb-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="b89bb-128">Visual Studio 會在 [**輸出**] 視窗中顯示記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="b89bb-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="b89bb-129">選取 [ **ASP.NET Core Web 服務器**] 下拉選項。</span><span class="sxs-lookup"><span data-stu-id="b89bb-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="b89bb-130">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b89bb-130">Azure App Service</span></span>

<span data-ttu-id="b89bb-131">在 Azure App Service 入口網站的 [**診斷記錄**] 區段中，啟用 [**應用程式記錄（Filesystem）** ] 選項，並將**層級**設定為 `Verbose` 。</span><span class="sxs-lookup"><span data-stu-id="b89bb-131">Enable the **Application Logging (Filesystem)** option in the **Diagnostics logs** section of the Azure App Service portal and configure the **Level** to `Verbose`.</span></span> <span data-ttu-id="b89bb-132">記錄檔**串流**服務以及 App Service 的檔案系統上的記錄檔中，都應該可供使用。</span><span class="sxs-lookup"><span data-stu-id="b89bb-132">Logs should be available from the **Log streaming** service and in logs on the file system of the App Service.</span></span> <span data-ttu-id="b89bb-133">如需詳細資訊，請參閱[Azure 記錄串流](xref:fundamentals/logging/index#azure-log-streaming)。</span><span class="sxs-lookup"><span data-stu-id="b89bb-133">For more information, see [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="b89bb-134">其他環境</span><span class="sxs-lookup"><span data-stu-id="b89bb-134">Other environments</span></span>

<span data-ttu-id="b89bb-135">如果應用程式部署到另一個環境（例如 Docker、Kubernetes 或 Windows 服務），請參閱，以 <xref:fundamentals/logging/index> 取得有關如何設定適用于環境之記錄提供者的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b89bb-135">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="b89bb-136">JavaScript 用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="b89bb-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="b89bb-137">用戶端記錄可能包含來自您應用程式的機密資訊。</span><span class="sxs-lookup"><span data-stu-id="b89bb-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="b89bb-138">**絕對不要**將未經處理的記錄從生產應用程式張貼到 GitHub 之類的公用論壇。</span><span class="sxs-lookup"><span data-stu-id="b89bb-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="b89bb-139">使用 JavaScript 用戶端時，您可以使用上的方法來設定記錄選項 `configureLogging` `HubConnectionBuilder` ：</span><span class="sxs-lookup"><span data-stu-id="b89bb-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="b89bb-140">若要完全停用記錄，請 `signalR.LogLevel.None` 在方法中指定 `configureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="b89bb-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="b89bb-141">下表顯示 JavaScript 用戶端可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="b89bb-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="b89bb-142">將記錄層級設定為其中一個值，即可在該層級和資料表中其上方的所有層級進行記錄。</span><span class="sxs-lookup"><span data-stu-id="b89bb-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="b89bb-143">層級</span><span class="sxs-lookup"><span data-stu-id="b89bb-143">Level</span></span> | <span data-ttu-id="b89bb-144">描述</span><span class="sxs-lookup"><span data-stu-id="b89bb-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="b89bb-145">不會記錄任何訊息。</span><span class="sxs-lookup"><span data-stu-id="b89bb-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="b89bb-146">指出整個應用程式失敗的訊息。</span><span class="sxs-lookup"><span data-stu-id="b89bb-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="b89bb-147">指出目前操作失敗的訊息。</span><span class="sxs-lookup"><span data-stu-id="b89bb-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="b89bb-148">指出非嚴重問題的訊息。</span><span class="sxs-lookup"><span data-stu-id="b89bb-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="b89bb-149">參考用訊息。</span><span class="sxs-lookup"><span data-stu-id="b89bb-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="b89bb-150">適用于偵錯工具的診斷訊息。</span><span class="sxs-lookup"><span data-stu-id="b89bb-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="b89bb-151">專為診斷特定問題而設計的詳細診斷訊息。</span><span class="sxs-lookup"><span data-stu-id="b89bb-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="b89bb-152">設定詳細資訊之後，記錄將會寫入至瀏覽器主控台（或 NodeJS 應用程式中的標準輸出）。</span><span class="sxs-lookup"><span data-stu-id="b89bb-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="b89bb-153">如果您想要將記錄檔傳送至自訂記錄系統，您可以提供執行介面的 JavaScript 物件 `ILogger` 。</span><span class="sxs-lookup"><span data-stu-id="b89bb-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="b89bb-154">唯一需要實作為的方法是 `log` ，它會接受事件的層級以及與事件相關聯的訊息。</span><span class="sxs-lookup"><span data-stu-id="b89bb-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="b89bb-155">例如：</span><span class="sxs-lookup"><span data-stu-id="b89bb-155">For example:</span></span>

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="b89bb-156">.NET 用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="b89bb-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="b89bb-157">用戶端記錄可能包含來自您應用程式的機密資訊。</span><span class="sxs-lookup"><span data-stu-id="b89bb-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="b89bb-158">**絕對不要**將未經處理的記錄從生產應用程式張貼到 GitHub 之類的公用論壇。</span><span class="sxs-lookup"><span data-stu-id="b89bb-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="b89bb-159">若要從 .NET 用戶端取得記錄，您可以 `ConfigureLogging` 在上使用方法 `HubConnectionBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="b89bb-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="b89bb-160">其運作方式與 `ConfigureLogging` 和上的方法相同 `WebHostBuilder` `HostBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="b89bb-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="b89bb-161">您可以設定您在 ASP.NET Core 中使用的相同記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="b89bb-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="b89bb-162">不過，您必須手動安裝並啟用個別記錄提供者的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="b89bb-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="b89bb-163">主控台記錄</span><span class="sxs-lookup"><span data-stu-id="b89bb-163">Console logging</span></span>

<span data-ttu-id="b89bb-164">若要啟用主控台記錄，請新增 [ [Microsoft Extensions] 主控台](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console)套件。</span><span class="sxs-lookup"><span data-stu-id="b89bb-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="b89bb-165">然後，使用 `AddConsole` 方法來設定主控台記錄器：</span><span class="sxs-lookup"><span data-stu-id="b89bb-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="b89bb-166">[調試輸出] 視窗記錄</span><span class="sxs-lookup"><span data-stu-id="b89bb-166">Debug output window logging</span></span>

<span data-ttu-id="b89bb-167">您也可以將記錄檔設定為移至 Visual Studio 中的 [**輸出**] 視窗。</span><span class="sxs-lookup"><span data-stu-id="b89bb-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="b89bb-168">安裝[Microsoft Extensions. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug)封裝，並使用 `AddDebug` 方法：</span><span class="sxs-lookup"><span data-stu-id="b89bb-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="b89bb-169">其他記錄提供者</span><span class="sxs-lookup"><span data-stu-id="b89bb-169">Other logging providers</span></span>

SignalR<span data-ttu-id="b89bb-170">支援其他記錄提供者，例如 Serilog、Seq、NLog，或與整合的任何其他記錄系統 `Microsoft.Extensions.Logging` 。</span><span class="sxs-lookup"><span data-stu-id="b89bb-170"> supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="b89bb-171">如果您的記錄系統提供 `ILoggerProvider` ，您可以向註冊 `AddProvider` ：</span><span class="sxs-lookup"><span data-stu-id="b89bb-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="b89bb-172">控制詳細資訊</span><span class="sxs-lookup"><span data-stu-id="b89bb-172">Control verbosity</span></span>

<span data-ttu-id="b89bb-173">如果您是從應用程式中的其他位置進行記錄，將預設層級變更為 `Debug` 可能會太詳細。</span><span class="sxs-lookup"><span data-stu-id="b89bb-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="b89bb-174">您可以使用篩選器來設定記錄檔的記錄層級 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="b89bb-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="b89bb-175">這可以在程式碼中完成，與伺服器上的方式大致相同：</span><span class="sxs-lookup"><span data-stu-id="b89bb-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="b89bb-176">網路追蹤</span><span class="sxs-lookup"><span data-stu-id="b89bb-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="b89bb-177">網路追蹤包含您的應用程式所傳送之每個訊息的完整內容。</span><span class="sxs-lookup"><span data-stu-id="b89bb-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="b89bb-178">**絕對不要**將來自生產應用程式的原始網路追蹤，張貼到 GitHub 等公用論壇。</span><span class="sxs-lookup"><span data-stu-id="b89bb-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="b89bb-179">如果您遇到問題，網路追蹤有時可能會提供很多有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="b89bb-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="b89bb-180">如果您要在問題追蹤程式上提出問題，這會特別有用。</span><span class="sxs-lookup"><span data-stu-id="b89bb-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="b89bb-181">使用 Fiddler 收集網路追蹤（偏好選項）</span><span class="sxs-lookup"><span data-stu-id="b89bb-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="b89bb-182">這個方法適用于所有應用程式。</span><span class="sxs-lookup"><span data-stu-id="b89bb-182">This method works for all apps.</span></span>

<span data-ttu-id="b89bb-183">Fiddler 是一種非常強大的工具，可用於收集 HTTP 追蹤。</span><span class="sxs-lookup"><span data-stu-id="b89bb-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="b89bb-184">從[telerik.com/fiddler](https://www.telerik.com/fiddler)安裝它，啟動它，然後執行您的應用程式並重現問題。</span><span class="sxs-lookup"><span data-stu-id="b89bb-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="b89bb-185">Fiddler 適用于 Windows，而且有適用于 macOS 和 Linux 的 Beta 版。</span><span class="sxs-lookup"><span data-stu-id="b89bb-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="b89bb-186">如果您使用 HTTPS 進行連接，則有一些額外的步驟可確保 Fiddler 可以解密 HTTPS 流量。</span><span class="sxs-lookup"><span data-stu-id="b89bb-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="b89bb-187">如需詳細資訊，請參閱[Fiddler 檔](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS)。</span><span class="sxs-lookup"><span data-stu-id="b89bb-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="b89bb-188">收集追蹤之後，您可以從功能表列選擇 [檔案] [ **File**  >  **儲存**  >  **所有會話**] 來匯出追蹤。</span><span class="sxs-lookup"><span data-stu-id="b89bb-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions** from the menu bar.</span></span>

![從 Fiddler 匯出所有會話](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="b89bb-190">使用 tcpdump 收集網路追蹤（僅限 macOS 和 Linux）</span><span class="sxs-lookup"><span data-stu-id="b89bb-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="b89bb-191">這個方法適用于所有應用程式。</span><span class="sxs-lookup"><span data-stu-id="b89bb-191">This method works for all apps.</span></span>

<span data-ttu-id="b89bb-192">您可以從命令 shell 執行下列命令，以使用 tcpdump 來收集原始 TCP 追蹤。</span><span class="sxs-lookup"><span data-stu-id="b89bb-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="b89bb-193">`root` `sudo` 如果您收到許可權錯誤，您可能需要使用或在命令前面加上：</span><span class="sxs-lookup"><span data-stu-id="b89bb-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="b89bb-194">將取代為 `[interface]` 您想要在其中捕獲的網路介面。</span><span class="sxs-lookup"><span data-stu-id="b89bb-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="b89bb-195">通常，這類似于 `/dev/eth0` （適用于您的標準 Ethernet 介面）或 `/dev/lo0` （適用于 localhost 流量）。</span><span class="sxs-lookup"><span data-stu-id="b89bb-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="b89bb-196">如需詳細資訊，請參閱 `tcpdump` 主機系統上的 man 頁面。</span><span class="sxs-lookup"><span data-stu-id="b89bb-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="b89bb-197">在瀏覽器中收集網路追蹤</span><span class="sxs-lookup"><span data-stu-id="b89bb-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="b89bb-198">這個方法僅適用于以瀏覽器為基礎的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b89bb-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="b89bb-199">大部分的瀏覽器開發人員工具都有一個 [網路] 索引標籤，可讓您在瀏覽器與伺服器之間捕獲網路活動。</span><span class="sxs-lookup"><span data-stu-id="b89bb-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="b89bb-200">不過，這些追蹤不會包含 WebSocket 和伺服器傳送的事件訊息。</span><span class="sxs-lookup"><span data-stu-id="b89bb-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="b89bb-201">如果您使用這些傳輸，使用 Fiddler 或 TcpDump 之類的工具（如下所述）是較好的方法。</span><span class="sxs-lookup"><span data-stu-id="b89bb-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="b89bb-202">Microsoft Edge 和 Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="b89bb-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="b89bb-203">（Edge 和 Internet Explorer 的指示都相同）</span><span class="sxs-lookup"><span data-stu-id="b89bb-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="b89bb-204">按 F12 開啟開發工具</span><span class="sxs-lookup"><span data-stu-id="b89bb-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="b89bb-205">按一下 [網路] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="b89bb-205">Click the Network Tab</span></span>
3. <span data-ttu-id="b89bb-206">重新整理頁面（如有需要）並重現問題</span><span class="sxs-lookup"><span data-stu-id="b89bb-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="b89bb-207">按一下工具列中的 [儲存] 圖示，將追蹤匯出為「HAR」檔案：</span><span class="sxs-lookup"><span data-stu-id="b89bb-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![[Microsoft Edge 開發人員工具網路] 索引標籤上的 [儲存] 圖示](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="b89bb-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="b89bb-209">Google Chrome</span></span>

1. <span data-ttu-id="b89bb-210">按 F12 開啟開發工具</span><span class="sxs-lookup"><span data-stu-id="b89bb-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="b89bb-211">按一下 [網路] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="b89bb-211">Click the Network Tab</span></span>
3. <span data-ttu-id="b89bb-212">重新整理頁面（如有需要）並重現問題</span><span class="sxs-lookup"><span data-stu-id="b89bb-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="b89bb-213">以滑鼠右鍵按一下要求清單中的任何位置，然後選擇 [以內容另存為 HAR]：</span><span class="sxs-lookup"><span data-stu-id="b89bb-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Google Chrome Dev Tools 網路索引標籤中的 [以內容另存為 HAR] 選項](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="b89bb-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="b89bb-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="b89bb-216">按 F12 開啟開發工具</span><span class="sxs-lookup"><span data-stu-id="b89bb-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="b89bb-217">按一下 [網路] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="b89bb-217">Click the Network Tab</span></span>
3. <span data-ttu-id="b89bb-218">重新整理頁面（如有需要）並重現問題</span><span class="sxs-lookup"><span data-stu-id="b89bb-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="b89bb-219">以滑鼠右鍵按一下要求清單中的任何位置，然後選擇 [全部儲存為 HAR]</span><span class="sxs-lookup"><span data-stu-id="b89bb-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Mozilla Firefox 開發工具 [網路] 索引標籤中的 [另存為 HAR] 選項](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="b89bb-221">將診斷檔案附加至 GitHub 問題</span><span class="sxs-lookup"><span data-stu-id="b89bb-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="b89bb-222">您可以藉由將診斷檔案重新命名，將它們附加至 GitHub 問題，使其具有 `.txt` 延伸模組，然後將其拖放到問題上。</span><span class="sxs-lookup"><span data-stu-id="b89bb-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="b89bb-223">請不要將記錄檔或網路追蹤的內容貼到 GitHub 問題中。</span><span class="sxs-lookup"><span data-stu-id="b89bb-223">Please don't paste the content of log files or network traces into a GitHub issue.</span></span> <span data-ttu-id="b89bb-224">這些記錄和追蹤可能很大，而且 GitHub 通常會截斷它們。</span><span class="sxs-lookup"><span data-stu-id="b89bb-224">These logs and traces can be quite large, and GitHub usually truncates them.</span></span>

![將記錄檔拖曳至 GitHub 問題](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="b89bb-226">其他資源</span><span class="sxs-lookup"><span data-stu-id="b89bb-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
