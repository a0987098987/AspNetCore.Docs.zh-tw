---
title: 使用 HTTP REPL 來測試 web API
author: scottaddie
description: 了解如何使用 HTTP REPL .NET Core 全域工具來瀏覽和測試 ASP.NET Core Web API。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/29/2019
uid: web-api/http-repl
ms.openlocfilehash: 8ef49797fed3379e33810f311bfc474e524122e0
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082592"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="2d36a-103">使用 HTTP REPL 來測試 web API</span><span class="sxs-lookup"><span data-stu-id="2d36a-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="2d36a-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="2d36a-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="2d36a-105">HTTP「讀取、求值、輸出」迴圈 (REPL) 是：</span><span class="sxs-lookup"><span data-stu-id="2d36a-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="2d36a-106">輕量型的跨平台命令列工具，其支援需求和 .NET Core 相同。</span><span class="sxs-lookup"><span data-stu-id="2d36a-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="2d36a-107">用來提出 HTTP 要求來測試 ASP.NET Core Web API (及非 ASP.NET Core 的 Web API) 並檢視其結果。</span><span class="sxs-lookup"><span data-stu-id="2d36a-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="2d36a-108">能夠測試裝載於任何環境中的 Web API，包括 localhost 和 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="2d36a-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="2d36a-109">支援的 [HTTP 動詞命令](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)如下：</span><span class="sxs-lookup"><span data-stu-id="2d36a-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="2d36a-110">DELETE</span><span class="sxs-lookup"><span data-stu-id="2d36a-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="2d36a-111">GET</span><span class="sxs-lookup"><span data-stu-id="2d36a-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="2d36a-112">HEAD</span><span class="sxs-lookup"><span data-stu-id="2d36a-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="2d36a-113">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="2d36a-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="2d36a-114">PATCH</span><span class="sxs-lookup"><span data-stu-id="2d36a-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="2d36a-115">POST</span><span class="sxs-lookup"><span data-stu-id="2d36a-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="2d36a-116">提出</span><span class="sxs-lookup"><span data-stu-id="2d36a-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="2d36a-117">若要跟著做，[請檢視或下載範例 ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="2d36a-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d36a-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="2d36a-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="2d36a-119">安裝</span><span class="sxs-lookup"><span data-stu-id="2d36a-119">Installation</span></span>

<span data-ttu-id="2d36a-120">若要安裝 HTTP REPL，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2d36a-120">To install the HTTP REPL, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="2d36a-121">會從 [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) \(英文\) NuGet 套件安裝 [.NET Core 全域工具](/dotnet/core/tools/global-tools#install-a-global-tool)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="2d36a-122">使用量</span><span class="sxs-lookup"><span data-stu-id="2d36a-122">Usage</span></span>

<span data-ttu-id="2d36a-123">成功安裝工具後，請執行以下命令來啟動 HTTP REPL：</span><span class="sxs-lookup"><span data-stu-id="2d36a-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
httprepl
```

<span data-ttu-id="2d36a-124">若要檢視可用的 HTTP REPL 命令，請執行以下其中一個命令：</span><span class="sxs-lookup"><span data-stu-id="2d36a-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
httprepl -h
```

```console
httprepl --help
```

<span data-ttu-id="2d36a-125">下列輸出隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="2d36a-125">The following output is displayed:</span></span>

```console
Usage:
  httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

Setup Commands:
Use these commands to configure the tool for your API server

connect        Configures the directory structure and base address of the api server
set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
ls             Show all endpoints for the current path
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands
exit           Exit the shell

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\\Program Files\\Microsoft VS Code\\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line
ui             Displays the Swagger UI page, if available, in the default browser

Use `help <COMMAND>` for more detail on an individual command. e.g. `help get`.
For detailed tool info, see https://aka.ms/http-repl-doc.
```

<span data-ttu-id="2d36a-126">HTTP REPL 提供命令完成。</span><span class="sxs-lookup"><span data-stu-id="2d36a-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="2d36a-127">按 <kbd>Tab</kbd> 鍵會逐一查看完成您所鍵入之字元或 API 端點的命令清單。</span><span class="sxs-lookup"><span data-stu-id="2d36a-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="2d36a-128">下列各節將概述可用的 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="2d36a-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="2d36a-129">連線至 web API</span><span class="sxs-lookup"><span data-stu-id="2d36a-129">Connect to the web API</span></span>

<span data-ttu-id="2d36a-130">執行下列命令來連線至 web API：</span><span class="sxs-lookup"><span data-stu-id="2d36a-130">Connect to a web API by running the following command:</span></span>

```console
httprepl <ROOT URI>
```

<span data-ttu-id="2d36a-131">`<ROOT URI>` 是 web API 的基底 URI。</span><span class="sxs-lookup"><span data-stu-id="2d36a-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="2d36a-132">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-132">For example:</span></span>

```console
httprepl https://localhost:5001
```

<span data-ttu-id="2d36a-133">或是在 HTTP REPL 執行期間執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="2d36a-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="2d36a-134">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="2d36a-135">手動指向 Web API 的 Swagger 文件</span><span class="sxs-lookup"><span data-stu-id="2d36a-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="2d36a-136">上述的連線命令將會嘗試自動尋找 Swagger 文件。</span><span class="sxs-lookup"><span data-stu-id="2d36a-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="2d36a-137">如果因為某些原因而無法這麼做，您可以使用 `--swagger` 選項來指定 Web API 的 Swagger 文件 URI：</span><span class="sxs-lookup"><span data-stu-id="2d36a-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="2d36a-138">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="2d36a-139">瀏覽 web API</span><span class="sxs-lookup"><span data-stu-id="2d36a-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="2d36a-140">檢視可用的端點</span><span class="sxs-lookup"><span data-stu-id="2d36a-140">View available endpoints</span></span>

<span data-ttu-id="2d36a-141">若要列出 web API 位址目前路徑上的不同端點 (控制器)，請執行 `ls` 或 `dir` 命令：</span><span class="sxs-lookup"><span data-stu-id="2d36a-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="2d36a-142">下列輸出格式會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="2d36a-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="2d36a-143">上述輸出代表有兩個控制器可用：`Fruits` 與 `People`。</span><span class="sxs-lookup"><span data-stu-id="2d36a-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="2d36a-144">兩個控制器均支援無參數的 HTTP GET 和 POST 作業。</span><span class="sxs-lookup"><span data-stu-id="2d36a-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="2d36a-145">瀏覽至特定控制器會顯示更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="2d36a-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="2d36a-146">舉例來說，以下命令的輸出會顯示 `Fruits` 控制器也支援 HTTP GET、PUT 和 DELETE 作業。</span><span class="sxs-lookup"><span data-stu-id="2d36a-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="2d36a-147">這些作業在路由中都需要 `id` 參數：</span><span class="sxs-lookup"><span data-stu-id="2d36a-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="2d36a-148">或者，執行 `ui` 命令在瀏覽器中開啟 web API 的 Swagger UI 頁面。</span><span class="sxs-lookup"><span data-stu-id="2d36a-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="2d36a-149">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="2d36a-150">瀏覽至端點</span><span class="sxs-lookup"><span data-stu-id="2d36a-150">Navigate to an endpoint</span></span>

<span data-ttu-id="2d36a-151">若要瀏覽至 web API 上的不同端點，請執行 `cd` 命令：</span><span class="sxs-lookup"><span data-stu-id="2d36a-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="2d36a-152">接著 `cd` 命令的路徑不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="2d36a-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="2d36a-153">下列輸出格式會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="2d36a-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="2d36a-154">自訂 HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="2d36a-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="2d36a-155">您可自訂 HTTP RPEL 的預設[色彩](#set-color-preferences)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="2d36a-156">此外，還可定義[預設文字編輯器](#set-the-default-text-editor)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="2d36a-157">HTTP REPL 喜好設定會存在目前的各工作階段，且會套用至後續的工作階段。</span><span class="sxs-lookup"><span data-stu-id="2d36a-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="2d36a-158">修改後，喜好設定會儲存在以下檔案中：</span><span class="sxs-lookup"><span data-stu-id="2d36a-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="2d36a-159">Linux</span><span class="sxs-lookup"><span data-stu-id="2d36a-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="2d36a-160">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="2d36a-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="2d36a-161">macOS</span><span class="sxs-lookup"><span data-stu-id="2d36a-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="2d36a-162">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="2d36a-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2d36a-163">Windows</span><span class="sxs-lookup"><span data-stu-id="2d36a-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="2d36a-164">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="2d36a-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="2d36a-165">*.httpreplprefs* 檔案會於啟動時載入，且其變更不會於執行階段受到監視。</span><span class="sxs-lookup"><span data-stu-id="2d36a-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="2d36a-166">對檔案進行的手動修改只會在重新啟動工具後生效。</span><span class="sxs-lookup"><span data-stu-id="2d36a-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="2d36a-167">檢視設定</span><span class="sxs-lookup"><span data-stu-id="2d36a-167">View the settings</span></span>

<span data-ttu-id="2d36a-168">若要檢視可用的設定，請執行 `pref get` 命令。</span><span class="sxs-lookup"><span data-stu-id="2d36a-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="2d36a-169">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="2d36a-170">上述命令會顯示可用的機碼值組：</span><span class="sxs-lookup"><span data-stu-id="2d36a-170">The preceding command displays the available key-value pairs:</span></span>

```console
colors.json=Green
colors.json.arrayBrace=BoldCyan
colors.json.comma=BoldYellow
colors.json.name=BoldMagenta
colors.json.nameSeparator=BoldWhite
colors.json.objectBrace=Cyan
colors.protocol=BoldGreen
colors.status=BoldYellow
```

### <a name="set-color-preferences"></a><span data-ttu-id="2d36a-171">設定色彩喜好設定</span><span class="sxs-lookup"><span data-stu-id="2d36a-171">Set color preferences</span></span>

<span data-ttu-id="2d36a-172">目前僅為 JSON 支援回應著色。</span><span class="sxs-lookup"><span data-stu-id="2d36a-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="2d36a-173">若要自訂預設 HTTP REPL 工具著色，請找到與所要變更之色彩相對應的機碼。</span><span class="sxs-lookup"><span data-stu-id="2d36a-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="2d36a-174">如需如何尋找機碼的指示，請參閱[檢視設定](#view-the-settings)一節。</span><span class="sxs-lookup"><span data-stu-id="2d36a-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="2d36a-175">舉例來說，將 `colors.json` 機碼值從 `Green` 變更為 `White`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2d36a-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="2d36a-176">只能使用[允許的色彩](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="2d36a-177">後續的 HTTP 要求會顯示含有新著色的輸出。</span><span class="sxs-lookup"><span data-stu-id="2d36a-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="2d36a-178">未設定特定色彩機碼時，會使用較泛用的機碼。</span><span class="sxs-lookup"><span data-stu-id="2d36a-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="2d36a-179">為了示範此遞補行為，請參考以下範例：</span><span class="sxs-lookup"><span data-stu-id="2d36a-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="2d36a-180">如果 `colors.json.name` 沒有值，即使用 `colors.json.string`。</span><span class="sxs-lookup"><span data-stu-id="2d36a-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="2d36a-181">如果 `colors.json.string` 沒有值，即使用 `colors.json.literal`。</span><span class="sxs-lookup"><span data-stu-id="2d36a-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="2d36a-182">如果 `colors.json.literal` 沒有值，即使用 `colors.json`。</span><span class="sxs-lookup"><span data-stu-id="2d36a-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="2d36a-183">如果 `colors.json` 沒有值，即使用命令殼層的預設文字色彩 (`AllowedColors.None`)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="2d36a-184">設定縮排大小</span><span class="sxs-lookup"><span data-stu-id="2d36a-184">Set indentation size</span></span>

<span data-ttu-id="2d36a-185">目前僅為 JSON 支援回應縮排大小自訂。</span><span class="sxs-lookup"><span data-stu-id="2d36a-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="2d36a-186">預設大小為兩個空格。</span><span class="sxs-lookup"><span data-stu-id="2d36a-186">The default size is two spaces.</span></span> <span data-ttu-id="2d36a-187">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-187">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="2d36a-188">若要變更預設大小，請設定 `formatting.json.indentSize` 機碼。</span><span class="sxs-lookup"><span data-stu-id="2d36a-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="2d36a-189">舉例來說，若要一律使用四個空格：</span><span class="sxs-lookup"><span data-stu-id="2d36a-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="2d36a-190">後續回應皆會套用四個空格的設定：</span><span class="sxs-lookup"><span data-stu-id="2d36a-190">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-the-default-text-editor"></a><span data-ttu-id="2d36a-191">設定預設文字編輯器</span><span class="sxs-lookup"><span data-stu-id="2d36a-191">Set the default text editor</span></span>

<span data-ttu-id="2d36a-192">根據預設，HTTP REPL 並未設定要使用的文字編輯器。</span><span class="sxs-lookup"><span data-stu-id="2d36a-192">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="2d36a-193">您必須設定預設文字編輯器，才能測試需要 HTTP 要求本文的 web API 方法。</span><span class="sxs-lookup"><span data-stu-id="2d36a-193">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="2d36a-194">HTTP REPL 工具會啟動設定的文字編輯器，僅針對撰寫要求本文的目的使用。</span><span class="sxs-lookup"><span data-stu-id="2d36a-194">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="2d36a-195">請執行以下命令，來將您偏好的文字編輯器設為預設：</span><span class="sxs-lookup"><span data-stu-id="2d36a-195">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="2d36a-196">在上述命令中，`<EXECUTABLE>` 是文字編輯器可執行檔的完整路徑。</span><span class="sxs-lookup"><span data-stu-id="2d36a-196">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="2d36a-197">舉例來說，執行以下命令將 Visual Studio Code 設為預設文字編輯器：</span><span class="sxs-lookup"><span data-stu-id="2d36a-197">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="2d36a-198">Linux</span><span class="sxs-lookup"><span data-stu-id="2d36a-198">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2d36a-199">macOS</span><span class="sxs-lookup"><span data-stu-id="2d36a-199">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="2d36a-200">Windows</span><span class="sxs-lookup"><span data-stu-id="2d36a-200">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="2d36a-201">若要以特定 CLI 引數啟動預設文字編輯器，請設定 `editor.command.default.arguments` 機碼。</span><span class="sxs-lookup"><span data-stu-id="2d36a-201">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="2d36a-202">假設 Visual Studio Code 是預設文字編輯器，且您希望 HTTP REPL 在新的工作階段開啟 Visual Studio Code，但停用延伸模組。</span><span class="sxs-lookup"><span data-stu-id="2d36a-202">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="2d36a-203">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2d36a-203">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="2d36a-204">設定 Swagger 搜尋路徑</span><span class="sxs-lookup"><span data-stu-id="2d36a-204">Set the Swagger search paths</span></span>

<span data-ttu-id="2d36a-205">根據預設，HTTP REPL 有一組相對路徑，在執行 `connect` 命令時 (不使用 `--swagger` 選項)，HTTP REPL 會用來尋找 Swagger 文件。</span><span class="sxs-lookup"><span data-stu-id="2d36a-205">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="2d36a-206">這些相對路徑會與 `connect` 命令中指定的根路徑和基本路徑結合。</span><span class="sxs-lookup"><span data-stu-id="2d36a-206">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="2d36a-207">預設的相對路徑為：</span><span class="sxs-lookup"><span data-stu-id="2d36a-207">The default relative paths are:</span></span>

- <span data-ttu-id="2d36a-208">*swagger.json*</span><span class="sxs-lookup"><span data-stu-id="2d36a-208">*swagger.json*</span></span>
- <span data-ttu-id="2d36a-209">*swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="2d36a-209">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="2d36a-210">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="2d36a-210">*/swagger.json*</span></span>
- <span data-ttu-id="2d36a-211">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="2d36a-211">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="2d36a-212">若要在您的環境中使用一組不同的搜尋路徑，請設定 `swagger.searchPaths` 喜好設定。</span><span class="sxs-lookup"><span data-stu-id="2d36a-212">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="2d36a-213">此值必須是以管線分隔的相對路徑清單。</span><span class="sxs-lookup"><span data-stu-id="2d36a-213">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="2d36a-214">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-214">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="2d36a-215">測試 HTTP GET 要求</span><span class="sxs-lookup"><span data-stu-id="2d36a-215">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2d36a-216">概要</span><span class="sxs-lookup"><span data-stu-id="2d36a-216">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2d36a-217">引數</span><span class="sxs-lookup"><span data-stu-id="2d36a-217">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2d36a-218">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-218">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2d36a-219">選項</span><span class="sxs-lookup"><span data-stu-id="2d36a-219">Options</span></span>

<span data-ttu-id="2d36a-220">以下是使用 `get` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="2d36a-220">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="2d36a-221">範例</span><span class="sxs-lookup"><span data-stu-id="2d36a-221">Example</span></span>

<span data-ttu-id="2d36a-222">若要發出 HTTP GET 要求：</span><span class="sxs-lookup"><span data-stu-id="2d36a-222">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="2d36a-223">在支援的端點上執行 `get` 命令：</span><span class="sxs-lookup"><span data-stu-id="2d36a-223">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="2d36a-224">上述命令會顯示以下輸出格式：</span><span class="sxs-lookup"><span data-stu-id="2d36a-224">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 03:38:45 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "name": "Scott Hunter"
      },
      {
        "id": 2,
        "name": "Scott Hanselman"
      },
      {
        "id": 3,
        "name": "Scott Guthrie"
      }
    ]


    https://localhost:5001/people~
    ```

1. <span data-ttu-id="2d36a-225">對 `get` 命令傳遞參數來擷取特定記錄：</span><span class="sxs-lookup"><span data-stu-id="2d36a-225">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="2d36a-226">上述命令會顯示以下輸出格式：</span><span class="sxs-lookup"><span data-stu-id="2d36a-226">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 06:17:57 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 2,
        "name": "Scott Hanselman"
      }
    ]


    https://localhost:5001/people~
    ```

## <a name="test-http-post-requests"></a><span data-ttu-id="2d36a-227">測試 HTTP POST 要求</span><span class="sxs-lookup"><span data-stu-id="2d36a-227">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2d36a-228">概要</span><span class="sxs-lookup"><span data-stu-id="2d36a-228">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2d36a-229">引數</span><span class="sxs-lookup"><span data-stu-id="2d36a-229">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2d36a-230">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-230">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2d36a-231">選項</span><span class="sxs-lookup"><span data-stu-id="2d36a-231">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="2d36a-232">範例</span><span class="sxs-lookup"><span data-stu-id="2d36a-232">Example</span></span>

<span data-ttu-id="2d36a-233">若要發出 HTTP POST 要求：</span><span class="sxs-lookup"><span data-stu-id="2d36a-233">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="2d36a-234">在支援的端點上執行 `post` 命令：</span><span class="sxs-lookup"><span data-stu-id="2d36a-234">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="2d36a-235">在上述命令中，`Content-Type` HTTP 要求標頭設定為指出 JSON 類型的要求本文媒體。</span><span class="sxs-lookup"><span data-stu-id="2d36a-235">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="2d36a-236">預設文字編輯器會開啟 *.tmp* 檔案，其中包含代表 HTTP 要求本文的 JSON 範本。</span><span class="sxs-lookup"><span data-stu-id="2d36a-236">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="2d36a-237">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-237">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="2d36a-238">若要設定預設文字編輯器，請參閱[設定預設文字編輯器](#set-the-default-text-editor)一節。</span><span class="sxs-lookup"><span data-stu-id="2d36a-238">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="2d36a-239">修改 JSON 範本以滿足模型驗證需求：</span><span class="sxs-lookup"><span data-stu-id="2d36a-239">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="2d36a-240">儲存 *.tmp* 檔案，然後關閉文字編輯器。</span><span class="sxs-lookup"><span data-stu-id="2d36a-240">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="2d36a-241">下列輸出會出現在命令殼層中：</span><span class="sxs-lookup"><span data-stu-id="2d36a-241">The following output appears in the command shell:</span></span>

    ```console
    HTTP/1.1 201 Created
    Content-Type: application/json; charset=utf-8
    Date: Thu, 27 Jun 2019 21:24:18 GMT
    Location: https://localhost:5001/people/4
    Server: Kestrel
    Transfer-Encoding: chunked

    {
      "id": 4,
      "name": "Scott Addie"
    }


    https://localhost:5001/people~
    ```

## <a name="test-http-put-requests"></a><span data-ttu-id="2d36a-242">測試 HTTP PUT 要求</span><span class="sxs-lookup"><span data-stu-id="2d36a-242">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2d36a-243">概要</span><span class="sxs-lookup"><span data-stu-id="2d36a-243">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2d36a-244">引數</span><span class="sxs-lookup"><span data-stu-id="2d36a-244">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2d36a-245">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-245">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2d36a-246">選項</span><span class="sxs-lookup"><span data-stu-id="2d36a-246">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="2d36a-247">範例</span><span class="sxs-lookup"><span data-stu-id="2d36a-247">Example</span></span>

<span data-ttu-id="2d36a-248">若要發出 HTTP PUT 要求：</span><span class="sxs-lookup"><span data-stu-id="2d36a-248">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="2d36a-249">*選擇性*：執行 `get` 命令以在修改前檢視資料：</span><span class="sxs-lookup"><span data-stu-id="2d36a-249">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `put` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ put 2 -h Content-Type=application/json
    ```

    <span data-ttu-id="2d36a-250">在上述命令中，`Content-Type` HTTP 要求標頭設定為指出 JSON 類型的要求本文媒體。</span><span class="sxs-lookup"><span data-stu-id="2d36a-250">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="2d36a-251">預設文字編輯器會開啟 *.tmp* 檔案，其中包含代表 HTTP 要求本文的 JSON 範本。</span><span class="sxs-lookup"><span data-stu-id="2d36a-251">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="2d36a-252">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-252">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="2d36a-253">若要設定預設文字編輯器，請參閱[設定預設文字編輯器](#set-the-default-text-editor)一節。</span><span class="sxs-lookup"><span data-stu-id="2d36a-253">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="2d36a-254">修改 JSON 範本以滿足模型驗證需求：</span><span class="sxs-lookup"><span data-stu-id="2d36a-254">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="2d36a-255">儲存 *.tmp* 檔案，然後關閉文字編輯器。</span><span class="sxs-lookup"><span data-stu-id="2d36a-255">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="2d36a-256">下列輸出會出現在命令殼層中：</span><span class="sxs-lookup"><span data-stu-id="2d36a-256">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="2d36a-257">*選擇性*：發出 `get` 命令來查看修改。</span><span class="sxs-lookup"><span data-stu-id="2d36a-257">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="2d36a-258">舉例來說，如果您在文字編輯器中鍵入 "Cherry"，`get` 會傳回以下內容：</span><span class="sxs-lookup"><span data-stu-id="2d36a-258">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:08:20 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Cherry"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-delete-requests"></a><span data-ttu-id="2d36a-259">測試 HTTP DELETE 要求</span><span class="sxs-lookup"><span data-stu-id="2d36a-259">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2d36a-260">概要</span><span class="sxs-lookup"><span data-stu-id="2d36a-260">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2d36a-261">引數</span><span class="sxs-lookup"><span data-stu-id="2d36a-261">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2d36a-262">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-262">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2d36a-263">選項</span><span class="sxs-lookup"><span data-stu-id="2d36a-263">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="2d36a-264">範例</span><span class="sxs-lookup"><span data-stu-id="2d36a-264">Example</span></span>

<span data-ttu-id="2d36a-265">若要發出 HTTP DELETE 要求：</span><span class="sxs-lookup"><span data-stu-id="2d36a-265">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="2d36a-266">*選擇性*：執行 `get` 命令以在修改前檢視資料：</span><span class="sxs-lookup"><span data-stu-id="2d36a-266">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `delete` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ delete 2
    ```

    <span data-ttu-id="2d36a-267">上述命令會顯示以下輸出格式：</span><span class="sxs-lookup"><span data-stu-id="2d36a-267">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="2d36a-268">*選擇性*：發出 `get` 命令來查看修改。</span><span class="sxs-lookup"><span data-stu-id="2d36a-268">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="2d36a-269">在本範例中，`get` 會傳回以下內容：</span><span class="sxs-lookup"><span data-stu-id="2d36a-269">In this example, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:16:30 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-patch-requests"></a><span data-ttu-id="2d36a-270">測試 HTTP PATCH 要求</span><span class="sxs-lookup"><span data-stu-id="2d36a-270">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2d36a-271">概要</span><span class="sxs-lookup"><span data-stu-id="2d36a-271">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2d36a-272">引數</span><span class="sxs-lookup"><span data-stu-id="2d36a-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2d36a-273">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2d36a-274">選項</span><span class="sxs-lookup"><span data-stu-id="2d36a-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="2d36a-275">測試 HTTP HEAD 要求</span><span class="sxs-lookup"><span data-stu-id="2d36a-275">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2d36a-276">概要</span><span class="sxs-lookup"><span data-stu-id="2d36a-276">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2d36a-277">引數</span><span class="sxs-lookup"><span data-stu-id="2d36a-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2d36a-278">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2d36a-279">選項</span><span class="sxs-lookup"><span data-stu-id="2d36a-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="2d36a-280">測試 HTTP OPTIONS 要求</span><span class="sxs-lookup"><span data-stu-id="2d36a-280">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2d36a-281">概要</span><span class="sxs-lookup"><span data-stu-id="2d36a-281">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2d36a-282">引數</span><span class="sxs-lookup"><span data-stu-id="2d36a-282">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2d36a-283">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="2d36a-283">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2d36a-284">選項</span><span class="sxs-lookup"><span data-stu-id="2d36a-284">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="2d36a-285">設定 HTTP 要求標頭</span><span class="sxs-lookup"><span data-stu-id="2d36a-285">Set HTTP request headers</span></span>

<span data-ttu-id="2d36a-286">若要設定 HTTP 要求標頭，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="2d36a-286">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="2d36a-287">與 HTTP 要求一同設定。</span><span class="sxs-lookup"><span data-stu-id="2d36a-287">Set inline with the HTTP request.</span></span> <span data-ttu-id="2d36a-288">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-288">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="2d36a-289">若使用上述方法，則各相異的 HTTP 要求標頭都需要自己的 `-h` 選項。</span><span class="sxs-lookup"><span data-stu-id="2d36a-289">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="2d36a-290">於傳送 HTTP 要求之前設定。</span><span class="sxs-lookup"><span data-stu-id="2d36a-290">Set before sending the HTTP request.</span></span> <span data-ttu-id="2d36a-291">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-291">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="2d36a-292">若在傳送要求之前設定標頭，則標頭會保留命令殼層工作階段的持續時間設定。</span><span class="sxs-lookup"><span data-stu-id="2d36a-292">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="2d36a-293">若要清除標頭，請提供空白值。</span><span class="sxs-lookup"><span data-stu-id="2d36a-293">To clear the header, provide an empty value.</span></span> <span data-ttu-id="2d36a-294">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-294">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="2d36a-295">切換 HTTP 要求顯示</span><span class="sxs-lookup"><span data-stu-id="2d36a-295">Toggle HTTP request display</span></span>

<span data-ttu-id="2d36a-296">根據預設，會隱藏所傳送之 HTTP 要求的顯示。</span><span class="sxs-lookup"><span data-stu-id="2d36a-296">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="2d36a-297">您可以針對命令殼層工作階段的持續時間變更對應的設定。</span><span class="sxs-lookup"><span data-stu-id="2d36a-297">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="2d36a-298">啟用要求顯示</span><span class="sxs-lookup"><span data-stu-id="2d36a-298">Enable request display</span></span>

<span data-ttu-id="2d36a-299">透過執行 `echo on` 命令來檢視要傳送的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="2d36a-299">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="2d36a-300">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-300">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="2d36a-301">目前工作階段中的後續 HTTP 要求會顯示要求標頭。</span><span class="sxs-lookup"><span data-stu-id="2d36a-301">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="2d36a-302">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-302">For example:</span></span>

```console
https://localhost:5001/people~ post

[main 2019-06-28T18:50:11.930Z] update#setState idle
Request to https://localhost:5001...

POST /people HTTP/1.1
Content-Length: 41
Content-Type: application/json
User-Agent: HTTP-REPL

{
  "id": 0,
  "name": "Scott Addie"
}

Response from https://localhost:5001...

HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Date: Fri, 28 Jun 2019 18:50:21 GMT
Location: https://localhost:5001/people/4
Server: Kestrel
Transfer-Encoding: chunked

{
  "id": 4,
  "name": "Scott Addie"
}


https://localhost:5001/people~
```

### <a name="disable-request-display"></a><span data-ttu-id="2d36a-303">停用要求顯示</span><span class="sxs-lookup"><span data-stu-id="2d36a-303">Disable request display</span></span>

<span data-ttu-id="2d36a-304">透過執行 `echo off` 命令來隱藏要傳送的 HTTP 要求顯示。</span><span class="sxs-lookup"><span data-stu-id="2d36a-304">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="2d36a-305">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-305">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="2d36a-306">執行指令碼</span><span class="sxs-lookup"><span data-stu-id="2d36a-306">Run a script</span></span>

<span data-ttu-id="2d36a-307">如果您經常執行一組相同的 HTTP REPL 命令，請考慮將它們儲存在文字檔中。</span><span class="sxs-lookup"><span data-stu-id="2d36a-307">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="2d36a-308">檔案中的命令會採用與手動在命令列上執行的命令相同的格式。</span><span class="sxs-lookup"><span data-stu-id="2d36a-308">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="2d36a-309">您可使用 `run` 命令以批次的方式執行命令。</span><span class="sxs-lookup"><span data-stu-id="2d36a-309">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="2d36a-310">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-310">For example:</span></span>

1. <span data-ttu-id="2d36a-311">建立包含一組以新行分隔命令的文字檔。</span><span class="sxs-lookup"><span data-stu-id="2d36a-311">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="2d36a-312">為了說明，請參考包含以下命令的 *people-script.txt* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2d36a-312">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="2d36a-313">執行 `run` 命令，傳入文字檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="2d36a-313">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="2d36a-314">例如：</span><span class="sxs-lookup"><span data-stu-id="2d36a-314">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="2d36a-315">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="2d36a-315">The following output appears:</span></span>

    ```console
    https://localhost:5001/~ set base https://localhost:5001
    Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json
    
    https://localhost:5001/~ ls
    .        []
    Fruits   [get|post]
    People   [get|post]
    
    https://localhost:5001/~ cd People
    /People    [get|post]
    
    https://localhost:5001/People~ ls
    .      [get|post]
    ..     []
    {id}   [get|put|delete]
    
    https://localhost:5001/People~ get 1
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 12 Jul 2019 19:20:10 GMT
    Server: Kestrel
    Transfer-Encoding: chunked
    
    {
      "id": 1,
      "name": "Scott Hunter"
    }
    
    
    https://localhost:5001/People~
    ```

## <a name="clear-the-output"></a><span data-ttu-id="2d36a-316">清除輸出</span><span class="sxs-lookup"><span data-stu-id="2d36a-316">Clear the output</span></span>

<span data-ttu-id="2d36a-317">若要移除由 HTTP REPL 工具寫入命令殼層的所有輸出，請執行 `clear` 或 `cls` 命令。</span><span class="sxs-lookup"><span data-stu-id="2d36a-317">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="2d36a-318">為了說明，請參考包含以下輸出的命令殼層：</span><span class="sxs-lookup"><span data-stu-id="2d36a-318">To illustrate, imagine the command shell contains the following output:</span></span>

```console
httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="2d36a-319">執行以下命令來清除輸出：</span><span class="sxs-lookup"><span data-stu-id="2d36a-319">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="2d36a-320">執行上述命令後，命令殼層只會包含以下輸出：</span><span class="sxs-lookup"><span data-stu-id="2d36a-320">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="2d36a-321">其他資源</span><span class="sxs-lookup"><span data-stu-id="2d36a-321">Additional resources</span></span>

* [<span data-ttu-id="2d36a-322">REST API 要求</span><span class="sxs-lookup"><span data-stu-id="2d36a-322">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="2d36a-323">HTTP REPL GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="2d36a-323">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
