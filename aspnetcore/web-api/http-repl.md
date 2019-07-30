---
title: 使用 HTTP REPL 來測試 web API
author: scottaddie
description: 了解如何使用 HTTP REPL .NET Core 全域工具來瀏覽和測試 ASP.NET Core Web API。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2019
uid: web-api/http-repl
ms.openlocfilehash: 1ceda6182c62bb1be06cd95f14e6a46a1809253e
ms.sourcegitcommit: 059ab380744fa3be3b69aa90d431b563c57092cf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2019
ms.locfileid: "68410882"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="8cebf-103">使用 HTTP REPL 來測試 web API</span><span class="sxs-lookup"><span data-stu-id="8cebf-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="8cebf-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="8cebf-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8cebf-105">HTTP「讀取、求值、輸出」迴圈 (REPL) 是：</span><span class="sxs-lookup"><span data-stu-id="8cebf-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="8cebf-106">輕量型的跨平台命令列工具，其支援需求和 .NET Core 相同。</span><span class="sxs-lookup"><span data-stu-id="8cebf-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="8cebf-107">可用來提出 HTTP 要求來測試 ASP.NET Core web API 並檢視其結果。</span><span class="sxs-lookup"><span data-stu-id="8cebf-107">Used for making HTTP requests to test ASP.NET Core web APIs and view their results.</span></span>

