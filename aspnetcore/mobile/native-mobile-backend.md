---
title: 使用 ASP.NET Core 建立原生行動應用程式的後端服務
author: ardalis
description: 了解如何使用 ASP.NET Core MVC 建立後端服務，以支援原生行動應用程式。
ms.author: riande
ms.date: 10/14/2016
uid: mobile/native-mobile-backend
ms.openlocfilehash: 13149dd4b877b8c17d33d428779ad31d8c51ae9e
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488724"
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a><span data-ttu-id="65d33-103">使用 ASP.NET Core 建立原生行動應用程式的後端服務</span><span class="sxs-lookup"><span data-stu-id="65d33-103">Create backend services for native mobile apps with ASP.NET Core</span></span>

<span data-ttu-id="65d33-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="65d33-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="65d33-105">行動裝置應用程式可以與 ASP.NET Core 後端服務通訊。</span><span class="sxs-lookup"><span data-stu-id="65d33-105">Mobile apps can communicate with ASP.NET Core backend services.</span></span> <span data-ttu-id="65d33-106">如需如何從 iOS 模擬器和 Android 模擬器連線到本機 Web 服務的指示，請參閱[從 iOS 模擬器和 Android 模擬器連線到本機 Web 服務](/xamarin/cross-platform/deploy-test/connect-to-local-web-services)。</span><span class="sxs-lookup"><span data-stu-id="65d33-106">For instructions on connecting local web services from iOS simulators and Android emulators, see [Connect to Local Web Services from iOS Simulators and Android Emulators](/xamarin/cross-platform/deploy-test/connect-to-local-web-services).</span></span>

