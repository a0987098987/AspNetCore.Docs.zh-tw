---
title: 使用 ASP.NET Core 建立原生行動應用程式的後端服務
author: ardalis
description: 了解如何使用 ASP.NET Core MVC 建立後端服務，以支援原生行動應用程式。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mobile/native-mobile-backend
ms.openlocfilehash: 18aecea00eb9cda3462ede7e478616a99cf302f8
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a>使用 ASP.NET Core 建立原生行動應用程式的後端服務

作者：[Steve Smith](https://ardalis.com/)

行動裝置應用程式可以輕鬆的與 ASP.NET Core 後端服務進行通訊。

[檢視或下載簡易後端服務程式碼範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>原生行動應用程式範例

本教學課程會示範如何使用 ASP.NET Core MVC 建立後端服務，以支援原生行動裝置應用程式。 它會使用 [Xamarin Forms ToDoRest 應用程式](/xamarin/xamarin-forms/data-cloud/consuming/rest) 作為其原生用戶端，其包含了適用於 Android、iOS、Windows 通用，以及 Windows Phone 裝置的個別原生用戶端。 您可以遵循連結的教學課程來建立原生應用程式 (並安裝必要的免費 Xamarin 工具)，也可以下載 Xamarin 範例解決方案。 Xamarin 範例包含了 ASP.NET Web API 2 服務專案。該專案已由此文章的 ASP.NET Core 應用程式取代 (但用戶端不需要進行任何變更)。

![To Do Rest 應用程式在 Android 智慧型手機上執行](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>功能

ToDoRest 應用程式支援列出、新增、刪除和更新待辦項目。 每個項目都有識別碼、名稱、記事和標示其是否已完成的屬性。

項目的主要檢視，如上所示，會列出每個項目的名稱，並使用勾選記號表示其是否已完成。

點選 `+` 圖示便會開啟 [新增項目] 對話方塊：

![[新增項目] 對話方塊](native-mobile-backend/_static/todo-android-new-item.png)

在主要清單畫面中點選項目，便會開啟 [編輯] 對話方塊，讓使用者修改項目的名稱、記事及完成狀態，或是刪除該項目：

![[編輯項目] 對話方塊](native-mobile-backend/_static/todo-android-edit-item.png)

這個範例根據預設已設定為使用託管於 developer.xamarin.com 上的後端服務，允許唯讀操作。 若要自行測試您在下一個章節建立的，於您的電腦上執行的 ASP.NET Core 應用程式，您必須更新應用程式的 `RestUrl` 常數。 巡覽至 `ToDoREST` 專案並開啟 *Constants.cs* 檔案。 將 `RestUrl` 取代為包含您電腦 IP 位址的 URL (不是 localhost 或 127.0.0.1，因為這些位址僅用於裝置模擬器，而非您的電腦)。 在其中包含連接埠號碼 (5000)。 為了測試您的服務可以在裝置上運作，請確認您沒有作用中的防火牆正在封鎖此連接埠的存取。

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>建立 ASP.NET Core 專案

在 Visual Studio 中建立新的 ASP.NET Core Web 應用程式。 選擇 [Web API template] (Web API 範本) 及 [No Authentication] (無驗證)。 將專案命名為 *ToDoApi*。

![新的 ASP.NET Web 應用程式對話方塊，當中已選取了 Web API 專案範本](native-mobile-backend/_static/web-api-template.png)

應用程式現在應該會回應所有傳送到連接埠 5000 的要求。 更新 *Program.cs*，使其包含 `.UseUrls("http://*:5000")`。若要完成這項操作：

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> 請確定您是直接執行應用程式，而非在 IIS Express 之後執行，因為其根據預設會忽略所有非本機的要求。 在命令提示字元中執行 [dotnet run](/dotnet/core/tools/dotnet-run)，或從 Visual Studio 工具列的 [偵錯目標] 下拉式功能表中選擇應用程式名稱設定檔。

新增一個模型類別來代表待辦項目。 使用 `[Required]` 屬性來標記必要欄位：

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

API 方法需要一些方式才能操作資料。 使用原始 Xamarin 範本使用的相同 `IToDoRepository` 介面：

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

此範例中，實作只會使用私用項目集合：

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

設定 *Startup.cs* 中的實作：

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

此時，您已準備就緒可建立 *ToDoItemsController*。

> [!TIP]
> 在[使用 ASP.NET Core MVC 及 Visual Studio 建置第一個 Web API](../tutorials/first-web-api.md) 中深入了解如何建立 Web API。

## <a name="creating-the-controller"></a>建立控制器

將新的控制器新增到專案，*ToDoItemsController*。 它應該繼承自 Microsoft.AspNetCore.Mvc.Controller。 新增一個 `Route` 屬性來表示控制器將會處理所有傳送到以 `api/todoitems` 開頭之路徑的要求。 路由中的 `[controller]` 權杖會由控制器的名稱取代 (省略 `Controller` 尾碼)，特別在全域路由時將會非常有幫助。 深入了解[路由](../fundamentals/routing.md)。

控制器需要一個 `IToDoRepository` 才能發揮功能，請在控制器的建構函式中要求此類型的執行個體。 在執行階段，這個執行個體會使用架構的[相依性插入](../fundamentals/dependency-injection.md)支援提供。

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

這個 API 支援四種不同的 HTTP 動詞命令來在資料來源上執行 CRUD (建立、讀取、更新、刪除) 作業。 其中最簡單的便是「讀取」作業，其對應到 HTTP GET 要求。

### <a name="reading-items"></a>讀取項目

您可以藉由對 `List` 方法傳送 GET 要求來要求項目清單。 `List` 方法上的 `[HttpGet]` 屬性表示這項動作應該僅用於處理 GET 要求。 此動作的路由為在控制器上指定的路由。 您不見得需要使用動作名稱作為路由的一部分。 您只需要確認每個動作都有一個唯一且明確的路由。 路由屬性可套用在控制器和方法層級上，以建置特定的路由。

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

`List` 方法會傳回一個 200 OK 回應碼，以及所有已序列化為 JSON 的待辦項目。

您可以使用各種不同的工具測試您的新 API 方法，例如 [Postman](https://www.getpostman.com/docs/)，如這裡所示：

![Postman 主控台會顯示針對待辦項目的 GET 要求，以及顯示傳回了三個項目之 JSON 的回應主體](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>建立項目

根據慣例，建立新的資料項目會對應到 HTTP POST 動詞命令。 `Create` 方法套用了一個 `[HttpPost]` 屬性，並且接受一個 `ToDoItem` 執行個體。 由於 `item` 引數會在 POST 主體中傳遞，此參數會使用 `[FromBody]` 屬性進行修飾。

在方法中，項目會經過有效性的檢查，以及是否先前存在過資料存放區中。若沒有發現任何問題，則該項目便會新增至存放庫中。 檢查 `ModelState.IsValid` 會執行 [模型驗證](../mvc/models/validation.md)，並且應該要在每個接受使用者輸入的 API 方法中執行。

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

此範例使用了一個包含了傳遞給行動裝置用戶端錯誤代碼的列舉：

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

藉由選擇 POST 動詞並在要求主體中使用 JSON 格式提供新物件，來使用 Postman 測試新增新項目。 您也應該新增一個要求標頭，將 `Content-Type` 指定為 `application/json`。

![Postman 主控台顯示 POST 及回應](native-mobile-backend/_static/postman-post.png)

方法會在回應中傳回新建立的項目。

### <a name="updating-items"></a>更新項目

修改記錄會使用到 HTTP PUT 要求。 除了這項變更之外，`Edit` 方法與 `Create` 方法基本上都完全相同。 請注意，若找不到記錄，`Edit` 動作會傳回一個 `NotFound` (404) 回應。

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

若要使用 Postman 進行測試，請將動詞變更為 PUT。 在要求主體中指定更新物件資料。

![Postman 主控台顯示 PUT 及回應](native-mobile-backend/_static/postman-put.png)

這個方法會在成功時傳回一個 `NoContent` (204) 回應 (為了與先前存在的 API 保持一致)。

### <a name="deleting-items"></a>刪除項目

刪除記錄可透過傳送 DELETE 要求到服務，並傳遞要刪除之項目的識別碼來完成。 與更新時一樣，若要求項目不存在，使用者便會接收到 `NotFound` 回應。 否則，成功的要求會取得一個 `NoContent` (204) 回應。

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

請注意，當測試刪除功能時，要求主體中不需要任何內容。

![Postman 主控台顯示 DELETE 及回應](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>常見 Web API 慣例

當您為您的應用程式開發後端服務時，您可能會想要想出一個一致的慣例組或原則來處理跨領域關注。 例如，在上述的服務中，要求找不到的特定記錄會讓使用者接收到 `NotFound` 回應，而非 `BadRequest` 回應。 同樣地，傳送到此服務的命令在傳遞到模型繫結類型時，也會檢查 `ModelState.IsValid`並針對無效的模型類型傳回 `BadRequest`。

當您找到了適用於您 API 的常見原則時，您通常可以在[篩選條件](../mvc/controllers/filters.md)中封裝它。 深入了解[如何在 ASP.NET Core MVC 應用程式中封裝常見的 API 原則](https://msdn.microsoft.com/magazine/mt767699.aspx)。
