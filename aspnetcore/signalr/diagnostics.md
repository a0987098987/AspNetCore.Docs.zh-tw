---
title: 記錄和診斷在 ASP.NET Core SignalR
author: anurse
description: 了解如何從您的 ASP.NET Core SignalR 應用程式收集診斷。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 06/19/2019
uid: signalr/diagnostics
ms.openlocfilehash: 69dbd057b3dcadeb3ca5d94ede1234530fb447db
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313702"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="8d55f-103">記錄和診斷在 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8d55f-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="8d55f-104">藉由[Andrew Stanton-nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="8d55f-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="8d55f-105">本文提供指引，以收集從您的 ASP.NET Core SignalR 應用程式，來協助疑難排解問題的診斷。</span><span class="sxs-lookup"><span data-stu-id="8d55f-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="8d55f-106">伺服器端記錄功能</span><span class="sxs-lookup"><span data-stu-id="8d55f-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="8d55f-107">伺服器端記錄檔可能包含從您的應用程式的機密資訊。</span><span class="sxs-lookup"><span data-stu-id="8d55f-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="8d55f-108">**永遠不會**GitHub 等公開論壇，從生產環境應用程式張貼的未經處理記錄。</span><span class="sxs-lookup"><span data-stu-id="8d55f-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="8d55f-109">SignalR 是 ASP.NET Core 的一部分，因為它會使用 ASP.NET Core 記錄系統。</span><span class="sxs-lookup"><span data-stu-id="8d55f-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="8d55f-110">在預設組態中，SignalR 資訊非常少，但這可以設定的記錄。</span><span class="sxs-lookup"><span data-stu-id="8d55f-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="8d55f-111">請參閱文件[ASP.NET Core 記錄](xref:fundamentals/logging/index#configuration)如需設定 ASP.NET Core 記錄的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="8d55f-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="8d55f-112">SignalR 使用兩個記錄器類別：</span><span class="sxs-lookup"><span data-stu-id="8d55f-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="8d55f-113">`Microsoft.AspNetCore.SignalR` &ndash; 為與中樞通訊協定相關的記錄檔，啟用中樞叫用方法和其他中樞相關的活動。</span><span class="sxs-lookup"><span data-stu-id="8d55f-113">`Microsoft.AspNetCore.SignalR` &ndash; for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="8d55f-114">`Microsoft.AspNetCore.Http.Connections` &ndash; 記錄檔傳輸 WebSockets、 等長輪詢和 Server-Sent 事件低階 SignalR infrastructure 相關。</span><span class="sxs-lookup"><span data-stu-id="8d55f-114">`Microsoft.AspNetCore.Http.Connections` &ndash; for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="8d55f-115">若要啟用詳細的記錄檔從 SignalR，設定這兩個上述的前置詞`Debug`層級中您*appsettings.json*檔案中的新增下列項目以`LogLevel`子區段中`Logging`:</span><span class="sxs-lookup"><span data-stu-id="8d55f-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="8d55f-116">您也可以設定此程式碼中您`CreateWebHostBuilder`方法：</span><span class="sxs-lookup"><span data-stu-id="8d55f-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="8d55f-117">如果您未使用 JSON 為基礎的組態，設定下列設定值，組態系統中：</span><span class="sxs-lookup"><span data-stu-id="8d55f-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="8d55f-118">請檢查您的組態系統，以判斷如何指定巢狀的組態值的文件。</span><span class="sxs-lookup"><span data-stu-id="8d55f-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="8d55f-119">例如，當使用環境變數，兩個`_`字元來取代`:`(比方說， `Logging__LogLevel__Microsoft.AspNetCore.SignalR`)。</span><span class="sxs-lookup"><span data-stu-id="8d55f-119">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="8d55f-120">我們建議使用`Debug`收集更詳細的診斷您的應用程式時，層級。</span><span class="sxs-lookup"><span data-stu-id="8d55f-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="8d55f-121">`Trace`層級會產生非常低層級的診斷和很少會需要診斷應用程式中的問題。</span><span class="sxs-lookup"><span data-stu-id="8d55f-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="8d55f-122">存取伺服器端記錄檔</span><span class="sxs-lookup"><span data-stu-id="8d55f-122">Access server-side logs</span></span>

<span data-ttu-id="8d55f-123">存取伺服器端記錄檔的方式取決於您執行環境。</span><span class="sxs-lookup"><span data-stu-id="8d55f-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="8d55f-124">IIS 外部的主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="8d55f-124">As a console app outside IIS</span></span>

<span data-ttu-id="8d55f-125">如果您執行主控台應用程式中[主控台記錄器](xref:fundamentals/logging/index#console-provider)應該依預設啟用。</span><span class="sxs-lookup"><span data-stu-id="8d55f-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="8d55f-126">SignalR 記錄會顯示在主控台中。</span><span class="sxs-lookup"><span data-stu-id="8d55f-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="8d55f-127">從 Visual Studio 的 IIS Express 中</span><span class="sxs-lookup"><span data-stu-id="8d55f-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="8d55f-128">Visual Studio 會顯示中的記錄輸出**輸出**視窗。</span><span class="sxs-lookup"><span data-stu-id="8d55f-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="8d55f-129">選取  **ASP.NET Core 網頁伺服器**下拉式清單選項。</span><span class="sxs-lookup"><span data-stu-id="8d55f-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="8d55f-130">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8d55f-130">Azure App Service</span></span>

<span data-ttu-id="8d55f-131">啟用**應用程式記錄 （檔案系統）** 選項**診斷記錄檔**Azure App Service 入口網站的區段，並設定**層級**至`Verbose`。</span><span class="sxs-lookup"><span data-stu-id="8d55f-131">Enable the **Application Logging (Filesystem)** option in the **Diagnostics logs** section of the Azure App Service portal and configure the **Level** to `Verbose`.</span></span> <span data-ttu-id="8d55f-132">記錄檔應該會出現**記錄資料流**服務，並在 App service 在檔案系統上的記錄檔中。</span><span class="sxs-lookup"><span data-stu-id="8d55f-132">Logs should be available from the **Log streaming** service and in logs on the file system of the App Service.</span></span> <span data-ttu-id="8d55f-133">如需詳細資訊，請參閱 < [Azure 記錄資料流](xref:fundamentals/logging/index#azure-log-streaming)。</span><span class="sxs-lookup"><span data-stu-id="8d55f-133">For more information, see [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="8d55f-134">其他環境</span><span class="sxs-lookup"><span data-stu-id="8d55f-134">Other environments</span></span>

<span data-ttu-id="8d55f-135">如果應用程式部署到另一個環境 （例如 Docker、 Kubernetes 或 Windows 服務），請參閱<xref:fundamentals/logging/index>如需有關如何設定適用於環境的記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="8d55f-135">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="8d55f-136">JavaScript 用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="8d55f-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="8d55f-137">用戶端記錄檔可能包含從您的應用程式的機密資訊。</span><span class="sxs-lookup"><span data-stu-id="8d55f-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="8d55f-138">**永遠不會**GitHub 等公開論壇，從生產環境應用程式張貼的未經處理記錄。</span><span class="sxs-lookup"><span data-stu-id="8d55f-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="8d55f-139">使用 JavaScript 用戶端時，您可以設定使用的記錄選項`configureLogging`方法`HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8d55f-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="8d55f-140">若要停用整個記錄，請指定`signalR.LogLevel.None`在`configureLogging`方法。</span><span class="sxs-lookup"><span data-stu-id="8d55f-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="8d55f-141">下表顯示可用的記錄層級的 JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="8d55f-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="8d55f-142">將記錄層級設定為下列值之一，可讓資料表中在它上方的所有層級，該層級的記錄。</span><span class="sxs-lookup"><span data-stu-id="8d55f-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="8d55f-143">層級</span><span class="sxs-lookup"><span data-stu-id="8d55f-143">Level</span></span> | <span data-ttu-id="8d55f-144">描述</span><span class="sxs-lookup"><span data-stu-id="8d55f-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="8d55f-145">會不記錄任何訊息。</span><span class="sxs-lookup"><span data-stu-id="8d55f-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="8d55f-146">表示在整個應用程式中的失敗的訊息。</span><span class="sxs-lookup"><span data-stu-id="8d55f-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="8d55f-147">表示目前的作業中失敗的訊息。</span><span class="sxs-lookup"><span data-stu-id="8d55f-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="8d55f-148">表示非嚴重問題的訊息。</span><span class="sxs-lookup"><span data-stu-id="8d55f-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="8d55f-149">參考用訊息。</span><span class="sxs-lookup"><span data-stu-id="8d55f-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="8d55f-150">適用於偵錯的診斷訊息。</span><span class="sxs-lookup"><span data-stu-id="8d55f-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="8d55f-151">設計用來診斷特定問題非常詳細的診斷訊息。</span><span class="sxs-lookup"><span data-stu-id="8d55f-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="8d55f-152">一旦您設定的詳細資訊，記錄檔會寫入瀏覽器主控台 （或 NodeJS 應用程式中的標準輸出）。</span><span class="sxs-lookup"><span data-stu-id="8d55f-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="8d55f-153">如果您想要將記錄傳送至自訂的記錄系統，您可以提供 JavaScript 物件，實作`ILogger`介面。</span><span class="sxs-lookup"><span data-stu-id="8d55f-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="8d55f-154">必須實作的唯一方法是`log`，它會接受事件的層級和訊息事件相關聯。</span><span class="sxs-lookup"><span data-stu-id="8d55f-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="8d55f-155">例如:</span><span class="sxs-lookup"><span data-stu-id="8d55f-155">For example:</span></span>

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="8d55f-156">.NET 用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="8d55f-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="8d55f-157">用戶端記錄檔可能包含從您的應用程式的機密資訊。</span><span class="sxs-lookup"><span data-stu-id="8d55f-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="8d55f-158">**永遠不會**GitHub 等公開論壇，從生產環境應用程式張貼的未經處理記錄。</span><span class="sxs-lookup"><span data-stu-id="8d55f-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="8d55f-159">若要從.NET 用戶端取得記錄檔，您可以使用`ConfigureLogging`方法`HubConnectionBuilder`。</span><span class="sxs-lookup"><span data-stu-id="8d55f-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="8d55f-160">其運作方式與`ConfigureLogging`方法`WebHostBuilder`和`HostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="8d55f-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="8d55f-161">ASP.NET Core 中，您可以設定您所使用的相同記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="8d55f-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="8d55f-162">不過，您必須手動安裝並啟用個別的記錄提供者 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="8d55f-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="8d55f-163">主控台記錄</span><span class="sxs-lookup"><span data-stu-id="8d55f-163">Console logging</span></span>

<span data-ttu-id="8d55f-164">若要讓主控台記錄，新增[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console)封裝。</span><span class="sxs-lookup"><span data-stu-id="8d55f-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="8d55f-165">然後，使用`AddConsole`方法來設定主控台記錄器：</span><span class="sxs-lookup"><span data-stu-id="8d55f-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="8d55f-166">偵錯輸出視窗中的記錄</span><span class="sxs-lookup"><span data-stu-id="8d55f-166">Debug output window logging</span></span>

<span data-ttu-id="8d55f-167">您也可以設定記錄移至**輸出**Visual Studio 中的視窗。</span><span class="sxs-lookup"><span data-stu-id="8d55f-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="8d55f-168">安裝[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug)封裝，然後使用`AddDebug`方法：</span><span class="sxs-lookup"><span data-stu-id="8d55f-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="8d55f-169">其他的記錄提供者</span><span class="sxs-lookup"><span data-stu-id="8d55f-169">Other logging providers</span></span>

<span data-ttu-id="8d55f-170">SignalR 支援其他的記錄提供者，例如 Serilog、 Seq、 NLog 或任何其他的記錄系統與整合`Microsoft.Extensions.Logging`。</span><span class="sxs-lookup"><span data-stu-id="8d55f-170">SignalR supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="8d55f-171">如果您的記錄系統提供`ILoggerProvider`，您也可以將它註冊`AddProvider`:</span><span class="sxs-lookup"><span data-stu-id="8d55f-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="8d55f-172">控制項的詳細資訊</span><span class="sxs-lookup"><span data-stu-id="8d55f-172">Control verbosity</span></span>

<span data-ttu-id="8d55f-173">如果您在您的應用程式記錄從其他地方，變更的預設層級`Debug`可能太詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="8d55f-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="8d55f-174">若要設定 SignalR 記錄的記錄層級，您可以使用篩選器。</span><span class="sxs-lookup"><span data-stu-id="8d55f-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="8d55f-175">這可以透過程式中大部分的程式碼，在伺服器上的相同方式來：</span><span class="sxs-lookup"><span data-stu-id="8d55f-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="8d55f-176">網路追蹤</span><span class="sxs-lookup"><span data-stu-id="8d55f-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="8d55f-177">網路追蹤中包含每個應用程式所傳送的訊息完整的內容。</span><span class="sxs-lookup"><span data-stu-id="8d55f-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="8d55f-178">**永遠不會**生產應用程式從 GitHub 等的公共論壇張貼原始的網路追蹤。</span><span class="sxs-lookup"><span data-stu-id="8d55f-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="8d55f-179">如果您遇到問題，網路追蹤有時可以提供許多有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="8d55f-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="8d55f-180">這是特別有用，如果您打算在問題追蹤程式上提出問題。</span><span class="sxs-lookup"><span data-stu-id="8d55f-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="8d55f-181">收集網路追蹤使用 Fiddler （偏好選項）</span><span class="sxs-lookup"><span data-stu-id="8d55f-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="8d55f-182">這個方法適用於所有應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d55f-182">This method works for all apps.</span></span>

<span data-ttu-id="8d55f-183">Fiddler 是功能強大的工具，來收集 HTTP 追蹤。</span><span class="sxs-lookup"><span data-stu-id="8d55f-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="8d55f-184">安裝從[telerik.com/fiddler](https://www.telerik.com/fiddler)、 啟動它，然後執行您的應用程式並重現問題。</span><span class="sxs-lookup"><span data-stu-id="8d55f-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="8d55f-185">Fiddler 是適用於 Windows，而且有適用於 macOS 和 Linux 的 beta 版。</span><span class="sxs-lookup"><span data-stu-id="8d55f-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="8d55f-186">如果您使用 HTTPS 連線，有一些額外的步驟，以確保 Fiddler 可以解密 HTTPS 流量。</span><span class="sxs-lookup"><span data-stu-id="8d55f-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="8d55f-187">如需詳細資訊，請參閱 < [Fiddler 文件](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS)。</span><span class="sxs-lookup"><span data-stu-id="8d55f-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="8d55f-188">一旦您已收集追蹤，您可以選擇匯出追蹤**檔案** > **儲存** > **所有工作階段**從功能表列。</span><span class="sxs-lookup"><span data-stu-id="8d55f-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions** from the menu bar.</span></span>

![Fiddler 從匯出的所有工作階段](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="8d55f-190">收集網路追蹤與 tcpdump （macOS 及僅限 Linux）</span><span class="sxs-lookup"><span data-stu-id="8d55f-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="8d55f-191">這個方法適用於所有應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d55f-191">This method works for all apps.</span></span>

<span data-ttu-id="8d55f-192">您可以收集未經處理的 TCP 追蹤使用 tcpdump 從命令殼層執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="8d55f-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="8d55f-193">您可能需要`root`前置詞使用的指令或`sudo`如果權限錯誤：</span><span class="sxs-lookup"><span data-stu-id="8d55f-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="8d55f-194">取代`[interface]`與您想要在擷取的網路介面。</span><span class="sxs-lookup"><span data-stu-id="8d55f-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="8d55f-195">通常，這會像下面`/dev/eth0`（適用於您標準的乙太網路介面） 或`/dev/lo0`（適用於 localhost 流量）。</span><span class="sxs-lookup"><span data-stu-id="8d55f-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="8d55f-196">如需詳細資訊，請參閱`tcpdump`手冊頁，在主機系統。</span><span class="sxs-lookup"><span data-stu-id="8d55f-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="8d55f-197">收集網路追蹤，在瀏覽器</span><span class="sxs-lookup"><span data-stu-id="8d55f-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="8d55f-198">這個方法只適用於瀏覽器為基礎的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d55f-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="8d55f-199">大部分的瀏覽器開發人員工具會有可讓您擷取瀏覽器與伺服器之間的網路活動的 [網路] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="8d55f-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="8d55f-200">不過，這些追蹤不包含 WebSocket 和 Server-Sent 事件訊息。</span><span class="sxs-lookup"><span data-stu-id="8d55f-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="8d55f-201">如果您使用這些傳輸，使用 Fiddler 或 TcpDump （如下所述） 之類的工具是較好的方法。</span><span class="sxs-lookup"><span data-stu-id="8d55f-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="8d55f-202">Microsoft Edge 及 Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="8d55f-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="8d55f-203">（指示也適用於 Edge 及 Internet Explorer）</span><span class="sxs-lookup"><span data-stu-id="8d55f-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="8d55f-204">按下 F12 以開啟 開發人員工具</span><span class="sxs-lookup"><span data-stu-id="8d55f-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="8d55f-205">按一下 [網路] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="8d55f-205">Click the Network Tab</span></span>
3. <span data-ttu-id="8d55f-206">（如有需要），請重新整理頁面並重現問題</span><span class="sxs-lookup"><span data-stu-id="8d55f-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="8d55f-207">按一下 [匯出為"HAR 」 檔案的追蹤] 工具列中的 [儲存] 圖示：</span><span class="sxs-lookup"><span data-stu-id="8d55f-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![儲存圖示上的 Microsoft Edge 開發人員工具 [網路] 索引標籤](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="8d55f-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="8d55f-209">Google Chrome</span></span>

1. <span data-ttu-id="8d55f-210">按下 F12 以開啟 開發人員工具</span><span class="sxs-lookup"><span data-stu-id="8d55f-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="8d55f-211">按一下 [網路] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="8d55f-211">Click the Network Tab</span></span>
3. <span data-ttu-id="8d55f-212">（如有需要），請重新整理頁面並重現問題</span><span class="sxs-lookup"><span data-stu-id="8d55f-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="8d55f-213">以滑鼠右鍵按一下清單中的要求的任何位置，然後選擇 「 另存為 HAR 內容 」:</span><span class="sxs-lookup"><span data-stu-id="8d55f-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Google Chrome Dev Tools 網路 索引標籤中的 「 另存為 HAR 內容 」 選項](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="8d55f-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="8d55f-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="8d55f-216">按下 F12 以開啟 開發人員工具</span><span class="sxs-lookup"><span data-stu-id="8d55f-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="8d55f-217">按一下 [網路] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="8d55f-217">Click the Network Tab</span></span>
3. <span data-ttu-id="8d55f-218">（如有需要），請重新整理頁面並重現問題</span><span class="sxs-lookup"><span data-stu-id="8d55f-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="8d55f-219">以滑鼠右鍵按一下清單中的要求的任何位置，然後選擇 「 儲存所有為 HAR"</span><span class="sxs-lookup"><span data-stu-id="8d55f-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Mozilla Firefox 開發人員工具網路 索引標籤中的 儲存所有為 HAR 」 選項](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="8d55f-221">將診斷檔案附加至 GitHub 問題</span><span class="sxs-lookup"><span data-stu-id="8d55f-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="8d55f-222">您可以將診斷檔案附加至 GitHub 問題重新命名，以便讓他們自己`.txt`延伸模組，然後拖曳和置放即可入問題。</span><span class="sxs-lookup"><span data-stu-id="8d55f-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="8d55f-223">請不要將網路追蹤或記錄檔的內容貼入 GitHub 問題。</span><span class="sxs-lookup"><span data-stu-id="8d55f-223">Please don't paste the content of log files or network traces into a GitHub issue.</span></span> <span data-ttu-id="8d55f-224">這些記錄檔和追蹤可能相當大，而且 GitHub 通常截斷。</span><span class="sxs-lookup"><span data-stu-id="8d55f-224">These logs and traces can be quite large, and GitHub usually truncates them.</span></span>

![拖曳到 GitHub 問題的記錄檔](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="8d55f-226">其他資源</span><span class="sxs-lookup"><span data-stu-id="8d55f-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
