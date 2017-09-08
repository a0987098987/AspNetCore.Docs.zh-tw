---
title: "使用 AngularJS 單一網頁應用程式 (SPAs)"
author: rick-anderson
description: "了解如何建置使用 AngularJS SPA 樣式 ASP.NET 應用程式"
keywords: "ASP.NET Core，AngularJS SPA"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 50d2e76c472e67c26238abee4f7b0ed64cd043ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>使用 AngularJS 與 ASP.NET Core 單一網頁應用程式 (SPAs)


由[Venkata Koppaka](http://blog.falafel.com/author/venkata-koppaka/)和[Scott Addie](https://scottaddie.com)

在本文中，您將學習如何建置使用 AngularJS SPA 樣式 ASP.NET 應用程式。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)

## <a name="what-is-angularjs"></a>AngularJS 是什麼？

[AngularJS](http://angularjs.org/)是現代化的 JavaScript 架構，將來自 Google 通常用於使用單一頁面應用程式 (SPAs)。 AngularJS 開啟來源 MIT 授權，並可以在之後的 AngularJS 開發進度[其 GitHub 儲存機制](https://github.com/angular/angular.js)。 稱為 Angular 文件庫，因為 HTML 使用角度形狀的方括號。

AngularJS 不是 DOM 操作程式庫，例如 jQuery，但是它會使用稱為 jQLite jQuery 的子集。 AngularJS 主要根據您可以將它加入至 HTML 標記的宣告式 HTML 屬性。 您可以在瀏覽器中使用嘗試 AngularJS[程式碼學校網站](https://www.codeschool.com/courses/shaping-up-with-angular-js)或[W3Schools 網站](https://www.w3schools.com/angular/)。

本文將重點放在 AngularJS Angular 標題的其中一些注意事項。

## <a name="getting-started"></a>使用者入門

若要開始使用 AngularJS ASP.NET 應用程式中，您必須將它安裝為您專案的一部分，或者從內容傳遞網路 (CDN) 中參考它。

### <a name="installation"></a>安裝

有幾種方式將 AngularJS 加入您的應用程式。 如果您在 Visual Studio 中開始新的 ASP.NET Core web 應用程式，您可以加入 AngularJS 使用內建[Bower](bower.md)支援。 開啟*bower.json*，並加入項目以`dependencies`屬性：

<a name=angular-bower-json></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

在儲存時*bower.json* Angular 的檔案將會安裝在您的專案*wwwroot/lib*資料夾。 此外，它會列於`Dependencies/Bower`資料夾。 請參閱下面的螢幕擷取畫面。

![使用 AngularJS 專案的方案總管](angular/_static/angular-solution-explorer.png)

接下來，加入`<script>`參考底部`<body>`HTML 頁面的區段或*_Layout.cshtml*檔案，如下所示：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

我們建議實際執行應用程式，利用像 AngularJS 的通用程式庫的 Cdn。 您可以從數個 Cdn，像是本節的其中一個參考 AngularJS:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

一旦您擁有的參考*angular.js*指令碼檔案，您就可以開始使用 AngularJS web 網頁中。

## <a name="key-components"></a>重要元件

AngularJS 包含數個主要元件，例如*指示詞*，*範本*，*中繼器*，*模組*， *控制站*，*元件*，*元件路由器*等等。 讓我們來檢查這些元件合作方式將行為新增至您的網頁。

### <a name="directives"></a>指示詞

使用 AngularJS[指示詞](https://docs.angularjs.org/guide/directive)以 HTML 擴充自訂的屬性和項目。 AngularJS 指示詞中定義透過`data-ng-*`或`ng-*`前置詞 (`ng`是短的角度)。 有兩種類型的 AngularJS 指示詞：

   1. **基本的指示詞**： 這些預先定義之角度的小組，而 AngularJS 架構的一部分。

   2. **自訂指示詞**： 這些是您可以定義自訂指示詞。

所有的 AngularJS 應用程式中使用的基本指示詞的其中一個是`ng-app`bootstraps AngularJS 應用程式指示詞。 這個指示詞可以套用至`<body>`標記或主體的子項目。 我們來看看動作中的範例。 假設您在 ASP.NET 專案中，您可以加入至 HTML 檔`wwwroot`資料夾，或加入新的控制器動作和相關聯的檢視。 在此情況下，我已將加入新`Directives`至動作方法`HomeController.cs`。 相關聯的檢視如下所示：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

若要保留這些範例彼此獨立的我不使用共用的版面配置檔案。 您可以看到我們裝飾與 body 標記`ng-app`指示詞，以指出此頁面是 AngularJS 應用程式。 `{{2+2}}`是您將深入了解在不久後的角度的資料繫結運算式。 如果您執行此應用程式，以下是結果：

![簡單的角度指示詞](angular/_static/simple-directive.png)

AngularJS 中的其他基本指示詞包括：

`ng-controller`判斷哪一個 JavaScript 控制站繫結至的檢視。

`ng-model`決定 HTML 元素的屬性的值會繫結的模型。

`ng-init`用來初始化目前範圍的運算式形式的應用程式資料。

`ng-if`移除或重新建立根據提供的運算式 truthiness DOM 中的指定的 HTML 項目。

`ng-repeat`重複的資料集上指定的區塊的 HTML。

`ng-show`顯示或隱藏指定的 HTML 元素根據提供的運算式。

如需 AngularJS 中支援的所有基本指示詞的完整清單，請參閱[AngularJS 文件網站上的指示詞的文件章節](https://docs.angularjs.org/api/ng/directive)。

### <a name="data-binding"></a>資料繫結

AngularJS 提供[資料繫結](https://docs.angularjs.org/guide/databinding)支援-蜪鎏使用`ng-bind`指示詞或資料繫結運算式語法，例如`{{expression}}`。 AngularJS 支援如下： 模型中的資料保留在檢視表範本的所有權的同步處理的雙向資料繫結。 若要檢視任何變更會自動反映在模型中。 同樣地，在模型中的任何變更會反映在檢視中。

建立名為伴隨檢視 HTML 檔或控制器動作`Databinding`。 在檢視中包含下列設定：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

請注意，您可以顯示使用指示詞或資料繫結的模型值 (`ng-bind`)。 結果頁面看起來應該像這樣：

![簡單資料繫結](angular/_static/simple-databinding.png)

### <a name="templates"></a>範本

[範本](https://docs.angularjs.org/guide/templates)中 AngularJS 無窮 HTML 網頁使用裝飾 AngularJS 指示詞和成品。 中的 AngularJS 範本是指示詞、 運算式、 篩選和結合以形成檢視的 HTML 控制項的混合。

新增另一個檢視，以示範範本，並加入下列：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

該範本含有 AngularJS 指示詞，像`ng-app`， `ng-init`，`ng-model`和繫結的資料繫結運算式語法`personName`屬性。 在瀏覽器中執行，檢視看起來像下列螢幕擷取畫面：

![簡單的範本範例 1](angular/_static/simple-templates-1.png)

如果您在輸入欄位中輸入變更名稱，即會輸入欄位旁的文字以動態方式更新，顯示角度雙向資料繫結中的動作。

![簡單的範本範例 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>運算式

[運算式](https://docs.angularjs.org/guide/expression)AngularJS 是在內部撰寫的類似 JavaScript 程式碼片段`{{ expression }}`語法。 這些運算式中的資料繫結至 HTML 一樣`ng-bind`指示詞。 AngularJS 運算式和 JavaScript 的規則運算式的主要差別在於運算式會評估該 AngularJS `$scope` AngularJS 中的物件。

下列繫結的範例中的 AngularJS 運算式`personName`而且簡單 JavaScript 計算運算式：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

在瀏覽器顯示執行此範例`personName`資料和計算的結果：

![簡單運算式](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>重複項

透過呼叫的基本指示詞在 AngularJS 重複進行`ng-repeat`。 `ng-repeat`指示詞會重複的資料陣列的長度對重複在檢視中指定的 HTML 項目。 中繼器 AngularJS 中的可以重複透過字串或物件的陣列。 以下是重複的字串陣列進行反覆的範例使用方式：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

[Repeat 指示詞](https://docs.angularjs.org/api/ng/directive/ngRepeat)會輸出一系列中的未排序的清單，清單項目，為您可以在這個螢幕擷取畫面所示的開發人員工具中看到：

![中繼器範例](angular/_static/repeater.png)

以下是對物件的陣列，重複的範例。 `ng-init`指示詞建立`names`陣列，其中每個項目是第一次包含的物件，而姓氏。 `ng-repeat`指派、 `name in names`，輸出每個陣列元素的清單項目。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

在此情況下，輸出是與前面範例相同。

角度提供一些其他指示詞，可協助提供根據位置迴圈在其執行中的行為。

`$index`

使用`$index`中`ng-repeat`迴圈，以判定哪一個索引位置您迴圈目前位於上。

`$even` 和 `$odd`

使用`$even`中`ng-repeat`迴圈，以判斷目前的索引，在迴圈中是否即使索引的資料列。 同樣地，使用`$odd`判斷目前的索引是否為奇數的索引資料列。

`$first` 和 `$last`

使用`$first`中`ng-repeat`迴圈，以判斷第一個資料列是否包含在迴圈中目前的索引。 同樣地，使用`$last`判斷目前的索引是否為最後一個資料列。

以下是範例，示範`$index`， `$even`， `$odd`， `$first`，和`$last`動作：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

以下是產生的輸出：

![中繼器範例 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`是 JavaScript 物件，可做為黏附 （範本） 檢視和控制器 （如下所述） 之間。 檢視中的範本 AngularJS 只知道附加到數值`$scope`控制器中的物件。

> [!NOTE]
> MVVM 世界`$scope`AngularJS 中的物件通常會定義為 ViewModel。 AngularJS 小組指`$scope`物件做為資料模型。 [深入了解範圍中 AngularJS](https://docs.angularjs.org/guide/scope)。

以下是簡單的範例示範如何設定屬性`$scope`在個別的 JavaScript 檔案中， *scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

觀察`$scope`參數傳遞至控制器，第 2 行上。 這個物件是檢視的知道。 在行 3，我們會設定名為"name"到"Mary Jane"的屬性。

由檢視找不到特定的屬性時，發生什麼事？ 下面定義的檢視是指"name"和"age"屬性：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

請注意第 9 我們要求要顯示 「 名稱 」 屬性使用運算式語法 Angular 行。 第 10 行然後是指"age"，不存在的屬性。 執行中範例顯示天數設為"Mary Jane"和執行任何動作的名稱。 會忽略遺漏的屬性。

![範圍範例](angular/_static/scope.png)

### <a name="modules"></a>模組

A[模組](https://docs.angularjs.org/guide/module)在 AngularJS 是集合的控制站、 服務、 指示詞等等。`angular.module()`函式呼叫用來建立、 註冊及擷取中 AngularJS 模組。 所有模組，包括隨附的 AngularJS 小組與第三方廠商程式庫，應該使用都註冊`angular.module()`函式。

以下是示範如何建立新的模組 AngularJS 中的程式碼的程式碼片段。 第一個參數是模組的名稱。 第二個參數會定義在其他模組相依性。 在本文中，稍後我們將會顯示如何將這些相依性才能傳遞`angular.module()`方法呼叫。

```javascript
var personApp = angular.module('personApp', []);
```

使用`ng-app`代表頁面上的 AngularJS 模組的指示詞。 若要使用模組，請指定模組名稱`personApp`在此範例中，以`ng-app`我們範本指示詞。

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>控制站

[控制站](https://docs.angularjs.org/guide/controller)AngularJS 是第一個程式碼的進入點。 `<module name>.controller()`函式呼叫用來建立及註冊中 AngularJS 控制器。 `ng-controller`指示詞用來表示 HTML 網頁上的 AngularJS 控制器。 若要設定狀態和資料模型的行為是 Angular 控制站的角色 (`$scope`)。 控制站不應該用於直接操作的 DOM。

以下是註冊新的控制站的程式碼的程式碼片段。 `personApp`片段中的變數參考一個角度的模組，請在第 2 行定義。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

檢視使用`ng-controller`指示詞指定的控制器名稱：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

頁面會顯示 「 Mary"和"Jane"對應至`firstName`和`lastName`屬性附加至`$scope`物件：

![控制站範例](angular/_static/controllers.png)

### <a name="components"></a>元件

[元件](https://docs.angularjs.org/guide/component)中 Angular 1.5.x 允許封裝及建立個別的 HTML 元素的能力。 您無法在角度 1.4.x 能夠達到使用.directive() 方法相同的功能。

使用.component() 方法，開發已經過簡化獲得指示詞和控制器的功能。 其他優點包括：範圍隔離固有的最佳作法，並移轉至角度 2 變得更容易的工作。 `<module name>.component()`函式呼叫用來建立及註冊 AngularJS 中的元件。

以下是註冊的新元件的程式碼的程式碼片段。 `personApp`片段中的變數參考一個角度的模組，請在第 2 行定義。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

檢視，我們要在其中顯示的自訂 HTML 項目。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

相關聯的範本，供元件使用：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

頁面會顯示 「 Aftab"和"Ansari"對應至`firstName`和`lastName`屬性附加至`vm`物件：

![元件範例](angular/_static/components.png)

### <a name="services"></a>服務

[服務](https://docs.angularjs.org/guide/services)AngularJS 中常用的抽象成可在整個存留期角度的應用程式檔案的共用程式碼。 服務會延遲的方式具現化，這表示，將不會服務的執行個體，除非使用依存於服務的元件。 處理站會使用 AngularJS 應用程式的服務的範例。 使用建立處理站`myApp.factory()`函式呼叫，其中`myApp`是模組。

以下是示範如何使用中的 AngularJS 處理站的範例：

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

若要從控制器中呼叫此處理站，請傳遞`personFactory`當做參數`controller`函式：

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>使用與 REST 端點通訊的服務

以下是端對端範例使用服務中 AngularJS 與 ASP.NET Core Web API 端點互動。 範例會從 Web API 取得資料，並檢視範本中顯示的資料。 讓我們先開始進行檢視：

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

在此檢視中，我們有 Angular 模組呼叫`PersonsApp`和控制器呼叫`personController`。 我們使用`ng-repeat`來反覆查看的人員清單。 我們正在參考 17-19 版的程式行的三個自訂 JavaScript 檔案。

*PersonApp.js*檔案用來註冊`PersonsApp`模組; 以及的語法是類似於先前的範例。 我們使用`angular.module`函式來建立新的執行個體，我們將使用的模組。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

讓我們看看*personFactory.js*底下。 我們正在撥打的模組`factory`方法來建立處理站。 第 12 顯示內建 Angular`$http`從 web 服務擷取使用者資訊的服務。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

在*personController.js*，我們正在撥打的模組`controller`方法來建立控制器。 `$scope`物件的`people`personFactory （行 13） 從傳回的資料指派給屬性。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

讓我們快速查看 Web 應用程式開發介面和其背後的模型。 `Person`模型是 POCO （純舊 CLR 物件） 使用`Id`， `FirstName`，和`LastName`屬性：

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

`Person`控制站會傳回 JSON 格式的清單`Person`物件：

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

我們來看看應用程式執行動作：

![結果顯示其餘的控制器](angular/_static/rest-bound.png)

您可以[檢視 GitHub 上的應用程式的結構](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)。

> [!NOTE]
> 如需建構 AngularJS 應用程式的相關詳細資訊，請參閱[John Papa 角度樣式指南](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> 若要建立 AngularJS 模組、 控制器、 處理站，指示詞和檢視檔案輕鬆地，請務必簽出 Sayed Hashimi [SideWaffle 範本組件適用於 Visual Studio](http://sidewaffle.com/)。 Sayed Hashimi 是 Visual Studio Web 小組，microsoft 資深程式管理員和 SideWaffle 範本會被視為金級標準。 在撰寫本文時，SideWaffle 適用於 Visual Studio 2012、 2013年和 2015年。

### <a name="routing-and-multiple-views"></a>路由和多個檢視

AngularJS 有內建路由提供者來處理 SPA （單一頁面應用程式），以瀏覽。 若要使用 AngularJS 中的路由，您必須新增`angular-route`使用 Bower 的程式庫。 您可以看到在[bower.json](#angular-bower-json)開頭本文，我們已參考它的受測專案的參考檔案。

安裝封裝之後，加入指令碼參考 (*角度 route.js*) 至您的檢視。

現在讓我們來看我們有建置，並瀏覽加入個人應用程式。 首先，我們將複製的應用程式來建立新`PeopleController`呼叫動作`Spa`和對應`Spa.cshtml`藉由複製 Index.cshtml 檢視中的檢視`People`資料夾。 加入指令碼參考`angular-route`（請參閱第 11 行）。 也新增`div`標示`ng-view`指示詞 （請參閱第 6 行） 做為將檢視中的預留位置。 我們將會使用數個其他*.js* 13-16 的程式行所參考的檔案。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

讓我們看看*personModule.js*檔案以查看如何我們會執行個體化路由模組。 我們傳遞`ngRoute`做為程式庫的模組。 此模組會處理路由在我們的應用程式。

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

*PersonRoutes.js*檔案中下, 面定義的路由提供者為基礎的路由。 行 4-7 定義所有效地說，當使用的 URL 巡覽`/persons`是要求，請使用範本，呼叫`partials/personlist`工作透過`personListController`。 線條 8-11 表示使用路由參數的詳細資料頁面`personId`。 如果 URL 不符合的模式之一，Angular 會預設為`/persons`檢視。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

`personlist.html`檔案是包含只顯示人員清單所需的 HTML 的部分檢視。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

控制器會定義使用的模組`controller`函式在*personListController.js*。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

如果我們執行此應用程式，並瀏覽至`people/spa#/persons`URL，我們會看到：

![人員清單檢視](angular/_static/spa-persons.png)

如果我們瀏覽至 [詳細資料] 頁面上，例如`people/spa#/persons/2`，我們會看到詳細資料的部分檢視：

![連絡人詳細資料檢視](angular/_static/spa-persons-2.png)

您可以檢視完整的來源並不會顯示在本文中的任何檔案[GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)。

### <a name="event-handlers"></a>事件處理常式

數目中 HTML DOM 中的輸入項目加入事件處理功能的 AngularJS 指示詞 以下是內建 AngularJS 之事件的清單。

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> 您可以加入自己的事件處理常式使用[AngularJS 中功能的自訂指示詞](https://docs.angularjs.org/guide/directive)。

讓我們看看`ng-click`固定事件。 建立新的 JavaScript 檔名為*eventHandlerController.js*，並加入下列：

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

請注意新`sayName`函式在`eventHandlerController`在線條上方 5。 正在進行的所有方法的現在會顯示歡迎訊息與使用者 JavaScript 警示。

下列檢視會將控制器函式繫結 AngularJS 事件。 第 9 行上有一個按鈕`ng-click`角度指示詞已套用。 它會呼叫我們`sayName`函式，以附加至`$scope`傳遞到這個檢視的物件。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

執行中範例會示範控制站的`sayName`按鈕便會自動呼叫函式。

![Click 事件](angular/_static/events.png)

如需詳細 AngularJS 內建的事件處理常式指示詞的詳細資訊，請務必磁頭[文件網站](https://docs.angularjs.org/api/ng/directive/ngClick)的 AngularJS。

## <a name="additional-resources"></a>其他資源

* [角度的文件](https://docs.angularjs.org)

* [角度 2 的資訊](http://angular.io)
