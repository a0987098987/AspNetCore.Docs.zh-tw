---
title: 使用 HTTP REPL 來測試 web API
author: scottaddie
description: 了解如何使用 HTTP REPL .NET Core 全域工具來瀏覽和測試 ASP.NET Core Web API。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2019
uid: web-api/http-repl
ms.openlocfilehash: 0e80fcd76a4d3efcd35140c52e0f6f0ae0f27932
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862955"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="d29f8-103">使用 HTTP REPL 來測試 web API</span><span class="sxs-lookup"><span data-stu-id="d29f8-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="d29f8-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d29f8-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d29f8-105">HTTP「讀取、求值、輸出」迴圈 (REPL) 是：</span><span class="sxs-lookup"><span data-stu-id="d29f8-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="d29f8-106">輕量型的跨平台命令列工具，其支援需求和 .NET Core 相同。</span><span class="sxs-lookup"><span data-stu-id="d29f8-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="d29f8-107">用來提出 HTTP 要求來測試 ASP.NET Core Web API (及非 ASP.NET Core 的 Web API) 並檢視其結果。</span><span class="sxs-lookup"><span data-stu-id="d29f8-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="d29f8-108">能夠測試裝載於任何環境中的 Web API，包括 localhost 和 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="d29f8-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="d29f8-109">支援的 [HTTP 動詞命令](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)如下：</span><span class="sxs-lookup"><span data-stu-id="d29f8-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="d29f8-110">DELETE</span><span class="sxs-lookup"><span data-stu-id="d29f8-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="d29f8-111">GET</span><span class="sxs-lookup"><span data-stu-id="d29f8-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="d29f8-112">HEAD</span><span class="sxs-lookup"><span data-stu-id="d29f8-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="d29f8-113">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="d29f8-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="d29f8-114">PATCH</span><span class="sxs-lookup"><span data-stu-id="d29f8-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="d29f8-115">POST</span><span class="sxs-lookup"><span data-stu-id="d29f8-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="d29f8-116">PUT</span><span class="sxs-lookup"><span data-stu-id="d29f8-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="d29f8-117">若要跟著做，[請檢視或下載範例 ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d29f8-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d29f8-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="d29f8-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="d29f8-119">安裝</span><span class="sxs-lookup"><span data-stu-id="d29f8-119">Installation</span></span>

<span data-ttu-id="d29f8-120">若要安裝 HTTP REPL，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d29f8-120">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="d29f8-121">會從 [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) \(英文\) NuGet 套件安裝 [.NET Core 全域工具](/dotnet/core/tools/global-tools#install-a-global-tool)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="d29f8-122">使用量</span><span class="sxs-lookup"><span data-stu-id="d29f8-122">Usage</span></span>

<span data-ttu-id="d29f8-123">成功安裝工具後，請執行以下命令來啟動 HTTP REPL：</span><span class="sxs-lookup"><span data-stu-id="d29f8-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="d29f8-124">若要檢視可用的 HTTP REPL 命令，請執行以下其中一個命令：</span><span class="sxs-lookup"><span data-stu-id="d29f8-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="d29f8-125">下列輸出隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="d29f8-125">The following output is displayed:</span></span>

```console
Usage:
  dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
set swagger    Sets the swagger document to use for information about the current server
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

<span data-ttu-id="d29f8-126">HTTP REPL 提供命令完成。</span><span class="sxs-lookup"><span data-stu-id="d29f8-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="d29f8-127">按 <kbd>Tab</kbd> 鍵會逐一查看完成您所鍵入之字元或 API 端點的命令清單。</span><span class="sxs-lookup"><span data-stu-id="d29f8-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="d29f8-128">下列各節將概述可用的 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="d29f8-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="d29f8-129">連線至 web API</span><span class="sxs-lookup"><span data-stu-id="d29f8-129">Connect to the web API</span></span>

<span data-ttu-id="d29f8-130">執行下列命令來連線至 web API：</span><span class="sxs-lookup"><span data-stu-id="d29f8-130">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="d29f8-131">`<BASE URI>` 是 web API 的基底 URI。</span><span class="sxs-lookup"><span data-stu-id="d29f8-131">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="d29f8-132">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-132">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="d29f8-133">或是在 HTTP REPL 執行期間執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="d29f8-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="d29f8-134">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-134">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="d29f8-135">指向 web API 的 Swagger 文件</span><span class="sxs-lookup"><span data-stu-id="d29f8-135">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="d29f8-136">若要適當地檢查 Web API，請將相對 URI 設定為 web API 的 Swagger 文件。</span><span class="sxs-lookup"><span data-stu-id="d29f8-136">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="d29f8-137">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d29f8-137">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="d29f8-138">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-138">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="d29f8-139">瀏覽 web API</span><span class="sxs-lookup"><span data-stu-id="d29f8-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="d29f8-140">檢視可用的端點</span><span class="sxs-lookup"><span data-stu-id="d29f8-140">View available endpoints</span></span>

<span data-ttu-id="d29f8-141">若要列出 web API 位址目前路徑上的不同端點 (控制器)，請執行 `ls` 或 `dir` 命令：</span><span class="sxs-lookup"><span data-stu-id="d29f8-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="d29f8-142">下列輸出格式會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="d29f8-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="d29f8-143">上述輸出代表有兩個控制器可用：`Fruits` 與 `People`。</span><span class="sxs-lookup"><span data-stu-id="d29f8-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="d29f8-144">兩個控制器均支援無參數的 HTTP GET 和 POST 作業。</span><span class="sxs-lookup"><span data-stu-id="d29f8-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="d29f8-145">瀏覽至特定控制器會顯示更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d29f8-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="d29f8-146">舉例來說，以下命令的輸出會顯示 `Fruits` 控制器也支援 HTTP GET、PUT 和 DELETE 作業。</span><span class="sxs-lookup"><span data-stu-id="d29f8-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="d29f8-147">這些作業在路由中都需要 `id` 參數：</span><span class="sxs-lookup"><span data-stu-id="d29f8-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="d29f8-148">或者，執行 `ui` 命令在瀏覽器中開啟 web API 的 Swagger UI 頁面。</span><span class="sxs-lookup"><span data-stu-id="d29f8-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="d29f8-149">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="d29f8-150">瀏覽至端點</span><span class="sxs-lookup"><span data-stu-id="d29f8-150">Navigate to an endpoint</span></span>

<span data-ttu-id="d29f8-151">若要瀏覽至 web API 上的不同端點，請執行 `cd` 命令：</span><span class="sxs-lookup"><span data-stu-id="d29f8-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="d29f8-152">接著 `cd` 命令的路徑不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d29f8-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="d29f8-153">下列輸出格式會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="d29f8-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="d29f8-154">自訂 HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="d29f8-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="d29f8-155">您可自訂 HTTP RPEL 的預設[色彩](#set-color-preferences)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="d29f8-156">此外，還可定義[預設文字編輯器](#set-the-default-text-editor)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="d29f8-157">HTTP REPL 喜好設定會存在目前的各工作階段，且會套用至後續的工作階段。</span><span class="sxs-lookup"><span data-stu-id="d29f8-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="d29f8-158">修改後，喜好設定會儲存在以下檔案中：</span><span class="sxs-lookup"><span data-stu-id="d29f8-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d29f8-159">Linux</span><span class="sxs-lookup"><span data-stu-id="d29f8-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="d29f8-160">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="d29f8-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d29f8-161">macOS</span><span class="sxs-lookup"><span data-stu-id="d29f8-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="d29f8-162">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="d29f8-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="d29f8-163">Windows</span><span class="sxs-lookup"><span data-stu-id="d29f8-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="d29f8-164">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="d29f8-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="d29f8-165">*.httpreplprefs* 檔案會於啟動時載入，且其變更不會於執行階段受到監視。</span><span class="sxs-lookup"><span data-stu-id="d29f8-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="d29f8-166">對檔案進行的手動修改只會在重新啟動工具後生效。</span><span class="sxs-lookup"><span data-stu-id="d29f8-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="d29f8-167">檢視設定</span><span class="sxs-lookup"><span data-stu-id="d29f8-167">View the settings</span></span>

<span data-ttu-id="d29f8-168">若要檢視可用的設定，請執行 `pref get` 命令。</span><span class="sxs-lookup"><span data-stu-id="d29f8-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="d29f8-169">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="d29f8-170">上述命令會顯示可用的機碼值組：</span><span class="sxs-lookup"><span data-stu-id="d29f8-170">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="d29f8-171">設定色彩喜好設定</span><span class="sxs-lookup"><span data-stu-id="d29f8-171">Set color preferences</span></span>

<span data-ttu-id="d29f8-172">目前僅為 JSON 支援回應著色。</span><span class="sxs-lookup"><span data-stu-id="d29f8-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="d29f8-173">若要自訂預設 HTTP REPL 工具著色，請找到與所要變更之色彩相對應的機碼。</span><span class="sxs-lookup"><span data-stu-id="d29f8-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="d29f8-174">如需如何尋找機碼的指示，請參閱[檢視設定](#view-the-settings)一節。</span><span class="sxs-lookup"><span data-stu-id="d29f8-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="d29f8-175">舉例來說，將 `colors.json` 機碼值從 `Green` 變更為 `White`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d29f8-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="d29f8-176">只能使用[允許的色彩](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="d29f8-177">後續的 HTTP 要求會顯示含有新著色的輸出。</span><span class="sxs-lookup"><span data-stu-id="d29f8-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="d29f8-178">未設定特定色彩機碼時，會使用較泛用的機碼。</span><span class="sxs-lookup"><span data-stu-id="d29f8-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="d29f8-179">為了示範此遞補行為，請參考以下範例：</span><span class="sxs-lookup"><span data-stu-id="d29f8-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="d29f8-180">如果 `colors.json.name` 沒有值，即使用 `colors.json.string`。</span><span class="sxs-lookup"><span data-stu-id="d29f8-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="d29f8-181">如果 `colors.json.string` 沒有值，即使用 `colors.json.literal`。</span><span class="sxs-lookup"><span data-stu-id="d29f8-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="d29f8-182">如果 `colors.json.literal` 沒有值，即使用 `colors.json`。</span><span class="sxs-lookup"><span data-stu-id="d29f8-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="d29f8-183">如果 `colors.json` 沒有值，即使用命令殼層的預設文字色彩 (`AllowedColors.None`)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="d29f8-184">設定縮排大小</span><span class="sxs-lookup"><span data-stu-id="d29f8-184">Set indentation size</span></span>

<span data-ttu-id="d29f8-185">目前僅為 JSON 支援回應縮排大小自訂。</span><span class="sxs-lookup"><span data-stu-id="d29f8-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="d29f8-186">預設大小為兩個空格。</span><span class="sxs-lookup"><span data-stu-id="d29f8-186">The default size is two spaces.</span></span> <span data-ttu-id="d29f8-187">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-187">For example:</span></span>

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

<span data-ttu-id="d29f8-188">若要變更預設大小，請設定 `formatting.json.indentSize` 機碼。</span><span class="sxs-lookup"><span data-stu-id="d29f8-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="d29f8-189">舉例來說，若要一律使用四個空格：</span><span class="sxs-lookup"><span data-stu-id="d29f8-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="d29f8-190">後續回應皆會套用四個空格的設定：</span><span class="sxs-lookup"><span data-stu-id="d29f8-190">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-indentation-size"></a><span data-ttu-id="d29f8-191">設定縮排大小</span><span class="sxs-lookup"><span data-stu-id="d29f8-191">Set indentation size</span></span>

<span data-ttu-id="d29f8-192">目前僅為 JSON 支援回應縮排大小自訂。</span><span class="sxs-lookup"><span data-stu-id="d29f8-192">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="d29f8-193">預設大小為兩個空格。</span><span class="sxs-lookup"><span data-stu-id="d29f8-193">The default size is two spaces.</span></span> <span data-ttu-id="d29f8-194">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-194">For example:</span></span>

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

<span data-ttu-id="d29f8-195">若要變更預設大小，請設定 `formatting.json.indentSize` 機碼。</span><span class="sxs-lookup"><span data-stu-id="d29f8-195">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="d29f8-196">舉例來說，若要一律使用四個空格：</span><span class="sxs-lookup"><span data-stu-id="d29f8-196">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="d29f8-197">後續回應皆會套用四個空格的設定：</span><span class="sxs-lookup"><span data-stu-id="d29f8-197">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="d29f8-198">設定預設文字編輯器</span><span class="sxs-lookup"><span data-stu-id="d29f8-198">Set the default text editor</span></span>

<span data-ttu-id="d29f8-199">根據預設，HTTP REPL 並未設定要使用的文字編輯器。</span><span class="sxs-lookup"><span data-stu-id="d29f8-199">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="d29f8-200">您必須設定預設文字編輯器，才能測試需要 HTTP 要求本文的 web API 方法。</span><span class="sxs-lookup"><span data-stu-id="d29f8-200">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="d29f8-201">HTTP REPL 工具會啟動設定的文字編輯器，僅針對撰寫要求本文的目的使用。</span><span class="sxs-lookup"><span data-stu-id="d29f8-201">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="d29f8-202">請執行以下命令，來將您偏好的文字編輯器設為預設：</span><span class="sxs-lookup"><span data-stu-id="d29f8-202">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="d29f8-203">在上述命令中，`<EXECUTABLE>` 是文字編輯器可執行檔的完整路徑。</span><span class="sxs-lookup"><span data-stu-id="d29f8-203">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="d29f8-204">舉例來說，執行以下命令將 Visual Studio Code 設為預設文字編輯器：</span><span class="sxs-lookup"><span data-stu-id="d29f8-204">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d29f8-205">Linux</span><span class="sxs-lookup"><span data-stu-id="d29f8-205">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="d29f8-206">macOS</span><span class="sxs-lookup"><span data-stu-id="d29f8-206">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="d29f8-207">Windows</span><span class="sxs-lookup"><span data-stu-id="d29f8-207">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="d29f8-208">若要以特定 CLI 引數啟動預設文字編輯器，請設定 `editor.command.default.arguments` 機碼。</span><span class="sxs-lookup"><span data-stu-id="d29f8-208">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="d29f8-209">假設 Visual Studio Code 是預設文字編輯器，且您希望 HTTP REPL 在新的工作階段開啟 Visual Studio Code，但停用延伸模組。</span><span class="sxs-lookup"><span data-stu-id="d29f8-209">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="d29f8-210">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d29f8-210">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="d29f8-211">測試 HTTP GET 要求</span><span class="sxs-lookup"><span data-stu-id="d29f8-211">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="d29f8-212">概要</span><span class="sxs-lookup"><span data-stu-id="d29f8-212">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="d29f8-213">引數</span><span class="sxs-lookup"><span data-stu-id="d29f8-213">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="d29f8-214">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-214">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="d29f8-215">選項</span><span class="sxs-lookup"><span data-stu-id="d29f8-215">Options</span></span>

<span data-ttu-id="d29f8-216">以下是使用 `get` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="d29f8-216">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="d29f8-217">範例</span><span class="sxs-lookup"><span data-stu-id="d29f8-217">Example</span></span>

<span data-ttu-id="d29f8-218">若要發出 HTTP GET 要求：</span><span class="sxs-lookup"><span data-stu-id="d29f8-218">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="d29f8-219">在支援的端點上執行 `get` 命令：</span><span class="sxs-lookup"><span data-stu-id="d29f8-219">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="d29f8-220">上述命令會顯示以下輸出格式：</span><span class="sxs-lookup"><span data-stu-id="d29f8-220">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="d29f8-221">對 `get` 命令傳遞參數來擷取特定記錄：</span><span class="sxs-lookup"><span data-stu-id="d29f8-221">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="d29f8-222">上述命令會顯示以下輸出格式：</span><span class="sxs-lookup"><span data-stu-id="d29f8-222">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="d29f8-223">測試 HTTP POST 要求</span><span class="sxs-lookup"><span data-stu-id="d29f8-223">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="d29f8-224">概要</span><span class="sxs-lookup"><span data-stu-id="d29f8-224">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="d29f8-225">引數</span><span class="sxs-lookup"><span data-stu-id="d29f8-225">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="d29f8-226">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-226">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="d29f8-227">選項</span><span class="sxs-lookup"><span data-stu-id="d29f8-227">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="d29f8-228">範例</span><span class="sxs-lookup"><span data-stu-id="d29f8-228">Example</span></span>

<span data-ttu-id="d29f8-229">若要發出 HTTP POST 要求：</span><span class="sxs-lookup"><span data-stu-id="d29f8-229">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="d29f8-230">在支援的端點上執行 `post` 命令：</span><span class="sxs-lookup"><span data-stu-id="d29f8-230">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="d29f8-231">在上述命令中，`Content-Type` HTTP 要求標頭設定為指出 JSON 類型的要求本文媒體。</span><span class="sxs-lookup"><span data-stu-id="d29f8-231">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="d29f8-232">預設文字編輯器會開啟 *.tmp* 檔案，其中包含代表 HTTP 要求本文的 JSON 範本。</span><span class="sxs-lookup"><span data-stu-id="d29f8-232">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="d29f8-233">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-233">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="d29f8-234">若要設定預設文字編輯器，請參閱[設定預設文字編輯器](#set-the-default-text-editor)一節。</span><span class="sxs-lookup"><span data-stu-id="d29f8-234">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="d29f8-235">修改 JSON 範本以滿足模型驗證需求：</span><span class="sxs-lookup"><span data-stu-id="d29f8-235">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="d29f8-236">儲存 *.tmp* 檔案，然後關閉文字編輯器。</span><span class="sxs-lookup"><span data-stu-id="d29f8-236">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="d29f8-237">下列輸出會出現在命令殼層中：</span><span class="sxs-lookup"><span data-stu-id="d29f8-237">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="d29f8-238">測試 HTTP PUT 要求</span><span class="sxs-lookup"><span data-stu-id="d29f8-238">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="d29f8-239">概要</span><span class="sxs-lookup"><span data-stu-id="d29f8-239">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="d29f8-240">引數</span><span class="sxs-lookup"><span data-stu-id="d29f8-240">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="d29f8-241">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-241">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="d29f8-242">選項</span><span class="sxs-lookup"><span data-stu-id="d29f8-242">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="d29f8-243">範例</span><span class="sxs-lookup"><span data-stu-id="d29f8-243">Example</span></span>

<span data-ttu-id="d29f8-244">若要發出 HTTP PUT 要求：</span><span class="sxs-lookup"><span data-stu-id="d29f8-244">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="d29f8-245">*選擇性*：執行 `get` 命令以在修改前檢視資料：</span><span class="sxs-lookup"><span data-stu-id="d29f8-245">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="d29f8-246">在上述命令中，`Content-Type` HTTP 要求標頭設定為指出 JSON 類型的要求本文媒體。</span><span class="sxs-lookup"><span data-stu-id="d29f8-246">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="d29f8-247">預設文字編輯器會開啟 *.tmp* 檔案，其中包含代表 HTTP 要求本文的 JSON 範本。</span><span class="sxs-lookup"><span data-stu-id="d29f8-247">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="d29f8-248">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-248">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="d29f8-249">若要設定預設文字編輯器，請參閱[設定預設文字編輯器](#set-the-default-text-editor)一節。</span><span class="sxs-lookup"><span data-stu-id="d29f8-249">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="d29f8-250">修改 JSON 範本以滿足模型驗證需求：</span><span class="sxs-lookup"><span data-stu-id="d29f8-250">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="d29f8-251">儲存 *.tmp* 檔案，然後關閉文字編輯器。</span><span class="sxs-lookup"><span data-stu-id="d29f8-251">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="d29f8-252">下列輸出會出現在命令殼層中：</span><span class="sxs-lookup"><span data-stu-id="d29f8-252">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="d29f8-253">*選擇性*：發出 `get` 命令來查看修改。</span><span class="sxs-lookup"><span data-stu-id="d29f8-253">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="d29f8-254">舉例來說，如果您在文字編輯器中鍵入 "Cherry"，`get` 會傳回以下內容：</span><span class="sxs-lookup"><span data-stu-id="d29f8-254">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="d29f8-255">測試 HTTP DELETE 要求</span><span class="sxs-lookup"><span data-stu-id="d29f8-255">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="d29f8-256">概要</span><span class="sxs-lookup"><span data-stu-id="d29f8-256">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="d29f8-257">引數</span><span class="sxs-lookup"><span data-stu-id="d29f8-257">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="d29f8-258">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-258">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="d29f8-259">選項</span><span class="sxs-lookup"><span data-stu-id="d29f8-259">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="d29f8-260">範例</span><span class="sxs-lookup"><span data-stu-id="d29f8-260">Example</span></span>

<span data-ttu-id="d29f8-261">若要發出 HTTP DELETE 要求：</span><span class="sxs-lookup"><span data-stu-id="d29f8-261">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="d29f8-262">*選擇性*：執行 `get` 命令以在修改前檢視資料：</span><span class="sxs-lookup"><span data-stu-id="d29f8-262">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="d29f8-263">上述命令會顯示以下輸出格式：</span><span class="sxs-lookup"><span data-stu-id="d29f8-263">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="d29f8-264">*選擇性*：發出 `get` 命令來查看修改。</span><span class="sxs-lookup"><span data-stu-id="d29f8-264">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="d29f8-265">在本範例中，`get` 會傳回以下內容：</span><span class="sxs-lookup"><span data-stu-id="d29f8-265">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="d29f8-266">測試 HTTP PATCH 要求</span><span class="sxs-lookup"><span data-stu-id="d29f8-266">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="d29f8-267">概要</span><span class="sxs-lookup"><span data-stu-id="d29f8-267">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="d29f8-268">引數</span><span class="sxs-lookup"><span data-stu-id="d29f8-268">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="d29f8-269">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-269">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="d29f8-270">選項</span><span class="sxs-lookup"><span data-stu-id="d29f8-270">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="d29f8-271">測試 HTTP HEAD 要求</span><span class="sxs-lookup"><span data-stu-id="d29f8-271">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="d29f8-272">概要</span><span class="sxs-lookup"><span data-stu-id="d29f8-272">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="d29f8-273">引數</span><span class="sxs-lookup"><span data-stu-id="d29f8-273">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="d29f8-274">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-274">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="d29f8-275">選項</span><span class="sxs-lookup"><span data-stu-id="d29f8-275">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="d29f8-276">測試 HTTP OPTIONS 要求</span><span class="sxs-lookup"><span data-stu-id="d29f8-276">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="d29f8-277">概要</span><span class="sxs-lookup"><span data-stu-id="d29f8-277">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="d29f8-278">引數</span><span class="sxs-lookup"><span data-stu-id="d29f8-278">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="d29f8-279">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d29f8-279">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="d29f8-280">選項</span><span class="sxs-lookup"><span data-stu-id="d29f8-280">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="d29f8-281">設定 HTTP 要求標頭</span><span class="sxs-lookup"><span data-stu-id="d29f8-281">Set HTTP request headers</span></span>

<span data-ttu-id="d29f8-282">若要設定 HTTP 要求標頭，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="d29f8-282">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="d29f8-283">與 HTTP 要求一同設定。</span><span class="sxs-lookup"><span data-stu-id="d29f8-283">Set inline with the HTTP request.</span></span> <span data-ttu-id="d29f8-284">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-284">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="d29f8-285">若使用上述方法，則各相異的 HTTP 要求標頭都需要自己的 `-h` 選項。</span><span class="sxs-lookup"><span data-stu-id="d29f8-285">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="d29f8-286">於傳送 HTTP 要求之前設定。</span><span class="sxs-lookup"><span data-stu-id="d29f8-286">Set before sending the HTTP request.</span></span> <span data-ttu-id="d29f8-287">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-287">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="d29f8-288">若在傳送要求之前設定標頭，則標頭會保留命令殼層工作階段的持續時間設定。</span><span class="sxs-lookup"><span data-stu-id="d29f8-288">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="d29f8-289">若要清除標頭，請提供空白值。</span><span class="sxs-lookup"><span data-stu-id="d29f8-289">To clear the header, provide an empty value.</span></span> <span data-ttu-id="d29f8-290">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-290">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="d29f8-291">切換 HTTP 要求顯示</span><span class="sxs-lookup"><span data-stu-id="d29f8-291">Toggle HTTP request display</span></span>

<span data-ttu-id="d29f8-292">根據預設，會隱藏所傳送之 HTTP 要求的顯示。</span><span class="sxs-lookup"><span data-stu-id="d29f8-292">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="d29f8-293">您可以針對命令殼層工作階段的持續時間變更對應的設定。</span><span class="sxs-lookup"><span data-stu-id="d29f8-293">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="d29f8-294">啟用要求顯示</span><span class="sxs-lookup"><span data-stu-id="d29f8-294">Enable request display</span></span>

<span data-ttu-id="d29f8-295">透過執行 `echo on` 命令來檢視要傳送的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="d29f8-295">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="d29f8-296">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-296">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="d29f8-297">目前工作階段中的後續 HTTP 要求會顯示要求標頭。</span><span class="sxs-lookup"><span data-stu-id="d29f8-297">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="d29f8-298">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-298">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="d29f8-299">停用要求顯示</span><span class="sxs-lookup"><span data-stu-id="d29f8-299">Disable request display</span></span>

<span data-ttu-id="d29f8-300">透過執行 `echo off` 命令來隱藏要傳送的 HTTP 要求顯示。</span><span class="sxs-lookup"><span data-stu-id="d29f8-300">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="d29f8-301">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-301">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="d29f8-302">執行指令碼</span><span class="sxs-lookup"><span data-stu-id="d29f8-302">Run a script</span></span>

<span data-ttu-id="d29f8-303">如果您經常執行一組相同的 HTTP REPL 命令，請考慮將它們儲存在文字檔中。</span><span class="sxs-lookup"><span data-stu-id="d29f8-303">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="d29f8-304">檔案中的命令會採用與手動在命令列上執行的命令相同的格式。</span><span class="sxs-lookup"><span data-stu-id="d29f8-304">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="d29f8-305">您可使用 `run` 命令以批次的方式執行命令。</span><span class="sxs-lookup"><span data-stu-id="d29f8-305">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="d29f8-306">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-306">For example:</span></span>

1. <span data-ttu-id="d29f8-307">建立包含一組以新行分隔命令的文字檔。</span><span class="sxs-lookup"><span data-stu-id="d29f8-307">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="d29f8-308">為了說明，請參考包含以下命令的 *people-script.txt* 檔案：</span><span class="sxs-lookup"><span data-stu-id="d29f8-308">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="d29f8-309">執行 `run` 命令，傳入文字檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="d29f8-309">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="d29f8-310">例如：</span><span class="sxs-lookup"><span data-stu-id="d29f8-310">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="d29f8-311">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="d29f8-311">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="d29f8-312">清除輸出</span><span class="sxs-lookup"><span data-stu-id="d29f8-312">Clear the output</span></span>

<span data-ttu-id="d29f8-313">若要移除由 HTTP REPL 工具寫入命令殼層的所有輸出，請執行 `clear` 或 `cls` 命令。</span><span class="sxs-lookup"><span data-stu-id="d29f8-313">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="d29f8-314">為了說明，請參考包含以下輸出的命令殼層：</span><span class="sxs-lookup"><span data-stu-id="d29f8-314">To illustrate, imagine the command shell contains the following output:</span></span>

```console
dotnet httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="d29f8-315">執行以下命令來清除輸出：</span><span class="sxs-lookup"><span data-stu-id="d29f8-315">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="d29f8-316">執行上述命令後，命令殼層只會包含以下輸出：</span><span class="sxs-lookup"><span data-stu-id="d29f8-316">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="d29f8-317">其他資源</span><span class="sxs-lookup"><span data-stu-id="d29f8-317">Additional resources</span></span>

* [<span data-ttu-id="d29f8-318">REST API 要求</span><span class="sxs-lookup"><span data-stu-id="d29f8-318">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="d29f8-319">HTTP REPL GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="d29f8-319">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
