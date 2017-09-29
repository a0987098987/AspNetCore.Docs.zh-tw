---
title: "建立原生行動應用程式的後端服務"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3b6a32f2-5af9-4ede-9b7f-17ab300526d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: be1cd9f4fe41f1a79669975cb6a89439cdd9e5c7
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="creating-backend-services-for-native-mobile-applications"></a>建立原生行動應用程式的後端服務

由[Steve Smith](https://ardalis.com/)

行動裝置應用程式可以輕鬆地進行通訊與 ASP.NET Core 後端服務。

[檢視或下載後端服務程式碼範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>範例原生行動應用程式

本教學課程會示範如何建立使用 ASP.NET Core MVC 支援原生行動應用程式的後端服務。 它會使用[Xamarin Forms ToDoRest 應用程式](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/)為其原生用戶端，包括 Android、 iOS、 Windows 通用和 Windows Phone 裝置的個別原生用戶端。 您可以依照連結的教學課程，若要建立原生應用程式 （並安裝所需的免費 Xamarin 工具），以及下載 Xamarin 範例方案。 Xamarin 範例包含 ASP.NET Web API 2 services 專案，這篇文章 ASP.NET Core 應用程式取代 （用戶端所需的任何變更）。

![若要執行的 Rest 應用程式在 Android 手機上執行](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>功能

ToDoRest 應用程式支援清單、 加入、 刪除和更新待辦項目。 每個項目有識別碼、 名稱、 提示和屬性，指出是否已完成尚未。

主要檢視的項目，如上所示，列出每個項目名稱，並指出它透過核取記號。

點選`+`圖示會開啟 [新增項目] 對話方塊：

![新增項目 對話方塊](native-mobile-backend/_static/todo-android-new-item.png)

點選 [主要清單] 畫面上的項目會開啟編輯對話方塊，其中的項目名稱、 提示和完成的設定，您可以修改，或者刪除項目：

![[編輯項目] 對話方塊](native-mobile-backend/_static/todo-android-edit-item.png)

這個範例會使用後端服務裝載於 developer.xamarin.com，允許唯讀作業的預設設定。 若要測試自己對您的電腦上執行的下一節中所建立的 ASP.NET Core 應用程式，您將需要更新的應用程式`RestUrl`常數。 瀏覽至`ToDoREST`專案，並開啟*Constants.cs*檔案。 取代`RestUrl`具有 URL，其中包含您的電腦 IP 位址 （不 localhost 或 127.0.0.1，因為此位址是使用從裝置模擬器，不是從您的電腦）。 包含連接埠號碼 (5000)。 若要測試您的服務使用的裝置，請確定您沒有使用中防火牆封鎖這個連接埠的存取。

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>建立 ASP.NET Core 專案

Visual Studio 中建立新的 ASP.NET Core Web 應用程式。 選擇 Web API 範本和非驗證。 將專案命名*ToDoApi*。

![新的 ASP.NET Web 應用程式 對話方塊，以選取的 Web API 專案範本](native-mobile-backend/_static/web-api-template.png)

應用程式應該回應建立到 5000 之間的所有要求。 更新*Program.cs*包含`.UseUrls("http://*:5000")`為了達成此目的：

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> 請確定您執行應用程式直接管理，而不是後置 IIS Express，預設情況下忽略非本機要求。 執行`dotnet run`從命令提示字元，或從 Visual Studio 工具列中的偵錯目標下拉式清單中選擇應用程式名稱設定檔。

將模型類別來代表待辦項目。 標記所需的欄位使用`[Required]`屬性：

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

API 方法需要某種方式來處理資料。 使用相同`IToDoRepository`介面原始 Xamarin 範例會使用：

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

此範例中，實作只會使用私用集合的項目：

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

設定中的實作*Startup.cs*:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

此時，您可以建立*ToDoItemsController*。

> [!TIP]
> 了解有關建立多個 web 應用程式開發介面中的[建置您第一個 Web 應用程式開發介面使用 ASP.NET Core MVC 和 Visual Studio](../tutorials/first-web-api.md)。

## <a name="creating-the-controller"></a>建立控制器

將新的控制站新增至專案， *ToDoItemsController*。 它應該是繼承自 Microsoft.AspNetCore.Mvc.Controller。 新增`Route`屬性來指出控制器將會處理做為開頭的路徑的要求`api/todoitems`。 `[controller]`路由中的權杖會由控制器的名稱取代 (省略`Controller`尾碼)，並對全域路由來說特別有用。 深入了解[路由](../fundamentals/routing.md)。

控制器必須`IToDoRepository`至函式，請要求此類型透過控制站的建構函式的執行個體。 在執行階段，這個執行個體將會提供使用 framework 支援的[相依性插入](../fundamentals/dependency-injection.md)。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

這個 API 支援四種不同的 HTTP 動詞命令，以資料來源上執行 CRUD （建立、 讀取、 更新、 刪除） 作業。 這些最簡單的是對應至 HTTP GET 要求的 「 讀取 」 操作。

### <a name="reading-items"></a>讀取項目

要求的項目清單會在 GET 要求來完成`List`方法。 `[HttpGet]`屬性`List`方法會指出此動作，才應該處理 GET 要求。 此動作的路由為控制器上指定的路由。 您不一定需要的動作名稱做為路由的一部分。 您只需要確保每個動作都有唯一且明確的路由。 路由的屬性可以套用在控制器和方法層級，來建立特定的路由。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

`List`方法會傳回 200 OK 回應程式碼和所有 ToDo 項目，序列化為 JSON。

您可以測試您新的應用程式開發介面方法，使用各種工具，例如[郵差](https://www.getpostman.com/docs/)，如下所示：

![郵差主控台顯示 todoitems 和顯示三個項目，傳回的 JSON 回應主體的 GET 要求](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>建立項目

依照慣例，建立新資料的項目會對應至 HTTP POST 動詞命令。 `Create`方法有`[HttpPost]`屬性套用至其中，並接受`ToDoItem`執行個體。 因為`item`引數會傳遞給在 post 要求主體中，這個參數以裝飾`[FromBody]`屬性。

在方法內，會檢查有效性和資料存放區中的預先存在的項目，如果不發生任何問題，就會加入使用儲存機制。 檢查`ModelState.IsValid`執行[模型驗證](../mvc/models/validation.md)，而且應該在每個應用程式開發介面方法可接受使用者輸入。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

此範例會使用列舉，包含錯誤代碼會傳遞給行動用戶端：

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

測試加入新項目使用郵差選擇 POST 動詞命令，提供要求主體中的 JSON 格式中的新物件。 您也應該將要求標頭，指定`Content-Type`的`application/json`。

![郵差主控台顯示 POST 和回應](native-mobile-backend/_static/postman-post.png)

方法會在回應中傳回新建立的項目。

### <a name="updating-items"></a>更新的項目

修改記錄使用 HTTP PUT 要求。 這項變更，以外`Edit`方法是幾乎完全相同`Create`。 請注意，如果找不到記錄、`Edit`動作將會傳回`NotFound`(404) 回應。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

若要測試郵差，變更 PUT 動詞命令。 要求主體中指定更新的物件資料。

![郵差主控台顯示 PUT 和回應](native-mobile-backend/_static/postman-put.png)

這個方法會傳回`NoContent`(204) 回應成功時，與現有的應用程式開發介面的一致性。

### <a name="deleting-items"></a>刪除項目

刪除資料錄被透過對服務進行的 DELETE 要求，並傳遞要刪除的項目 ID。 使用更新時，會收到不存在之項目的要求時`NotFound`回應。 否則，成功的要求將會得到`NoContent`(204) 回應。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

請注意，在測試時的刪除功能，執行任何動作都需要在要求主體。

![郵差主控台顯示的刪除和回應](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>常見的 Web API 慣例

當您開發應用程式後端服務時，您會想要的一組一致的慣例或處理跨碼橫切入顧慮的原則。 例如，如上所示，在服務中要求的特定記錄，找不到接收`NotFound`回應，而非`BadRequest`回應。 同樣地，對這項服務的繫結的模型類型一律檢查傳入的命令`ModelState.IsValid`並傳回`BadRequest`無效的模型型別。

一旦您已經為您的應用程式開發介面識別通用的原則，您可以通常封裝在[篩選](../mvc/controllers/filters.md)。 深入了解[如何封裝 ASP.NET Core MVC 應用程式中常見的 API 原則](https://msdn.microsoft.com/magazine/mt767699.aspx)。
