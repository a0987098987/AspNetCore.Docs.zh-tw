---
title: "在 ASP.NET Core 中使用 Gulp"
author: rick-anderson
description: "了解如何在 ASP.NET Core 中使用 Gulp。"
keywords: ASP.NET Core Gulp
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: 4095d273-bf3f-46cf-bdcc-18cf6815cbad
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-gulp
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05ea4d5f0a0be08cbbdd114320d3544aae054dd2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a>在 ASP.NET Core 中使用 Gulp 簡介 

由[Erik Reitan](https://github.com/Erikre)， [Scott Addie](https://scottaddie.com)，[奧 Roth](https://github.com/danroth27)，和[Shayne Boyer](https://twitter.com/spboyer)

在典型的現代 web 應用程式中，可能會在建置程序：

* 配套並縮短 JavaScript 和 CSS 的檔案。
* 執行工具，以呼叫統合及縮製的工作，再每個組建。
* 較不編譯或 SASS CSS 檔案。
* CoffeeScript 或 TypeScript 檔案編譯至 JavaScript。

A*工作執行器*是會自動將這些常式的開發工作，以及更多的工具。 Visual Studio 提供內建支援的兩個常用的 JavaScript 型工作 5d: [Gulp](http://gulpjs.com)和[Grunt](using-grunt.md)。

## <a name="gulp"></a>Gulp

Gulp 是 JavaScript 型資料流的建置工具組用戶端程式碼。 它通常用來建置環境中觸發特定事件時，資料流執行一連串的處理程序的用戶端檔案。 比方說，Gulp 可用來自動化[統合及縮製](bundling-and-minification.md)或開發環境，新的組建之前的清理。

中定義一組 Gulp 工作*gulpfile.js*。 下列 JavaScript 包含 Gulp 模組，並指定檔案路徑，以參考中推出的工作：

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

上述程式碼會指定哪一個節點模組所需。 `require`函式匯入每個模組，以便的相依工作可以使用其功能。 每個匯入的模組會指派給變數。 模組可以位於依名稱或路徑。 在此範例中，將模組命名為`gulp`， `rimraf`， `gulp-concat`， `gulp-cssmin`，和`gulp-uglify`會依名稱擷取。 此外，能夠重複使用及工作內所參考的 CSS 和 JavaScript 檔案的位置，會建立一系列的路徑。 下表提供的模組中包含描述*gulpfile.js*。

|模組名稱|說明|
|---|---|
|gulp|Gulp 串流建置系統。 如需詳細資訊，請參閱[gulp](https://www.npmjs.com/package/gulp)。|
|rimraf|節點刪除模組。 如需詳細資訊，請參閱[rimraf](https://www.npmjs.com/package/rimraf)。|
|gulp concat|模組可串連作業系統的新行字元為基礎的檔案。 如需詳細資訊，請參閱[gulp concat](https://www.npmjs.com/package/gulp-concat)。|
|gulp cssmin|縮短 CSS 檔案的模組。 如需詳細資訊，請參閱[gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)。|
|gulp uglify|縮短模組*.js*檔案。 如需詳細資訊，請參閱[gulp uglify](https://www.npmjs.com/package/gulp-uglify)。|

一旦匯入必要模組，您可以指定工作。 這裡有六個工作註冊中，由下列程式碼：

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

下表提供上述程式碼中指定之工作的說明：

|任務名稱|說明|
|--- |--- |
|全新： js|若要移除 site.js 檔案的縮短的版本使用 rimraf 節點刪除模組的工作。|
|全新： css|若要移除 site.css 檔案的縮短的版本使用 rimraf 節點刪除模組的工作。|
|清除|呼叫工作`clean:js`工作中，後面接著`clean:css`工作。|
|min:js|工作並縮短，串連的 js 資料夾中的所有.js 檔案。 。 Min.js 檔案會排除。|
|min:css|縮短及串連的 css 資料夾中的所有.css 檔案的工作。 。 Min.css 檔案會排除。|
|min|呼叫工作`min:js`工作中，後面接著`min:css`工作。|

## <a name="running-default-tasks"></a>執行預設的工作

如果您尚未建立新的 Web 應用程式，請在 Visual Studio 中建立新的 ASP.NET Web 應用程式專案。

1.  將新的 JavaScript 檔案加入至您的專案並將其命名*gulpfile.js*，然後複製下列程式碼。

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  開啟*package.json*檔案 (新增如果不存在)，然後將下列。

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*，然後選取**工作執行器總管**。
    
    ![從 [方案總管] 中開啟工作執行器總管](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **工作執行器總管**顯示 Gulp 工作的清單。 (您可能必須按一下**重新整理**會顯示專案名稱的左邊的按鈕。)
    
    ![工作執行器總管](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  下面**工作**中**工作執行器總管**，以滑鼠右鍵按一下**全新**，然後選取**執行**從快顯功能表。

    ![工作執行器總管清除 」 工作](using-gulp/_static/04-TaskRunner-clean.png)

    **工作執行器總管**會建立新的索引標籤，名為**全新**並執行 「 清除 」 工作，如其定義在*gulpfile.js*。

5.  以滑鼠右鍵按一下**全新**工作，然後選取 **繫結** > **之前建置**。

    ![繫結 BeforeBuild 工作執行器總管](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **之前建置**繫結會設定自動每個專案的組建之前執行 「 清除 」 工作。

繫結的設定**工作執行器總管**頂端的註解的形式儲存您*gulpfile.js*和只適用於 Visual Studio。 不需要 Visual Studio 的替代方式是設定自動執行的 gulp 工作中您*.csproj*檔案。 例如，將此字串放您*.csproj*檔案：

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

現在當您執行專案，Visual Studio 中，或從命令提示字元使用，會執行 「 清除 」 工作`dotnet run`命令 (執行`npm install`第一個)。

## <a name="defining-and-running-a-new-task"></a>定義和執行新工作

若要定義新的 Gulp 工作，請修改*gulpfile.js*。

1.  結尾加入下列 JavaScript *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    這項工作中名為`first`，並只會顯示字串。

2.  儲存*gulpfile.js*。

3.  在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*，然後選取*工作執行器總管*。

4.  在**工作執行器總管**，以滑鼠右鍵按一下**第一個**，然後選取**執行**。

    ![執行第一項工作的工作執行器總管](using-gulp/_static/06-TaskRunner-First.png)

    您會看到輸出文字會顯示。 如果您有興趣的常見案例為基礎的範例，請參閱的 Gulp 的配方。

## <a name="defining-and-running-tasks-in-a-series"></a>定義並執行序列中的工作

當您執行多個工作時，工作同時執行的預設值。 不過，如果您需要以特定順序執行的工作，您必須指定當每一項工作完成時，也做為哪些工作相依於另一項工作完成。

1.  若要定義一系列的順序執行的工作，取代`first`工作，您在上面加入*gulpfile.js*為下列：

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    您現在有三個工作： `series:first`， `series:second`，和`series`。 `series:second`工作包含指定的工作來執行和完成之前陣列的第二個參數`series:second`工作將會執行。  如同在上方，唯一的程式碼中指定`series:first`之前必須完成工作`series:second`工作將會執行。

2.  儲存*gulpfile.js*。

3.  在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*選取**工作執行器總管**如果尚未開啟。

4.  在**工作執行器總管**，以滑鼠右鍵按一下**數列**選取**執行**。

    ![工作執行器總管執行系列的工作](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense 提供程式碼完成功能、 參數說明和其他功能，以提高生產力，並減少錯誤。 Gulp 工作 JavaScript 撰寫的。因此，IntelliSense 可提供開發時的協助。 當您使用 JavaScript，IntelliSense 就會列出物件、 函式、 屬性和參數可根據您目前的內容。 從 IntelliSense 完成程式碼所提供的快顯清單中選取程式碼撰寫選項。

![IntelliSense 的 gulp](using-gulp/_static/08-IntelliSense.png)

如需有關 IntelliSense 的詳細資訊，請參閱[JavaScript IntelliSense](https://msdn.microsoft.com/library/bb385682)。

## <a name="development-staging-and-production-environments"></a>開發、 預備及生產環境

當 Gulp 用來最佳化預備和生產環境的用戶端檔案中時，處理的檔案會儲存至本機臨時區域與生產位置。 *_Layout.cshtml*檔案使用**環境**標記協助程式提供的 CSS 檔案的兩個不同版本。 一個版本的 CSS 檔案供程式開發，另一個版本中最適合用於預備和生產環境。 在 Visual Studio 2017，當您變更**ASPNETCORE_ENVIRONMENT**環境變數，以`Production`，Visual Studio 會建置的 Web 應用程式和連結至最小化的 CSS 檔案。 下列標記顯示**環境**標記包含連結標記協助程式`Development`CSS 檔案及縮短`Staging, Production`CSS 檔案。

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>環境之間切換

若要編譯不同環境之間切換，請修改**ASPNETCORE_ENVIRONMENT**環境變數的值。

1.  在**工作執行器總管**，確認**min**工作已設定為執行**之前建置**。

2.  在**方案總管 中**，以滑鼠右鍵按一下專案名稱，然後選取**屬性**。

    Web 應用程式的屬性工作表會顯示。

3.  按一下**偵錯** 索引標籤。

4.  值設定**主控： 環境**環境變數，以`Production`。

5.  按**F5**瀏覽器中執行應用程式。

6.  在瀏覽器視窗中，以滑鼠右鍵按一下 [] 頁面上，選取**檢視原始檔**來檢視 HTML 頁面。

    請注意，樣式表連結指向縮短 CSS 檔案。

7.  關閉瀏覽器以停止 Web 應用程式。

8.  在 Visual Studio 中，返回 Web 應用程式的屬性工作表，並變更**主控： 環境**環境變數傳回到`Development`。

9.  按**F5**再次在瀏覽器中執行應用程式。

10. 在瀏覽器視窗中，以滑鼠右鍵按一下 [] 頁面上，選取**檢視原始檔**以查看頁面的 HTML。

    請注意，樣式表連結指向 unminified CSS 檔案的版本。

如需 ASP.NET Core 的環境相關的資訊，請參閱[使用多個環境](../fundamentals/environments.md)。

## <a name="task-and-module-details"></a>工作和模組的詳細資料

Gulp 工作已註冊的函式名稱。  如果目前的工作之前，必須執行其他工作，您可以指定相依性。 其他函式可讓您執行和觀賞 Gulp 的工作，以及將來源設定 (*src*) 和目的地 (*目的地*) 正在修改的檔案。 以下是主要的 Gulp API 函式：

|Gulp 函式|語法|說明|
|---   |--- |--- |
|task  |`gulp.task(name[, deps], fn) { }`|`task`函式建立的工作。 `name`參數會定義工作的名稱。 `deps`參數包含才可以執行此工作已完成工作的陣列。 `fn`參數所代表的回呼函式會執行工作的作業。|
|監看式 |`gulp.watch(glob [, opts], tasks) { }`|`watch`函式監視檔案和執行工作，當檔案變更時。 `glob`參數是`string`或`array`，決定要監看的檔案。 `opts`參數提供額外監控選項的檔案。|
|src   |`gulp.src(globs[, options]) { }`|`src`函式提供符合 glob 值的檔案。 `glob`參數是`string`或`array`，決定要讀取的檔案。 `options`參數提供的其他檔案選項。|
|目的地  |`gulp.dest(path[, options]) { }`|`dest`函式會定義可以寫入檔案的位置。 `path`參數是字串或函式，判斷目的地資料夾。 `options`參數是物件，指定輸出資料夾的選項。|

其他的 Gulp 應用程式開發介面的參考資訊，請參閱[Gulp 文件 API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)。

## <a name="gulp-recipes"></a>Gulp 配方

Gulp 社群也提供 Gulp[配方](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)。 這些配方組成 Gulp 工作，以解決常見的案例。

## <a name="additional-resources"></a>其他資源

* [Gulp 文件](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [統合及縮製中 ASP.NET Core](bundling-and-minification.md)
* [使用 ASP.NET Core Grunt](using-grunt.md)
