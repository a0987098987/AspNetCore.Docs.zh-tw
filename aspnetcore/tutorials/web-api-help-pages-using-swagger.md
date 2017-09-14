---
title: "使用 Swagger 的 ASP.NET Core Web API 說明頁面"
author: spboyer
description: "本教學課程提供新增 Swagger，以產生 Web API 應用程式之文件和說明頁面的逐步解說。"
keywords: "ASP.NET Core, Swagger, Swashbuckle, 說明頁面, Web API"
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 92136a6e5db68b4d7e5245e38960e4a1f01bfb73
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2017
---
# <a name="aspnet-web-api-help-pages-using-swagger"></a><span data-ttu-id="451a6-104">使用 Swagger 的 ASP.NET Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="451a6-104">ASP.NET Web API Help Pages using Swagger</span></span>

<a name=web-api-help-pages-using-swagger></a>

<span data-ttu-id="451a6-105">由 [Shayne Boyer](https://twitter.com/spboyer) 和 [Scott Addie](https://twitter.com/Scott_Addie) 提供</span><span class="sxs-lookup"><span data-stu-id="451a6-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="451a6-106">建置使用的應用程式時，了解 API 的各種方法對於開發人員而言可能是一項挑戰。</span><span class="sxs-lookup"><span data-stu-id="451a6-106">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="451a6-107">搭配使用 [Swagger](http://swagger.io) 與 .NET Core 實作 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)，為您的 Web API 產生優良的文件與說明頁面，就像新增幾個 NuGet 套件和修改 *Startup.cs* 一樣簡單。</span><span class="sxs-lookup"><span data-stu-id="451a6-107">Generating good documentation and help pages for your Web API, using [Swagger](http://swagger.io) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="451a6-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 是用來為 ASP.NET Core Web API 產生 Swagger 文件的開放原始碼專案。</span><span class="sxs-lookup"><span data-stu-id="451a6-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="451a6-109">[Swagger](http://swagger.io) 是 RESTful API 的電腦可讀取表示法，可支援互動式文件、產生用戶端 SDK，以及可探索性。</span><span class="sxs-lookup"><span data-stu-id="451a6-109">[Swagger](http://swagger.io) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="451a6-110">本教學課程是基於[使用 ASP.NET Core MVC 和 Visual Studio 建置第一個 Web API](xref:tutorials/first-web-api) 的範例。</span><span class="sxs-lookup"><span data-stu-id="451a6-110">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="451a6-111">如果您想要跟著做，請下載 [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) 上的範例。</span><span class="sxs-lookup"><span data-stu-id="451a6-111">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="451a6-112">快速入門</span><span class="sxs-lookup"><span data-stu-id="451a6-112">Getting Started</span></span>

<span data-ttu-id="451a6-113">Swashbuckle 有三個主要元件：</span><span class="sxs-lookup"><span data-stu-id="451a6-113">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="451a6-114">`Swashbuckle.AspNetCore.Swagger`：Swagger 物件模型和中介軟體，用来公開 `SwaggerDocument` 物件作為 JSON 端點。</span><span class="sxs-lookup"><span data-stu-id="451a6-114">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="451a6-115">`Swashbuckle.AspNetCore.SwaggerGen`：Swagger 產生器，可直接從您的路由、控制器和模型建置 `SwaggerDocument` 物件。</span><span class="sxs-lookup"><span data-stu-id="451a6-115">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="451a6-116">它通常會結合 Swagger 端點中介軟體，以便自動公開 Swagger JSON。</span><span class="sxs-lookup"><span data-stu-id="451a6-116">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="451a6-117">`Swashbuckle.AspNetCore.SwaggerUI`：Swagger UI 工具的內嵌版本，可解譯 Swagger JSON 來建置描述 Web API 功能的可自訂豐富體驗。</span><span class="sxs-lookup"><span data-stu-id="451a6-117">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="451a6-118">它包含公用方法的內建測試載入器。</span><span class="sxs-lookup"><span data-stu-id="451a6-118">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="451a6-119">NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="451a6-119">NuGet Packages</span></span>

<span data-ttu-id="451a6-120">可使用下列方法新增 Swashbuckle：</span><span class="sxs-lookup"><span data-stu-id="451a6-120">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="451a6-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="451a6-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="451a6-122">從 [套件管理員主控台] 視窗中：</span><span class="sxs-lookup"><span data-stu-id="451a6-122">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="451a6-123">從 [管理 NuGet 套件] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="451a6-123">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="451a6-124">在方案總管 > [管理 NuGet 套件] 中，以滑鼠右鍵按一下您的專案</span><span class="sxs-lookup"><span data-stu-id="451a6-124">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="451a6-125">將 [套件來源] 設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="451a6-125">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="451a6-126">在搜尋方塊中輸入 "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="451a6-126">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="451a6-127">從 [瀏覽] 索引標籤中選取 "Swashbuckle.AspNetCore" 套件，並按一下 [安裝]</span><span class="sxs-lookup"><span data-stu-id="451a6-127">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="451a6-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="451a6-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="451a6-129">在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 *Packages* 資料夾</span><span class="sxs-lookup"><span data-stu-id="451a6-129">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="451a6-130">將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="451a6-130">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="451a6-131">在搜尋方塊中輸入 Swashbuckle.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="451a6-131">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="451a6-132">從結果窗格中選取 Swashbuckle.AspNetCore 套件，並按一下 [新增套件]</span><span class="sxs-lookup"><span data-stu-id="451a6-132">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="451a6-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="451a6-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="451a6-134">從 [整合式終端機] 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="451a6-134">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="451a6-135">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="451a6-135">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="451a6-136">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="451a6-136">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="451a6-137">新增 Swagger 並將其設定為中介軟體</span><span class="sxs-lookup"><span data-stu-id="451a6-137">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="451a6-138">將 Swagger 產生器新增至 *Startup.cs* 的 `ConfigureServices` 方法中的服務集合：</span><span class="sxs-lookup"><span data-stu-id="451a6-138">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

<span data-ttu-id="451a6-139">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]</span><span class="sxs-lookup"><span data-stu-id="451a6-139">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]</span></span>

