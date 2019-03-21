---
title: ASP.NET Core 中使用 Gulp
author: rick-anderson
description: 了解如何使用 Gulp，在 ASP.NET Core 中。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 9f6d03a1e8a81bceca15cb1e1aa664c22c31e1d3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209868"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="d76fb-103">ASP.NET Core 中使用 Gulp</span><span class="sxs-lookup"><span data-stu-id="d76fb-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="d76fb-104">作者：[Scott Addie](https://scottaddie.com)， [Shayne Boyer](https://twitter.com/spboyer) 與 [David Pine](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="d76fb-104">By [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="d76fb-105">在典型的現代 web 應用程式中，可能會在建置程序：</span><span class="sxs-lookup"><span data-stu-id="d76fb-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="d76fb-106">包裝並最小化 JavaScript 和 CSS 的檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="d76fb-107">每次建置前執行工具以呼叫包裝與最小化工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="d76fb-108">編譯 LESS 或 SASS 檔案為 CSS 。</span><span class="sxs-lookup"><span data-stu-id="d76fb-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="d76fb-109">編譯 CoffeeScript 或 TypeScript 檔案為 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="d76fb-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="d76fb-110">「工作執行器」是一種自動執行例行性開發工作等的工具。</span><span class="sxs-lookup"><span data-stu-id="d76fb-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="d76fb-111">Visual Studio 提供兩個熱門 JavaScript 為基礎的工作執行器內建的支援：[Gulp](https://gulpjs.com/)並[Grunt](using-grunt.md)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="d76fb-112">Gulp</span><span class="sxs-lookup"><span data-stu-id="d76fb-112">Gulp</span></span>

<span data-ttu-id="d76fb-113">Gulp 是以 JavaScript 為基礎的資料流建置工具組，用於用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="d76fb-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="d76fb-114">當在建置環境中觸發特定事件時，通常會使用它透過一系列的程序串流用戶端檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="d76fb-115">比方說，Gulp 可用來自動化[統合和縮製](bundling-and-minification.md)或開發環境，新的組建之前的清除。</span><span class="sxs-lookup"><span data-stu-id="d76fb-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleaning of a development environment before a new build.</span></span>

<span data-ttu-id="d76fb-116">在 *gulpfile.js* 中定義一組 Gulp 工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="d76fb-117">下列 JavaScript 包含 Gulp 模組，並指定要在即將到來的工作中引用的文件路徑：</span><span class="sxs-lookup"><span data-stu-id="d76fb-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="d76fb-118">上述程式碼會指定哪些節點模組所需。</span><span class="sxs-lookup"><span data-stu-id="d76fb-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="d76fb-119">`require`函式匯入每個模組，以便讓相依的工作能夠利用其功能。</span><span class="sxs-lookup"><span data-stu-id="d76fb-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="d76fb-120">每個匯入的模組會指派給變數。</span><span class="sxs-lookup"><span data-stu-id="d76fb-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="d76fb-121">模組可以位於依名稱或路徑。</span><span class="sxs-lookup"><span data-stu-id="d76fb-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="d76fb-122">在此範例中，將模組命名為`gulp`， `rimraf`， `gulp-concat`， `gulp-cssmin`，和`gulp-uglify`會依名稱擷取。</span><span class="sxs-lookup"><span data-stu-id="d76fb-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="d76fb-123">此外，以便可以重複使用和工作內參照的 CSS 和 JavaScript 檔案的位置，會建立一系列的路徑。</span><span class="sxs-lookup"><span data-stu-id="d76fb-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="d76fb-124">下表描述中包含的模組*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="d76fb-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="d76fb-125">模組名稱</span><span class="sxs-lookup"><span data-stu-id="d76fb-125">Module Name</span></span> | <span data-ttu-id="d76fb-126">描述</span><span class="sxs-lookup"><span data-stu-id="d76fb-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="d76fb-127">gulp</span><span class="sxs-lookup"><span data-stu-id="d76fb-127">gulp</span></span>        | <span data-ttu-id="d76fb-128">Gulp 串流處理組建系統。</span><span class="sxs-lookup"><span data-stu-id="d76fb-128">The Gulp streaming build system.</span></span> <span data-ttu-id="d76fb-129">如需詳細資訊，請參閱 < [gulp](https://www.npmjs.com/package/gulp)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="d76fb-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="d76fb-130">rimraf</span></span>      | <span data-ttu-id="d76fb-131">節點刪除模組。</span><span class="sxs-lookup"><span data-stu-id="d76fb-131">A Node deletion module.</span></span> <span data-ttu-id="d76fb-132">如需詳細資訊，請參閱 < [rimraf](https://www.npmjs.com/package/rimraf)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="d76fb-133">gulp-concat</span><span class="sxs-lookup"><span data-stu-id="d76fb-133">gulp-concat</span></span> | <span data-ttu-id="d76fb-134">這種模組串連根據作業系統的新行字元的檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="d76fb-135">如需詳細資訊，請參閱 < [gulp concat](https://www.npmjs.com/package/gulp-concat)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="d76fb-136">gulp-cssmin</span><span class="sxs-lookup"><span data-stu-id="d76fb-136">gulp-cssmin</span></span> | <span data-ttu-id="d76fb-137">可將 CSS 檔案最小化的模組。</span><span class="sxs-lookup"><span data-stu-id="d76fb-137">A module that minifies CSS files.</span></span> <span data-ttu-id="d76fb-138">如需詳細資訊，請參閱 < [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="d76fb-139">gulp-uglify</span><span class="sxs-lookup"><span data-stu-id="d76fb-139">gulp-uglify</span></span> | <span data-ttu-id="d76fb-140">縮短模組 *.js*檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="d76fb-141">如需詳細資訊，請參閱 < [gulp uglify](https://www.npmjs.com/package/gulp-uglify)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="d76fb-142">一旦匯入必要模組，您可以指定工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="d76fb-143">這裡有六個工作登錄，以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d76fb-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="d76fb-144">下表提供上述程式碼中指定的工作的說明：</span><span class="sxs-lookup"><span data-stu-id="d76fb-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="d76fb-145">任務名稱</span><span class="sxs-lookup"><span data-stu-id="d76fb-145">Task Name</span></span>|<span data-ttu-id="d76fb-146">描述</span><span class="sxs-lookup"><span data-stu-id="d76fb-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="d76fb-147">clean:js</span><span class="sxs-lookup"><span data-stu-id="d76fb-147">clean:js</span></span>|<span data-ttu-id="d76fb-148">若要移除 site.js 檔案的縮短的版本使用 rimraf 節點刪除模組的工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="d76fb-149">清除： css</span><span class="sxs-lookup"><span data-stu-id="d76fb-149">clean:css</span></span>|<span data-ttu-id="d76fb-150">若要移除 site.css 檔案的縮短的版本使用 rimraf 節點刪除模組的工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="d76fb-151">清除</span><span class="sxs-lookup"><span data-stu-id="d76fb-151">clean</span></span>|<span data-ttu-id="d76fb-152">工作，以呼叫`clean:js`工作，後面接著`clean:css`工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="d76fb-153">min:js</span><span class="sxs-lookup"><span data-stu-id="d76fb-153">min:js</span></span>|<span data-ttu-id="d76fb-154">工作並縮短，串連的 js 資料夾中的所有.js 檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="d76fb-155">。 排除 min.js 檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="d76fb-156">min:css</span><span class="sxs-lookup"><span data-stu-id="d76fb-156">min:css</span></span>|<span data-ttu-id="d76fb-157">縮短，並串連所有的.css 檔案，在 css 資料夾中的工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="d76fb-158">。 排除 min.css 檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="d76fb-159">min</span><span class="sxs-lookup"><span data-stu-id="d76fb-159">min</span></span>|<span data-ttu-id="d76fb-160">工作，以呼叫`min:js`工作，後面接著`min:css`工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="d76fb-161">執行預設工作</span><span class="sxs-lookup"><span data-stu-id="d76fb-161">Running default tasks</span></span>

<span data-ttu-id="d76fb-162">如果您尚未建立新的 Web 應用程式，請在 Visual Studio 中建立新的 ASP.NET Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1. <span data-ttu-id="d76fb-163">開啟*package.json*檔案 (新增如果不存在)，並新增下列。</span><span class="sxs-lookup"><span data-stu-id="d76fb-163">Open the *package.json* file (add if not there) and add the following.</span></span>

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

2. <span data-ttu-id="d76fb-164">將新的 JavaScript 檔案加入至專案並將它命名*gulpfile.js*，然後複製下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="d76fb-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

3. <span data-ttu-id="d76fb-165">在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*，然後選取**Task Runner Explorer**。</span><span class="sxs-lookup"><span data-stu-id="d76fb-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>

    ![從 [方案總管] 中開啟 [Task Runner explorer]](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)

    <span data-ttu-id="d76fb-167">**Task Runner Explorer**顯示 Gulp 工作的清單。</span><span class="sxs-lookup"><span data-stu-id="d76fb-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="d76fb-168">(您可能必須按一下 **重新整理**專案名稱的左邊會出現的按鈕。)</span><span class="sxs-lookup"><span data-stu-id="d76fb-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>

    ![Task Runner explorer](using-gulp/_static/03-TaskRunnerExplorer.png)

    > [!IMPORTANT]
    > <span data-ttu-id="d76fb-170">**Task Runner Explorer**操作功能表項目才會出現*gulpfile.js*專案根目錄中。</span><span class="sxs-lookup"><span data-stu-id="d76fb-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4. <span data-ttu-id="d76fb-171">下面**工作**中**Task Runner Explorer**，以滑鼠右鍵按一下**全新**，然後選取**執行**從快顯功能表。</span><span class="sxs-lookup"><span data-stu-id="d76fb-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![工作執行器總管清除工作](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="d76fb-173">**Task Runner Explorer**會建立名為新的索引標籤**全新**並執行 「 清除 」 工作，因為它定義在*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="d76fb-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5. <span data-ttu-id="d76fb-174">以滑鼠右鍵按一下**全新**工作，然後選取**繫結** > **之前建置**。</span><span class="sxs-lookup"><span data-stu-id="d76fb-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![繫結 BeforeBuild task Runner explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="d76fb-176">**再建置**繫結會設定專案的每個組建之前自動執行 「 清除 」 工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="d76fb-177">繫結您設有**Task Runner Explorer**頂端的註解的形式儲存您*gulpfile.js 中*和只適用於 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="d76fb-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="d76fb-178">不需要 Visual Studio 的替代方式是設定自動執行 gulp 工作，在您 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="d76fb-179">例如，將它放在您 *.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="d76fb-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="d76fb-180">現在當您執行專案時 Visual Studio 中，或從命令提示字元使用，「 清除 」 工作會執行[執行的 dotnet](/dotnet/core/tools/dotnet-run)命令 (執行`npm install`第一次)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="d76fb-181">定義和執行新工作</span><span class="sxs-lookup"><span data-stu-id="d76fb-181">Defining and running a new task</span></span>

<span data-ttu-id="d76fb-182">若要定義新的 Gulp 工作，修改*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="d76fb-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1. <span data-ttu-id="d76fb-183">結尾新增下列 JavaScript *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="d76fb-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    <span data-ttu-id="d76fb-184">這項工作中名為`first`，和它就會顯示為字串。</span><span class="sxs-lookup"><span data-stu-id="d76fb-184">This task is named `first`, and it simply displays a string.</span></span>

2. <span data-ttu-id="d76fb-185">儲存*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="d76fb-185">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="d76fb-186">在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*，然後選取*Task Runner Explorer*。</span><span class="sxs-lookup"><span data-stu-id="d76fb-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4. <span data-ttu-id="d76fb-187">在**Task Runner Explorer**，以滑鼠右鍵按一下**第一次**，然後選取**執行**。</span><span class="sxs-lookup"><span data-stu-id="d76fb-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![執行第一項工作的工作執行器總管](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="d76fb-189">會顯示輸出文字。</span><span class="sxs-lookup"><span data-stu-id="d76fb-189">The output text is displayed.</span></span> <span data-ttu-id="d76fb-190">若要查看根據常見案例的範例，請參閱[Gulp 配方](#gulp-recipes)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="d76fb-191">定義及執行一連串的工作</span><span class="sxs-lookup"><span data-stu-id="d76fb-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="d76fb-192">當您執行多個工作時，工作同時執行的預設值。</span><span class="sxs-lookup"><span data-stu-id="d76fb-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="d76fb-193">不過，如果您需要以特定順序執行工作，您必須指定當每個工作完成時，也為哪些工作相依於另一項工作完成。</span><span class="sxs-lookup"><span data-stu-id="d76fb-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1. <span data-ttu-id="d76fb-194">若要定義一系列的工作順序執行，取代`first`中新增上述的工作*gulpfile.js*取代下列項目：</span><span class="sxs-lookup"><span data-stu-id="d76fb-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

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

    <span data-ttu-id="d76fb-195">您現在有三項工作： `series:first`， `series:second`，和`series`。</span><span class="sxs-lookup"><span data-stu-id="d76fb-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="d76fb-196">`series:second` 」 工作包含第二個參數，用以指定工作要執行和之前完成陣列`series:second`工作將會執行。</span><span class="sxs-lookup"><span data-stu-id="d76fb-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="d76fb-197">在上方，唯一的程式碼中所指定`series:first`前，必須完成工作`series:second`工作將會執行。</span><span class="sxs-lookup"><span data-stu-id="d76fb-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2. <span data-ttu-id="d76fb-198">儲存*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="d76fb-198">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="d76fb-199">中**方案總管**，以滑鼠右鍵按一下*gulpfile.js* ，然後選取**Task Runner Explorer**如果尚未開啟。</span><span class="sxs-lookup"><span data-stu-id="d76fb-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4. <span data-ttu-id="d76fb-200">在  **Task Runner Explorer**，以滑鼠右鍵按一下**系列**，然後選取**執行**。</span><span class="sxs-lookup"><span data-stu-id="d76fb-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Task Runner explorer，執行序列工作](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="d76fb-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="d76fb-202">IntelliSense</span></span>

<span data-ttu-id="d76fb-203">IntelliSense 會提供程式碼完成、 參數說明和其他功能，大幅提升生產力，並減少錯誤。</span><span class="sxs-lookup"><span data-stu-id="d76fb-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="d76fb-204">Gulp 工作會以 JavaScript;因此，IntelliSense 可提供開發時的協助。</span><span class="sxs-lookup"><span data-stu-id="d76fb-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="d76fb-205">當您使用 JavaScript 時，IntelliSense 會列出物件、 函式、 屬性和參數，可根據您目前的內容。</span><span class="sxs-lookup"><span data-stu-id="d76fb-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="d76fb-206">從提供 IntelliSense 完成程式碼的快顯清單中選取程式碼撰寫的選項。</span><span class="sxs-lookup"><span data-stu-id="d76fb-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="d76fb-208">如需有關 IntelliSense 的詳細資訊，請參閱 < [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="d76fb-209">開發、 預備及生產環境</span><span class="sxs-lookup"><span data-stu-id="d76fb-209">Development, staging, and production environments</span></span>

<span data-ttu-id="d76fb-210">當 Gulp 用來最佳化預備和生產環境的用戶端檔案時，處理的檔案會儲存到本機預備與生產位置。</span><span class="sxs-lookup"><span data-stu-id="d76fb-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="d76fb-211">*_Layout.cshtml*檔案使用**環境**標記協助程式提供 CSS 檔案的兩個不同版本。</span><span class="sxs-lookup"><span data-stu-id="d76fb-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="d76fb-212">一個 CSS 檔案版本適用於開發，而另一個版本已同時針對預備與生產環境最佳化。</span><span class="sxs-lookup"><span data-stu-id="d76fb-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="d76fb-213">在 Visual Studio 2017 中，當您將 **ASPNETCORE_ENVIRONMENT** 環境變數變更為 `Production` 時，Visual Studio 會建置 Web 應用程式並連結到最小化的 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="d76fb-214">下列標記顯示**環境**標記協助程式，此協助程式包 `Development` CSS 檔案與最小化之 `Staging, Production` CSS 檔案的含連結標記。</span><span class="sxs-lookup"><span data-stu-id="d76fb-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="d76fb-215">切換環境</span><span class="sxs-lookup"><span data-stu-id="d76fb-215">Switching between environments</span></span>

<span data-ttu-id="d76fb-216">若要切換為不同的環境中編譯，修改**ASPNETCORE_ENVIRONMENT**環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="d76fb-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1. <span data-ttu-id="d76fb-217">在**Task Runner Explorer**，確認**min**已設為執行的工作**之前建置**。</span><span class="sxs-lookup"><span data-stu-id="d76fb-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2. <span data-ttu-id="d76fb-218">在 **方案總管**，以滑鼠右鍵按一下專案名稱，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="d76fb-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="d76fb-219">Web 應用程式的屬性工作表會顯示。</span><span class="sxs-lookup"><span data-stu-id="d76fb-219">The property sheet for the Web app is displayed.</span></span>

3. <span data-ttu-id="d76fb-220">按一下 [偵錯] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d76fb-220">Click the **Debug** tab.</span></span>

4. <span data-ttu-id="d76fb-221">設定的值**主控： 環境**環境變數，以`Production`。</span><span class="sxs-lookup"><span data-stu-id="d76fb-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5. <span data-ttu-id="d76fb-222">按下**F5**瀏覽器中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d76fb-222">Press **F5** to run the application in a browser.</span></span>

6. <span data-ttu-id="d76fb-223">在瀏覽器視窗中，以滑鼠右鍵按一下  頁面上，然後選取**檢視原始檔**來檢視 HTML 頁面。</span><span class="sxs-lookup"><span data-stu-id="d76fb-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="d76fb-224">請注意，樣式表連結指向最小化的 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7. <span data-ttu-id="d76fb-225">關閉瀏覽器以停止 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d76fb-225">Close the browser to stop the Web app.</span></span>

8. <span data-ttu-id="d76fb-226">在 Visual Studio 中，返回 Web 應用程式的屬性工作表，並將變更**主控： 環境**環境變數將會回到`Development`。</span><span class="sxs-lookup"><span data-stu-id="d76fb-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9. <span data-ttu-id="d76fb-227">按下**F5**在瀏覽器中再次執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d76fb-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="d76fb-228">在瀏覽器視窗中，以滑鼠右鍵按一下  頁面上，然後選取**檢視原始檔**以查看頁面的 HTML。</span><span class="sxs-lookup"><span data-stu-id="d76fb-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="d76fb-229">請注意，樣式表連結指向 unminified CSS 檔案的版本。</span><span class="sxs-lookup"><span data-stu-id="d76fb-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="d76fb-230">與 ASP.NET Core 中的環境相關的詳細資訊，請參閱[使用多個環境](../fundamentals/environments.md)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="d76fb-231">工作和模組的詳細資料</span><span class="sxs-lookup"><span data-stu-id="d76fb-231">Task and module details</span></span>

<span data-ttu-id="d76fb-232">Gulp 工作會向函式名稱。</span><span class="sxs-lookup"><span data-stu-id="d76fb-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="d76fb-233">如果其他工作必須執行目前的工作之前，您可以指定相依性。</span><span class="sxs-lookup"><span data-stu-id="d76fb-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="d76fb-234">其他函數可讓您執行和監看的 Gulp 工作，以及將來源設定 (*src*) 和目的地 (*dest*) 所修改的檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="d76fb-235">以下是主要的 Gulp API 函式：</span><span class="sxs-lookup"><span data-stu-id="d76fb-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="d76fb-236">Gulp 函式</span><span class="sxs-lookup"><span data-stu-id="d76fb-236">Gulp Function</span></span>|<span data-ttu-id="d76fb-237">語法</span><span class="sxs-lookup"><span data-stu-id="d76fb-237">Syntax</span></span>|<span data-ttu-id="d76fb-238">描述</span><span class="sxs-lookup"><span data-stu-id="d76fb-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="d76fb-239">task</span><span class="sxs-lookup"><span data-stu-id="d76fb-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="d76fb-240">`task`函式會建立一項工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-240">The `task` function creates a task.</span></span> <span data-ttu-id="d76fb-241">`name`參數會定義工作的名稱。</span><span class="sxs-lookup"><span data-stu-id="d76fb-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="d76fb-242">`deps`參數會包含這項工作執行前已完成工作的陣列。</span><span class="sxs-lookup"><span data-stu-id="d76fb-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="d76fb-243">`fn`參數代表的回呼函式執行工作的作業。</span><span class="sxs-lookup"><span data-stu-id="d76fb-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="d76fb-244">監看式</span><span class="sxs-lookup"><span data-stu-id="d76fb-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="d76fb-245">`watch`檔案變更時，函式會監視檔案和執行工作。</span><span class="sxs-lookup"><span data-stu-id="d76fb-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="d76fb-246">`glob`參數是`string`或`array`，決定要監看的檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="d76fb-247">`opts`參數會提供額外的檔案監看的選項。</span><span class="sxs-lookup"><span data-stu-id="d76fb-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="d76fb-248">src</span><span class="sxs-lookup"><span data-stu-id="d76fb-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="d76fb-249">`src`函式會提供符合 glob 值的檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="d76fb-250">`glob`參數是`string`或`array`，決定要讀取的檔案。</span><span class="sxs-lookup"><span data-stu-id="d76fb-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="d76fb-251">`options`參數提供的其他檔案選項。</span><span class="sxs-lookup"><span data-stu-id="d76fb-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="d76fb-252">目的地</span><span class="sxs-lookup"><span data-stu-id="d76fb-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="d76fb-253">`dest`函式會定義可以寫入檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="d76fb-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="d76fb-254">`path`參數是字串或函式，判斷目的地資料夾。</span><span class="sxs-lookup"><span data-stu-id="d76fb-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="d76fb-255">`options`參數是物件，指定輸出資料夾的選項。</span><span class="sxs-lookup"><span data-stu-id="d76fb-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="d76fb-256">如需其他的 Gulp API 參考資訊，請參閱[Gulp Docs API](https://gulpjs.org/API.html)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-256">For additional Gulp API reference information, see [Gulp Docs API](https://gulpjs.org/API.html).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="d76fb-257">Gulp 配方</span><span class="sxs-lookup"><span data-stu-id="d76fb-257">Gulp recipes</span></span>

<span data-ttu-id="d76fb-258">Gulp 社群也提供 Gulp[配方](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)。</span><span class="sxs-lookup"><span data-stu-id="d76fb-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="d76fb-259">這些配方所組成 Gulp 工作，以解決常見的案例。</span><span class="sxs-lookup"><span data-stu-id="d76fb-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d76fb-260">其他資源</span><span class="sxs-lookup"><span data-stu-id="d76fb-260">Additional resources</span></span>

* [<span data-ttu-id="d76fb-261">Gulp 文件</span><span class="sxs-lookup"><span data-stu-id="d76fb-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="d76fb-262">在 ASP.NET Core 中包裝及最小化</span><span class="sxs-lookup"><span data-stu-id="d76fb-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="d76fb-263">在 ASP.NET Core 中使用 Grunt</span><span class="sxs-lookup"><span data-stu-id="d76fb-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
