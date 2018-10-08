---
title: ASP.NET Core 中的 Less、Sass 與 Font Awesome
author: ardalis
description: 了解如何在 ASP.NET Core 應用程式中使用 Less、Sass 與 Font Awesome。
ms.author: tdykstra
ms.date: 10/14/2016
uid: client-side/less-sass-fa
ms.openlocfilehash: 2229c4e3b0238ff17c15e78f657b9acb10495c72
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275561"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a>ASP.NET Core 中的 Less、Sass 與 Font Awesome

作者：[Steve Smith](https://ardalis.com/)

說到樣式與整體體驗時，Web 應用程式的使用者會有越來越高的期望。現代化 Web 應用程式經常會運用豐富的工具與架構以一致的方式定義及管理其外觀及操作。如 [Bootstrap](http://getbootstrap.com/) 的架構在定義常用樣式集與版面配置選項時可非常成功。不過，大部分的非一般站台也可以從能夠有效地定義及維護樣式與階層式樣式表 (CSS) 檔案，以及輕鬆存取協助讓站台的介面更直覺的非影像圖示。這是支援 [Less](http://lesscss.org/) 與 [Sass](http://sass-lang.com/) 以及程式庫 (例如 [Font Awesome](http://fontawesome.io/)) 之語言與工具的存在之處。

## <a name="css-preprocessor-languages"></a>CSS 前置處理器的語言

編譯為其他語言以改善使用底層語言之體驗的語言稱為前置處理器。有兩個常見的 CSS 前置處理器，Less 與 Sass。這些前置處理器可為 CSS 新增功能，例如對變數與巢狀規則的支援，這可以改善大型、複雜樣式表的可維護性。CSS 做為語言是非常基本，甚至不支援簡單的功能 (例如變數)，而這使得 CSS 檔案重複性高且繁雜。透過前置處理器加入實際程式設計語言功能有助於減少重複項目並提供更好的樣式規則組織方式。Visual Studio 原生支援 Less 與 Sass，以及可進一步改善當使用這些語言時可進一步改善開發體驗的延伸模組。

做為前置處理器可以如何改善可讀性和可維護性的樣式資訊的快速範例，請考慮這個 CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

使用 Less，這可以使用 *mixin* 重寫以排除所有重複項 (如此命名是因為它可讓您將來自一個類別或規則集的屬性 "mix" 到另一個類別或規則集)：

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>Less

Less CSS 前置處理器會使用 Node.js 來執行。若要安裝 Less，請從命令提示字元使用節點套件管理員 (npm) (-g 表示「全域」)：

```console
npm install -g less
```

如果您使用 Visual Studio，您可以開始使用 Less，方式是將一或多個 Less 檔案加入到您的專案，然後設定在編譯時期使用 Gulp (或 Grunt) 來處理它們。新增 *Style* 資料夾到您的專案，然後加入命名為 *main.less* 的新 Less 檔案到此資料夾。

![加入較少的檔案](less-sass-fa/_static/add-less-file.png)

一旦加入，您的資料夾結構看起來應該像這樣：

![資料夾結構](less-sass-fa/_static/folder-structure.png)

現在您可以加入一些基本的樣式會部署到的 wwwroot 資料夾 Gulp 並編譯成 CSS 的檔案。

修改*main.less*包含以下內容，從單一基底的色彩會建立簡單的色彩調色盤。

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base` 與其他開頭為 @ 的項目是變數。每個都代表一個色彩。除了 `@base` 之外，它們是使用色彩函式來設定：淡化、暗化與微調。淡化與暗化可執行您預期的動作，而微調則會依度數 (色彩轉輪上所示) 調整色彩的色調。 Less 處理器夠聰明，可以忽略不使用的變數，因此為了示範這些變數如何運作，我們必須在某處使用它們。`.baseColor` 等類別將會示範 CSS 檔案中產生之每個變數的計算值。

### <a name="get-started"></a>開始使用

建立**npm 組態檔**(*package.json*) 在您的專案資料夾和編輯，以參考`gulp`和`gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

在您的專案資料夾，或在 Visual Studio 中安裝的相依性，請在命令提示字元**方案總管 中**(**相依性 > npm > 還原封裝**)。

```console
npm install
```

![VS 還原封裝](less-sass-fa/_static/restore-packages.png)

在專案資料夾中，建立**Gulp 組態檔**(*gulpfile.js*) 來定義自動化的程序。  將變數加入頂端的較低，代表檔案和要執行更少的工作：

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

開啟**工作執行器總管**(**檢視 > 其他 Windows > 工作執行器總管**)。 在工作中，您應該會看到名為的新工作`less`。 您可能必須重新整理視窗。

執行`less`工作中，而且您看到類似於這裡所顯示的輸出：

![較少的工作執行器](less-sass-fa/_static/less-task-runner.png)

*Wwwroot/css*資料夾現在包含新的檔案， *main.css*:

![建立主要 css](less-sass-fa/_static/main-css-created.png)

開啟*main.css*而且您會看到類似下列：

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

加入簡單的 HTML 網頁至*wwwroot*資料夾，然後參考*main.css*若要查看作用中的色彩調色盤。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

您可以看到在上旋轉 180 度`@base`用來產生`@background`導致團體的色彩的色彩滾輪`@base`:

![較不測試範例](less-sass-fa/_static/less-test-screenshot.png)

Less 也提供對巢狀規則與巢狀媒體查詢的支援。例如，定義如功能表的巢狀階層可能會導致繁複的 CSS 規則，例如這些：

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

在理想情況下的所有相關的樣式規則將會放置在一起在 CSS 檔案中，但實際上沒有強制執行這項規則慣例和可能的註解區塊以外的項目。

使用 LESS 定義這些相同規則看起來像這樣：

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

請注意，在此情況下，所有的附屬項目`nav`所包含的範圍內。 不再有任何重複的父項目 (`nav`， `li`， `a`)，而且總計的行數以及 （雖然部分是將值放在第二個範例中相同的程式行的結果）。 它可以是檔案的很有幫助組織，若要看到的所有規則明確繫結的範圍內指定的 UI 項目在此情況下設定其餘大括號。

`&`語法是使用較少的選取器功能，與代表目前的選取器父系。 因此，在 {...} 區塊中，`&`代表`a`標記，因此`&:link`相當於`a:link`。

在建立回應式設計中非常實用的媒體查詢也可能導致 CSS 中的大量重複項與複雜度。Less 允許媒體查詢在類別中放在巢狀結構中，因此整個類別定義不需要在不同的頂層 `@media` 元素中重複。例如，以下是回應式功能表的 CSS：

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

這可以在 Less 內進一步定義為：

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

我們已經看過的另一個 Less 功能是它支援數學運算，允許從預先定義的變數建構樣式屬性。這使得更新相關的樣式更容易，因為只要修改基底變數就可以讓所有相依值自動變更。

CSS 檔案，特別是針對大型網站 (而且特別是如果使用媒體查詢)，在經過一段時間之後會變得相當大，造成使用上的不便。您可以分開定義多個 Less 檔案，然後使用 `@import` 指示詞合併。如果有需要，Less 也可以用來匯入個別 CSS 檔案。

*Mixins* 可以接受參數，而且 Less 支援 mixin 成立條件形式的條件式邏輯，這提供宣告式方式來定義特定 mixins 何時生效。mixin 成立條件的常見用途是根據來源色彩的深淺度來調整色彩。在給定一個可接受參數之 mixin 的情況下，mixin 成立條件可以用來根據該色彩修改 mixin：

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

指定我們目前`@base`值`#663333`，這個較少的指令碼會產生下列 CSS:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Less 提供一些其他功能，但以上內容應該能讓您了解這個前置處理語言的一些概念。

## <a name="sass"></a>Sass

Sass 很少類似提供支援的許多相同的功能，但稍微不同的語法。 它內建使用 Ruby，而非 JavaScript 中，並且擁有不同的安裝需求。 原始的 Sass 語言並未使用大括號或分號分隔，而定義使用泛空白字元和縮排的範圍。 中的 Sass 第 3 版，引進一種新語法， **SCSS** (「 Sassy CSS")。 SCSS 是類似於 CSS，在於，它會忽略縮排層級和空白字元，並改為使用分號和大括號。

若要安裝 Sass，通常您會先安裝 Ruby （預先安裝於 macOS），並接著執行：

```console
gem install sass
```

不過，如果您執行 Visual Studio，開始使用 Sass 大致就像開始使用 Less 一樣。開啟 *package.json* 並將 "gulp sass" 套件新增到 `devDependencies`：

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

接下來，修改*gulpfile.js*加入 sass 變數和 Sass 檔案編譯，並將結果放在 [wwwroot] 資料夾的工作：

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

現在您可以將 Sass 檔案 *main2.scss* 加入到專案根目錄的 *Style* 資料夾中：

![加入 scss 檔案](less-sass-fa/_static/add-scss-file.png)

開啟 *main2.scss* 並加入下列內容：

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

儲存您的所有檔案。 現在當您重新整理**工作執行器總管**，您會看到`sass`工作。 執行此程式碼，然後查看 */wwwroot/css*資料夾。 現在沒有*main2.css*檔案，與這些內容：

```css
body {
    background-color: #CC0000;
}
```

Sass 對巢狀的支援大致與 Less 相同。檔案可依功能分割，並使用 `@import` 指示詞來包含：

```sass
@import 'anotherfile';
```

Sass 支援 mixins，使用`@mixin`加以定義的關鍵字和`@include`來包含它們，如這個範例所示[sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

除了 mixins，sass 此類也支援繼承，允許以擴充另一個類別的概念。 它在概念上類似 mixin，但是在較少的 CSS 程式碼中的結果。 它會藉由`@extend`關鍵字。 若要試用 mixins，將下列內容加入您*main2.scss*檔案：

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

檢查輸出中的*main2.css*之後執行`sass`中工作**工作執行器總管**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

請注意，所有警示的 mixin 的通用屬性會重複在每個類別。 Mixin 好協助消除重複，在開發階段，但它仍然具有大於必要 CSS 檔案的潛在效能問題所導致的重複，大量建立 CSS。

現在取代了使用警示的 mixin`.alert`類別，並變更`@include`至`@extend`(記住擴充`.alert`，而非`alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

執行 Sass 一次，並檢查產生的 CSS:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

屬性現在定義只為視需要多次，CSS 會產生更好。

Sass 也包含函式與條件式邏輯作業，類似於 Less。 事實上，這兩種語言的功能非常類似。

## <a name="less-or-sass"></a>Less 或 Sass？

使用 Less 或 sass (或甚至是建議使用原始 Sass 嘿 SaaS 內較新的 SCSS 語法) 並無共識。最重要的決策可能是**使用這些工具的其中一個**，而不是手動撰寫 CSS 檔案程式碼。一旦您做出該決策，Less 與 Sass 都是很好的選擇。

## <a name="font-awesome"></a>Font Awesome

除了 CSS 前處理器，另一個樣式現代化 web 應用程式的最佳資源是字型。 字型都會提供超過 500 個可縮放向量圖示可以自由地使用 web 應用程式中的工具組。 它原本設計為能夠與啟動程序，但在該架構或任何 JavaScript 程式庫上有沒有相依性。

若要開始使用字型臻最簡單的方式是加入它的參考，使用其公用內容傳遞網路 (CDN) 位置：

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

您也可以加入它的 Visual Studio 專案加入至 「 相依性 」 中*bower.json*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

一旦您擁有的參考臻字型頁面上，您可以將圖示加入至您的應用程式藉由套用字型實用的類別，通常加上"fa-"，以您的內嵌 HTML 項目 (例如`<span>`或`<i>`)。  例如，您可以將圖示加入至簡單清單和功能表類似的程式碼：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

這會產生下列瀏覽器中的-請注意每個項目旁邊的圖示：

![清單圖示](less-sass-fa/_static/list-icons-screenshot.png)

您可以檢視可用的圖示的完整清單：

http://fontawesome.io/icons/

## <a name="summary"></a>總結

現代化 web 應用程式愈來愈要求能有效回應，流暢的設計是全新、 直覺而且容易使用的各種裝置。 最適合進行管理複雜的達成這些目標所需的 CSS 樣式表，較不使用前置處理器 like 或 Sass。 此外，像是字型實用的工具套件快速提供已知圖示，將文字巡覽功能表和按鈕，以改善整體的使用者體驗的應用程式。