<span data-ttu-id="451a6-140">在 `Info` 類別新增下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="451a6-140">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="451a6-141">在 *Startup.cs* 的 `Configure` 方法中，啟用中介軟體為產生的 JSON 文件和 SwaggerUI 提供服務：</span><span class="sxs-lookup"><span data-stu-id="451a6-141">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

<span data-ttu-id="451a6-142">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]</span><span class="sxs-lookup"><span data-stu-id="451a6-142">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]</span></span>

<span data-ttu-id="451a6-143">啟動應用程式，並巡覽至 `http://localhost:<random_port>/swagger/v1/swagger.json`。</span><span class="sxs-lookup"><span data-stu-id="451a6-143">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="451a6-144">描述端點的已產生文件隨即出現。</span><span class="sxs-lookup"><span data-stu-id="451a6-144">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="451a6-145">**注意：**Microsoft Edge、Google Chrome 和 Firefox 會以原生方式顯示 JSON 文件。</span><span class="sxs-lookup"><span data-stu-id="451a6-145">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="451a6-146">Chrome 具有延伸模組，可格式化文件以方便閱讀。</span><span class="sxs-lookup"><span data-stu-id="451a6-146">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="451a6-147">*為求簡單明瞭，已減少下列範例。*</span><span class="sxs-lookup"><span data-stu-id="451a6-147">*The following example is reduced for brevity.*</span></span>

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

<span data-ttu-id="451a6-148">這份文件會驅動 Swagger UI，而此 Swagger UI 可透過巡覽至 `http://localhost:<random_port>/swagger` 進行檢視：</span><span class="sxs-lookup"><span data-stu-id="451a6-148">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="451a6-150">`TodoController` 中的每個公用動作方法都可以從 UI 進行測試。</span><span class="sxs-lookup"><span data-stu-id="451a6-150">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="451a6-151">按一下方法名稱以展開該區段。</span><span class="sxs-lookup"><span data-stu-id="451a6-151">Click a method name to expand the section.</span></span> <span data-ttu-id="451a6-152">新增任何必要的參數，然後按一下 [試試看!]。</span><span class="sxs-lookup"><span data-stu-id="451a6-152">Add any necessary parameters, and click "Try it out!".</span></span>

