---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 單一頁面應用程式： KnockoutJS 範本 |Microsoft Docs
author: MikeWasson
description: Knockout 範本
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 328046363666944f121dedc1883bbe83f5b079d2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831819"
---
<a name="single-page-application-knockoutjs-template"></a>單一頁面應用程式： KnockoutJS 範本
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> Knockout MVC 範本屬於 ASP.NET 和 Web 工具 2012.2
> 
> [下載 ASP.NET 和 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


ASP.NET 和 Web 工具 2012.2 更新包含 ASP.NET MVC 4 單一頁面應用程式 (SPA) 範本。 此範本被設計來協助您開始快速建置互動式用戶端 web 應用程式。

「 單一頁面應用程式 」 (SPA) 是載入單一 HTML 頁面，並以動態方式，而不是載入新的頁面更新頁面的 web 應用程式的一般詞彙。 在初始網頁載入後，SPA 會與透過 AJAX 要求的伺服器。

![](knockoutjs-template/_static/image1.png)

AJAX 是新奇，但今天很輕鬆地建置及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。 此外，html5 和 css3 等讓您更輕鬆地建立豐富的 Ui。

若要開始，SPA 範本會建立 「 待辦事項清單 」 應用程式範例。 在本教學課程中，我們將帶導覽的範本。 第一次我們將查看待辦事項清單應用程式本身，並檢查技術項目，讓它運作。

## <a name="create-a-new-spa-template-project"></a>建立新的 SPA 專案範本

需求：

- Visual Studio 2012 或 Visual Studio Express 2012 for Web
- ASP.NET Web 工具 2012.2 更新。 您可以安裝更新[此處](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)。

啟動 Visual Studio，然後選取**新的專案**從 [開始] 頁面。 或從**檔案**功能表上，選取**新增**，然後**專案**。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名，然後按一下**確定**。

![](knockoutjs-template/_static/image2.png)

在 **新的專案**精靈中，選取**單一頁面應用程式**。

![](knockoutjs-template/_static/image3.png)

按 F5 鍵建置並執行應用程式。 當第一次執行應用程式時，它會顯示登入畫面。

![](knockoutjs-template/_static/image4.png)

按一下 &quot;註冊&quot;連結，並建立新的使用者。

![](knockoutjs-template/_static/image5.png)

登入之後，應用程式會建立預設待辦事項清單與兩個項目。 您可以按一下 [新增待辦事項清單] 以新增新的清單。

![](knockoutjs-template/_static/image6.png)

重新命名清單、 將項目加入清單中，並取消選取這些工作。 您也可以刪除項目，或刪除整個清單。 所做的變更會自動保存至伺服器上的資料庫 (實際 LocalDB 到目前為止，因為您在本機執行應用程式)。

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>SPA 範本的架構

下圖顯示應用程式的主要建置組塊。

![](knockoutjs-template/_static/image8.png)

在伺服器端，ASP.NET MVC 提供 HTML，並也會處理表單型驗證。

ASP.NET Web API 會處理與 ToDoLists 和 ToDoItems，包括取得、 建立、 更新及刪除相關的所有要求。 用戶端會交換使用 Web API 以 JSON 格式的資料。

Entity Framework (EF) 是 O/RM 層。 它會調解物件導向的世界的 ASP.NET 和基礎資料庫之間。 資料庫使用 LocalDB，但您可以變更這個 Web.config 檔案中。 通常您會使用 LocalDB，適用於本機開發，並接著部署至 SQL 資料庫的伺服器上，使用 EF code first 移轉。

在用戶端上，Knockout.js 程式庫會處理從 AJAX 要求的網頁更新。 Knockout 與最新的資料同步處理頁面使用資料繫結。 如此一來，您不需要撰寫任何程式碼，逐步解說的 JSON 資料，並更新 DOM 相反地，您將宣告式屬性放在告訴 Knockout 如何將資料呈現的 HTML。

此架構中的一大優點是，它會區隔展示層與應用程式邏輯。 您可以建立的 Web API 的部分，而不需要知道任何關於您的網頁的外觀。 在用戶端，您必須建立 「 檢視模型 」 來表示該資料，並檢視模型使用 Knockout 繫結至 HTML。 可讓您輕鬆地變更 HTML，而不需要變更檢視模型。 （我們將探討 Knockout 稍待片刻。）

## <a name="models"></a>模型

在 Visual Studio 專案中，於 Models 資料夾會包含伺服器端所使用的模型。 （用戶端上還有模型，我們會）。

![](knockoutjs-template/_static/image9.png)

**TodoItem TodoList**

這些是 Entity Framework Code First 的資料庫模型。 請注意，這些模型屬性，指向彼此。 `ToDoList` 包含的 ToDoItems，和每個集合`ToDoItem`ToDoList 其父代參考。 這些屬性稱為導覽屬性，以及它們在待辦事項清單，以及其待辦事項項目代表一個對多關聯性。

`ToDoItem`類別也會使用 **[ForeignKey]** 屬性來指定`ToDoListId`為外部索引鍵到`ToDoList`資料表。 這會告知 EF 來將外部索引鍵條件約束加入至資料庫。

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto TodoListDto**

這些類別會定義將會傳送至用戶端的資料。 "DTO"代表 「 資料傳輸物件 」。 DTO 會定義實體序列化為 JSON 的方式。 一般情況下，有幾個原因會使用 Dto:

- 若要控制序列化的屬性。 DTO 可以包含從領域模型屬性的子集。 您可能會基於安全性考量 （若要隱藏敏感資料），或只是執行這可減少您所傳送的資料。
- 若要變更圖形的資料，例如壓平合併的更複雜的資料結構。
- 若要保留 DTO （關注點分離） 超出任何商務邏輯。
- 如果基於某些原因，無法序列化您的網域模型。 比方說，循環參考可能會造成問題時您將有的物件的序列化方式來處理此問題，Web API 中的 (請參閱[處理循環物件參考](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); 但即可使用 DTO 完全避免此問題。

SPA 範本，在 Dto 會包含與領域模型相同的資料。 不過，它們是仍然有用，因為它們避免循環參考導覽屬性，並示範一般的 DTO 模式。

**AccountModels.cs**

此檔案包含模型的站台成員資格。 `UserProfile`類別定義成員資格資料庫中的使用者設定檔的結構描述。 （在此情況下，唯一的資訊是使用者識別碼和使用者名稱）。此檔案中的其他模型類別用來建立使用者註冊和登入表單。

## <a name="entity-framework"></a>Entity Framework

SPA 範本會使用 EF Code First。 在 Code First 開發中，您定義模型，第一次程式碼中，，然後 EF 使用模型來建立資料庫。 您也可以使用 EF 與現有的資料庫 ([Database First](https://msdn.microsoft.com/data/jj206878.aspx))。

`TodoItemContext` Models 資料夾中的類別衍生自**DbContext**。 這個類別會提供 「 黏附 」 模型與 EF 之間。 `TodoItemContext`保存`ToDoItem`集合和`TodoList`集合。 若要查詢資料庫，只要撰寫 LINQ 查詢，針對這些集合。 例如，以下是選取所有的使用者"Alice"的待辦事項清單：

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

也可以將新的項目新增至集合、 更新項目，或從集合中，刪除項目和保存資料庫的變更。

## <a name="aspnet-web-api-controllers"></a>ASP.NET Web API 控制器

在 ASP.NET Web API 中，控制器是處理 HTTP 要求的物件。 如前所述，SPA 範本就會使用 Web API，讓上的 CRUD 作業`ToDoList`和`ToDoItem`執行個體。 在 Controllers 資料夾中的方案位於控制器。

![](knockoutjs-template/_static/image10.png)

- `TodoController`： 用來處理 HTTP 要求的待辦事項項目
- `TodoListController`： 會處理 HTTP 要求的待辦事項清單。

因為 Web API 符合控制器名稱的 URI 路徑，是有意義的這些名稱。 (若要深入了解 Web API 將 HTTP 要求路由至控制器的方式，請參閱[ASP.NET Web API 中的路由](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)。)

讓我們看看`ToDoListController`類別。 它包含一個單一的資料成員：

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext`用來與 EF，通訊中，如先前所述。 在控制器上的方法實作的 CRUD 作業。 Web API 會對應至控制器方法，從用戶端的 HTTP 要求，如下所示：

| HTTP 要求 | 控制器方法 | 描述 |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | 取得待辦事項清單的集合。 |
| 取得/api/todo/*識別碼* | `GetTodoList` | 取得待辦事項清單識別碼 |
| PUT/api/todo/*識別碼* | `PutTodoList` | 更新待辦事項清單。 |
| POST /api/todo | `PostTodoList` | 建立新的待辦事項清單。 |
| 刪除/api/todo/*識別碼* | `DeleteTodoList` | 刪除待辦事項清單。 |

請注意，某些作業的 Uri 包含的識別碼值的預留位置。 例如，若要刪除以清單識別碼為 42，URI 是`/api/todo/42`。

若要深入了解使用 Web API 的 CRUD 作業，請參閱[建立 Web API 的支援 CRUD 作業](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md)。 此控制器的程式碼就相當簡單。 以下是一些有趣的要點：

- `GetTodoLists`方法使用 LINQ 查詢來篩選結果的識別碼的登入的使用者。 如此一來，使用者只能看到屬於他或她的資料。 另外，請注意，Select 陳述式用來轉換`ToDoList`執行個體到`TodoListDto`執行個體。
- PUT 和 POST 方法會檢查之前修改資料庫的模型狀態。 如果**ModelState.IsValid**為 false，這些方法會傳回 HTTP 400 不正確的要求。 深入了解在 Web API 中的模型驗證[模型驗證](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md)。
- 控制器類別也附有 **[Authorize]** 屬性。 這個屬性會檢查是否已驗證的 HTTP 要求。 如果要求未經過驗證，用戶端收到 HTTP 401 未經授權。 深入了解在驗證[ASP.NET Web API 中驗證和授權](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md)。

`TodoController`類別是非常類似於`TodoListController`。 最大的差異是，它並未定義任何的 GET 方法，因為用戶端會取得待辦事項項目，以及每個待辦事項清單。

## <a name="mvc-controllers-and-views"></a>MVC 控制器和檢視

MVC 控制站也位於 [控制器] 資料夾的解決方案。 `HomeController` 應用程式的主要 HTML 呈現。 主控制器的檢視被定義在 Views/Home/Index.cshtml。 [首頁] 檢視會呈現不同的內容，根據使用者是否登入：

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

當使用者登入時，他們會看到主要的 UI。 否則，他們會看到登入面板。 請注意，此條件式呈現的作業會在伺服器端。 永遠不會嘗試隱藏用戶端上的機密內容 & #8212anything 您傳送 HTTP 回應中會顯示為監看的未經處理的 HTTP 訊息的任何人。

## <a name="client-side-javascript-and-knockoutjs"></a>用戶端 JavaScript 和 Knockout.js

現在讓我們將從用戶端應用程式的伺服器端。 SPA 範本會使用 jQuery 和 Knockout.js 的組合，來建立平滑的互動式 UI。 Knockout.js 是 JavaScript 程式庫，可讓您輕鬆地繫結至資料的 HTML。 Knockout.js 使用模式，稱為 「 模型-檢視-ViewModel。 」

- 此模型是網域有更多的資料 （「 待辦事項清單 」 和 「 待辦事項項目 」）。
- HTML 文件的檢視。
- 檢視模型是可儲存模型資料的 JavaScript 物件。 檢視模型是程式碼的抽象概念的 UI。 它並不知道 HTML 的表示法。 相反地，它代表檢視，例如 「 的待辦事項清單 」 的抽象功能。

檢視是資料繫結至檢視模型。 檢視模型的更新會自動反映在檢視中。 繫結的另一種方式運作。 （例如按一下） 在 DOM 中的事件是資料繫結至函式檢視模型中，在觸發 AJAX 呼叫。

SPA 範本會將用戶端 JavaScript 組織成三個層級：

- todo.datacontext.js： 傳送 AJAX 要求。
- todo.model.js： 定義模型。
- todo.viewmodel.js： 定義檢視模型。

![](knockoutjs-template/_static/image11.png)

這些指令碼檔案位於方案的指令碼/應用程式資料夾中。

![](knockoutjs-template/_static/image12.png)

**todo.datacontext**會處理所有的 AJAX 呼叫至 Web API 控制器。 （登入的 AJAX 呼叫會定義其他地方，ajaxlogin.js 中）。

**todo.model.js**定義待辦事項清單的用戶端 （瀏覽器） 模型。 有兩個模型類別： todoItem 和 todoList。

許多模型類別中的屬性都屬於類型"ko.observable 」。 可預見值都是 Knockout 發揮它的運作方式。 從[Knockout 文件](http://knockoutjs.com/documentation/introduction.html)： 可預見值會是 「 JavaScript 物件，可通知有關變更的訂閱者 」。 當可預見值的值變更時，Knockout 會更新繫結至這些可預見值的任何 HTML 項目。 比方說，todoItem 具有可預見值的標題] 和 [作業屬性：

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

您也可以訂閱程式碼中的可預見值。 比方說，todoItem 類別訂閱中的 「 作業 」 和 「 title 」 屬性的變更：

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**檢視模型**

檢視模型被定義在 todo.viewmodel.js。 檢視模型是其中的應用程式會將繫結的 HTML 網頁項目網域資料的中央點。 SPA 範本，在檢視模型會包含可觀察的 todoLists 陣列。 檢視模型中的下列程式碼會告訴 Knockout 套用繫結：

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML 和資料繫結

主要的 HTML 頁面被定義在 Views/Home/Index.cshtml。 因為我們使用資料繫結，HTML 只是一個樣板的項目實際呈現。 使用 knockout*宣告式*繫結。 藉由將項目中的 「 資料繫結 」 屬性，將頁面項目的資料繫結。 以下是一個非常簡單的範例，取自 Knockout 文件：

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

在此範例中，Knockout 更新的內容**&lt;跨越&gt;** 的值的項目`myItems.count()`。 此值變更時，每當 Knockout 更新的文件。

油墨廓清提供數種不同的繫結類型。 以下是一些使用 SPA 範本中的繫結：

- **foreach**： 可讓您透過迴圈逐一查看並套用相同的標記清單中的每個項目。 這用來呈現的待辦事項清單 」 和 「 待辦事項項目。 內**foreach**，繫結會套用至清單的項目。
- **可見**： 用來切換可見性。 當集合是空的、 隱藏的標記，或顯示錯誤訊息。
- **值**： 用來填入表單值。
- **按一下 **： 將 click 事件繫結至檢視模型中的函式。

## <a name="anti-csrf-protection"></a>防 CSRF 防護

跨站台要求偽造 (CSRF) 攻擊會將惡意網站傳送要求給使用者目前登入的有弱點網站的位置。 為了協助防止 CSRF 攻擊，使用 ASP.NET MVC*防偽語彙基元*，也稱為要求驗證權杖。 其概念是，伺服器會將隨機產生的語彙基元放入網頁。 當用戶端提交資料給伺服器時，必須在要求訊息中包含此值。

防偽語彙基元運作，因為惡意的頁面無法讀取使用者的權杖，因為同源原則。 （同源原則會防止存取彼此的內容的兩個不同站台上裝載的文件）。

ASP.NET MVC 提供防偽語彙基元，內建支援，透過[AntiForgery](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx)類別和[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx)屬性。 目前，這項功能不會建置到 Web API。 不過，SPA 範本包含 Web API 的自訂實作。 此程式碼定義於`ValidateHttpAntiForgeryTokenAttribute`解決方案的 [篩選器] 資料夾中的類別。 若要深入了解防 CSRF Web API 中，請參閱[防止跨網站要求偽造 （csrf） 等攻擊的攻擊](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md)。

## <a name="conclusion"></a>結論

SPA 範本可協助您開始快速地撰寫現代化、 互動式 web 應用程式。 它會使用 Knockout.js 文件庫不同的簡報 （HTML 標記） 的資料和應用程式邏輯。 但 Knockout 不是唯一可用來建立 SPA 的 JavaScript 程式庫。 如果您想要探索一些其他選項，看看[社群建立 SPA 範本](../templates/index.md)。
