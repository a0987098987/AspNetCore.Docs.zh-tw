---
title: 教學課程：使用 ASP.NET Core 以 jQuery 呼叫 Web API
author: rick-anderson
description: 了解如何使用 jQuery 呼叫 ASP.NET Core Web API。
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: eb8b2453fd037170a49f531fea4c3ef1c056292d
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602567"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a><span data-ttu-id="2b5a3-103">教學課程：使用 jQuery 呼叫 ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="2b5a3-103">Tutorial: Call an ASP.NET Core web API with jQuery</span></span>

<span data-ttu-id="2b5a3-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2b5a3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2b5a3-105">本教學課程會示範如何使用 jQuery 呼叫 ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="2b5a3-105">This tutorial shows how to call an ASP.NET Core web API with jQuery</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2b5a3-106">若為 ASP.NET Core 2.2，請參閱[使用 jQuery 呼叫 Web API](xref:tutorials/first-web-api#call-the-api-with-jquery) 的 2.2 版。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-106">For ASP.NET Core 2.2, see the 2.2 version of [Call the Web API with jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="2b5a3-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="2b5a3-107">Prerequisites</span></span>

<span data-ttu-id="2b5a3-108">完成[教學課程：建立 Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="2b5a3-108">Complete [Tutorial: Create a web API](xref:tutorials/first-web-api)</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="2b5a3-109">使用 jQuery 呼叫 API</span><span class="sxs-lookup"><span data-stu-id="2b5a3-109">Call the API with jQuery</span></span>

<span data-ttu-id="2b5a3-110">在本節中，將會新增 HTML 網頁，以使用 jQuery 來呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-110">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="2b5a3-111">jQuery 會起始要求，並使用來自 API 回應的詳細資料更新頁面。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-111">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="2b5a3-112">藉由使用下列反白顯示的程式碼更新 *Startup.cs*，來設定應用程式[提供靜態檔案](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)並[啟用預設檔案對應](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：</span><span class="sxs-lookup"><span data-stu-id="2b5a3-112">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

<span data-ttu-id="2b5a3-113">在專案目錄中建立 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-113">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="2b5a3-114">將名為 *index.html* 的 HTML 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-114">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="2b5a3-115">將其內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="2b5a3-115">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

<span data-ttu-id="2b5a3-116">將名為 *site.js* 的 JavaScript 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-116">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="2b5a3-117">將其內容取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="2b5a3-117">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="2b5a3-118">若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：</span><span class="sxs-lookup"><span data-stu-id="2b5a3-118">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="2b5a3-119">開啟 *Properties\launchSettings.json*。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-119">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="2b5a3-120">移除 `launchUrl` 屬性，以強制應用程式於 *index.html* 處開啟 &mdash; 專案的預設檔案。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-120">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="2b5a3-121">您可以使用幾種方式來取得 jQuery。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-121">There are several ways to get jQuery.</span></span> <span data-ttu-id="2b5a3-122">在上述的程式碼片段中，從 CDN 載入程式庫。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-122">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="2b5a3-123">此範例會呼叫 API 的所有 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-123">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="2b5a3-124">以下是關於呼叫 API 的說明。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-124">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="2b5a3-125">取得待辦事項的清單</span><span class="sxs-lookup"><span data-stu-id="2b5a3-125">Get a list of to-do items</span></span>

<span data-ttu-id="2b5a3-126">JQuery [ajax](https://api.jquery.com/jquery.ajax/) 函式會將 `GET` 要求傳送至 API，API 則會傳回代表待辦事項陣列的 JSON。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-126">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="2b5a3-127">如果要求成功，則會叫用 `success` 回呼函式。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-127">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="2b5a3-128">在回呼中，DOM 已使用待辦事項資訊進行更新。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-128">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="2b5a3-129">新增待辦事項</span><span class="sxs-lookup"><span data-stu-id="2b5a3-129">Add a to-do item</span></span>

<span data-ttu-id="2b5a3-130">[ajax](https://api.jquery.com/jquery.ajax/) 函式會傳送 `POST` 要求，並在要求本文中包含待辦事項。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-130">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="2b5a3-131">`accepts` 和 `contentType` 選項都設定為 `application/json`，以指定接收和傳送的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-131">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="2b5a3-132">待辦事項會使用 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-132">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="2b5a3-133">當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-133">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="2b5a3-134">更新待辦事項</span><span class="sxs-lookup"><span data-stu-id="2b5a3-134">Update a to-do item</span></span>

<span data-ttu-id="2b5a3-135">更新待辦事項類似於新增待辦事項。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-135">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="2b5a3-136">`url` 會變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-136">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="2b5a3-137">刪除待辦事項</span><span class="sxs-lookup"><span data-stu-id="2b5a3-137">Delete a to-do item</span></span>

<span data-ttu-id="2b5a3-138">刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="2b5a3-138">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

<span data-ttu-id="2b5a3-139">前進到下一個教學課程來了解如何產生 API 說明頁面：</span><span class="sxs-lookup"><span data-stu-id="2b5a3-139">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end