<span data-ttu-id="8cebf-108">支援的 [HTTP 動詞命令](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)如下：</span><span class="sxs-lookup"><span data-stu-id="8cebf-108">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="8cebf-109">DELETE</span><span class="sxs-lookup"><span data-stu-id="8cebf-109">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="8cebf-110">GET</span><span class="sxs-lookup"><span data-stu-id="8cebf-110">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="8cebf-111">HEAD</span><span class="sxs-lookup"><span data-stu-id="8cebf-111">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="8cebf-112">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="8cebf-112">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="8cebf-113">PATCH</span><span class="sxs-lookup"><span data-stu-id="8cebf-113">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="8cebf-114">POST</span><span class="sxs-lookup"><span data-stu-id="8cebf-114">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="8cebf-115">PUT</span><span class="sxs-lookup"><span data-stu-id="8cebf-115">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="8cebf-116">若要跟著做，[請檢視或下載範例 ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="8cebf-116">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8cebf-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="8cebf-117">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="8cebf-118">安裝</span><span class="sxs-lookup"><span data-stu-id="8cebf-118">Installation</span></span>

<span data-ttu-id="8cebf-119">若要安裝 HTTP REPL，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8cebf-119">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version 3.0.0-*
```

<span data-ttu-id="8cebf-120">會從 [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) \(英文\) NuGet 套件安裝 [.NET Core 全域工具](/dotnet/core/tools/global-tools#install-a-global-tool)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-120">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="8cebf-121">使用量</span><span class="sxs-lookup"><span data-stu-id="8cebf-121">Usage</span></span>

<span data-ttu-id="8cebf-122">成功安裝工具後，請執行以下命令來啟動 HTTP REPL：</span><span class="sxs-lookup"><span data-stu-id="8cebf-122">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="8cebf-123">若要檢視可用的 HTTP REPL 命令，請執行以下其中一個命令：</span><span class="sxs-lookup"><span data-stu-id="8cebf-123">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="8cebf-124">下列輸出隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="8cebf-124">The following output is displayed:</span></span>

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

<span data-ttu-id="8cebf-125">HTTP REPL 提供命令完成。</span><span class="sxs-lookup"><span data-stu-id="8cebf-125">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="8cebf-126">按 <kbd>Tab</kbd> 鍵會逐一查看完成您所鍵入之字元或 API 端點的命令清單。</span><span class="sxs-lookup"><span data-stu-id="8cebf-126">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="8cebf-127">下列各節將概述可用的 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="8cebf-127">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="8cebf-128">連線至 web API</span><span class="sxs-lookup"><span data-stu-id="8cebf-128">Connect to the web API</span></span>

<span data-ttu-id="8cebf-129">執行下列命令來連線至 web API：</span><span class="sxs-lookup"><span data-stu-id="8cebf-129">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="8cebf-130">`<BASE URI>` 是 web API 的基底 URI。</span><span class="sxs-lookup"><span data-stu-id="8cebf-130">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="8cebf-131">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-131">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="8cebf-132">或是在 HTTP REPL 執行期間執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="8cebf-132">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="8cebf-133">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-133">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="8cebf-134">指向 web API 的 Swagger 文件</span><span class="sxs-lookup"><span data-stu-id="8cebf-134">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="8cebf-135">若要適當地檢查 Web API，請將相對 URI 設定為 web API 的 Swagger 文件。</span><span class="sxs-lookup"><span data-stu-id="8cebf-135">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="8cebf-136">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8cebf-136">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="8cebf-137">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-137">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="8cebf-138">瀏覽 web API</span><span class="sxs-lookup"><span data-stu-id="8cebf-138">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="8cebf-139">檢視可用的端點</span><span class="sxs-lookup"><span data-stu-id="8cebf-139">View available endpoints</span></span>

<span data-ttu-id="8cebf-140">若要列出 web API 位址目前路徑上的不同端點 (控制器)，請執行 `ls` 或 `dir` 命令：</span><span class="sxs-lookup"><span data-stu-id="8cebf-140">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="8cebf-141">下列輸出格式會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="8cebf-141">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="8cebf-142">上述輸出代表有兩個控制器可用：`Fruits` 與 `People`。</span><span class="sxs-lookup"><span data-stu-id="8cebf-142">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="8cebf-143">兩個控制器均支援無參數的 HTTP GET 和 POST 作業。</span><span class="sxs-lookup"><span data-stu-id="8cebf-143">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="8cebf-144">瀏覽至特定控制器會顯示更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="8cebf-144">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="8cebf-145">舉例來說，以下命令的輸出會顯示 `Fruits` 控制器也支援 HTTP GET、PUT 和 DELETE 作業。</span><span class="sxs-lookup"><span data-stu-id="8cebf-145">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="8cebf-146">這些作業在路由中都需要 `id` 參數：</span><span class="sxs-lookup"><span data-stu-id="8cebf-146">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="8cebf-147">或者，執行 `ui` 命令在瀏覽器中開啟 web API 的 Swagger UI 頁面。</span><span class="sxs-lookup"><span data-stu-id="8cebf-147">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="8cebf-148">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-148">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="8cebf-149">瀏覽至端點</span><span class="sxs-lookup"><span data-stu-id="8cebf-149">Navigate to an endpoint</span></span>

<span data-ttu-id="8cebf-150">若要瀏覽至 web API 上的不同端點，請執行 `cd` 命令：</span><span class="sxs-lookup"><span data-stu-id="8cebf-150">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="8cebf-151">接著 `cd` 命令的路徑不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="8cebf-151">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="8cebf-152">下列輸出格式會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="8cebf-152">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="8cebf-153">自訂 HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="8cebf-153">Customize the HTTP REPL</span></span>

<span data-ttu-id="8cebf-154">您可自訂 HTTP RPEL 的預設[色彩](#set-color-preferences)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-154">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="8cebf-155">此外，還可定義[預設文字編輯器](#set-the-default-text-editor)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-155">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="8cebf-156">HTTP REPL 喜好設定會存在目前的各工作階段，且會套用至後續的工作階段。</span><span class="sxs-lookup"><span data-stu-id="8cebf-156">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="8cebf-157">修改後，喜好設定會儲存在以下檔案中：</span><span class="sxs-lookup"><span data-stu-id="8cebf-157">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="8cebf-158">Linux</span><span class="sxs-lookup"><span data-stu-id="8cebf-158">Linux</span></span>](#tab/linux)

<span data-ttu-id="8cebf-159">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="8cebf-159">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="8cebf-160">macOS</span><span class="sxs-lookup"><span data-stu-id="8cebf-160">macOS</span></span>](#tab/macos)

<span data-ttu-id="8cebf-161">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="8cebf-161">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="8cebf-162">Windows</span><span class="sxs-lookup"><span data-stu-id="8cebf-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="8cebf-163">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="8cebf-163">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="8cebf-164">*.httpreplprefs* 檔案會於啟動時載入，且其變更不會於執行階段受到監視。</span><span class="sxs-lookup"><span data-stu-id="8cebf-164">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="8cebf-165">對檔案進行的手動修改只會在重新啟動工具後生效。</span><span class="sxs-lookup"><span data-stu-id="8cebf-165">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="8cebf-166">檢視設定</span><span class="sxs-lookup"><span data-stu-id="8cebf-166">View the settings</span></span>

<span data-ttu-id="8cebf-167">若要檢視可用的設定，請執行 `pref get` 命令。</span><span class="sxs-lookup"><span data-stu-id="8cebf-167">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="8cebf-168">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-168">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="8cebf-169">上述命令會顯示可用的機碼值組：</span><span class="sxs-lookup"><span data-stu-id="8cebf-169">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="8cebf-170">設定色彩喜好設定</span><span class="sxs-lookup"><span data-stu-id="8cebf-170">Set color preferences</span></span>

<span data-ttu-id="8cebf-171">目前僅為 JSON 支援回應著色。</span><span class="sxs-lookup"><span data-stu-id="8cebf-171">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="8cebf-172">若要自訂預設 HTTP REPL 工具著色，請找到與所要變更之色彩相對應的機碼。</span><span class="sxs-lookup"><span data-stu-id="8cebf-172">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="8cebf-173">如需如何尋找機碼的指示，請參閱[檢視設定](#view-the-settings)一節。</span><span class="sxs-lookup"><span data-stu-id="8cebf-173">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="8cebf-174">舉例來說，將 `colors.json` 機碼值從 `Green` 變更為 `White`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8cebf-174">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="8cebf-175">只能使用[允許的色彩](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-175">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="8cebf-176">後續的 HTTP 要求會顯示含有新著色的輸出。</span><span class="sxs-lookup"><span data-stu-id="8cebf-176">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="8cebf-177">未設定特定色彩機碼時，會使用較泛用的機碼。</span><span class="sxs-lookup"><span data-stu-id="8cebf-177">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="8cebf-178">為了示範此遞補行為，請參考以下範例：</span><span class="sxs-lookup"><span data-stu-id="8cebf-178">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="8cebf-179">如果 `colors.json.name` 沒有值，即使用 `colors.json.string`。</span><span class="sxs-lookup"><span data-stu-id="8cebf-179">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="8cebf-180">如果 `colors.json.string` 沒有值，即使用 `colors.json.literal`。</span><span class="sxs-lookup"><span data-stu-id="8cebf-180">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="8cebf-181">如果 `colors.json.literal` 沒有值，即使用 `colors.json`。</span><span class="sxs-lookup"><span data-stu-id="8cebf-181">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="8cebf-182">如果 `colors.json` 沒有值，即使用命令殼層的預設文字色彩 (`AllowedColors.None`)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-182">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="8cebf-183">設定縮排大小</span><span class="sxs-lookup"><span data-stu-id="8cebf-183">Set indentation size</span></span>

<span data-ttu-id="8cebf-184">目前僅為 JSON 支援回應縮排大小自訂。</span><span class="sxs-lookup"><span data-stu-id="8cebf-184">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="8cebf-185">預設大小為兩個空格。</span><span class="sxs-lookup"><span data-stu-id="8cebf-185">The default size is two spaces.</span></span> <span data-ttu-id="8cebf-186">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-186">For example:</span></span>

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

<span data-ttu-id="8cebf-187">若要變更預設大小，請設定 `formatting.json.indentSize` 機碼。</span><span class="sxs-lookup"><span data-stu-id="8cebf-187">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="8cebf-188">舉例來說，若要一律使用四個空格：</span><span class="sxs-lookup"><span data-stu-id="8cebf-188">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="8cebf-189">後續回應皆會套用四個空格的設定：</span><span class="sxs-lookup"><span data-stu-id="8cebf-189">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-indentation-size"></a><span data-ttu-id="8cebf-190">設定縮排大小</span><span class="sxs-lookup"><span data-stu-id="8cebf-190">Set indentation size</span></span>

<span data-ttu-id="8cebf-191">目前僅為 JSON 支援回應縮排大小自訂。</span><span class="sxs-lookup"><span data-stu-id="8cebf-191">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="8cebf-192">預設大小為兩個空格。</span><span class="sxs-lookup"><span data-stu-id="8cebf-192">The default size is two spaces.</span></span> <span data-ttu-id="8cebf-193">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-193">For example:</span></span>

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

<span data-ttu-id="8cebf-194">若要變更預設大小，請設定 `formatting.json.indentSize` 機碼。</span><span class="sxs-lookup"><span data-stu-id="8cebf-194">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="8cebf-195">舉例來說，若要一律使用四個空格：</span><span class="sxs-lookup"><span data-stu-id="8cebf-195">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="8cebf-196">後續回應皆會套用四個空格的設定：</span><span class="sxs-lookup"><span data-stu-id="8cebf-196">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="8cebf-197">設定預設文字編輯器</span><span class="sxs-lookup"><span data-stu-id="8cebf-197">Set the default text editor</span></span>

<span data-ttu-id="8cebf-198">根據預設，HTTP REPL 並未設定要使用的文字編輯器。</span><span class="sxs-lookup"><span data-stu-id="8cebf-198">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="8cebf-199">您必須設定預設文字編輯器，才能測試需要 HTTP 要求本文的 web API 方法。</span><span class="sxs-lookup"><span data-stu-id="8cebf-199">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="8cebf-200">HTTP REPL 工具會啟動設定的文字編輯器，僅針對撰寫要求本文的目的使用。</span><span class="sxs-lookup"><span data-stu-id="8cebf-200">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="8cebf-201">請執行以下命令，來將您偏好的文字編輯器設為預設：</span><span class="sxs-lookup"><span data-stu-id="8cebf-201">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="8cebf-202">在上述命令中，`<EXECUTABLE>` 是文字編輯器可執行檔的完整路徑。</span><span class="sxs-lookup"><span data-stu-id="8cebf-202">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="8cebf-203">舉例來說，執行以下命令將 Visual Studio Code 設為預設文字編輯器：</span><span class="sxs-lookup"><span data-stu-id="8cebf-203">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="8cebf-204">Linux</span><span class="sxs-lookup"><span data-stu-id="8cebf-204">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="8cebf-205">macOS</span><span class="sxs-lookup"><span data-stu-id="8cebf-205">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="8cebf-206">Windows</span><span class="sxs-lookup"><span data-stu-id="8cebf-206">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="8cebf-207">若要以特定 CLI 引數啟動預設文字編輯器，請設定 `editor.command.default.arguments` 機碼。</span><span class="sxs-lookup"><span data-stu-id="8cebf-207">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="8cebf-208">假設 Visual Studio Code 是預設文字編輯器，且您希望 HTTP REPL 在新的工作階段開啟 Visual Studio Code，但停用延伸模組。</span><span class="sxs-lookup"><span data-stu-id="8cebf-208">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="8cebf-209">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8cebf-209">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="8cebf-210">測試 HTTP GET 要求</span><span class="sxs-lookup"><span data-stu-id="8cebf-210">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="8cebf-211">概要</span><span class="sxs-lookup"><span data-stu-id="8cebf-211">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="8cebf-212">引數</span><span class="sxs-lookup"><span data-stu-id="8cebf-212">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="8cebf-213">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-213">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="8cebf-214">選項</span><span class="sxs-lookup"><span data-stu-id="8cebf-214">Options</span></span>

<span data-ttu-id="8cebf-215">以下是使用 `get` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="8cebf-215">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="8cebf-216">範例</span><span class="sxs-lookup"><span data-stu-id="8cebf-216">Example</span></span>

<span data-ttu-id="8cebf-217">若要發出 HTTP GET 要求：</span><span class="sxs-lookup"><span data-stu-id="8cebf-217">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="8cebf-218">在支援的端點上執行 `get` 命令：</span><span class="sxs-lookup"><span data-stu-id="8cebf-218">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="8cebf-219">上述命令會顯示以下輸出格式：</span><span class="sxs-lookup"><span data-stu-id="8cebf-219">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="8cebf-220">對 `get` 命令傳遞參數來擷取特定記錄：</span><span class="sxs-lookup"><span data-stu-id="8cebf-220">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="8cebf-221">上述命令會顯示以下輸出格式：</span><span class="sxs-lookup"><span data-stu-id="8cebf-221">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="8cebf-222">測試 HTTP POST 要求</span><span class="sxs-lookup"><span data-stu-id="8cebf-222">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="8cebf-223">概要</span><span class="sxs-lookup"><span data-stu-id="8cebf-223">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="8cebf-224">引數</span><span class="sxs-lookup"><span data-stu-id="8cebf-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="8cebf-225">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="8cebf-226">選項</span><span class="sxs-lookup"><span data-stu-id="8cebf-226">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="8cebf-227">範例</span><span class="sxs-lookup"><span data-stu-id="8cebf-227">Example</span></span>

<span data-ttu-id="8cebf-228">若要發出 HTTP POST 要求：</span><span class="sxs-lookup"><span data-stu-id="8cebf-228">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="8cebf-229">在支援的端點上執行 `post` 命令：</span><span class="sxs-lookup"><span data-stu-id="8cebf-229">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="8cebf-230">在上述命令中，`Content-Type` HTTP 要求標頭設定為指出 JSON 類型的要求本文媒體。</span><span class="sxs-lookup"><span data-stu-id="8cebf-230">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="8cebf-231">預設文字編輯器會開啟 *.tmp* 檔案，其中包含代表 HTTP 要求本文的 JSON 範本。</span><span class="sxs-lookup"><span data-stu-id="8cebf-231">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="8cebf-232">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-232">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="8cebf-233">若要設定預設文字編輯器，請參閱[設定預設文字編輯器](#set-the-default-text-editor)一節。</span><span class="sxs-lookup"><span data-stu-id="8cebf-233">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="8cebf-234">修改 JSON 範本以滿足模型驗證需求：</span><span class="sxs-lookup"><span data-stu-id="8cebf-234">Modify the JSON template to satisfy model validation requirements:</span></span>

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. <span data-ttu-id="8cebf-235">儲存 *.tmp* 檔案，然後關閉文字編輯器。</span><span class="sxs-lookup"><span data-stu-id="8cebf-235">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="8cebf-236">下列輸出會出現在命令殼層中：</span><span class="sxs-lookup"><span data-stu-id="8cebf-236">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="8cebf-237">測試 HTTP PUT 要求</span><span class="sxs-lookup"><span data-stu-id="8cebf-237">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="8cebf-238">概要</span><span class="sxs-lookup"><span data-stu-id="8cebf-238">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="8cebf-239">引數</span><span class="sxs-lookup"><span data-stu-id="8cebf-239">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="8cebf-240">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-240">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="8cebf-241">選項</span><span class="sxs-lookup"><span data-stu-id="8cebf-241">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="8cebf-242">範例</span><span class="sxs-lookup"><span data-stu-id="8cebf-242">Example</span></span>

<span data-ttu-id="8cebf-243">若要發出 HTTP PUT 要求：</span><span class="sxs-lookup"><span data-stu-id="8cebf-243">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="8cebf-244">*選擇性*：執行 `get` 命令以在修改前檢視資料：</span><span class="sxs-lookup"><span data-stu-id="8cebf-244">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="8cebf-245">在上述命令中，`Content-Type` HTTP 要求標頭設定為指出 JSON 類型的要求本文媒體。</span><span class="sxs-lookup"><span data-stu-id="8cebf-245">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="8cebf-246">預設文字編輯器會開啟 *.tmp* 檔案，其中包含代表 HTTP 要求本文的 JSON 範本。</span><span class="sxs-lookup"><span data-stu-id="8cebf-246">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="8cebf-247">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-247">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="8cebf-248">若要設定預設文字編輯器，請參閱[設定預設文字編輯器](#set-the-default-text-editor)一節。</span><span class="sxs-lookup"><span data-stu-id="8cebf-248">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="8cebf-249">修改 JSON 範本以滿足模型驗證需求：</span><span class="sxs-lookup"><span data-stu-id="8cebf-249">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="8cebf-250">儲存 *.tmp* 檔案，然後關閉文字編輯器。</span><span class="sxs-lookup"><span data-stu-id="8cebf-250">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="8cebf-251">下列輸出會出現在命令殼層中：</span><span class="sxs-lookup"><span data-stu-id="8cebf-251">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="8cebf-252">*選擇性*：發出 `get` 命令來查看修改。</span><span class="sxs-lookup"><span data-stu-id="8cebf-252">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="8cebf-253">舉例來說，如果您在文字編輯器中鍵入 "Cherry"，`get` 會傳回以下內容：</span><span class="sxs-lookup"><span data-stu-id="8cebf-253">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="8cebf-254">測試 HTTP DELETE 要求</span><span class="sxs-lookup"><span data-stu-id="8cebf-254">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="8cebf-255">概要</span><span class="sxs-lookup"><span data-stu-id="8cebf-255">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="8cebf-256">引數</span><span class="sxs-lookup"><span data-stu-id="8cebf-256">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="8cebf-257">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-257">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="8cebf-258">選項</span><span class="sxs-lookup"><span data-stu-id="8cebf-258">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="8cebf-259">範例</span><span class="sxs-lookup"><span data-stu-id="8cebf-259">Example</span></span>

<span data-ttu-id="8cebf-260">若要發出 HTTP DELETE 要求：</span><span class="sxs-lookup"><span data-stu-id="8cebf-260">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="8cebf-261">*選擇性*：執行 `get` 命令以在修改前檢視資料：</span><span class="sxs-lookup"><span data-stu-id="8cebf-261">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="8cebf-262">上述命令會顯示以下輸出格式：</span><span class="sxs-lookup"><span data-stu-id="8cebf-262">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="8cebf-263">*選擇性*：發出 `get` 命令來查看修改。</span><span class="sxs-lookup"><span data-stu-id="8cebf-263">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="8cebf-264">在本範例中，`get` 會傳回以下內容：</span><span class="sxs-lookup"><span data-stu-id="8cebf-264">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="8cebf-265">測試 HTTP PATCH 要求</span><span class="sxs-lookup"><span data-stu-id="8cebf-265">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="8cebf-266">概要</span><span class="sxs-lookup"><span data-stu-id="8cebf-266">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="8cebf-267">引數</span><span class="sxs-lookup"><span data-stu-id="8cebf-267">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="8cebf-268">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-268">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="8cebf-269">選項</span><span class="sxs-lookup"><span data-stu-id="8cebf-269">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="8cebf-270">測試 HTTP HEAD 要求</span><span class="sxs-lookup"><span data-stu-id="8cebf-270">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="8cebf-271">概要</span><span class="sxs-lookup"><span data-stu-id="8cebf-271">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="8cebf-272">引數</span><span class="sxs-lookup"><span data-stu-id="8cebf-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="8cebf-273">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="8cebf-274">選項</span><span class="sxs-lookup"><span data-stu-id="8cebf-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="8cebf-275">測試 HTTP OPTIONS 要求</span><span class="sxs-lookup"><span data-stu-id="8cebf-275">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="8cebf-276">概要</span><span class="sxs-lookup"><span data-stu-id="8cebf-276">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="8cebf-277">引數</span><span class="sxs-lookup"><span data-stu-id="8cebf-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="8cebf-278">相關控制器動作方法預期的路由參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="8cebf-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="8cebf-279">選項</span><span class="sxs-lookup"><span data-stu-id="8cebf-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="8cebf-280">設定 HTTP 要求標頭</span><span class="sxs-lookup"><span data-stu-id="8cebf-280">Set HTTP request headers</span></span>

<span data-ttu-id="8cebf-281">若要設定 HTTP 要求標頭，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="8cebf-281">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="8cebf-282">與 HTTP 要求一同設定。</span><span class="sxs-lookup"><span data-stu-id="8cebf-282">Set inline with the HTTP request.</span></span> <span data-ttu-id="8cebf-283">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-283">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="8cebf-284">若使用上述方法，則各相異的 HTTP 要求標頭都需要自己的 `-h` 選項。</span><span class="sxs-lookup"><span data-stu-id="8cebf-284">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="8cebf-285">於傳送 HTTP 要求之前設定。</span><span class="sxs-lookup"><span data-stu-id="8cebf-285">Set before sending the HTTP request.</span></span> <span data-ttu-id="8cebf-286">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-286">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="8cebf-287">若在傳送要求之前設定標頭，則標頭會保留命令殼層工作階段的持續時間設定。</span><span class="sxs-lookup"><span data-stu-id="8cebf-287">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="8cebf-288">若要清除標頭，請提供空白值。</span><span class="sxs-lookup"><span data-stu-id="8cebf-288">To clear the header, provide an empty value.</span></span> <span data-ttu-id="8cebf-289">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-289">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="8cebf-290">切換 HTTP 要求顯示</span><span class="sxs-lookup"><span data-stu-id="8cebf-290">Toggle HTTP request display</span></span>

<span data-ttu-id="8cebf-291">根據預設，會隱藏所傳送之 HTTP 要求的顯示。</span><span class="sxs-lookup"><span data-stu-id="8cebf-291">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="8cebf-292">您可以針對命令殼層工作階段的持續時間變更對應的設定。</span><span class="sxs-lookup"><span data-stu-id="8cebf-292">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="8cebf-293">啟用要求顯示</span><span class="sxs-lookup"><span data-stu-id="8cebf-293">Enable request display</span></span>

<span data-ttu-id="8cebf-294">透過執行 `echo on` 命令來檢視要傳送的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="8cebf-294">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="8cebf-295">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-295">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="8cebf-296">目前工作階段中的後續 HTTP 要求會顯示要求標頭。</span><span class="sxs-lookup"><span data-stu-id="8cebf-296">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="8cebf-297">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-297">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="8cebf-298">停用要求顯示</span><span class="sxs-lookup"><span data-stu-id="8cebf-298">Disable request display</span></span>

<span data-ttu-id="8cebf-299">透過執行 `echo off` 命令來隱藏要傳送的 HTTP 要求顯示。</span><span class="sxs-lookup"><span data-stu-id="8cebf-299">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="8cebf-300">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-300">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="8cebf-301">執行指令碼</span><span class="sxs-lookup"><span data-stu-id="8cebf-301">Run a script</span></span>

<span data-ttu-id="8cebf-302">如果您經常執行一組相同的 HTTP REPL 命令，請考慮將它們儲存在文字檔中。</span><span class="sxs-lookup"><span data-stu-id="8cebf-302">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="8cebf-303">檔案中的命令會採用與手動在命令列上執行的命令相同的格式。</span><span class="sxs-lookup"><span data-stu-id="8cebf-303">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="8cebf-304">您可使用 `run` 命令以批次的方式執行命令。</span><span class="sxs-lookup"><span data-stu-id="8cebf-304">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="8cebf-305">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-305">For example:</span></span>

1. <span data-ttu-id="8cebf-306">建立包含一組以新行分隔命令的文字檔。</span><span class="sxs-lookup"><span data-stu-id="8cebf-306">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="8cebf-307">為了說明，請參考包含以下命令的 *people-script.txt* 檔案：</span><span class="sxs-lookup"><span data-stu-id="8cebf-307">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="8cebf-308">執行 `run` 命令，傳入文字檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="8cebf-308">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="8cebf-309">例如：</span><span class="sxs-lookup"><span data-stu-id="8cebf-309">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="8cebf-310">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="8cebf-310">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="8cebf-311">清除輸出</span><span class="sxs-lookup"><span data-stu-id="8cebf-311">Clear the output</span></span>

<span data-ttu-id="8cebf-312">若要移除由 HTTP REPL 工具寫入命令殼層的所有輸出，請執行 `clear` 或 `cls` 命令。</span><span class="sxs-lookup"><span data-stu-id="8cebf-312">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="8cebf-313">為了說明，請參考包含以下輸出的命令殼層：</span><span class="sxs-lookup"><span data-stu-id="8cebf-313">To illustrate, imagine the command shell contains the following output:</span></span>

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

<span data-ttu-id="8cebf-314">執行以下命令來清除輸出：</span><span class="sxs-lookup"><span data-stu-id="8cebf-314">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="8cebf-315">執行上述命令後，命令殼層只會包含以下輸出：</span><span class="sxs-lookup"><span data-stu-id="8cebf-315">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="8cebf-316">其他資源</span><span class="sxs-lookup"><span data-stu-id="8cebf-316">Additional resources</span></span>

* [<span data-ttu-id="8cebf-317">REST API 要求</span><span class="sxs-lookup"><span data-stu-id="8cebf-317">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="8cebf-318">HTTP REPL GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="8cebf-318">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
