---
uid: single-page-application/overview/introduction/knockoutjs-template
title: "單一網頁應用程式： KnockoutJS 範本 |Microsoft 文件"
author: MikeWasson
description: "Knockout 範本"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 6e84dcc16345e33fcd3a3f83c4b35bc993c03ca6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="single-page-application-knockoutjs-template"></a>單一網頁應用程式： KnockoutJS 範本
====================
由[Mike Wasson](https://github.com/MikeWasson)

> Knockout MVC 範本屬於的 ASP.NET 和 Web 工具 2012.2
> 
> [下載 ASP.NET 及 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


ASP.NET 和 Web 工具 2012.2 更新包含可用於 ASP.NET MVC 4 單一頁面應用程式 (SPA) 範本。 此範本被設計來協助您快速地開始建置互動式用戶端 web 應用程式。

「 單一頁面應用程式 」 (SPA) 是載入單一 HTML 頁面，並以動態方式，而不必載入新的頁面更新頁面的 web 應用程式的一般詞彙。 初始頁面載入之後, SPA 交談 AJAX 要求透過與伺服器。

![](knockoutjs-template/_static/image1.png)

AJAX 沒有新增，但現在有更輕鬆地建立及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。 此外，html5 和 CSS3 會讓您更輕鬆地建立豐富的 Ui。

若要取得您開始使用，SPA 範本會建立範例 「 待辦事項清單 」 應用程式。 在本教學課程中，我們要花導覽的範本。 首先我們要查看 待辦事項清單應用程式本身，，然後檢查技術片段，讓它運作。

## <a name="create-a-new-spa-template-project"></a>建立新的 SPA 範本專案

需求：

- Visual Studio 2012 或 Visual Studio Express 2012 for Web
- ASP.NET Web 工具 2012.2 更新。 您可以安裝更新[這裡](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)。

啟動 Visual Studio，然後選取**新專案**從 [開始] 頁面。 或從**檔案**功能表上，選取**新增**然後**專案**。

在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。 在下**Visual C#**，選取**Web**。 在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。 為專案名稱，然後按一下**確定**。

![](knockoutjs-template/_static/image2.png)

在**新專案**精靈中，選取**單一網頁應用程式**。

![](knockoutjs-template/_static/image3.png)

按 F5 鍵建置並執行應用程式。 當第一次執行應用程式時，它會顯示登入畫面。

![](knockoutjs-template/_static/image4.png)

按一下&quot;註冊&quot;連結，並建立新的使用者。

![](knockoutjs-template/_static/image5.png)

登入之後，應用程式會建立預設 Todo 清單與兩個項目。 您可以按一下 [加入 Todo 清單] 以加入新的清單。

![](knockoutjs-template/_static/image6.png)

重新命名清單、 將項目加入至清單中，並取消選取這些工作。 您也可以刪除的項目，或刪除整個清單。 所做的變更會自動保存至伺服器上的資料庫 (實際上 LocalDB 此時，因為您在本機執行應用程式)。

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>SPA 範本的架構

這個圖表會顯示主要的建置組塊，應用程式。

![](knockoutjs-template/_static/image8.png)

在伺服器端，ASP.NET MVC 提供 HTML 和也會處理表單型驗證。

ASP.NET Web 應用程式開發介面會處理與 ToDoLists 及 ToDoItems，包括取得、 建立、 更新和刪除相關聯的所有要求。 用戶端交換資料的 Web API 以 JSON 格式。

Entity Framework (EF) 是 O/RM 層。 它會調解物件導向的世界 ASP.NET 和基礎資料庫之間。 資料庫使用 LocalDB，但您可以變更這個 Web.config 檔案中。 通常您會使用本機開發 LocalDB 然後再部署到 SQL 資料庫的伺服器上，使用 EF 程式碼優先 （contract-first） 移轉。

在用戶端，解 Knockout.js 程式庫會處理從 AJAX 要求的頁面更新。 Knockout 與最新的資料同步處理頁面使用資料繫結。 這樣一來，您不需要撰寫任何程式碼，逐步解說的 JSON 資料，並更新 DOM 相反地，您將宣告式屬性放在告訴 Knockout 如何將資料呈現的 HTML。

這個架構的一大優點是它會展示層應用程式邏輯分隔。 您可以建立 Web 應用程式開發介面部分，而不需要知道任何關於您的網頁的外觀。 在用戶端，您必須建立 「 檢視模型 」 來表示該資料，並檢視模型使用 Knockout 繫結的 html。 可讓您輕鬆地變更而不需要變更檢視模式的 HTML。 （我們會探討 Knockout 稍待片刻。）

## <a name="models"></a>模型

在 Visual Studio 專案中，[模型] 資料夾會包含伺服器端會使用的模型。 （用戶端上還有模型，我們會開始將那些）。

![](knockoutjs-template/_static/image9.png)

**TodoItem TodoList**

這些是資料庫模型的 Entity Framework Code First。 請注意，這些模型會有個點的內容。 `ToDoList`包含的 ToDoItems，以及每個集合`ToDoItem`ToDoList 其父代參考。 這些屬性稱為導覽屬性，它們代表的一對多關聯性，其待辦項目和待辦事項清單。

`ToDoItem`類別也會使用**[ForeignKey]**屬性來指定`ToDoListId`是外部索引鍵`ToDoList`資料表。 這會告訴 EF 外部索引鍵條件約束加入資料庫。

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto TodoListDto**

這些類別會定義將會傳送至用戶端的資料。 「 DTO"代表 「 資料傳輸物件 」。 DTO 定義實體將如何序列化為 JSON。 一般情況下，有幾個原因會使用 DTOs:

- 若要控制序列化的屬性。 DTO 可包含從網域模型屬性的子集。 基於安全性考量 （若要隱藏敏感性資料） 或只是您可能會執行這來減少您所傳送的資料量。
- 若要將資料 – 例如壓平合併的更複雜的資料結構。
- 若要保留 DTO （的重要性分離） 超出任何商務邏輯。
- 如果基於某些原因，無法序列化網域模型。 比方說，循環參考可能會造成問題時您序列化的物件有方式處理這個問題，Web API 中的 (請參閱[處理循環的物件參考](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); 但只要使用 DTO 完全避免此問題。

SPA 在範本中 DTOs 包含網域模型相同的資料。 不過，它們仍很實用是因為它們避免了循環參考的導覽屬性，並示範一般 DTO 模式。

**AccountModels.cs**

這個檔案包含站台的成員資格的模型。 `UserProfile`類別定義成員資格資料庫中的使用者設定檔的結構描述。 （在此情況下，唯一資訊是使用者識別碼和使用者名稱）。此檔案中的其他模型類別可用來建立使用者註冊和登入表單。

## <a name="entity-framework"></a>Entity Framework

SPA 範本使用 EF Code First。 在 Code First 開發中，您定義模型，先在程式碼中，然後 EF 使用模型來建立資料庫。 您也可以使用 EF 與現有的資料庫 ([Database First](https://msdn.microsoft.com/en-us/data/jj206878.aspx))。

`TodoItemContext` Models 資料夾中的類別衍生自**DbContext**。 這個類別會提供 「 黏附 」 模型與 EF 之間。 `TodoItemContext`保存`ToDoItem`集合和`TodoList`集合。 若要查詢資料庫，您只要撰寫 LINQ 查詢，針對這些集合。 例如，以下是選取所有的使用者"Alice"待辦事項清單：

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

也可以加入新項目至集合、 更新項目，或從集合中，刪除項目和保存至資料庫的變更。

## <a name="aspnet-web-api-controllers"></a>ASP.NET Web API 控制器

在 ASP.NET Web API 中，控制器會處理 HTTP 要求的物件。 如前所述，SPA 範本就會使用 Web API，讓上的 CRUD 作業`ToDoList`和`ToDoItem`執行個體。 控制站位於 [控制器] 資料夾的方案。

![](knockoutjs-template/_static/image10.png)

- `TodoController`： 用來處理 HTTP 要求的待辦項目
- `TodoListController`： 會處理 HTTP 要求的待辦事項清單。

這些名稱皆屬於顯著，因為 Web API 符合控制器名稱的 URI 路徑。 (若要深入了解 Web API 將 HTTP 要求路由到控制器的方式，請參閱[中 ASP.NET Web API 的路由](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)。)

讓我們看看`ToDoListController`類別。 它包含單一資料成員：

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext`用於通訊使用 EF，如先前所述。 在控制器上的方法實作 CRUD 作業。 Web API 會對應至控制器方法的 HTTP 要求從用戶端，如下所示：

| HTTP 要求 | 控制器方法 | 說明 |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | 取得集合的待辦事項清單。 |
| GET/api/todo/*識別碼* | `GetTodoList` | 取得依識別碼待辦事項清單 |
| PUT/api/todo/*識別碼* | `PutTodoList` | 更新 待辦事項清單。 |
| POST /api/todo | `PostTodoList` | 建立新的待辦事項清單。 |
| 刪除/api/todo/*識別碼* | `DeleteTodoList` | 刪除 TODO 清單。 |

請注意，某些作業的 Uri 包含預留位置的識別碼值。 例如，若要刪除以清單、 識別碼為 42，URI 是`/api/todo/42`。

若要了解有關使用 CRUD 作業的 Web API 的詳細資訊，請參閱[建立 Web API 的支援 CRUD 操作](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md)。 此控制站的程式碼就相當簡單。 以下是一些有趣的重點：

- `GetTodoLists`方法使用 LINQ 查詢來篩選結果的識別碼登入使用者。 這樣一來，使用者只能看到他/她屬於的資料。 另外而且請注意，Select 陳述式用來轉換`ToDoList`到執行個體`TodoListDto`執行個體。
- PUT 和 POST 方法修改資料庫前，先檢查模型狀態。 如果**ModelState.IsValid**為 false，這些方法會傳回 HTTP 400 不正確的要求。 深入了解 Web API 中的模型驗證[模型驗證](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md)。
- 控制器類別也以裝飾**[Authorize]**屬性。 這個屬性會檢查是否已驗證的 HTTP 要求。 如果要求未經過驗證，用戶端收到 HTTP 401 未經授權。 深入了解在驗證[驗證和授權的 ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md)。

`TodoController`類別是非常類似於`TodoListController`。 最大的差異是，它並未定義任何 GET 方法，因為用戶端會收到的待辦項目，以及每個待辦事項清單。

## <a name="mvc-controllers-and-views"></a>MVC 控制器和檢視

MVC 控制器也位於 [控制器] 資料夾的方案。 `HomeController`應用程式的主要 HTML 呈現。 Views/Home/Index.cshtml 中定義的檢視主控制器。 [首頁] 檢視會呈現不同的內容，根據使用者是否登入：

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

當使用者登入時，他們會看到的主要 UI。 否則，他們會看到 [登入] 面板。 請注意，在伺服器端上發生的這個條件的轉譯。 永遠不會嘗試隱藏機密用戶端上的內容 & #8212anything 您傳送的 HTTP 回應中會顯示給監看的未經處理的 HTTP 訊息的任何人。

## <a name="client-side-javascript-and-knockoutjs"></a>用戶端 JavaScript 和解 Knockout.js

現在讓我們先從伺服器端的用戶端應用程式。 SPA 範本會建立 smooth、 更互動的 UI 使用 jQuery 和解 Knockout.js 的組合。 解 Knockout.js 是 JavaScript 程式庫，以簡化繫結至資料的 HTML。 會使用稱為 「 模型-檢視-ViewModel。 」 模式，解 Knockout.js

- （ToDo 清單和 ToDo 項目） 的網域資料模型。
- 檢視是 HTML 文件。
- 檢視模型是 JavaScript 物件保存的模型資料。 檢視模型是抽象的 ui 的程式碼。 它不了解 HTML 表示法。 相反地，它代表抽象檢視，例如 「 ToDo 項目的清單 」 的功能。

檢視是資料繫結至檢視模型。 檢視模型的更新會自動反映在檢視中。 繫結都可以從另一方向以及。 在 DOM 中 （例如按下） 的事件資料繫結函式上檢視模型中，觸發 AJAX 呼叫。

SPA 範本會將用戶端 JavaScript 組織成三個層級：

- todo.datacontext.js： 傳送 AJAX 要求。
- todo.model.js： 定義模型。
- todo.viewmodel.js： 定義檢視模型。

![](knockoutjs-template/_static/image11.png)

這些指令碼檔案位於方案的指令碼/應用程式資料夾中。

![](knockoutjs-template/_static/image12.png)

**todo.datacontext**處理所有 AJAX 呼叫 Web API 控制器。 （登入的 AJAX 呼叫已定義在其他地方，ajaxlogin.js 中。）

**todo.model.js**定義的待辦事項清單中的用戶端 （瀏覽器） 模型。 有兩種模型類別： todoItem 和 todoList。

許多模型類別中的屬性都屬於類型"ko.observable"。 可預見值是 Knockout 如何工作。 從[Knockout 文件](http://knockoutjs.com/documentation/introduction.html): observable 為 「 JavaScript 物件通知有關變更的訂閱者 」。 可觀察的值變更時發生，Knockout 更新繫結至這些可預見值的任何 HTML 項目。 例如，todoItem 具有標題和 isDone 屬性可預見值：

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

您也可以訂閱可觀察程式碼中。 例如，todoItem 類別訂閱中的"isDone"和"title"屬性的變更：

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**檢視模型**

檢視模型被定義於 todo.viewmodel.js。 檢視模型是在應用程式繫結的 HTML 頁面項目網域資料中心點。 SPA 在範本中檢視模型包含可觀察 todoLists 的陣列。 檢視模型中的下列程式碼會告知 Knockout 套用繫結：

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML 和資料繫結

Views/Home/Index.cshtml 中定義主要的 HTML 頁面。 因為我們使用資料繫結，HTML 是什麼實際取得呈現的範本。 使用 knockout*宣告式*繫結。 加入至項目 」 資料繫結 」 屬性，將頁面項目的資料繫結。 以下是非常簡單的範例中，取自 Knockout 文件：

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

在此範例中，Knockout 更新的內容**&lt;跨越&gt;**項目，其值為`myItems.count()`。 每當此值變更時，Knockout 更新文件。

Knockout 提供數種不同的繫結的類型。 以下是一些使用 SPA 範本中的繫結：

- **foreach**： 可讓您逐一查看迴圈，並在清單中的每個項目套用相同的標記。 這用來呈現待辦事項清單和待辦項目。 內**foreach**，繫結會套用至清單的項目。
- **可見**： 用來切換可見性。 當集合是空的隱藏的標記，或顯示錯誤訊息。
- **值**： 用來填入表單值。
- **按一下**： 將 click 事件繫結至檢視模型中的函式。

## <a name="anti-csrf-protection"></a>反 CSRF 防護

跨站台要求偽造 (CSRF) 攻擊是惡意網站傳送要求到易受攻擊的站台，使用者目前登入的位置。 為了避免 CSRF 攻擊，ASP.NET MVC 會使用*防偽語彙基元*，也稱為驗證語彙基元要求。 這個概念是，伺服器會將隨機產生的語彙基元放入網頁。 當用戶端送出至伺服器的資料時，它必須在要求訊息中包含此值。

防偽語彙基元運作，因為惡意的頁面無法讀取使用者的權杖，因為相同原始原則。 （相同原始原則可避免兩個不同的站台存取彼此的內容上裝載的文件）。

ASP.NET MVC 提供防偽語彙基元內建支援，透過[AntiForgery](https://msdn.microsoft.com/en-us/library/system.web.helpers.antiforgery.aspx)類別和[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute.aspx)屬性。 目前，這項功能不會建立到 Web API。 不過，SPA 範本包含 Web API 的自訂實作。 此代碼會定義在`ValidateHttpAntiForgeryTokenAttribute`類別位於方案的 [篩選器] 資料夾中。 若要深入了解反-CSRF Web API 中，請參閱[防止跨站台要求偽造 」 (CSRF) 攻擊](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md)。

## <a name="conclusion"></a>結論

SPA 範本被設計來協助您開始快速地撰寫現代化的互動式 web 應用程式。 它會解 Knockout.js 程式庫使用來分隔資料和應用程式邏輯的呈現方式 （HTML 標記）。 但是 Knockout 不是唯一可用來建立 SPA 的 JavaScript 程式庫。 如果您想要瀏覽一些其他選項，看看[社群建立的 SPA 樣板](../templates/index.md)。