[<span data-ttu-id="65d33-107">檢視或下載簡易後端服務程式碼範例</span><span class="sxs-lookup"><span data-stu-id="65d33-107">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="65d33-108">原生行動應用程式範例</span><span class="sxs-lookup"><span data-stu-id="65d33-108">The Sample Native Mobile App</span></span>

<span data-ttu-id="65d33-109">本教學課程會示範如何使用 ASP.NET Core MVC 建立後端服務，以支援原生行動裝置應用程式。</span><span class="sxs-lookup"><span data-stu-id="65d33-109">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="65d33-110">它會使用 [Xamarin Forms ToDoRest 應用程式](/xamarin/xamarin-forms/data-cloud/consuming/rest) 作為其原生用戶端，其包含了適用於 Android、iOS、Windows 通用，以及 Windows Phone 裝置的個別原生用戶端。</span><span class="sxs-lookup"><span data-stu-id="65d33-110">It uses the [Xamarin Forms ToDoRest app](/xamarin/xamarin-forms/data-cloud/consuming/rest) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="65d33-111">您可以遵循連結的教學課程來建立原生應用程式 (並安裝必要的免費 Xamarin 工具)，也可以下載 Xamarin 範例解決方案。</span><span class="sxs-lookup"><span data-stu-id="65d33-111">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="65d33-112">Xamarin 範例包含了 ASP.NET Web API 2 服務專案。該專案已由此文章的 ASP.NET Core 應用程式取代 (但用戶端不需要進行任何變更)。</span><span class="sxs-lookup"><span data-stu-id="65d33-112">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![To Do Rest 應用程式在 Android 智慧型手機上執行](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="65d33-114">功能</span><span class="sxs-lookup"><span data-stu-id="65d33-114">Features</span></span>

<span data-ttu-id="65d33-115">ToDoRest 應用程式支援列出、新增、刪除和更新待辦項目。</span><span class="sxs-lookup"><span data-stu-id="65d33-115">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="65d33-116">每個項目都有識別碼、名稱、記事和標示其是否已完成的屬性。</span><span class="sxs-lookup"><span data-stu-id="65d33-116">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="65d33-117">項目的主要檢視，如上所示，會列出每個項目的名稱，並使用勾選記號表示其是否已完成。</span><span class="sxs-lookup"><span data-stu-id="65d33-117">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="65d33-118">點選 `+` 圖示便會開啟 [新增項目] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="65d33-118">Tapping the `+` icon opens an add item dialog:</span></span>

![[新增項目] 對話方塊](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="65d33-120">在主要清單畫面中點選項目，便會開啟 [編輯] 對話方塊，讓使用者修改項目的名稱、記事及完成狀態，或是刪除該項目：</span><span class="sxs-lookup"><span data-stu-id="65d33-120">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![[編輯項目] 對話方塊](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="65d33-122">這個範例根據預設已設定為使用託管於 developer.xamarin.com 上的後端服務，允許唯讀操作。</span><span class="sxs-lookup"><span data-stu-id="65d33-122">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="65d33-123">若要自行測試您在下一個章節建立的，於您的電腦上執行的 ASP.NET Core 應用程式，您必須更新應用程式的 `RestUrl` 常數。</span><span class="sxs-lookup"><span data-stu-id="65d33-123">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="65d33-124">巡覽至 `ToDoREST` 專案並開啟 *Constants.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="65d33-124">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="65d33-125">將 `RestUrl` 取代為包含您電腦 IP 位址的 URL (不是 localhost 或 127.0.0.1，因為這些位址僅用於裝置模擬器，而非您的電腦)。</span><span class="sxs-lookup"><span data-stu-id="65d33-125">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="65d33-126">在其中包含連接埠號碼 (5000)。</span><span class="sxs-lookup"><span data-stu-id="65d33-126">Include the port number as well (5000).</span></span> <span data-ttu-id="65d33-127">為了測試您的服務可以在裝置上運作，請確認您沒有作用中的防火牆正在封鎖此連接埠的存取。</span><span class="sxs-lookup"><span data-stu-id="65d33-127">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="65d33-128">建立 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="65d33-128">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="65d33-129">在 Visual Studio 中建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="65d33-129">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="65d33-130">選擇 [Web API template] (Web API 範本) 及 [No Authentication] (無驗證)。</span><span class="sxs-lookup"><span data-stu-id="65d33-130">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="65d33-131">將專案命名為 *ToDoApi*。</span><span class="sxs-lookup"><span data-stu-id="65d33-131">Name the project *ToDoApi*.</span></span>

![新的 ASP.NET Web 應用程式對話方塊，當中已選取了 Web API 專案範本](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="65d33-133">應用程式現在應該會回應所有傳送到連接埠 5000 的要求。</span><span class="sxs-lookup"><span data-stu-id="65d33-133">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="65d33-134">更新 *Program.cs*，使其包含 `.UseUrls("http://*:5000")`。若要完成這項操作：</span><span class="sxs-lookup"><span data-stu-id="65d33-134">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="65d33-135">請確定您是直接執行應用程式，而非在 IIS Express 之後執行，因為其根據預設會忽略所有非本機的要求。</span><span class="sxs-lookup"><span data-stu-id="65d33-135">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="65d33-136">在命令提示字元中執行 [dotnet run](/dotnet/core/tools/dotnet-run)，或從 Visual Studio 工具列的 [偵錯目標] 下拉式功能表中選擇應用程式名稱設定檔。</span><span class="sxs-lookup"><span data-stu-id="65d33-136">Run [dotnet run](/dotnet/core/tools/dotnet-run) from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="65d33-137">新增一個模型類別來代表待辦項目。</span><span class="sxs-lookup"><span data-stu-id="65d33-137">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="65d33-138">使用 `[Required]` 屬性來標記必要欄位：</span><span class="sxs-lookup"><span data-stu-id="65d33-138">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="65d33-139">API 方法需要一些方式才能操作資料。</span><span class="sxs-lookup"><span data-stu-id="65d33-139">The API methods require some way to work with data.</span></span> <span data-ttu-id="65d33-140">使用原始 Xamarin 範本使用的相同 `IToDoRepository` 介面：</span><span class="sxs-lookup"><span data-stu-id="65d33-140">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="65d33-141">此範例中，實作只會使用私用項目集合：</span><span class="sxs-lookup"><span data-stu-id="65d33-141">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="65d33-142">設定 *Startup.cs* 中的實作：</span><span class="sxs-lookup"><span data-stu-id="65d33-142">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="65d33-143">此時，您已準備就緒可建立 *ToDoItemsController*。</span><span class="sxs-lookup"><span data-stu-id="65d33-143">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="65d33-144">在[使用 ASP.NET Core MVC 及 Visual Studio 建置第一個 Web API](../tutorials/first-web-api.md) 中深入了解如何建立 Web API。</span><span class="sxs-lookup"><span data-stu-id="65d33-144">Learn more about creating web APIs in [Build your first Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="65d33-145">建立控制器</span><span class="sxs-lookup"><span data-stu-id="65d33-145">Creating the Controller</span></span>

<span data-ttu-id="65d33-146">將新的控制器新增到專案，*ToDoItemsController*。</span><span class="sxs-lookup"><span data-stu-id="65d33-146">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="65d33-147">它應該繼承自 Microsoft.AspNetCore.Mvc.Controller。</span><span class="sxs-lookup"><span data-stu-id="65d33-147">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="65d33-148">新增一個 `Route` 屬性來表示控制器將會處理所有傳送到以 `api/todoitems` 開頭之路徑的要求。</span><span class="sxs-lookup"><span data-stu-id="65d33-148">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="65d33-149">路由中的 `[controller]` 權杖會由控制器的名稱取代 (省略 `Controller` 尾碼)，特別在全域路由時將會非常有幫助。</span><span class="sxs-lookup"><span data-stu-id="65d33-149">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="65d33-150">深入了解[路由](../fundamentals/routing.md)。</span><span class="sxs-lookup"><span data-stu-id="65d33-150">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="65d33-151">控制器需要一個 `IToDoRepository` 才能發揮功能，請在控制器的建構函式中要求此類型的執行個體。</span><span class="sxs-lookup"><span data-stu-id="65d33-151">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="65d33-152">在執行階段，這個執行個體會使用架構的[相依性插入](../fundamentals/dependency-injection.md)支援提供。</span><span class="sxs-lookup"><span data-stu-id="65d33-152">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="65d33-153">這個 API 支援四種不同的 HTTP 動詞命令來在資料來源上執行 CRUD (建立、讀取、更新、刪除) 作業。</span><span class="sxs-lookup"><span data-stu-id="65d33-153">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="65d33-154">其中最簡單的便是「讀取」作業，其對應到 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="65d33-154">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="65d33-155">讀取項目</span><span class="sxs-lookup"><span data-stu-id="65d33-155">Reading Items</span></span>

<span data-ttu-id="65d33-156">您可以藉由對 `List` 方法傳送 GET 要求來要求項目清單。</span><span class="sxs-lookup"><span data-stu-id="65d33-156">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="65d33-157">`List` 方法上的 `[HttpGet]` 屬性表示這項動作應該僅用於處理 GET 要求。</span><span class="sxs-lookup"><span data-stu-id="65d33-157">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="65d33-158">此動作的路由為在控制器上指定的路由。</span><span class="sxs-lookup"><span data-stu-id="65d33-158">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="65d33-159">您不見得需要使用動作名稱作為路由的一部分。</span><span class="sxs-lookup"><span data-stu-id="65d33-159">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="65d33-160">您只需要確認每個動作都有一個唯一且明確的路由。</span><span class="sxs-lookup"><span data-stu-id="65d33-160">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="65d33-161">路由屬性可套用在控制器和方法層級上，以建置特定的路由。</span><span class="sxs-lookup"><span data-stu-id="65d33-161">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="65d33-162">`List` 方法會傳回一個 200 OK 回應碼，以及所有已序列化為 JSON 的待辦項目。</span><span class="sxs-lookup"><span data-stu-id="65d33-162">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="65d33-163">您可以使用各種不同的工具測試您的新 API 方法，例如 [Postman](https://www.getpostman.com/docs/)，如這裡所示：</span><span class="sxs-lookup"><span data-stu-id="65d33-163">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Postman 主控台會顯示針對待辦項目的 GET 要求，以及顯示傳回了三個項目之 JSON 的回應主體](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="65d33-165">建立項目</span><span class="sxs-lookup"><span data-stu-id="65d33-165">Creating Items</span></span>

<span data-ttu-id="65d33-166">根據慣例，建立新的資料項目會對應到 HTTP POST 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="65d33-166">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="65d33-167">`Create` 方法套用了一個 `[HttpPost]` 屬性，並且接受一個 `ToDoItem` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="65d33-167">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="65d33-168">由於 `item` 引數會在 POST 主體中傳遞，此參數會使用 `[FromBody]` 屬性進行修飾。</span><span class="sxs-lookup"><span data-stu-id="65d33-168">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="65d33-169">在方法中，項目會經過有效性的檢查，以及是否先前存在過資料存放區中。若沒有發現任何問題，則該項目便會新增至存放庫中。</span><span class="sxs-lookup"><span data-stu-id="65d33-169">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="65d33-170">檢查 `ModelState.IsValid` 會執行 [模型驗證](../mvc/models/validation.md)，並且應該要在每個接受使用者輸入的 API 方法中執行。</span><span class="sxs-lookup"><span data-stu-id="65d33-170">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="65d33-171">此範例使用了一個包含了傳遞給行動裝置用戶端錯誤代碼的列舉：</span><span class="sxs-lookup"><span data-stu-id="65d33-171">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="65d33-172">藉由選擇 POST 動詞並在要求主體中使用 JSON 格式提供新物件，來使用 Postman 測試新增新項目。</span><span class="sxs-lookup"><span data-stu-id="65d33-172">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="65d33-173">您也應該新增一個要求標頭，將 `Content-Type` 指定為 `application/json`。</span><span class="sxs-lookup"><span data-stu-id="65d33-173">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Postman 主控台顯示 POST 及回應](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="65d33-175">方法會在回應中傳回新建立的項目。</span><span class="sxs-lookup"><span data-stu-id="65d33-175">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="65d33-176">更新項目</span><span class="sxs-lookup"><span data-stu-id="65d33-176">Updating Items</span></span>

<span data-ttu-id="65d33-177">修改記錄會使用到 HTTP PUT 要求。</span><span class="sxs-lookup"><span data-stu-id="65d33-177">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="65d33-178">除了這項變更之外，`Edit` 方法與 `Create` 方法基本上都完全相同。</span><span class="sxs-lookup"><span data-stu-id="65d33-178">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="65d33-179">請注意，若找不到記錄，`Edit` 動作會傳回一個 `NotFound` (404) 回應。</span><span class="sxs-lookup"><span data-stu-id="65d33-179">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="65d33-180">若要使用 Postman 進行測試，請將動詞變更為 PUT。</span><span class="sxs-lookup"><span data-stu-id="65d33-180">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="65d33-181">在要求主體中指定更新物件資料。</span><span class="sxs-lookup"><span data-stu-id="65d33-181">Specify the updated object data in the Body of the request.</span></span>

![Postman 主控台顯示 PUT 及回應](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="65d33-183">這個方法會在成功時傳回一個 `NoContent` (204) 回應 (為了與先前存在的 API 保持一致)。</span><span class="sxs-lookup"><span data-stu-id="65d33-183">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="65d33-184">刪除項目</span><span class="sxs-lookup"><span data-stu-id="65d33-184">Deleting Items</span></span>

<span data-ttu-id="65d33-185">刪除記錄可透過傳送 DELETE 要求到服務，並傳遞要刪除之項目的識別碼來完成。</span><span class="sxs-lookup"><span data-stu-id="65d33-185">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="65d33-186">與更新時一樣，若要求項目不存在，使用者便會接收到 `NotFound` 回應。</span><span class="sxs-lookup"><span data-stu-id="65d33-186">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="65d33-187">否則，成功的要求會取得一個 `NoContent` (204) 回應。</span><span class="sxs-lookup"><span data-stu-id="65d33-187">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="65d33-188">請注意，當測試刪除功能時，要求主體中不需要任何內容。</span><span class="sxs-lookup"><span data-stu-id="65d33-188">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Postman 主控台顯示 DELETE 及回應](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="65d33-190">常見 Web API 慣例</span><span class="sxs-lookup"><span data-stu-id="65d33-190">Common Web API Conventions</span></span>

<span data-ttu-id="65d33-191">當您為您的應用程式開發後端服務時，您可能會想要想出一個一致的慣例組或原則來處理跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="65d33-191">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="65d33-192">例如，在上述的服務中，要求找不到的特定記錄會讓使用者接收到 `NotFound` 回應，而非 `BadRequest` 回應。</span><span class="sxs-lookup"><span data-stu-id="65d33-192">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="65d33-193">同樣地，傳送到此服務的命令在傳遞到模型繫結類型時，也會檢查 `ModelState.IsValid`並針對無效的模型類型傳回 `BadRequest`。</span><span class="sxs-lookup"><span data-stu-id="65d33-193">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="65d33-194">當您找到了適用於您 API 的常見原則時，您通常可以在[篩選條件](../mvc/controllers/filters.md)中封裝它。</span><span class="sxs-lookup"><span data-stu-id="65d33-194">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="65d33-195">深入了解[如何在 ASP.NET Core MVC 應用程式中封裝常見的 API 原則](https://msdn.microsoft.com/magazine/mt767699.aspx)。</span><span class="sxs-lookup"><span data-stu-id="65d33-195">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="65d33-196">其他資源</span><span class="sxs-lookup"><span data-stu-id="65d33-196">Additional resources</span></span>

* [<span data-ttu-id="65d33-197">驗證與授權</span><span class="sxs-lookup"><span data-stu-id="65d33-197">Authentication and Authorization</span></span>](/xamarin/xamarin-forms/enterprise-application-patterns/authentication-and-authorization)
