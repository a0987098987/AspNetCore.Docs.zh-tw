---
title: "在 ASP.NET Core 解 Knockout.js MVVM 架構"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>在 ASP.NET Core 解 Knockout.js MVVM 架構

由[Steve Smith](https://ardalis.com/)

Knockout 是常用的 JavaScript 程式庫，可簡化建立複雜資料為基礎的使用者介面。 它可以是單獨使用或與其他程式庫，例如 jQuery。 其主要用途是定義為 JavaScript 物件，基礎資料模型繫結 UI 項目，如此當 ui 進行變更，則會更新模型，反之亦然。 Knockout 有助於模型-檢視-ViewModel (MVVM) 模式的 web 應用程式的用戶端行為的使用。 其中一個必須了解 Knockout 的 MVVM 實作使用時的兩個主要概念是可預見值 」 和 「 繫結。

## <a name="getting-started"></a>使用者入門

Knockout 會部署為單一的 JavaScript 檔案，因此安裝和使用它是非常直接使用[bower](bower.md)。 假設您已經有[bower](bower.md)和[gulp](using-gulp.md)設定，開啟*bower.json*中 ASP.NET Core 專案，並加入 knockout 相依性，如下所示：

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

有了這個位置，然後您可以手動執行 bower 開啟工作執行器總管 （在其他視窗 ‣ 工作執行器總管的檢視 ‣） 然後下工作，以滑鼠右鍵按一下 bower 並選取 [執行]。 結果看起來應該類似這樣：

![執行中工作執行器總管 knockout bower](knockout/_static/bower-knockout.png)

現在，如果您在您的專案中尋找`wwwroot`資料夾中，您應該會看到 knockout 安裝的 lib 資料夾底下。

![knockout lib 資料夾中安裝](knockout/_static/wwwroot-knockout.png)

建議在生產環境中您參考 knockout 透過內容傳遞網路或 CDN，這會增加您的使用者將已經快取的檔案的副本，並因此將不需要完全下載它的可能性。 Knockout 位於數個 Cdn，包括 Microsoft Ajax CDN，這裡：

[http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

若要包含 Knockout 會使用此頁面上，只要加入`<script>`無論您將主控它 （應用程式，或透過 CDN） 從參考檔案的項目：

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>可預見值、 ViewModels 和簡單繫結

您可能已經很熟悉使用 JavaScript 來管理在網頁上，透過直接存取 DOM 項目，或使用像 jQuery 程式庫。 通常這種行為是藉由撰寫程式碼直接設定項目值以回應特定的使用者動作來達成。 使用 Knockout、 宣告式方法不會執行相反地，透過此頁面上的項目會繫結至屬性的物件上。 不需要撰寫程式碼來操作 DOM 項目，ViewModel 物件時，只需互動使用者動作和 Knockout 會負責確保頁面項目同步處理。

簡單的範例，請考量以下頁面清單。 它包含`<span>`具有項目`data-bind`屬性，指出文字內容，應該繫結至 authorName。 接下來，在單一屬性，定義變數 viewModel 在 JavaScript 區塊`authorName`，設為某個值。 最後，呼叫`ko.applyBindings`進行時，傳入這個 viewModel 變數。

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

在瀏覽器的內容中檢視時<span>viewModel 變數中的值會取代項目：

![knockout 簡單繫結](knockout/_static/simple-binding-screenshot.png)

我們現在有簡單的單向繫結工作。 請注意，不在程式碼中未我們撰寫 JavaScript 將值指派至範圍的內容。 如果我們想要管理 ViewModel，我們可以採用此步驟進一步加入 HTML 輸入的文字方塊中，和例如繫結至它的值，因此：

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

我們重新載入該頁面，請參閱此值確實會繫結至輸入方塊：

![knockout 輸入繫結](knockout/_static/input-binding-screenshot.png)

不過，若我們變更在文字方塊中的值，對應中的值`<span>`項目不會變更。 為什麼？

問題在於，執行任何動作都已收到通知`<span>`，它會更新。 只要更新 ViewModel 不足夠，本身，除非 ViewModel 屬性會包裝在一種特殊類型。 我們需要使用**可預見值**中針對任何屬性，必須先變更自動更新發生 ViewModel。 藉由變更使用 ViewModel`ko.observable("value")`而不是只是 「 值 」 ViewModel 會更新任何 HTML 項目發生變更時繫結至它的值。 請注意輸入的方塊沒有更新它們的值，直到它們失去焦點，因此您看不到繫結項目，當您輸入的變更。

> [!NOTE]
> 新增支援即時更新之後每個按鍵動作就是新增的,`valueUpdate: "afterkeydown"`至`data-bind`屬性的內容。 您也可以使用，取得此行為`data-bind="textInput: authorName"`取得即時更新的值。 

它使用 ko.observable 在更新之後，我們 viewModel:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Knockout 支援許多不同類型的繫結。 到目前為止我們已看到如何繫結至`text`和`value`。 您也可以繫結至任何給定的屬性。 例如，若要建立超連結與錨定標記，`src`屬性可以繫結到 viewModel。 Knockout 也支援函式繫結。 為了示範這點，讓我們來更新以包含作者的 twitter 控點，viewModel 和作者的 twitter 網頁的連結，以顯示 twitter 控點。 我們將會執行這三個階段。

首先，新增 HTML，以便顯示超連結，我們將示範括號括住的作者名稱後面：

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

接下來，更新以包含 twitterUrl 和 twitterAlias 屬性 viewModel:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

請注意，此時我們尚未您尚未更新到正確的 URL，這個 twitter 別名 twitterUrl – 只要指向 twitter.com。同時也請注意，我們使用新的 Knockout 函式， `computed`，如 twitterUrl。 這是會通知任何 UI 項目，如果變更了它的可觀察函式。 不過，讓它 viewModel 中有其他屬性的存取權，我們需要變更方式，我們會建立 viewModel，使每一個屬性是其本身的陳述式。

修訂的 viewModel 宣告如下所示。 它現在會宣告為函式。 請注意，每一個屬性自己陳述式現在，請以分號結束。 也請注意，若要存取 twitterAlias 屬性值，我們需要執行它，使其參考包含 （）。

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

結果如預期般運作的瀏覽器中：

![knockout 超連結](knockout/_static/hyperlink-screenshot.png)

Knockout 也支援特定 UI 項目事件，例如 click 事件的繫結。 這可讓您輕鬆並以宣告方式繫結 UI 項目內的應用程式 viewModel 函式。 簡單舉例如下，我們可以加入的按鈕，按一下時，會修改為全部大寫的作者 twitterAlias。

首先，我們加入按鈕、 繫結至按鈕的按一下事件，並參考函式名稱，我們將新增到 viewModel:

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

然後將函數加入 viewModel 裝設修改 viewModel 的狀態。 請注意，若要設定新值給 twitterAlias 屬性，我們呼叫做為方法並傳入新值。

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

執行程式碼，並按一下按鈕會如預期般修改顯示的連結：

![超連結大寫](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>控制流程

Knockout 包含可執行條件式和迴圈作業的繫結。 迴圈作業是特別適用於繫結至 UI 清單、 功能表和方格或資料表的資料的清單。 Foreach 繫結會逐一查看陣列。 可觀察陣列搭配使用時，它會在加入或移除從該陣列，而不需要重新建立 UI 樹狀目錄中的每個項目中的項目時自動更新 UI 項目。 下列範例會使用新的 viewModel 包括遊戲結果的可觀察陣列。 它使用兩個資料行與繫結至簡單的資料表`foreach`上的繫結`<tbody>`項目。 每個`<tr>`內的項目`<tbody>`gameResults 集合項目會繫結。

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

請注意，這次我們使用 ViewModel 以大寫"V"因為我們希望能建構它使用"new"（在 applyBindings 呼叫）。 在執行時，頁面會產生下列輸出：

![knockout 資料錄檢視模型](knockout/_static/record-screenshot.png)

為了示範可觀察的集合運作，讓我們加入更多的功能。 我們可以包含的另一個遊戲 ViewModel 結果記錄的能力，並將按鈕和一些 UI，才能使用這個新的函式。  首先，讓我們來建立 addResult 方法：

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

將這個方法繫結至按鈕使用`click`繫結：

```html
<button data-bind="click: addResult">Add New Result</button>
```

在瀏覽器中開啟頁面，並按一下按鈕數次，導致每按一下新的資料表資料列：

![將結果加入](knockout/_static/record-addresult-screenshot.png)

有幾種方式以支援在 UI 中，通常為內嵌，或在另一個表單，加入新的記錄。 我們可以輕易修改要使用文字方塊和 dropdownlists 以便轉變為可編輯的資料表。 只要變更`<tr>`項目所示：

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

請注意，`$root`指的是根 ViewModel，這是可能的選項公開的位置。 `$data`是指任何目前的模型是在指定的內容位在此情況下，它會參考 resultChoices 陣列，其中每一個都是簡單字串的個別項目。

這項變更，與整個方格會變成可編輯：

![可編輯的格線](knockout/_static/editable-grid-screenshot.png)

如果不使用 Knockout，我們無法達成這些全部都使用 jQuery，但它可能不會幾乎最大效率。 Knockout 的 UI 項目中，追蹤 ViewModel 中的項目相對應的繫結的資料，並只會更新需要新增、 移除或更新這些項目。 花費的心力，以達到這個目的使用 jQuery 或直接的 DOM 管理自己和即使如此如果我們要顯示資料表的資料為基礎 （例如 win 遺失記錄） 彙總的結果時，我們需要一次循環它和剖析HTML 項目。  使用 Knockout，顯示 win 遺失記錄是一般。 我們可以計算 ViewModel 本身內，並且顯示搭配簡單的文字繫結和`<span>`。

若要建置 win 遺失記錄字串，我們可以使用計算的可觀察。 請注意，參考到 ViewModel 內的可觀察屬性必須是函式呼叫，否則它們將不會擷取可觀察的值 (也就是`gameResults()`不`gameResults`中顯示的程式碼):

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

將此函式繫結至的範圍內`<h1>`在頁面頂端的項目：

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

結果：

![Win 遺失](knockout/_static/record-winloss-screenshot.png)

將資料列加入或修改任何資料列的結果資料行中選取的元素會更新在視窗的頂端顯示的記錄。

除了為值的繫結，您也可以使用幾乎任何合法的 JavaScript 運算式內的繫結。 比方說，如果 UI 項目應該只會出現在某些情況下，例如當值超過特定閾值，您可以指定這以邏輯方式繫結運算式內：

```html
<div data-bind="visible: customerValue > 100"></div>
```

這`<div>`才會顯示 customerValue 時超過 100。

## <a name="templates"></a>範本

Knockout 有支援範本，以便您可以輕鬆地從您的行為，將您的 UI，或以累加方式載入視的大型應用程式的 UI 項目。 我們可以更新自己的範本進行每個資料列，就是直接提取 HTML 縮小成範本，並在資料繫結呼叫中依名稱指定的範本上, 前一個範例`<tbody>`。

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

Knockout 也支援其他範本化引擎，例如 jQuery.tmpl 程式庫和 Underscore.js 的範本化引擎。

## <a name="components"></a>元件

元件可讓您組織及重複使用的 UI 程式碼通常以及 UI 程式碼所依賴的 ViewModel 資料。 若要建立元件，您只需要指定其範本和其 viewModel，並為它命名。 藉由呼叫 `ko.components.register()` 即可達到此目的。 除了定義的範本和 viewmodel 內嵌，可以載入從外部檔案，使用程式庫如*require.js*，並產生非常全新且更有效率的程式碼。

## <a name="communicating-with-apis"></a>應用程式開發介面與通訊

Knockout 可以使用任何 JSON 格式的資料。 擷取並儲存使用 Knockout 資料的常見方式是使用 jQuery，支援`$.getJSON()`函式來擷取資料，而`$.post()`方法將資料從瀏覽器傳送至應用程式開發介面端點。 當然，如果您想以不同的方式來傳送和接收 JSON 資料，Knockout 會與其也適用。

## <a name="summary"></a>總結

Knockout 提供簡單且精緻的方法，來繫結至 ViewModel 中定義的用戶端應用程式的目前狀態的 UI 項目。 Knockout 的繫結語法會使用資料繫結屬性中，套用至要處理的 HTML 項目。 Knockout 可以有效率地呈現，並藉由追蹤 UI 項目更新大型資料集，只處理的變更影響的項目。 UI 邏輯使用範本，可以視需要從外部檔案載入的元件可以分割大型應用程式。 目前第 3 版，Knockout 是穩定的 JavaScript 程式庫，可以改善需要豐富型用戶端互動的 web 應用程式。
