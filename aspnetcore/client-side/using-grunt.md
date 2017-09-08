---
title: "使用 ASP.NET Core Grunt"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 471112e9-2c33-454b-96fc-32916102ce73
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-grunt
ms.openlocfilehash: df20c3a31fce81ab039ef2f63bf38ed9943c7c6c
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="using-grunt-in-aspnet-core"></a><span data-ttu-id="fcd36-103">使用 ASP.NET Core Grunt</span><span class="sxs-lookup"><span data-stu-id="fcd36-103">Using Grunt in ASP.NET Core</span></span> 

<span data-ttu-id="fcd36-104">由[Noel Rice](http://blog.falafel.com/author/noel-rice/)</span><span class="sxs-lookup"><span data-stu-id="fcd36-104">By [Noel Rice](http://blog.falafel.com/author/noel-rice/)</span></span>

<span data-ttu-id="fcd36-105">Grunt 是會自動將指令碼縮製、 TypeScript 編譯、 程式碼品質 「 不 」 工具，CSS 前處理器，以及幾乎任何重複的工程又需要執行以支援用戶端開發 JavaScript 工作執行器。</span><span class="sxs-lookup"><span data-stu-id="fcd36-105">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="fcd36-106">雖然 ASP.NET 專案範本預設會使用 Gulp grunt 完全支援 Visual Studio 中，(請參閱[使用 Gulp](using-gulp.md))。</span><span class="sxs-lookup"><span data-stu-id="fcd36-106">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Using Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="fcd36-107">這個範例會使用空的 ASP.NET Core 專案做為起點，顯示如何從可用的用戶端建置程序自動化。</span><span class="sxs-lookup"><span data-stu-id="fcd36-107">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="fcd36-108">完成的範例會清除目標部署目錄、 結合 JavaScript 檔案、 檢查程式碼品質、 總是 JavaScript 檔案內容和部署到您的 web 應用程式的根目錄。</span><span class="sxs-lookup"><span data-stu-id="fcd36-108">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="fcd36-109">我們將使用下列套件：</span><span class="sxs-lookup"><span data-stu-id="fcd36-109">We will use the following packages:</span></span>

* <span data-ttu-id="fcd36-110">**grunt**: grunt 所完成的工作執行器套件。</span><span class="sxs-lookup"><span data-stu-id="fcd36-110">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="fcd36-111">**grunt contrib 清除**： 移除檔案或目錄的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="fcd36-111">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="fcd36-112">**grunt-contrib-jshint**： 檢閱 JavaScript 程式碼品質的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="fcd36-112">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="fcd36-113">**grunt-contrib-concat**： 外掛程式聯結成單一檔案的檔案。</span><span class="sxs-lookup"><span data-stu-id="fcd36-113">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="fcd36-114">**grunt contrib uglify**： 縮短 JavaScript，以減少大小的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="fcd36-114">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="fcd36-115">**監看 contrib grunt 式**： 監看檔案 」 活動的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="fcd36-115">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="fcd36-116">準備應用程式</span><span class="sxs-lookup"><span data-stu-id="fcd36-116">Preparing the application</span></span>

<span data-ttu-id="fcd36-117">若要開始，設定新的空白 web 應用程式並新增 TypeScript 範例檔案。</span><span class="sxs-lookup"><span data-stu-id="fcd36-117">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="fcd36-118">TypeScript 檔案會自動編譯成 JavaScript 使用 Visual Studio 的預設設定，且將我們原料處理使用 grunt 所完成。</span><span class="sxs-lookup"><span data-stu-id="fcd36-118">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1.  <span data-ttu-id="fcd36-119">在 Visual Studio 中，建立新`ASP.NET Web Application`。</span><span class="sxs-lookup"><span data-stu-id="fcd36-119">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2.  <span data-ttu-id="fcd36-120">在**新增 ASP.NET 專案**] 對話方塊中，選取 [ASP.NET Core**空**範本，然後按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fcd36-120">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3.  <span data-ttu-id="fcd36-121">在 [方案總管] 中，檢閱專案結構。</span><span class="sxs-lookup"><span data-stu-id="fcd36-121">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="fcd36-122">`\src`資料夾包含空白`wwwroot`和`Dependencies`節點。</span><span class="sxs-lookup"><span data-stu-id="fcd36-122">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![空的 web 方案](using-grunt/_static/grunt-solution-explorer.png)

4.  <span data-ttu-id="fcd36-124">加入新的資料夾，名為`TypeScript`至專案目錄。</span><span class="sxs-lookup"><span data-stu-id="fcd36-124">Add a new folder named `TypeScript` to your project directory.</span></span>

5.  <span data-ttu-id="fcd36-125">加入之前的任何檔案，請確定 Visual Studio 的選項 '編譯儲存' 的簽入 TypeScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="fcd36-125">Before adding any files, let’s make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="fcd36-126">*工具 > 選項 > 文字編輯器 > Typescript > 專案*</span><span class="sxs-lookup"><span data-stu-id="fcd36-126">*Tools > Options > Text Editor > Typescript > Project*</span></span>

    ![設定自動 compliation TypeScript 檔案的選項](using-grunt/_static/typescript-options.png)

6.  <span data-ttu-id="fcd36-128">以滑鼠右鍵按一下`TypeScript`目錄中，而且選取**新增 > 新的項目**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="fcd36-128">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="fcd36-129">選取**JavaScript 檔案**項目，並將檔案命名*Tastes.ts* (請注意\*.ts 副檔名)。</span><span class="sxs-lookup"><span data-stu-id="fcd36-129">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="fcd36-130">將下面 TypeScript 程式碼行複製到檔案 (當您儲存時，新*Tastes.js* JavaScript 來源檔案中將會出現)。</span><span class="sxs-lookup"><span data-stu-id="fcd36-130">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  <span data-ttu-id="fcd36-131">加入第二個檔案**TypeScript**目錄並將其命名`Food.ts`。</span><span class="sxs-lookup"><span data-stu-id="fcd36-131">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="fcd36-132">將下列程式碼複製到檔案。</span><span class="sxs-lookup"><span data-stu-id="fcd36-132">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="fcd36-133">設定 NPM</span><span class="sxs-lookup"><span data-stu-id="fcd36-133">Configuring NPM</span></span>

<span data-ttu-id="fcd36-134">接著，設定 NPM 以便下載 grunt grunt 所完成工作。</span><span class="sxs-lookup"><span data-stu-id="fcd36-134">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="fcd36-135">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增 > 新的項目**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="fcd36-135">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="fcd36-136">選取**NPM 組態檔**項目，保留預設名稱， *package.json*，然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fcd36-136">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="fcd36-137">在*package.json*內部檔案`devDependencies`物件大括號中，輸入 「 grunt"。</span><span class="sxs-lookup"><span data-stu-id="fcd36-137">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="fcd36-138">選取`grunt`從 Intellisense 清單，然後按 Enter 鍵。</span><span class="sxs-lookup"><span data-stu-id="fcd36-138">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="fcd36-139">Visual Studio 會加上引號 grunt 所完成的封裝名稱，並加入冒號。</span><span class="sxs-lookup"><span data-stu-id="fcd36-139">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="fcd36-140">右邊的冒號，請從 Intellisense 清單頂端選取封裝的最新穩定版本 (按`Ctrl-Space`如果 Intellisense 不會出現)。</span><span class="sxs-lookup"><span data-stu-id="fcd36-140">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense does not appear).</span></span>

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > <span data-ttu-id="fcd36-142">NPM 使用[語意版本設定](http://semver.org/)將組織的相依性。</span><span class="sxs-lookup"><span data-stu-id="fcd36-142">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="fcd36-143">語意版本設定，也稱為 SemVer 編號方式識別封裝<major>。<minor>。<patch>.Intellisense 會顯示只有幾個常見的選項，以簡化語意版本設定。</span><span class="sxs-lookup"><span data-stu-id="fcd36-143">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme <major>.<minor>.<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="fcd36-144">Intellisense 清單 (在上述範例 0.4.5) 中的最上層項目會被視為封裝的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="fcd36-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="fcd36-145">插入號 (^) 符號符合最新的主要版本和波狀符號 （~） 符合最新的次要版本。</span><span class="sxs-lookup"><span data-stu-id="fcd36-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="fcd36-146">請參閱[NPM semver 版本剖析器參考](https://www.npmjs.com/package/semver)做為 SemVer 提供的完整表現的指南。</span><span class="sxs-lookup"><span data-stu-id="fcd36-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="fcd36-147">新增更多的相依性，以便載入 grunt-contrib-\*封裝的*全新*， *jshint*， *concat*， *uglify*，和*監看式*如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="fcd36-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="fcd36-148">不需要符合範例版本。</span><span class="sxs-lookup"><span data-stu-id="fcd36-148">The versions do not need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="fcd36-149">儲存*package.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="fcd36-149">Save the *package.json* file.</span></span>

<span data-ttu-id="fcd36-150">將下載之套件的每個 devDependencies 項目，以及每個封裝所需的任何檔案。</span><span class="sxs-lookup"><span data-stu-id="fcd36-150">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="fcd36-151">您可以找到套件檔案中的`node_modules`目錄啟用**顯示所有檔案**[方案總管] 中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="fcd36-151">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="fcd36-153">如果需要您可以以滑鼠右鍵按一下，以手動方式還原方案總管 中的相依性`Dependencies\NPM`，然後選取**還原封裝**功能表選項。</span><span class="sxs-lookup"><span data-stu-id="fcd36-153">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![還原封裝](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="fcd36-155">設定 Grunt</span><span class="sxs-lookup"><span data-stu-id="fcd36-155">Configuring Grunt</span></span>

<span data-ttu-id="fcd36-156">Grunt 已設定為使用名為資訊清單*Gruntfile.js*定義、 載入和註冊工作，可以手動執行或設定為基礎而執行自動 Visual Studio 中的事件。</span><span class="sxs-lookup"><span data-stu-id="fcd36-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1.  <span data-ttu-id="fcd36-157">以滑鼠右鍵按一下專案，然後選取**新增 > 新的項目**。</span><span class="sxs-lookup"><span data-stu-id="fcd36-157">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="fcd36-158">選取**Grunt 組態檔**選項，請保留預設名稱， *Gruntfile.js*，然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fcd36-158">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

    <span data-ttu-id="fcd36-159">初始程式碼包含模組定義和`grunt.initConfig()`方法。</span><span class="sxs-lookup"><span data-stu-id="fcd36-159">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="fcd36-160">`initConfig()`用來設定每一個封裝的選項和模組的其餘部分將會載入並註冊工作。</span><span class="sxs-lookup"><span data-stu-id="fcd36-160">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. <span data-ttu-id="fcd36-161">內部`initConfig()`方法，加入選項`clean`工作範例所示*Gruntfile.js*下方。</span><span class="sxs-lookup"><span data-stu-id="fcd36-161">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="fcd36-162">「 清除 」 工作會接受目錄字串的陣列。</span><span class="sxs-lookup"><span data-stu-id="fcd36-162">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="fcd36-163">此工作從 wwwroot/程式庫移除檔案，並移除整個/暫存目錄。</span><span class="sxs-lookup"><span data-stu-id="fcd36-163">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="fcd36-164">以下 initConfig() 方法中，將呼叫加入`grunt.loadNpmTasks()`。</span><span class="sxs-lookup"><span data-stu-id="fcd36-164">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="fcd36-165">這會讓工作可從 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="fcd36-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="fcd36-166">儲存*Gruntfile.js*。</span><span class="sxs-lookup"><span data-stu-id="fcd36-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="fcd36-167">檔案應看起來像下列螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="fcd36-167">The file should look something like the screenshot below.</span></span>

    ![初始 gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="fcd36-169">以滑鼠右鍵按一下*Gruntfile.js*選取**工作執行器總管**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="fcd36-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="fcd36-170">工作執行器總管視窗隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="fcd36-170">The Task Runner Explorer window will open.</span></span>

    ![工作執行器總管功能表](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="fcd36-172">確認`clean`會顯示在下方**工作**工作執行器總管中。</span><span class="sxs-lookup"><span data-stu-id="fcd36-172">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![工作執行器總管工作清單](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="fcd36-174">「 清除 」 工作上按一下滑鼠右鍵，然後選取**執行**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="fcd36-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="fcd36-175">命令視窗會顯示工作的進度。</span><span class="sxs-lookup"><span data-stu-id="fcd36-175">A command window displays progress of the task.</span></span>

    ![工作執行器總管 中執行清理工作](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > <span data-ttu-id="fcd36-177">沒有任何檔案或目錄尚未清除。</span><span class="sxs-lookup"><span data-stu-id="fcd36-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="fcd36-178">如有需要，您可以在 [方案總管] 中手動建立它們，並再做為測試執行 「 清除 」 工作。</span><span class="sxs-lookup"><span data-stu-id="fcd36-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>
    
8. <span data-ttu-id="fcd36-179">在 initConfig() 方法中，新增的項目`concat`使用下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="fcd36-179">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="fcd36-180">`src`屬性陣列會列出檔案，以結合，它們應該要結合的順序。</span><span class="sxs-lookup"><span data-stu-id="fcd36-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="fcd36-181">`dest`屬性指派給所產生的合併檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="fcd36-181">The `dest` property assigns the path to the combined file that is produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > <span data-ttu-id="fcd36-182">`all`上述程式碼中的屬性為目標的名稱。</span><span class="sxs-lookup"><span data-stu-id="fcd36-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="fcd36-183">目標用於某些 grunt 所完成的工作，以允許多個組建環境。</span><span class="sxs-lookup"><span data-stu-id="fcd36-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="fcd36-184">您可以檢視的內建的目標，使用 Intellisense，或指派自己。</span><span class="sxs-lookup"><span data-stu-id="fcd36-184">You can view the built-in targets using Intellisense or assign your own.</span></span>
    
9. <span data-ttu-id="fcd36-185">新增`jshint`工作使用下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="fcd36-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="fcd36-186">針對每個找到暫存目錄中的 JavaScript 檔案，執行 jshint 程式碼品質公用程式。</span><span class="sxs-lookup"><span data-stu-id="fcd36-186">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="fcd36-187">選項"-W069 」 錯誤所產生 jshint JavaScript 使用括號語法來指定的屬性，而不是點標記法，也就是當`Tastes["Sweet"]`而不是`Tastes.Sweet`。</span><span class="sxs-lookup"><span data-stu-id="fcd36-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="fcd36-188">選項會關閉警告，以允許其他處理序繼續進行。</span><span class="sxs-lookup"><span data-stu-id="fcd36-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

10.  <span data-ttu-id="fcd36-189">新增`uglify`工作使用下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="fcd36-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="fcd36-190">工作縮短*combined.js*檔案找到暫存目錄中，並會將結果檔案建立 wwwroot/lib 遵循標準命名慣例*\<檔案名稱\>。 min.js*.</span><span class="sxs-lookup"><span data-stu-id="fcd36-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. <span data-ttu-id="fcd36-191">在載入 grunt contrib 清除呼叫 grunt.loadNpmTasks()，包含相同的呼叫，如 jshint，concat，uglify 使用下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="fcd36-191">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="fcd36-192">儲存*Gruntfile.js*。</span><span class="sxs-lookup"><span data-stu-id="fcd36-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="fcd36-193">檔案看起來應該像下面範例。</span><span class="sxs-lookup"><span data-stu-id="fcd36-193">The file should look something like the example below.</span></span>

    ![完成 grunt 檔案範例](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="fcd36-195">請注意，工作執行器總管工作清單包括`clean`， `concat`，`jshint`和`uglify`工作。</span><span class="sxs-lookup"><span data-stu-id="fcd36-195">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="fcd36-196">執行每項工作順序，並觀察 [方案總管] 中的結果。</span><span class="sxs-lookup"><span data-stu-id="fcd36-196">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="fcd36-197">每個工作應該執行無誤。</span><span class="sxs-lookup"><span data-stu-id="fcd36-197">Each task should run without errors.</span></span>
    
    ![執行每項工作的工作執行器總管](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    <span data-ttu-id="fcd36-199">Concat 工作建立新*combined.js*檔案，並將它放入暫存目錄。</span><span class="sxs-lookup"><span data-stu-id="fcd36-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="fcd36-200">只要 jshint 工作執行，並不會產生輸出。</span><span class="sxs-lookup"><span data-stu-id="fcd36-200">The jshint task simply runs and doesn’t produce output.</span></span> <span data-ttu-id="fcd36-201">Uglify 工作建立新*combined.min.js*檔案，並將它放入 wwwroot/lib。</span><span class="sxs-lookup"><span data-stu-id="fcd36-201">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="fcd36-202">完成時，方案看起來應該類似下面的螢幕擷取畫面：</span><span class="sxs-lookup"><span data-stu-id="fcd36-202">On completion, the solution should look something like the screenshot below:</span></span>
    
    ![方案總管 在所有工作](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > <span data-ttu-id="fcd36-204">如需有關每個封裝的選項的詳細資訊，請瀏覽[https://www.npmjs.com/](https://www.npmjs.com/)和查閱的主頁面上的 [搜尋] 方塊中的封裝名稱。</span><span class="sxs-lookup"><span data-stu-id="fcd36-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="fcd36-205">例如，您可以查閱 grunt contrib 清除封裝，以取得文件的連結，說明及其所有的參數。</span><span class="sxs-lookup"><span data-stu-id="fcd36-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="fcd36-206">一堂</span><span class="sxs-lookup"><span data-stu-id="fcd36-206">All together now</span></span>

<span data-ttu-id="fcd36-207">使用 Grunt`registerTask()`方法，以特定順序執行一系列的工作。</span><span class="sxs-lookup"><span data-stu-id="fcd36-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="fcd36-208">例如，若要執行上述步驟，在初始狀態的順序]-> [範例 concat]-> [jshint]-> [uglify、 將下列程式碼加入至模組。</span><span class="sxs-lookup"><span data-stu-id="fcd36-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="fcd36-209">程式碼應加入至外部 initConfig loadNpmTasks() 呼叫，相同的層級。</span><span class="sxs-lookup"><span data-stu-id="fcd36-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="fcd36-210">別名 [工作] 下的工作執行器總管中出現新的工作。</span><span class="sxs-lookup"><span data-stu-id="fcd36-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="fcd36-211">您可以以滑鼠右鍵按一下，然後執行就如同其他工作。</span><span class="sxs-lookup"><span data-stu-id="fcd36-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="fcd36-212">`all`工作將會執行`clean`， `concat`，`jshint`和`uglify`，順序。</span><span class="sxs-lookup"><span data-stu-id="fcd36-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![別名 grunt 所完成的工作](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="fcd36-214">監看的變更</span><span class="sxs-lookup"><span data-stu-id="fcd36-214">Watching for changes</span></span>

<span data-ttu-id="fcd36-215">A`watch`工作會監看檔案及目錄上的。</span><span class="sxs-lookup"><span data-stu-id="fcd36-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="fcd36-216">監看式自動觸發工作，如果它偵測到變更。</span><span class="sxs-lookup"><span data-stu-id="fcd36-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="fcd36-217">下列程式碼加入監看的變更 initConfig \*TypeScript 目錄中的.js 檔案。</span><span class="sxs-lookup"><span data-stu-id="fcd36-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="fcd36-218">如果變更的 JavaScript 檔案，`watch`將執行`all`工作。</span><span class="sxs-lookup"><span data-stu-id="fcd36-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="fcd36-219">將呼叫加入`loadNpmTasks()`顯示`watch`工作執行器總管中的工作。</span><span class="sxs-lookup"><span data-stu-id="fcd36-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="fcd36-220">以滑鼠右鍵按一下 監看式中的工作工作執行器總管，並從操作功能表選取 執行。</span><span class="sxs-lookup"><span data-stu-id="fcd36-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="fcd36-221">顯示 監看式工作執行此命令視窗會顯示 等候</span><span class="sxs-lookup"><span data-stu-id="fcd36-221">The command window that shows the watch task running will display a "Waiting…"</span></span> <span data-ttu-id="fcd36-222">訊息。</span><span class="sxs-lookup"><span data-stu-id="fcd36-222">message.</span></span> <span data-ttu-id="fcd36-223">開啟其中一個 TypeScript 檔案，加入空格，並儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="fcd36-223">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="fcd36-224">這將會觸發監看式工作，並觸發其他工作，以執行順序。</span><span class="sxs-lookup"><span data-stu-id="fcd36-224">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="fcd36-225">下列螢幕擷取畫面顯示範例執行。</span><span class="sxs-lookup"><span data-stu-id="fcd36-225">The screenshot below shows a sample run.</span></span>

![執行工作的輸出](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="fcd36-227">繫結至 Visual Studio 事件</span><span class="sxs-lookup"><span data-stu-id="fcd36-227">Binding to Visual Studio events</span></span>

<span data-ttu-id="fcd36-228">除非您想要以手動方式啟動您的工作，每次您使用 Visual Studio 中，您可以繫結工作**之前建置**，**後建置**，**清除**，和**專案開啟**事件。</span><span class="sxs-lookup"><span data-stu-id="fcd36-228">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="fcd36-229">讓我們來繫結`watch`使其執行每次開啟 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="fcd36-229">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="fcd36-230">在工作執行器總管中，以滑鼠右鍵按一下 監看式工作，然後選取**繫結 > 開啟專案**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="fcd36-230">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![繫結至專案開啟的工作](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="fcd36-232">卸載並重新載入專案。</span><span class="sxs-lookup"><span data-stu-id="fcd36-232">Unload and reload the project.</span></span> <span data-ttu-id="fcd36-233">當專案重新載入時，監看式工作會開始自動執行。</span><span class="sxs-lookup"><span data-stu-id="fcd36-233">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="fcd36-234">總結</span><span class="sxs-lookup"><span data-stu-id="fcd36-234">Summary</span></span>

<span data-ttu-id="fcd36-235">Grunt 是功能強大的工作執行器，可以讓大部分的用戶端組建工作自動執行。</span><span class="sxs-lookup"><span data-stu-id="fcd36-235">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="fcd36-236">Grunt 會利用 NPM 傳遞其封裝和工具與 Visual Studio 整合的功能。</span><span class="sxs-lookup"><span data-stu-id="fcd36-236">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="fcd36-237">Visual Studio 工作執行器總管會偵測到組態檔變更，並提供方便的介面來執行工作、 檢視正在執行的工作，以及將工作繫結至 Visual Studio 事件。</span><span class="sxs-lookup"><span data-stu-id="fcd36-237">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fcd36-238">其他資源</span><span class="sxs-lookup"><span data-stu-id="fcd36-238">Additional resources</span></span>

   * [<span data-ttu-id="fcd36-239">使用 Gulp</span><span class="sxs-lookup"><span data-stu-id="fcd36-239">Using Gulp</span></span>](using-gulp.md)
