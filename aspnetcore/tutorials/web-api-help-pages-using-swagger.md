---
title: "使用 Swagger 的 ASP.NET Core Web API 說明頁面"
author: spboyer
description: "本教學課程提供新增 Swagger，以產生 Web API 應用程式之文件和說明頁面的逐步解說。"
manager: wpickett
ms.author: spboyer
ms.date: 09/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 911504d9472ae78a0d1d002f1feb57f3a160d5bf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="e681b-103">使用 Swagger 的 ASP.NET Core Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="e681b-103">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="e681b-104">由 [Shayne Boyer](https://twitter.com/spboyer) 和 [Scott Addie](https://twitter.com/Scott_Addie) 提供</span><span class="sxs-lookup"><span data-stu-id="e681b-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e681b-105">建置使用的應用程式時，了解 API 的各種方法對於開發人員而言可能是一項挑戰。</span><span class="sxs-lookup"><span data-stu-id="e681b-105">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="e681b-106">搭配使用 [Swagger](https://swagger.io/) 與 .NET Core 實作 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)，為您的 Web API 產生優良的文件與說明頁面，就像新增幾個 NuGet 套件和修改 *Startup.cs* 一樣簡單。</span><span class="sxs-lookup"><span data-stu-id="e681b-106">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="e681b-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 是用來為 ASP.NET Core Web API 產生 Swagger 文件的開放原始碼專案。</span><span class="sxs-lookup"><span data-stu-id="e681b-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="e681b-108">[Swagger](https://swagger.io/) 是 RESTful API 的電腦可讀取表示法，可支援互動式文件、產生用戶端 SDK，以及可探索性。</span><span class="sxs-lookup"><span data-stu-id="e681b-108">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="e681b-109">本教學課程是基於[使用 ASP.NET Core MVC 和 Visual Studio 建置第一個 Web API](xref:tutorials/first-web-api) 的範例。</span><span class="sxs-lookup"><span data-stu-id="e681b-109">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="e681b-110">如果您想要跟著做，請下載 [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) 上的範例。</span><span class="sxs-lookup"><span data-stu-id="e681b-110">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="e681b-111">快速入門</span><span class="sxs-lookup"><span data-stu-id="e681b-111">Getting Started</span></span>

<span data-ttu-id="e681b-112">Swashbuckle 有三個主要元件：</span><span class="sxs-lookup"><span data-stu-id="e681b-112">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="e681b-113">`Swashbuckle.AspNetCore.Swagger`：Swagger 物件模型和中介軟體，用来公開 `SwaggerDocument` 物件作為 JSON 端點。</span><span class="sxs-lookup"><span data-stu-id="e681b-113">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="e681b-114">`Swashbuckle.AspNetCore.SwaggerGen`：Swagger 產生器，可直接從您的路由、控制器和模型建置 `SwaggerDocument` 物件。</span><span class="sxs-lookup"><span data-stu-id="e681b-114">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="e681b-115">它通常會結合 Swagger 端點中介軟體，以便自動公開 Swagger JSON。</span><span class="sxs-lookup"><span data-stu-id="e681b-115">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="e681b-116">`Swashbuckle.AspNetCore.SwaggerUI`：Swagger UI 工具的內嵌版本，可解譯 Swagger JSON 來建置描述 Web API 功能的可自訂豐富體驗。</span><span class="sxs-lookup"><span data-stu-id="e681b-116">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="e681b-117">它包含公用方法的內建測試載入器。</span><span class="sxs-lookup"><span data-stu-id="e681b-117">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="e681b-118">NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="e681b-118">NuGet Packages</span></span>

<span data-ttu-id="e681b-119">可使用下列方法新增 Swashbuckle：</span><span class="sxs-lookup"><span data-stu-id="e681b-119">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e681b-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e681b-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e681b-121">從 [套件管理員主控台] 視窗中：</span><span class="sxs-lookup"><span data-stu-id="e681b-121">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="e681b-122">從 [管理 NuGet 套件] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="e681b-122">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="e681b-123">在方案總管 > [管理 NuGet 套件] 中，以滑鼠右鍵按一下您的專案</span><span class="sxs-lookup"><span data-stu-id="e681b-123">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="e681b-124">將 [套件來源] 設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="e681b-124">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="e681b-125">在搜尋方塊中輸入 "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="e681b-125">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="e681b-126">從 [瀏覽] 索引標籤中選取 "Swashbuckle.AspNetCore" 套件，並按一下 [安裝]</span><span class="sxs-lookup"><span data-stu-id="e681b-126">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e681b-127">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e681b-127">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e681b-128">在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 *Packages* 資料夾</span><span class="sxs-lookup"><span data-stu-id="e681b-128">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="e681b-129">將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="e681b-129">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="e681b-130">在搜尋方塊中輸入 Swashbuckle.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="e681b-130">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="e681b-131">從結果窗格中選取 Swashbuckle.AspNetCore 套件，並按一下 [新增套件]</span><span class="sxs-lookup"><span data-stu-id="e681b-131">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e681b-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e681b-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e681b-133">從 [整合式終端機] 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e681b-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e681b-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e681b-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e681b-135">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e681b-135">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="e681b-136">新增 Swagger 並將其設定為中介軟體</span><span class="sxs-lookup"><span data-stu-id="e681b-136">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="e681b-137">將 Swagger 產生器新增至 *Startup.cs* 的 `ConfigureServices` 方法中的服務集合：</span><span class="sxs-lookup"><span data-stu-id="e681b-137">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="e681b-138">在 `Info` 類別新增下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="e681b-138">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="e681b-139">在 *Startup.cs* 的 `Configure` 方法中，啟用中介軟體為產生的 JSON 文件和 SwaggerUI 提供服務：</span><span class="sxs-lookup"><span data-stu-id="e681b-139">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="e681b-140">啟動應用程式，並巡覽至 `http://localhost:<random_port>/swagger/v1/swagger.json`。</span><span class="sxs-lookup"><span data-stu-id="e681b-140">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="e681b-141">描述端點的已產生文件隨即出現。</span><span class="sxs-lookup"><span data-stu-id="e681b-141">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="e681b-142">**注意：**Microsoft Edge、Google Chrome 和 Firefox 會以原生方式顯示 JSON 文件。</span><span class="sxs-lookup"><span data-stu-id="e681b-142">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="e681b-143">Chrome 具有延伸模組，可格式化文件以方便閱讀。</span><span class="sxs-lookup"><span data-stu-id="e681b-143">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="e681b-144">*為求簡單明瞭，已減少下列範例。*</span><span class="sxs-lookup"><span data-stu-id="e681b-144">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

<span data-ttu-id="e681b-145">這份文件會驅動 Swagger UI，而此 Swagger UI 可透過巡覽至 `http://localhost:<random_port>/swagger` 進行檢視：</span><span class="sxs-lookup"><span data-stu-id="e681b-145">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="e681b-147">`TodoController` 中的每個公用動作方法都可以從 UI 進行測試。</span><span class="sxs-lookup"><span data-stu-id="e681b-147">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="e681b-148">按一下方法名稱以展開該區段。</span><span class="sxs-lookup"><span data-stu-id="e681b-148">Click a method name to expand the section.</span></span> <span data-ttu-id="e681b-149">新增任何必要的參數，然後按一下 [試試看!]。</span><span class="sxs-lookup"><span data-stu-id="e681b-149">Add any necessary parameters, and click "Try it out!".</span></span>

![範例 Swagger GET 測試](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="e681b-151">自訂與擴充性</span><span class="sxs-lookup"><span data-stu-id="e681b-151">Customization & Extensibility</span></span>

<span data-ttu-id="e681b-152">Swagger 提供用來記載物件模型和自訂 UI 以符合佈景主題的選項。</span><span class="sxs-lookup"><span data-stu-id="e681b-152">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="e681b-153">API 資訊與描述</span><span class="sxs-lookup"><span data-stu-id="e681b-153">API Info and Description</span></span>

<span data-ttu-id="e681b-154">傳遞至 `AddSwaggerGen` 方法的組態動作可用來新增資訊，例如作者、授權和描述：</span><span class="sxs-lookup"><span data-stu-id="e681b-154">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="e681b-155">下列影像說明顯示版本資訊的 Swagger UI：</span><span class="sxs-lookup"><span data-stu-id="e681b-155">The following image depicts the Swagger UI displaying the version information:</span></span>

![含有版本資訊的 Swagger UI：描述、作者，以及查看更多連結](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="e681b-157">XML 註解</span><span class="sxs-lookup"><span data-stu-id="e681b-157">XML Comments</span></span>

<span data-ttu-id="e681b-158">XML 註解可以使用下列方式啟用：</span><span class="sxs-lookup"><span data-stu-id="e681b-158">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e681b-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e681b-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e681b-160">以滑鼠右鍵按一下方案總管中的專案，然後選取 [屬性]</span><span class="sxs-lookup"><span data-stu-id="e681b-160">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="e681b-161">檢查 [組建] 索引標籤的 [輸出] 區段下方的 [XML 文件檔]：</span><span class="sxs-lookup"><span data-stu-id="e681b-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![專案屬性的 [組建] 索引標籤](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e681b-163">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e681b-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e681b-164">開啟 [專案選項] 對話方塊 > [組建] > [編譯器]</span><span class="sxs-lookup"><span data-stu-id="e681b-164">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="e681b-165">檢查 [一般選項] 區段下方的 [產生 XML 文件] 方塊：</span><span class="sxs-lookup"><span data-stu-id="e681b-165">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![專案選項的 [一般選項] 區段](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e681b-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e681b-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e681b-168">將下列程式碼片段手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="e681b-168">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e681b-169">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e681b-169">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e681b-170">請參閱 Visual Studio Code。</span><span class="sxs-lookup"><span data-stu-id="e681b-170">See Visual Studio Code.</span></span>

---

<span data-ttu-id="e681b-171">啟用 XML 註解，可提供未記載之公用類型與成員的偵錯資訊。</span><span class="sxs-lookup"><span data-stu-id="e681b-171">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="e681b-172">未記載的類型和成員，會以警告訊息表示：遺漏公用可見類型或成員的 XML 註解。</span><span class="sxs-lookup"><span data-stu-id="e681b-172">Undocumented types and members are indicated by the warning message: *Missing XML comment for publicly visible type or member*.</span></span>

<span data-ttu-id="e681b-173">設定 Swagger 來使用產生的 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="e681b-173">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="e681b-174">對於 Linux 或非 Windows 作業系統，檔案名稱和路徑可以區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e681b-174">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="e681b-175">例如，*ToDoApi.XML* 檔案可位於 Windows，但不是 CentOS。</span><span class="sxs-lookup"><span data-stu-id="e681b-175">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="e681b-176">在上述程式碼中，`ApplicationBasePath` 會取得應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="e681b-176">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="e681b-177">基底路徑用來找出 XML 註解檔案。</span><span class="sxs-lookup"><span data-stu-id="e681b-177">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="e681b-178">*TodoApi.xml* 僅適用於此範例，因為所產生之 XML 註解檔案的名稱是以應用程式名稱為基礎。</span><span class="sxs-lookup"><span data-stu-id="e681b-178">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="e681b-179">將三斜線註解新增至方法，即可透過將描述加入區段標頭來增強 Swagger UI：</span><span class="sxs-lookup"><span data-stu-id="e681b-179">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![顯示 XML 註解 'Deletes a specific TodoItem.' 的 Swagger UI](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="e681b-182">UI 是由產生的 JSON 檔案所驅動，該檔案中也包含這些註解：</span><span class="sxs-lookup"><span data-stu-id="e681b-182">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="e681b-183">請將 [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) 標記新增至 `Create` 動作方法文件。</span><span class="sxs-lookup"><span data-stu-id="e681b-183">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="e681b-184">它會補充 `<summary>` 標記中指定的資訊，並提供更強固的 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="e681b-184">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="e681b-185">`<remarks>` 標記內容可以包含文字、JSON 或 XML。</span><span class="sxs-lookup"><span data-stu-id="e681b-185">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="e681b-186">請注意使用這些額外註解的 UI 增強功能。</span><span class="sxs-lookup"><span data-stu-id="e681b-186">Notice the UI enhancements with these additional comments.</span></span>

![顯示額外註解的 Swagger UI](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="e681b-188">資料註釋</span><span class="sxs-lookup"><span data-stu-id="e681b-188">Data Annotations</span></span>

<span data-ttu-id="e681b-189">使用 `System.ComponentModel.DataAnnotations` 中的屬性來裝飾模型，以協助驅動 Swagger UI 元件。</span><span class="sxs-lookup"><span data-stu-id="e681b-189">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="e681b-190">將 `[Required]` 屬性 (attribute) 新增至 `TodoItem` 類別的 `Name` 屬性 (property)：</span><span class="sxs-lookup"><span data-stu-id="e681b-190">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="e681b-191">這個屬性的存在會變更 UI 行為，並改變基礎的 JSON 結構描述：</span><span class="sxs-lookup"><span data-stu-id="e681b-191">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="e681b-192">將 `[Produces("application/json")]` 屬性新增至 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="e681b-192">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="e681b-193">其目的是要宣告控制器的動作支援傳回的內容類型為 *application/json*：</span><span class="sxs-lookup"><span data-stu-id="e681b-193">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="e681b-194">[回應內容類型] 下拉式清單會選取此內容類型，作為控制器 GET 動作的預設值：</span><span class="sxs-lookup"><span data-stu-id="e681b-194">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![含有預設回應內容類型的 Swagger UI](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="e681b-196">當 Web API 中的資料註釋使用量增加時，UI 和 API 說明頁面會變得更清楚且更有用。</span><span class="sxs-lookup"><span data-stu-id="e681b-196">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="e681b-197">描述回應類型</span><span class="sxs-lookup"><span data-stu-id="e681b-197">Describing Response Types</span></span>

<span data-ttu-id="e681b-198">取用的開發人員最關心的是傳回的 &mdash; 特別回應類型和錯誤碼 (如果不是標準的話)。</span><span class="sxs-lookup"><span data-stu-id="e681b-198">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="e681b-199">這些是以 XML 註解和資料註釋進行處理。</span><span class="sxs-lookup"><span data-stu-id="e681b-199">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="e681b-200">`Create` 動作會在成功時傳回 `201 Created`，或在發佈的要求主體為 null 時傳回 `400 Bad Request`。</span><span class="sxs-lookup"><span data-stu-id="e681b-200">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="e681b-201">如果 Swagger UI 中沒有適當的文件，取用者便會缺乏對這些預期結果的了解。</span><span class="sxs-lookup"><span data-stu-id="e681b-201">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="e681b-202">在下列範例中新增強調顯示的那幾行來修正該問題：</span><span class="sxs-lookup"><span data-stu-id="e681b-202">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="e681b-203">現在，Swagger UI 清楚記載了預期的 HTTP 回應碼：</span><span class="sxs-lookup"><span data-stu-id="e681b-203">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![顯示 POST 回應類別描述 'Returns the newly created Todo item'，並針對回應訊息下方的狀態碼和原因顯示 '400 - If the item is null' 的 Swagger UI](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="e681b-205">自訂 UI</span><span class="sxs-lookup"><span data-stu-id="e681b-205">Customizing the UI</span></span>

<span data-ttu-id="e681b-206">股票 UI 既可運作又能呈現；不過，在建置 API 的文件頁面時，您希望它表示您的品牌或佈景主題。</span><span class="sxs-lookup"><span data-stu-id="e681b-206">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="e681b-207">使用 Swashbuckle 元件完成該工作，需要新增資源來處理靜態檔案，然後建置裝載這些檔案的資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="e681b-207">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="e681b-208">如果目標設為 .NET Framework，請將 `Microsoft.AspNetCore.StaticFiles` NuGet 套件新增至專案：</span><span class="sxs-lookup"><span data-stu-id="e681b-208">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="e681b-209">啟用靜態檔案中介軟體：</span><span class="sxs-lookup"><span data-stu-id="e681b-209">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="e681b-210">從 [Swagger UI GitHub 儲存機制](https://github.com/swagger-api/swagger-ui/tree/2.x/dist)中取得 *dist* 資料夾的內容。</span><span class="sxs-lookup"><span data-stu-id="e681b-210">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="e681b-211">此資料夾包含 Swagger UI 頁面所需的資產。</span><span class="sxs-lookup"><span data-stu-id="e681b-211">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="e681b-212">建立 *wwwroot/swagger/ui* 資料夾，並將 *dist* 資料夾的內容複製到其中。</span><span class="sxs-lookup"><span data-stu-id="e681b-212">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="e681b-213">使用下列 CSS 建立 *wwwroot/swagger/ui/css/custom.css* 檔案，以自訂頁面標頭：</span><span class="sxs-lookup"><span data-stu-id="e681b-213">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="e681b-214">在 *index.html* 檔案中參考 *custom.css*：</span><span class="sxs-lookup"><span data-stu-id="e681b-214">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="e681b-215">瀏覽至 `http://localhost:<random_port>/swagger/ui/index.html` 上的 *index.html* 頁面。</span><span class="sxs-lookup"><span data-stu-id="e681b-215">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="e681b-216">在標頭的文字方塊中輸入 `http://localhost:<random_port>/swagger/v1/swagger.json`，然後按一下 [瀏覽] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e681b-216">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="e681b-217">結果頁面如下所示：</span><span class="sxs-lookup"><span data-stu-id="e681b-217">The resulting page looks as follows:</span></span>

![含有自訂標頭標題的 Swagger UI](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="e681b-219">您還可以透過此頁面進行更多功能。</span><span class="sxs-lookup"><span data-stu-id="e681b-219">There's much more you can do with the page.</span></span> <span data-ttu-id="e681b-220">請在 [Swagger UI GitHub 儲存機制](https://github.com/swagger-api/swagger-ui)中查看 UI 資源的完整功能。</span><span class="sxs-lookup"><span data-stu-id="e681b-220">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