![範例 Swagger GET 測試](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="451a6-154">自訂與擴充性</span><span class="sxs-lookup"><span data-stu-id="451a6-154">Customization & Extensibility</span></span>

<span data-ttu-id="451a6-155">Swagger 提供用來記載物件模型和自訂 UI 以符合佈景主題的選項。</span><span class="sxs-lookup"><span data-stu-id="451a6-155">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="451a6-156">API 資訊與描述</span><span class="sxs-lookup"><span data-stu-id="451a6-156">API Info and Description</span></span>

<span data-ttu-id="451a6-157">傳遞至 `AddSwaggerGen` 方法的組態動作可用來新增資訊，例如作者、授權和描述：</span><span class="sxs-lookup"><span data-stu-id="451a6-157">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

<span data-ttu-id="451a6-158">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]</span><span class="sxs-lookup"><span data-stu-id="451a6-158">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]</span></span>

<span data-ttu-id="451a6-159">下列影像說明顯示版本資訊的 Swagger UI：</span><span class="sxs-lookup"><span data-stu-id="451a6-159">The following image depicts the Swagger UI displaying the version information:</span></span>

![含有版本資訊的 Swagger UI：描述、作者，以及查看更多連結](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="451a6-161">XML 註解</span><span class="sxs-lookup"><span data-stu-id="451a6-161">XML Comments</span></span>

<span data-ttu-id="451a6-162">XML 註解可以使用下列方式啟用：</span><span class="sxs-lookup"><span data-stu-id="451a6-162">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="451a6-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="451a6-163">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="451a6-164">以滑鼠右鍵按一下方案總管中的專案，然後選取 [屬性]</span><span class="sxs-lookup"><span data-stu-id="451a6-164">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="451a6-165">檢查 [組建] 索引標籤的 [輸出] 區段下方的 [XML 文件檔]：</span><span class="sxs-lookup"><span data-stu-id="451a6-165">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![專案屬性的 [組建] 索引標籤](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="451a6-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="451a6-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="451a6-168">開啟 [專案選項] 對話方塊 > [組建] > [編譯器]</span><span class="sxs-lookup"><span data-stu-id="451a6-168">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="451a6-169">檢查 [一般選項] 區段下方的 [產生 XML 文件] 方塊：</span><span class="sxs-lookup"><span data-stu-id="451a6-169">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![專案選項的 [一般選項] 區段](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="451a6-171">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="451a6-171">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="451a6-172">將下列程式碼片段手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="451a6-172">Manually add the following snippet to the *.csproj* file:</span></span>

<span data-ttu-id="451a6-173">[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]</span><span class="sxs-lookup"><span data-stu-id="451a6-173">[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]</span></span>

---

<span data-ttu-id="451a6-174">設定 Swagger 來使用產生的 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="451a6-174">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="451a6-175">對於 Linux 或非 Windows 作業系統，檔案名稱和路徑可以區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="451a6-175">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="451a6-176">例如，*ToDoApi.XML* 檔案可位於 Windows，但不是 CentOS。</span><span class="sxs-lookup"><span data-stu-id="451a6-176">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

<span data-ttu-id="451a6-177">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]</span><span class="sxs-lookup"><span data-stu-id="451a6-177">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]</span></span>

<span data-ttu-id="451a6-178">在上述程式碼中，`ApplicationBasePath` 會取得應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="451a6-178">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="451a6-179">基底路徑用來找出 XML 註解檔案。</span><span class="sxs-lookup"><span data-stu-id="451a6-179">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="451a6-180">*TodoApi.xml* 僅適用於此範例，因為所產生之 XML 註解檔案的名稱是以應用程式名稱為基礎。</span><span class="sxs-lookup"><span data-stu-id="451a6-180">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="451a6-181">將三斜線註解新增至方法，即可透過將描述加入區段標頭來增強 Swagger UI：</span><span class="sxs-lookup"><span data-stu-id="451a6-181">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

