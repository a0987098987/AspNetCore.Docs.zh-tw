---
title: ASP.NET Core 中使用 Gulp
author: rick-anderson
description: 了解如何使用 Gulp，在 ASP.NET Core 中。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 4f383be0498b5b861bd43cc0f0685b1e62c7571b
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795514"
---
# <a name="use-gulp-in-aspnet-core"></a>ASP.NET Core 中使用 Gulp

作者：[Scott Addie](https://scottaddie.com)， [Shayne Boyer](https://twitter.com/spboyer) 與 [David Pine](https://twitter.com/davidpine7)

在典型的現代 web 應用程式中，可能會在建置程序：

* 包裝並最小化 JavaScript 和 CSS 的檔案。
* 每次建置前執行工具以呼叫包裝與最小化工作。
* 編譯 LESS 或 SASS 檔案為 CSS 。
* 編譯 CoffeeScript 或 TypeScript 檔案為 JavaScript。

「工作執行器」是一種自動執行例行性開發工作等的工具。 Visual Studio 為兩個熱門的 JavaScript 型工作執行器提供內建支援: [Gulp](https://gulpjs.com/)和[Grunt](using-grunt.md)。

## <a name="gulp"></a>Gulp

Gulp 是以 JavaScript 為基礎的資料流建置工具組，用於用戶端程式碼。 當在建置環境中觸發特定事件時，通常會使用它透過一系列的程序串流用戶端檔案。 例如，在新的建置之前，Gulp 可用於自動化[包裝及縮小](bundling-and-minification.md)或開發環境清理。

在 *gulpfile.js* 中定義一組 Gulp 工作。 下列 JavaScript 包含 Gulp 模組，並指定要在即將到來的工作中引用的文件路徑：

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

上述程式碼會指定哪些節點模組所需。 `require`函式匯入每個模組，以便讓相依的工作能夠利用其功能。 每個匯入的模組會指派給變數。 模組可以位於依名稱或路徑。 在此範例中，將模組命名為`gulp`， `rimraf`， `gulp-concat`， `gulp-cssmin`，和`gulp-uglify`會依名稱擷取。 此外，以便可以重複使用和工作內參照的 CSS 和 JavaScript 檔案的位置，會建立一系列的路徑。 下表描述中包含的模組*gulpfile.js*。

| 模組名稱 | 描述 |
| ----------- | ----------- |
| gulp        | Gulp 串流處理組建系統。 如需詳細資訊，請參閱 < [gulp](https://www.npmjs.com/package/gulp)。 |
| rimraf      | 節點刪除模組。 如需詳細資訊，請參閱 < [rimraf](https://www.npmjs.com/package/rimraf)。 |
| gulp concat | 這種模組串連根據作業系統的新行字元的檔案。 如需詳細資訊，請參閱 < [gulp concat](https://www.npmjs.com/package/gulp-concat)。 |
| gulp-cssmin | 可將 CSS 檔案最小化的模組。 如需詳細資訊，請參閱 < [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)。 |
| gulp uglify | 縮短模組 *.js*檔案。 如需詳細資訊，請參閱 < [gulp uglify](https://www.npmjs.com/package/gulp-uglify)。 |

一旦匯入必要模組，您可以指定工作。 這裡有六個工作登錄，以下列程式碼：

```javascript
gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

gulp.task("min:js", () => {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", () => {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", gulp.series(["min:js", "min:css"]));
    
// A 'default' task is required by Gulp v4
gulp.task("default", gulp.series(["min"]));
```

---

下表提供上述程式碼中指定的工作的說明：

|任務名稱|描述|
|--- |--- |
|清除： js|若要移除 site.js 檔案的縮短的版本使用 rimraf 節點刪除模組的工作。|
|清除： css|若要移除 site.css 檔案的縮短的版本使用 rimraf 節點刪除模組的工作。|
|清除|工作，以呼叫`clean:js`工作，後面接著`clean:css`工作。|
|min:js|工作並縮短，串連的 js 資料夾中的所有.js 檔案。 。 排除 min.js 檔案。|
|min:css|縮短，並串連所有的.css 檔案，在 css 資料夾中的工作。 。 排除 min.css 檔案。|
|min|工作，以呼叫`min:js`工作，後面接著`min:css`工作。|

## <a name="running-default-tasks"></a>執行預設工作

如果您尚未建立新的 Web 應用程式，請在 Visual Studio 中建立新的 ASP.NET Web 應用程式專案。

1.  開啟*package.json*檔案 (新增如果不存在)，並新增下列。

    ```json
    {
      "devDependencies": {
        "gulp": "^4.0.0",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.2.0",
        "gulp-uglify": "3.0.0",
        "rimraf": "2.6.1"
      }
    }
    ```

2.  將新的 JavaScript 檔案加入至專案並將它命名*gulpfile.js*，然後複製下列程式碼。

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    const gulp = require("gulp"),
          rimraf = require("rimraf"),
          concat = require("gulp-concat"),
          cssmin = require("gulp-cssmin"),
          uglify = require("gulp-uglify");
    
    const paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
    gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
    gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

    gulp.task("min:js", () => {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });

    gulp.task("min:css", () => {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });

    gulp.task("min", gulp.series(["min:js", "min:css"]));
    
    // A 'default' task is required by Gulp v4
    gulp.task("default", gulp.series(["min"]));
    ```

3.  在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*，然後選取**Task Runner Explorer**。
    
    ![從 [方案總管] 中開啟 [Task Runner explorer]](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Task Runner Explorer**顯示 Gulp 工作的清單。 (您可能必須按一下 **重新整理**專案名稱的左邊會出現的按鈕。)
    
    ![Task Runner explorer](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > **Task Runner Explorer**操作功能表項目才會出現*gulpfile.js*專案根目錄中。

4.  下面**工作**中**Task Runner Explorer**，以滑鼠右鍵按一下**全新**，然後選取**執行**從快顯功能表。

    ![工作執行器總管清除工作](using-gulp/_static/04-TaskRunner-clean.png)

    **Task Runner Explorer**會建立名為新的索引標籤**全新**並執行 「 清除 」 工作，因為它定義在*gulpfile.js*。

5.  以滑鼠右鍵按一下**全新**工作，然後選取**繫結** > **之前建置**。

    ![繫結 BeforeBuild task Runner explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **再建置**繫結會設定專案的每個組建之前自動執行 「 清除 」 工作。

繫結您設有**Task Runner Explorer**頂端的註解的形式儲存您*gulpfile.js 中*和只適用於 Visual Studio。 不需要 Visual Studio 的替代方式是設定自動執行 gulp 工作，在您 *.csproj*檔案。 例如，將它放在您 *.csproj*檔案：

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

現在當您執行專案時 Visual Studio 中，或從命令提示字元使用，「 清除 」 工作會執行[執行的 dotnet](/dotnet/core/tools/dotnet-run)命令 (執行`npm install`第一次)。

## <a name="defining-and-running-a-new-task"></a>定義和執行新工作

若要定義新的 Gulp 工作，修改*gulpfile.js*。

1.  結尾新增下列 JavaScript *gulpfile.js*:

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    這項工作中名為`first`，和它就會顯示為字串。

2.  儲存*gulpfile.js*。

3.  在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*，然後選取*Task Runner Explorer*。

4.  在**Task Runner Explorer**，以滑鼠右鍵按一下**第一次**，然後選取**執行**。

    ![執行第一項工作的工作執行器總管](using-gulp/_static/06-TaskRunner-First.png)

    會顯示輸出文字。 若要查看根據常見案例的範例，請參閱[Gulp 配方](#gulp-recipes)。

## <a name="defining-and-running-tasks-in-a-series"></a>定義及執行一連串的工作

當您執行多個工作時，工作同時執行的預設值。 不過，如果您需要以特定順序執行工作，您必須指定當每個工作完成時，也為哪些工作相依於另一項工作完成。

1.  若要定義一系列的工作順序執行，取代`first`中新增上述的工作*gulpfile.js*取代下列項目：

    ```javascript
    gulp.task('series:first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    gulp.task('series:second', done => {
      console.log('second task! <-----');
      done(); // signal completion
    });

    gulp.task('series', gulp.series(['series:first', 'series:second']), () => { });

    // A 'default' task is required by Gulp v4
    gulp.task('default', gulp.series('series'));
    ```
 
    您現在有三項工作： `series:first`， `series:second`，和`series`。 `series:second` 」 工作包含第二個參數，用以指定工作要執行和之前完成陣列`series:second`工作將會執行。 在上方，唯一的程式碼中所指定`series:first`前，必須完成工作`series:second`工作將會執行。

2.  儲存*gulpfile.js*。

3.  中**方案總管**，以滑鼠右鍵按一下*gulpfile.js* ，然後選取**Task Runner Explorer**如果尚未開啟。

4.  在  **Task Runner Explorer**，以滑鼠右鍵按一下**系列**，然後選取**執行**。

    ![Task Runner explorer，執行序列工作](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense 會提供程式碼完成、 參數說明和其他功能，大幅提升生產力，並減少錯誤。 Gulp 工作會以 JavaScript;因此，IntelliSense 可提供開發時的協助。 當您使用 JavaScript 時，IntelliSense 會列出物件、 函式、 屬性和參數，可根據您目前的內容。 從提供 IntelliSense 完成程式碼的快顯清單中選取程式碼撰寫的選項。

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

如需有關 IntelliSense 的詳細資訊，請參閱 < [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense)。

## <a name="development-staging-and-production-environments"></a>開發、 預備及生產環境

當 Gulp 用來最佳化預備和生產環境的用戶端檔案時，處理的檔案會儲存到本機預備與生產位置。 *_Layout.cshtml*檔案使用**環境**標記協助程式提供 CSS 檔案的兩個不同版本。 一個 CSS 檔案版本適用於開發，而另一個版本已同時針對預備與生產環境最佳化。 在 Visual Studio 2017 中，當您將 **ASPNETCORE_ENVIRONMENT** 環境變數變更為 `Production` 時，Visual Studio 會建置 Web 應用程式並連結到最小化的 CSS 檔案。 下列標記顯示**環境**標記協助程式，此協助程式包 `Development` CSS 檔案與最小化之 `Staging, Production` CSS 檔案的含連結標記。

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

## <a name="switching-between-environments"></a>切換環境

若要切換為不同的環境中編譯，修改**ASPNETCORE_ENVIRONMENT**環境變數的值。

1.  在**Task Runner Explorer**，確認**min**已設為執行的工作**之前建置**。

2.  在 **方案總管**，以滑鼠右鍵按一下專案名稱，然後選取**屬性**。

    Web 應用程式的屬性工作表會顯示。

3.  按一下 [偵錯] 索引標籤。

4.  設定的值**主控： 環境**環境變數，以`Production`。

5.  按下**F5**瀏覽器中執行應用程式。

6.  在瀏覽器視窗中，以滑鼠右鍵按一下  頁面上，然後選取**檢視原始檔**來檢視 HTML 頁面。

    請注意，樣式表連結指向最小化的 CSS 檔案。

7.  關閉瀏覽器以停止 Web 應用程式。

8.  在 Visual Studio 中，返回 Web 應用程式的屬性工作表，並將變更**主控： 環境**環境變數將會回到`Development`。

9.  按下**F5**在瀏覽器中再次執行應用程式。

10. 在瀏覽器視窗中，以滑鼠右鍵按一下  頁面上，然後選取**檢視原始檔**以查看頁面的 HTML。

    請注意，樣式表連結指向 unminified CSS 檔案的版本。

與 ASP.NET Core 中的環境相關的詳細資訊，請參閱[使用多個環境](../fundamentals/environments.md)。

## <a name="task-and-module-details"></a>工作和模組的詳細資料

Gulp 工作會向函式名稱。 如果其他工作必須執行目前的工作之前，您可以指定相依性。 其他函數可讓您執行和監看的 Gulp 工作，以及將來源設定 (*src*) 和目的地 (*dest*) 所修改的檔案。 以下是主要的 Gulp API 函式：

|Gulp 函式|語法|描述|
|---   |--- |--- |
|task  |`gulp.task(name[, deps], fn) { }`|`task`函式會建立一項工作。 `name`參數會定義工作的名稱。 `deps`參數會包含這項工作執行前已完成工作的陣列。 `fn`參數代表的回呼函式執行工作的作業。|
|監看式 |`gulp.watch(glob [, opts], tasks) { }`|`watch`檔案變更時，函式會監視檔案和執行工作。 `glob`參數是`string`或`array`，決定要監看的檔案。 `opts`參數會提供額外的檔案監看的選項。|
|src   |`gulp.src(globs[, options]) { }`|`src`函式會提供符合 glob 值的檔案。 `glob`參數是`string`或`array`，決定要讀取的檔案。 `options`參數提供的其他檔案選項。|
|目的地  |`gulp.dest(path[, options]) { }`|`dest`函式會定義可以寫入檔案的位置。 `path`參數是字串或函式，判斷目的地資料夾。 `options`參數是物件，指定輸出資料夾的選項。|

如需其他的 Gulp API 參考資訊，請參閱[Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)。

## <a name="gulp-recipes"></a>Gulp 配方

Gulp 社群也提供 Gulp[配方](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)。 這些配方所組成 Gulp 工作，以解決常見的案例。

## <a name="additional-resources"></a>其他資源

* [Gulp 文件](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [在 ASP.NET Core 中包裝及最小化](bundling-and-minification.md)
* [在 ASP.NET Core 中使用 Grunt](using-grunt.md)
