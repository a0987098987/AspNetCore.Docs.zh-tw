---
title: "小於，Sass，並且在 ASP.NET Core 實用的字型"
author: ardalis
description: "了解如何使用小於，Sass，並且字型臻 ASP.NET Core 應用程式中。"
manager: wpickett
ms.author: tdykstra
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/less-sass-fa
ms.openlocfilehash: 979f5639e382560d952df45ba6e0b8af3b132c2d
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/16/2018
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="6ab1d-103">樣式應用程式，以更少、 Sass，和字型臻中 ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="6ab1d-103">Introduction to styling applications with Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="6ab1d-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6ab1d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6ab1d-105">以設定樣式及整體體驗時，web 應用程式的使用者會有越來越多高的期望。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-105">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="6ab1d-106">現代化 web 應用程式經常會運用豐富的工具及架構定義和管理其外觀及操作，以一致的方式。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-106">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="6ab1d-107">架構喜歡[Bootstrap](http://getbootstrap.com/)可以對定義一組常用的樣式和網站的版面配置選項。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-107">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="6ab1d-108">不過，大部分的非一般站台也可以從能夠有效地定義和維護樣式和階層式樣式表 (CSS) 檔案，以及輕鬆存取協助讓站台的介面更直覺的非影像圖示。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-108">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="6ab1d-109">這是 where 語言和支援的工具[較少](http://lesscss.org/)和[Sass](http://sass-lang.com/)，如同程式庫和[字型臻](http://fontawesome.io/)，有。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-109">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="6ab1d-110">CSS 前置處理器的語言</span><span class="sxs-lookup"><span data-stu-id="6ab1d-110">CSS preprocessor languages</span></span>

<span data-ttu-id="6ab1d-111">編譯成其他語言，以改進的經驗為基礎的語言，使用的語言被指前置處理器。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-111">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="6ab1d-112">有兩個常用的前置處理器 css： 較不和 Sass。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-112">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="6ab1d-113">這些前置處理器會將功能加入 CSS，例如變數和改善的可維護性大型、 複雜的樣式表中的巢狀的規則的支援。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-113">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="6ab1d-114">CSS，做為語言是非常基本，缺少即使對於簡單的變數，以支援，這傾向於使 CSS 檔案重複和繁雜。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-114">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="6ab1d-115">加入透過前置處理器的實際程式設計語言功能，可協助減少重複，並提供較佳的組織的樣式規則。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-115">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="6ab1d-116">Visual Studio 提供內建支援這兩個無和 Sass，以及進一步改善開發經驗中，當使用這些語言的擴充功能。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-116">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="6ab1d-117">做為前置處理器可以如何改善可讀性和可維護性的樣式資訊的快速範例，請考慮這個 CSS:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-117">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

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

<span data-ttu-id="6ab1d-118">使用更少，這可以重寫以排除所有的重複使用*mixin* (如此命名，因為它可讓您"mix"自一個類別或規則集至另一個屬性):</span><span class="sxs-lookup"><span data-stu-id="6ab1d-118">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

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

## <a name="less"></a><span data-ttu-id="6ab1d-119">較低</span><span class="sxs-lookup"><span data-stu-id="6ab1d-119">Less</span></span>

<span data-ttu-id="6ab1d-120">較不 CSS 前置處理器會使用 Node.js 來執行。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-120">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="6ab1d-121">從命令提示字元安裝更少，請使用 Node 封裝管理員 (npm) (-g 表示 「 全域 」):</span><span class="sxs-lookup"><span data-stu-id="6ab1d-121">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="6ab1d-122">如果您使用 Visual Studio，您可以開始使用，將一個或多個較少檔案加入至您的專案，然後設定 加以處理在編譯時期 Gulp （或 Grunt） 更少。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-122">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="6ab1d-123">新增*樣式*資料夾，以您的專案，然後再加入新的檔案命名為小於*main.less*到這個資料夾。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-123">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![加入較少的檔案](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="6ab1d-125">一旦加入，您的資料夾結構看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-125">Once added, your folder structure should look something like this:</span></span>

![資料夾結構](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="6ab1d-127">現在您可以加入一些基本的樣式會部署到的 wwwroot 資料夾 Gulp 並編譯成 CSS 的檔案。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-127">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="6ab1d-128">修改*main.less*包含以下內容，從單一基底的色彩會建立簡單的色彩調色盤。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-128">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

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

<span data-ttu-id="6ab1d-129">`@base` 和其他@-prefixed項目是變數。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-129">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="6ab1d-130">每個代表色彩。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-130">Each of them represents a color.</span></span> <span data-ttu-id="6ab1d-131">除了`@base`，設定色彩函式的使用： 淡化、 暗化，和微調。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-131">Except for `@base`, they're set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="6ab1d-132">淡化和暗化執行幾乎不如預期。微調的數字為度 （大約是色彩滾輪） 以調整色彩的色調。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-132">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="6ab1d-133">較少的處理器是聰明，可以忽略不使用的變數，因此我們需要來示範這些變數的運作方式，使用這些位置。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-133">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="6ab1d-134">類別`.baseColor`，等將示範每個變數所產生的 CSS 檔案中的導出的值。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-134">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that's produced.</span></span>

### <a name="get-started"></a><span data-ttu-id="6ab1d-135">開始使用</span><span class="sxs-lookup"><span data-stu-id="6ab1d-135">Get started</span></span>

<span data-ttu-id="6ab1d-136">建立**npm 組態檔**(*package.json*) 在您的專案資料夾和編輯，以參考`gulp`和`gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-136">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

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

<span data-ttu-id="6ab1d-137">在您的專案資料夾，或在 Visual Studio 中安裝的相依性，請在命令提示字元**方案總管 中**(**相依性 > npm > 還原封裝**)。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-137">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![VS 還原封裝](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="6ab1d-139">在專案資料夾中，建立**Gulp 組態檔**(*gulpfile.js*) 來定義自動化的程序。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-139">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="6ab1d-140">將變數加入頂端的較低，代表檔案和要執行更少的工作：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-140">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

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

<span data-ttu-id="6ab1d-141">開啟**工作執行器總管**(**檢視 > 其他 Windows > 工作執行器總管**)。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-141">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="6ab1d-142">在工作中，您應該會看到名為的新工作`less`。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-142">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="6ab1d-143">您可能必須重新整理視窗。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-143">You might have to refresh the window.</span></span>

<span data-ttu-id="6ab1d-144">執行`less`工作中，而且您看到類似於這裡所顯示的輸出：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-144">Run the `less` task, and you see output similar to what is shown here:</span></span>

![較少的工作執行器](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="6ab1d-146">*Wwwroot/css*資料夾現在包含新的檔案， *main.css*:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-146">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![建立主要 css](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="6ab1d-148">開啟*main.css*而且您會看到類似下列：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-148">Open *main.css* and you see something like the following:</span></span>

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

<span data-ttu-id="6ab1d-149">加入簡單的 HTML 網頁至*wwwroot*資料夾，然後參考*main.css*若要查看作用中的色彩調色盤。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-149">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

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

<span data-ttu-id="6ab1d-150">您可以看到在上旋轉 180 度`@base`用來產生`@background`導致團體的色彩的色彩滾輪`@base`:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-150">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![較不測試範例](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="6ab1d-152">小於也提供支援巢狀的規則，以及巢狀的媒體查詢。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-152">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="6ab1d-153">例如，定義巢狀的階層，例如功能表可能會導致 verbose CSS 規則喜歡這些：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-153">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

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

<span data-ttu-id="6ab1d-154">在理想情況下的所有相關的樣式規則將會放置在一起在 CSS 檔案中，但實際上沒有強制執行這項規則慣例和可能的註解區塊以外的項目。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-154">Ideally all of the related style rules will be placed together within the CSS file, but in practice there's nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="6ab1d-155">定義小於使用這些相同的規則看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-155">Defining these same rules using Less looks like this:</span></span>

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

<span data-ttu-id="6ab1d-156">請注意，在此情況下，所有的附屬項目`nav`所包含的範圍內。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-156">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="6ab1d-157">不再有任何重複的父項目 (`nav`， `li`， `a`)，而且總計的行數以及 （雖然部分是將值放在第二個範例中相同的程式行的結果）。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-157">There's no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that's a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="6ab1d-158">它可以是檔案的很有幫助組織，若要看到的所有規則明確繫結的範圍內指定的 UI 項目在此情況下設定其餘大括號。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-158">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="6ab1d-159">`&`語法是使用較少的選取器功能，與代表目前的選取器父系。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-159">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="6ab1d-160">因此，在 {...}</span><span class="sxs-lookup"><span data-stu-id="6ab1d-160">So, within the a {...}</span></span> <span data-ttu-id="6ab1d-161">區塊中，`&`代表`a`標記，因此`&:link`相當於`a:link`。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-161">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="6ab1d-162">建立回應式設計中，非常有用的媒體查詢也可能導致大量重複和 CSS 的複雜度。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-162">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="6ab1d-163">小於允許的媒體查詢在類別中，巢狀，因此，整個類別定義不需要在不同中重複最上層`@media`項目。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-163">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="6ab1d-164">例如，以下是 CSS 回應功能表：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-164">For example, here is CSS for a responsive menu:</span></span>

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

<span data-ttu-id="6ab1d-165">這可以進一步定義都不做為：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-165">This can be better defined in Less as:</span></span>

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

<span data-ttu-id="6ab1d-166">小於我們已經看過的另一項功能是支援的數學運算，允許從預先定義的變數建構的樣式屬性。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-166">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="6ab1d-167">這使得更新的相關的樣式更容易，因為可以修改基底的變數，而所有相依的值會自動變更。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-167">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="6ab1d-168">CSS 檔案，特別是針對大型網站 （而且特別是如果使用媒體查詢），會變得相當大一段時間，使變得不便使用它們。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-168">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="6ab1d-169">Less 檔案可以分開定義，然後提取一起使用`@import`指示詞。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-169">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="6ab1d-170">小於也可用來匯入個別 CSS 檔案，如有需要。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-170">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="6ab1d-171">*Mixins*可以接受參數，而且小於支援條件式邏輯形式的 mixin 成立條件，提供宣告的方式，來定義特定 mixins 時才會生效。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-171">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="6ab1d-172">Mixin 成立條件的常見用法是調整色彩如何根據或深色來源色彩。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-172">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="6ab1d-173">指定可接受的參數，色彩的 mixin 的 mixin 成立條件可以用來修改該色彩為基礎的 mixin:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-173">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

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

<span data-ttu-id="6ab1d-174">指定我們目前`@base`值`#663333`，這個較少的指令碼會產生下列 CSS:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-174">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="6ab1d-175">小於提供一些其他功能，但這應該讓您瞭解這個電源的前置處理語言。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-175">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="6ab1d-176">Sass</span><span class="sxs-lookup"><span data-stu-id="6ab1d-176">Sass</span></span>

<span data-ttu-id="6ab1d-177">Sass 很少類似提供支援的許多相同的功能，但稍微不同的語法。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-177">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="6ab1d-178">它內建使用 Ruby，而非 JavaScript 中，並且擁有不同的安裝需求。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-178">It's built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="6ab1d-179">原始的 Sass 語言並未使用大括號或分號分隔，而定義使用泛空白字元和縮排的範圍。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-179">The original Sass language didn't use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="6ab1d-180">中的 Sass 第 3 版，引進一種新語法， **SCSS** (「 Sassy CSS")。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-180">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="6ab1d-181">SCSS 是類似於 CSS，在於，它會忽略縮排層級和空白字元，並改為使用分號和大括號。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-181">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="6ab1d-182">若要安裝 Sass，通常您會先安裝 Ruby （預先安裝於 macOS），並接著執行：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-182">To install Sass, typically you would first install Ruby (pre-installed on macOS), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="6ab1d-183">不過，如果您執行 Visual Studio，您可以開始使用 Sass 大致相同的方式和小於一樣。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-183">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="6ab1d-184">開啟*package.json*並加入至 「 gulp sass 」 封裝`devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-184">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="6ab1d-185">接下來，修改*gulpfile.js*加入 sass 變數和 Sass 檔案編譯，並將結果放在 [wwwroot] 資料夾的工作：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-185">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

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

<span data-ttu-id="6ab1d-186">現在您可以加入 Sass 檔案*main2.scss*至*樣式*中專案的根資料夾：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-186">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![加入 scss 檔案](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="6ab1d-188">開啟*main2.scss*並加入下列：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-188">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="6ab1d-189">儲存您的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-189">Save all of your files.</span></span> <span data-ttu-id="6ab1d-190">現在當您重新整理**工作執行器總管**，您會看到`sass`工作。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-190">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="6ab1d-191">執行此程式碼，然後查看*/wwwroot/css*資料夾。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-191">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="6ab1d-192">現在沒有*main2.css*檔案，與這些內容：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-192">There's now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="6ab1d-193">Sass 支援巢狀大致相同的小於會提供類似的優點。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-193">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="6ab1d-194">檔案可以分割函式，並包含使用`@import`指示詞：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-194">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="6ab1d-195">Sass 支援 mixins，使用`@mixin`加以定義的關鍵字和`@include`來包含它們，如這個範例所示[sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="6ab1d-195">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="6ab1d-196">除了 mixins，sass 此類也支援繼承，允許以擴充另一個類別的概念。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-196">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="6ab1d-197">它在概念上類似 mixin，但是在較少的 CSS 程式碼中的結果。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-197">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="6ab1d-198">它會藉由`@extend`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-198">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="6ab1d-199">若要試用 mixins，將下列內容加入您*main2.scss*檔案：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-199">To try out mixins, add the following to your *main2.scss* file:</span></span>

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

<span data-ttu-id="6ab1d-200">檢查輸出中的*main2.css*之後執行`sass`中工作**工作執行器總管**:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-200">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

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

<span data-ttu-id="6ab1d-201">請注意，所有警示的 mixin 的通用屬性會重複在每個類別。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-201">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="6ab1d-202">Mixin 好協助消除重複，在開發階段，但它仍然具有大於必要 CSS 檔案的潛在效能問題所導致的重複，大量建立 CSS。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-202">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="6ab1d-203">現在取代了使用警示的 mixin`.alert`類別，並變更`@include`至`@extend`(記住擴充`.alert`，而非`alert`):</span><span class="sxs-lookup"><span data-stu-id="6ab1d-203">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

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

<span data-ttu-id="6ab1d-204">執行 Sass 一次，並檢查產生的 CSS:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-204">Run Sass once more, and examine the resulting CSS:</span></span>

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

<span data-ttu-id="6ab1d-205">屬性現在定義只為視需要多次，CSS 會產生更好。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-205">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="6ab1d-206">Sass 也包含函式和條件式邏輯作業，類似於小於。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-206">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="6ab1d-207">事實上，有兩種語言的功能非常類似。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-207">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="6ab1d-208">較低或 Sass 嗎？</span><span class="sxs-lookup"><span data-stu-id="6ab1d-208">Less or Sass?</span></span>

<span data-ttu-id="6ab1d-209">仍有其是否通常較容易使用小於或 sass 此類不一致 （或甚至是是否要偏好使用原始 Sass 內較新的 SCSS 語法）。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-209">There's still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="6ab1d-210">最重要的決策可能是**使用這些工具的其中一個**，與只手動編碼 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-210">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="6ab1d-211">一旦您所做的決策，這兩個無和 Sass 是很好的選擇。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-211">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="6ab1d-212">實用的字型</span><span class="sxs-lookup"><span data-stu-id="6ab1d-212">Font Awesome</span></span>

<span data-ttu-id="6ab1d-213">除了 CSS 前處理器，另一個樣式現代化 web 應用程式的最佳資源是字型。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-213">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="6ab1d-214">字型都會提供超過 500 個可縮放向量圖示可以自由地使用 web 應用程式中的工具組。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-214">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="6ab1d-215">它原本設計為能夠與啟動程序，但在該架構或任何 JavaScript 程式庫上有沒有相依性。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-215">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="6ab1d-216">若要開始使用字型臻最簡單的方式是加入它的參考，使用其公用內容傳遞網路 (CDN) 位置：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-216">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="6ab1d-217">您也可以加入它的 Visual Studio 專案加入至 「 相依性 」 中*bower.json*:</span><span class="sxs-lookup"><span data-stu-id="6ab1d-217">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

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

<span data-ttu-id="6ab1d-218">一旦您擁有的參考臻字型頁面上，您可以將圖示加入至您的應用程式藉由套用字型實用的類別，通常加上"fa-"，以您的內嵌 HTML 項目 (例如`<span>`或`<i>`)。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-218">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="6ab1d-219">例如，您可以將圖示加入至簡單清單和功能表類似的程式碼：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-219">For example, you can add icons to simple lists and menus using code like this:</span></span>

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

<span data-ttu-id="6ab1d-220">這會產生下列瀏覽器中的-請注意每個項目旁邊的圖示：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-220">This produces the following in the browser - note the icon beside each item:</span></span>

![清單圖示](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="6ab1d-222">您可以檢視可用的圖示的完整清單：</span><span class="sxs-lookup"><span data-stu-id="6ab1d-222">You can view a complete list of the available icons here:</span></span>

http://fontawesome.io/icons/

## <a name="summary"></a><span data-ttu-id="6ab1d-223">總結</span><span class="sxs-lookup"><span data-stu-id="6ab1d-223">Summary</span></span>

<span data-ttu-id="6ab1d-224">現代化 web 應用程式愈來愈要求能有效回應，流暢的設計是全新、 直覺而且容易使用的各種裝置。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-224">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="6ab1d-225">最適合進行管理複雜的達成這些目標所需的 CSS 樣式表，較不使用前置處理器 like 或 Sass。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-225">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="6ab1d-226">此外，像是字型實用的工具套件快速提供已知圖示，將文字巡覽功能表和按鈕，以改善整體的使用者體驗的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ab1d-226">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