<span data-ttu-id="451a6-182">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="451a6-182">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]</span></span>

![顯示 XML 註解 'Deletes a specific TodoItem.' 的 Swagger UI](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="451a6-185">UI 是由產生的 JSON 檔案所驅動，該檔案中也包含這些註解：</span><span class="sxs-lookup"><span data-stu-id="451a6-185">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="451a6-186">請將 [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) 標記新增至 `Create` 動作方法文件。</span><span class="sxs-lookup"><span data-stu-id="451a6-186">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="451a6-187">它會補充 `<summary>` 標記中指定的資訊，並提供更強固的 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="451a6-187">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="451a6-188">`<remarks>` 標記內容可以包含文字、JSON 或 XML。</span><span class="sxs-lookup"><span data-stu-id="451a6-188">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

<span data-ttu-id="451a6-189">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="451a6-189">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>

<span data-ttu-id="451a6-190">請注意使用這些額外註解的 UI 增強功能。</span><span class="sxs-lookup"><span data-stu-id="451a6-190">Notice the UI enhancements with these additional comments.</span></span>

![顯示額外註解的 Swagger UI](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="451a6-192">資料註釋</span><span class="sxs-lookup"><span data-stu-id="451a6-192">Data Annotations</span></span>

<span data-ttu-id="451a6-193">使用 `System.ComponentModel.DataAnnotations` 中找到的屬性裝飾 API 控制器，有助於驅動 Swagger UI 元件。</span><span class="sxs-lookup"><span data-stu-id="451a6-193">Decorate the API controller with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="451a6-194">將 `[Required]` 屬性 (attribute) 新增至 `TodoItem` 類別的 `Name` 屬性 (property)：</span><span class="sxs-lookup"><span data-stu-id="451a6-194">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

<span data-ttu-id="451a6-195">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="451a6-195">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]</span></span>

<span data-ttu-id="451a6-196">這個屬性的存在會變更 UI 行為，並改變基礎的 JSON 結構描述：</span><span class="sxs-lookup"><span data-stu-id="451a6-196">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="451a6-197">將 `[Produces("application/json")]` 屬性新增至 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="451a6-197">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="451a6-198">其目的是要宣告控制器的動作支援傳回的內容類型為 *application/json*：</span><span class="sxs-lookup"><span data-stu-id="451a6-198">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

<span data-ttu-id="451a6-199">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="451a6-199">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]</span></span>

