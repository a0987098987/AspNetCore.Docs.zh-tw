---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS 範本 |Microsoft Docs
author: xqiu
description: EmberJS 範本
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: fbc3b1d299ace27d38d895e42b8e3bb3b51b36f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825341"
---
<a name="emberjs-template"></a>EmberJS 範本
====================
藉由[Xinyang Qiu](https://github.com/xqiu)

> EmberJS MVC 範本是由 Nathan Totten、 Thiago Santos 和 Xinyang Qiu 撰寫。
> 
> [下載 EmberJS MVC 範本](https://go.microsoft.com/fwlink/?LinkId=282647)


EmberJS SPA 範本可讓您開始快速建置使用 EmberJS 的互動式用戶端 web 應用程式。

「 單一頁面應用程式 」 (SPA) 是載入單一 HTML 頁面，並以動態方式，而不是載入新的頁面更新頁面的 web 應用程式的一般詞彙。 在初始網頁載入後，SPA 會與透過 AJAX 要求的伺服器。

![](emberjs-template/_static/image1.png)

AJAX 是新奇，但今天很輕鬆地建置及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。 此外，html5 和 css3 等讓您更輕鬆地建立豐富的 Ui。

EmberJS SPA 範本會使用[Ember](http://emberjs.com/)來處理從 AJAX 要求的頁面更新的 JavaScript 程式庫。 Ember.js 會使用資料繫結與最新的資料同步處理頁面。 如此一來，您不需要撰寫任何程式碼，逐步解說的 JSON 資料，並更新 DOM 相反地，您將宣告式屬性放在告訴 Ember.js 如何將資料呈現的 HTML。

伺服器端上 EmberJS 範本是幾乎等同[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)。 它會使用 ASP.NET MVC 提供 HTML 文件和 ASP.NET Web API，以處理來自用戶端的 AJAX 要求。 如需關於這些方面的範本，請參閱[KnockoutJS 範本](../introduction/knockoutjs-template.md)文件。 本主題著重於 Knockout 範本和 EmberJS 範本之間的差異。

## <a name="create-an-emberjs-spa-template-project"></a>建立 EmberJS SPA 範本專案

下載並安裝的範本，按一下上方的 [下載] 按鈕。 您可能需要重新啟動 Visual Studio。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名，然後按一下**確定**。

![](emberjs-template/_static/image2.png)

在 **新的專案**精靈中，選取**Ember.js SPA 專案**。

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>EmberJS SPA 範本概觀

EmberJS 範本會使用 jQuery、 Ember.js、 Handlebars.js 建立平滑的互動式 UI 的組合。

Ember.js 是使用用戶端 MVC 模式的 JavaScript 程式庫。

- A*範本*、 Handlebars 範本化語言中，以撰寫描述應用程式使用者介面。 在發行模式中， [Handlebars 編譯器](https://github.com/Myslik/csharp-ember-handlebars)用來包裝並編譯 handlebars 範本。
- A*模型*儲存應用程式資料，它會取得從伺服器 （「 待辦事項清單 」 和 「 待辦事項項目 」）。
- A*控制器*儲存應用程式的狀態。 控制器通常會顯示模型資料與對應的範本。
- A*檢視*會轉譯從應用程式的基本事件，並將傳遞到控制器。
- A*路由器*管理應用程式狀態，讓 Url 和範本保持同步。

颾魤 ㄛ Ember 資料程式庫可用來同步處理 （透過支援的 RESTful API 從伺服器取得） 的 JSON 物件和用戶端模型。

EmberJS SPA 範本會將指令碼組織成八個層級：

- webapi\_adapter.js、 webapi\_serializer.js： 擴充 Ember 資料程式庫，才能使用 ASP.NET Web API。
- Scripts/helpers.js： 定義新的 Ember Handlebars 協助程式。
- Scripts/app.js： 建立應用程式，並設定在配接器與序列化程式。
- 指令碼/應用程式/模型/\*.js： 定義模型。
- 指令碼/應用程式/檢視/\*.js： 定義檢視。
- 控制站的指令碼/應用程式/\*.js： 定義控制站。
- 指令碼/應用程式/routes、 Scripts/app/router.js： 定義的路由。
- 範本 /\*.hbs： 定義 handlebars 範本。

讓我們看看一些更多詳細資料中的這些指令碼。

## <a name="models"></a>模型

模型被定義在指令碼/應用程式/models 資料夾中。 有兩個模型檔案： todoItem.js 和 todoList.js。

**todo.model.js**定義待辦事項清單的用戶端 （瀏覽器） 模型。 有兩個模型類別： todoItem 和 todoList。 在 Ember，模型會是 DS 的子類別。模型。 模型可以具有與屬性的屬性：

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

模型可以定義其他模型的關聯性：

[!code-css[Main](emberjs-template/samples/sample2.css)]

模型可以有計算繫結至其他屬性的屬性：

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

模型可以具有觀察者函式，所觀察的屬性變更時，會叫用：

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>檢視

在 [指令碼/app/views] 資料夾中定義的檢視。 檢視轉譯應用程式 UI 中的事件。 事件處理常式可以回撥至控制器函式，或只是直接呼叫資料內容。

例如，下列程式碼取自於 views/TodoItemEditView.js。 它會定義之事件處理常式的輸入的文字欄位。

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>控制器

控制器會定義在指令碼/app/controllers 資料夾中。 若要表示的單一模型，擴充`Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

在控制站也可以藉由擴充代表的模型集合`Ember.ArrayController`。 比方說，TodoListController 代表陣列的`todoList`物件。 控制器會依 todoList 識別碼，以遞減順序排序：

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

控制器會定義名為函式`addTodoList`，這會建立新的 todoList，並將它加入至陣列。 若要查看如何呼叫這個函式時，開啟名為 todoListTemplate.html，在 [範本] 資料夾中的範本檔案。 下列範本程式碼會繫結至按鈕`addTodoList`函式：

[!code-html[Main](emberjs-template/samples/sample8.html)]

控制器也含有`error`屬性，其中包含錯誤訊息。 以下是顯示 （也是在 todoListTemplate.html) 的錯誤訊息的範本程式碼：

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>路由

Router.js 會定義路由和預設範本，若要顯示的方式，設定應用程式狀態，並比對路由的 Url:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js TodoListRoute 針對載入資料，藉由覆寫 setupController 函式：

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember 會使用命名慣例，來比對 Url、 路由名稱、 控制器及範本。 如需詳細資訊，請參閱 < [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS 說明文件。

## <a name="templates"></a>範本

[範本] 資料夾包含四個範本：

- application.hbs： 轉譯應用程式啟動時的預設範本。
- about.hbs:"/ [關於]"路由範本。
- index.hbs： 範本的根"/"的路由。
- todoList.hbs: 範本"/ 待辦事項 」 路由。
- \_navbar.hbs： 範本會定義導覽功能表。

應用程式範本的作用就像主版頁面。 它包含為頁首、 頁尾及"{{輸出}}"將取決於路由範本中。 如需有關 Ember 中的應用程式範本的詳細資訊，請參閱[ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/)。

"/ TodoList 」 範本包含兩個迴圈的運算式。 外部迴圈`{{#each controller}}`，與內部迴圈是`{{#each todos}}`。 下列程式碼會顯示內建`Ember.Checkbox`檢視，請自訂`App.TodoItemEditView`，並使用連結`deleteTodo`動作。

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs，所定義的類別定義協助程式函式來快取，並插入範本檔的時機**偵錯**設定為**true** Web.config 檔案中。 此函式呼叫從 Views/Home/App.cshtml 中定義的 ASP.NET MVC 檢視檔案：

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

呼叫不含引數，函式會呈現所有範本 資料夾中的範本檔案。 您也可以指定子資料夾或特定的範本檔案。

當**偵錯**是**false**在 Web.config 中，應用程式包含的套件組合的項目 」 ~/bundles/templates"。 這個套件組合項目會加入 BundleConfig.cs，使用 Handlebars 編譯器程式庫中：

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
