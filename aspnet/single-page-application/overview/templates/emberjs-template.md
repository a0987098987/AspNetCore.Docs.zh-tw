---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS 範本 |Microsoft 文件
author: xqiu
description: EmberJS 範本
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506797"
---
<a name="emberjs-template"></a>EmberJS 範本
====================
由[Xinyang Qiu](https://github.com/xqiu)

> Nathan Totten、 Thiago Santos 和 Xinyang Qiu 寫入 EmberJS MVC 範本。
> 
> [下載 EmberJS MVC 範本](https://go.microsoft.com/fwlink/?LinkId=282647)


EmberJS SPA 範本被設計來協助您快速地開始建置使用 EmberJS 互動的用戶端 web 應用程式。

「 單一頁面應用程式 」 (SPA) 是載入單一 HTML 頁面，並以動態方式，而不必載入新的頁面更新頁面的 web 應用程式的一般詞彙。 初始頁面載入之後, SPA 交談 AJAX 要求透過與伺服器。

![](emberjs-template/_static/image1.png)

AJAX 沒有新增，但現在有更輕鬆地建立及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。 此外，html5 和 CSS3 會讓您更輕鬆地建立豐富的 Ui。

EmberJS SPA 範本使用[Ember](http://emberjs.com/)來處理從 AJAX 要求的頁面更新 JavaScript 程式庫。 Ember.js 會同步處理的最新資料的網頁中使用資料繫結。 這樣一來，您不需要撰寫任何程式碼，逐步解說的 JSON 資料，並更新 DOM 相反地，您將宣告式屬性放在告訴 Ember.js 如何將資料呈現的 HTML。

在伺服器端，EmberJS 範本是幾乎等同[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)。 它會使用 ASP.NET MVC 提供 HTML 文件和 ASP.NET Web API，以處理來自用戶端的 AJAX 要求的服務。 如需這些層面的範本，請參閱[KnockoutJS 範本](../introduction/knockoutjs-template.md)文件。 本主題著重在 Knockout 範本和 EmberJS 範本之間的差異。

## <a name="create-an-emberjs-spa-template-project"></a>建立 EmberJS SPA 範本專案

下載並安裝的範本，按一下下載按鈕上方。 您可能需要重新啟動 Visual Studio。

在**範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 在下**Visual C#**，選取**Web**。 在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。 為專案名稱，然後按一下**確定**。

![](emberjs-template/_static/image2.png)

在**新專案**精靈中，選取**Ember.js SPA 專案**。

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>EmberJS SPA 範本概觀

EmberJS 範本使用 jQuery Ember.js，以建立平滑的互動式 UI Handlebars.js 的組合。

Ember.js 是使用用戶端 MVC 模式的 JavaScript 程式庫。

- A*範本*書面 Handlebars 樣板化的語言，描述應用程式使用者介面。 在發行模式中[Handlebars 編譯器](https://github.com/Myslik/csharp-ember-handlebars)用於配套並編譯 handlebars 範本。
- A*模型*儲存它會從伺服器 （ToDo 清單和 ToDo 項目） 取得的應用程式資料。
- A*控制器*儲存應用程式的狀態。 控制站通常會顯示模型資料與對應的範本。
- A*檢視*會轉譯從應用程式的基本事件和傳遞給控制器。
- A*路由器*管理應用程式狀態保持同步 Url 和範本。

此外，Ember 資料程式庫可以用來同步處理 （透過 rest 式 API 從伺服器取得） 的 JSON 物件和用戶端模型。

EmberJS SPA 範本會將指令碼組織成八個圖層：

- webapi\_adapter.js、 webapi\_serializer.js： 擴充 Ember 資料程式庫，才能使用 ASP.NET Web API。
- Scripts/helpers.js： 定義新 Ember Handlebars helper。
- Scripts/app.js： 建立應用程式，並設定配接器和序列化程式。
- 指令碼/應用程式模型/\*.js： 定義模型。
- 指令碼/應用程式檢視/\*.js： 定義的檢視。
- 控制站的指令碼/應用程式/\*.js： 定義控制站。
- 指令碼/應用程式/路由、 Scripts/app/router.js： 定義路由。
- 範本 /\*.hbs： 定義 handlebars 範本。

讓我們看看一些詳細指令碼。

## <a name="models"></a>模型

模型會定義指令碼/應用程式/models 資料夾中。 有兩個模型檔： todoItem.js 和 todoList.js。

**todo.model.js**定義的待辦事項清單中的用戶端 （瀏覽器） 模型。 有兩種模型類別： todoItem 和 todoList。 在 Ember，模型是 DS 的子類別。模型。 模型可以具有屬性與屬性：

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

模型可以定義其他模型的關聯性：

[!code-css[Main](emberjs-template/samples/sample2.css)]

模型可以有計算繫結至其他屬性的屬性：

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

模型可以具有觀察者函式，觀察到的屬性變更時叫用：

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>檢視

檢視中定義的指令碼/應用程式檢視資料夾中。 檢視轉譯的應用程式 UI 中的事件。 事件處理常式可以回呼叫控制器函式，或只是直接呼叫資料內容。

例如，下列程式碼取自 views/TodoItemEditView.js。 它會定義事件處理輸入的文字欄位。

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>控制器

控制器的控制站的指令碼/應用程式資料夾中定義。 若要表示的單一模型，擴充`Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

在控制站也可以藉由擴充代表的模型集合`Ember.ArrayController`。 比方說，TodoListController 表示陣列`todoList`物件。 控制器會依據 todoList 識別碼，以遞減順序排序：

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

控制器會定義名為函式`addTodoList`，建立新的 todoList，並將它加入至陣列。 若要查看呼叫此函式的方式，開啟名為 todoListTemplate.html，範本資料夾中的範本檔案。 下列範本程式碼將繫結至按鈕`addTodoList`函式：

[!code-html[Main](emberjs-template/samples/sample8.html)]

控制器也包含`error`屬性，其中包含錯誤訊息。 以下是顯示錯誤訊息 （也在 todoListTemplate.html) 的範本程式碼：

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>路由

Router.js 定義路由和預設範本，若要顯示，設定應用程式狀態，並符合為路由的 Url:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js 覆寫 setupController 函式，如 TodoListRoute 載入資料：

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember 會使用命名慣例，來比對 Url、 路由名稱、 控制器及範本。 如需詳細資訊，請參閱[ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS 說明文件。

## <a name="templates"></a>範本

[範本] 資料夾包含四個範本：

- application.hbs： 呈現應用程式啟動時的預設範本。
- about.hbs:"/ 關於 」 路由範本。
- index.hbs： 根範本"/"的路由。
- todoList.hbs： 的範本 」 / todo"路由。
- \_navbar.hbs: 範本會定義在導覽功能表。

應用程式範本就像是主版頁面。 它包含頁首、 頁尾和"{{插座}}"將根據路由範本中。 如需 Ember 中的應用程式範本的詳細資訊，請參閱[ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/)。

"/ TodoList 」 範本包含兩個迴圈運算式。 外部迴圈是`{{#each controller}}`，與內部迴圈是`{{#each todos}}`。 下列程式碼會示範內建`Ember.Checkbox`檢視，請自訂`App.TodoItemEditView`，以及與連結`deleteTodo`動作。

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs，在定義的類別定義協助程式函式來快取，並插入範本時檔案**偵錯**設**true** Web.config 檔案中。 呼叫此函式會從定義 Views/Home/App.cshtml 的 ASP.NET MVC 檢視檔案：

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

函式呼叫不含引數，會呈現所有範本資料夾中的範本檔案。 您也可以指定子資料夾或特定範本檔案。

當**偵錯**是**false**在 Web.config 中，應用程式包含組合項目 」 ~/bundles/templates"。 這個套件組合的項目會加入 BundleConfig.cs，使用 Handlebars 編譯器程式庫中：

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