<span data-ttu-id="451a6-200">[回應內容類型] 下拉式清單會選取此內容類型，作為控制器 GET 動作的預設值：</span><span class="sxs-lookup"><span data-stu-id="451a6-200">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![含有預設回應內容類型的 Swagger UI](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="451a6-202">當 Web API 中的資料註釋使用量增加時，UI 和 API 說明頁面會變得更清楚且更有用。</span><span class="sxs-lookup"><span data-stu-id="451a6-202">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="451a6-203">描述回應類型</span><span class="sxs-lookup"><span data-stu-id="451a6-203">Describing Response Types</span></span>

<span data-ttu-id="451a6-204">取用的開發人員最關心的是傳回的 &mdash; 特別回應類型和錯誤碼 (如果不是標準的話)。</span><span class="sxs-lookup"><span data-stu-id="451a6-204">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="451a6-205">這些是以 XML 註解和資料註釋進行處理。</span><span class="sxs-lookup"><span data-stu-id="451a6-205">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="451a6-206">`Create` 動作會在成功時傳回 `201 Created`，或在發佈的要求主體為 null 時傳回 `400 Bad Request`。</span><span class="sxs-lookup"><span data-stu-id="451a6-206">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="451a6-207">如果 Swagger UI 中沒有適當的文件，取用者便會缺乏對這些預期結果的了解。</span><span class="sxs-lookup"><span data-stu-id="451a6-207">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="451a6-208">在下列範例中新增強調顯示的那幾行來修正該問題：</span><span class="sxs-lookup"><span data-stu-id="451a6-208">That problem is fixed by adding the highlighted lines in the following example:</span></span>

<span data-ttu-id="451a6-209">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="451a6-209">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>

<span data-ttu-id="451a6-210">現在，Swagger UI 清楚記載了預期的 HTTP 回應碼：</span><span class="sxs-lookup"><span data-stu-id="451a6-210">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![顯示 POST 回應類別描述 'Returns the newly created Todo item'，並針對回應訊息下方的狀態碼和原因顯示 '400 - If the item is null' 的 Swagger UI](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="451a6-212">自訂 UI</span><span class="sxs-lookup"><span data-stu-id="451a6-212">Customizing the UI</span></span>

<span data-ttu-id="451a6-213">股票 UI 既可運作又能呈現；不過，在建置 API 的文件頁面時，您希望它表示您的品牌或佈景主題。</span><span class="sxs-lookup"><span data-stu-id="451a6-213">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="451a6-214">使用 Swashbuckle 元件完成該工作，需要新增資源來處理靜態檔案，然後建置裝載這些檔案的資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="451a6-214">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="451a6-215">如果目標設為 .NET Framework，請將 `Microsoft.AspNetCore.StaticFiles` NuGet 套件新增至專案：</span><span class="sxs-lookup"><span data-stu-id="451a6-215">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="451a6-216">啟用靜態檔案中介軟體：</span><span class="sxs-lookup"><span data-stu-id="451a6-216">Enable the static files middleware:</span></span>

<span data-ttu-id="451a6-217">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="451a6-217">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]</span></span>

<span data-ttu-id="451a6-218">從 [Swagger UI GitHub 儲存機制](https://github.com/swagger-api/swagger-ui/tree/2.x/dist)中取得 *dist* 資料夾的內容。</span><span class="sxs-lookup"><span data-stu-id="451a6-218">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="451a6-219">此資料夾包含 Swagger UI 頁面所需的資產。</span><span class="sxs-lookup"><span data-stu-id="451a6-219">This folder contains the necessary assets for the Swagger UI page.</span></span> <span data-ttu-id="451a6-220">將該資料夾的內容複製到 *wwwroot/swagger/ui* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="451a6-220">Copy the contents of that folder into the *wwwroot/swagger/ui* folder.</span></span>

<span data-ttu-id="451a6-221">使用下列 CSS 建立 *wwwroot/swagger/ui/css/custom.css* 檔案，以自訂頁面標頭：</span><span class="sxs-lookup"><span data-stu-id="451a6-221">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

<span data-ttu-id="451a6-222">[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]</span><span class="sxs-lookup"><span data-stu-id="451a6-222">[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]</span></span>

<span data-ttu-id="451a6-223">在 *index.html* 檔案中參考 *custom.css*：</span><span class="sxs-lookup"><span data-stu-id="451a6-223">Reference *custom.css* in the *index.html* file:</span></span>

<span data-ttu-id="451a6-224">[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]</span><span class="sxs-lookup"><span data-stu-id="451a6-224">[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]</span></span>

<span data-ttu-id="451a6-225">瀏覽至 `http://localhost:<random_port>/swagger/ui/index.html` 上的 *index.html* 頁面。</span><span class="sxs-lookup"><span data-stu-id="451a6-225">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="451a6-226">在標頭的文字方塊中輸入 `http://localhost:<random_port>/swagger/v1/swagger.json`，然後按一下 [瀏覽] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="451a6-226">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="451a6-227">結果頁面如下所示：</span><span class="sxs-lookup"><span data-stu-id="451a6-227">The resulting page looks as follows:</span></span>

![含有自訂標頭標題的 Swagger UI](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="451a6-229">還有更多可以在頁面中執行的功能。</span><span class="sxs-lookup"><span data-stu-id="451a6-229">There is much more you can do with the page.</span></span> <span data-ttu-id="451a6-230">請在 [Swagger UI GitHub 儲存機制](https://github.com/swagger-api/swagger-ui)中查看 UI 資源的完整功能。</span><span class="sxs-lookup"><span data-stu-id="451a6-230">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
