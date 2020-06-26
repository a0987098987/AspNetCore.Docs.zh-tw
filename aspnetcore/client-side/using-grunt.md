---
title: 在 ASP.NET Core 中使用 Grunt
author: rick-anderson
description: 在 ASP.NET Core 中使用 Grunt
ms.author: riande
ms.date: 12/05/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: client-side/using-grunt
ms.openlocfilehash: fc871e22f9bd5a9c137008f1d87019542c45b5d2
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85401762"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="371d0-103">在 ASP.NET Core 中使用 Grunt</span><span class="sxs-lookup"><span data-stu-id="371d0-103">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="371d0-104">Grunt 是一項 JavaScript 工作執行程式，可將腳本縮制、TypeScript 編譯、程式碼品質「不起毛」的工具、CSS 前置處理器，以及任何需要執行以支援用戶端開發的重複性工作自動化。</span><span class="sxs-lookup"><span data-stu-id="371d0-104">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="371d0-105">Visual Studio 中完全支援 Grunt。</span><span class="sxs-lookup"><span data-stu-id="371d0-105">Grunt is fully supported in Visual Studio.</span></span>

<span data-ttu-id="371d0-106">這個範例會使用空的 ASP.NET Core 專案做為其起點，以示範如何從頭開始自動執行用戶端組建程式。</span><span class="sxs-lookup"><span data-stu-id="371d0-106">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="371d0-107">完成後的範例會清除目標部署目錄、結合 JavaScript 檔案、檢查程式碼品質、總是 JavaScript 檔案內容，並部署至 web 應用程式的根目錄。</span><span class="sxs-lookup"><span data-stu-id="371d0-107">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="371d0-108">我們將使用下列套件：</span><span class="sxs-lookup"><span data-stu-id="371d0-108">We will use the following packages:</span></span>

* <span data-ttu-id="371d0-109">**grunt**： grunt 工作執行器套件。</span><span class="sxs-lookup"><span data-stu-id="371d0-109">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="371d0-110">**grunt-contrib-clean**：移除檔案或目錄的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="371d0-110">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="371d0-111">**grunt-contrib-jshint**：用來審查 JavaScript 程式碼品質的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="371d0-111">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="371d0-112">**grunt-contrib-concat**：將檔案加入單一檔案的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="371d0-112">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="371d0-113">**grunt-contrib-uglify**：縮短 JavaScript 以減少大小的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="371d0-113">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="371d0-114">**grunt-contrib-watch：監看**檔案活動的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="371d0-114">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="371d0-115">準備應用程式</span><span class="sxs-lookup"><span data-stu-id="371d0-115">Preparing the application</span></span>

<span data-ttu-id="371d0-116">若要開始，請設定新的空白 web 應用程式，並新增 TypeScript 範例檔案。</span><span class="sxs-lookup"><span data-stu-id="371d0-116">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="371d0-117">TypeScript 檔案會使用預設的 Visual Studio 設定自動編譯成 JavaScript，而我們將會使用 Grunt 來處理我們的原始內容。</span><span class="sxs-lookup"><span data-stu-id="371d0-117">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1. <span data-ttu-id="371d0-118">在 Visual Studio 中，建立新的 `ASP.NET Web Application` 。</span><span class="sxs-lookup"><span data-stu-id="371d0-118">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2. <span data-ttu-id="371d0-119">在 [**新增 ASP.NET 專案**] 對話方塊中，選取 ASP.NET Core**空白**範本，然後按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="371d0-119">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3. <span data-ttu-id="371d0-120">在 [方案總管中，檢查項目結構。</span><span class="sxs-lookup"><span data-stu-id="371d0-120">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="371d0-121">此 `\src` 資料夾包含空的 `wwwroot` 和 `Dependencies` 節點。</span><span class="sxs-lookup"><span data-stu-id="371d0-121">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![空白 web 解決方案](using-grunt/_static/grunt-solution-explorer.png)

4. <span data-ttu-id="371d0-123">將名為的新資料夾新增 `TypeScript` 至您的專案目錄。</span><span class="sxs-lookup"><span data-stu-id="371d0-123">Add a new folder named `TypeScript` to your project directory.</span></span>

5. <span data-ttu-id="371d0-124">在新增任何檔案之前，請確定 Visual Studio 已核取 [在儲存時編譯] 選項。</span><span class="sxs-lookup"><span data-stu-id="371d0-124">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="371d0-125">流覽至 [**工具**] [選項] [  >  **Options**  >  **文字編輯器**] [  >  **Typescript**  >  **專案**]：</span><span class="sxs-lookup"><span data-stu-id="371d0-125">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![設定 TypeScript 檔案自動編譯的選項](using-grunt/_static/typescript-options.png)

6. <span data-ttu-id="371d0-127">在目錄上按一下滑鼠右鍵 `TypeScript` ，然後從內容功能表中選取 [**新增 > 新專案**]。</span><span class="sxs-lookup"><span data-stu-id="371d0-127">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="371d0-128">選取**JavaScript**檔案專案，並將檔案命名為*Tastes* （請注意 \* ts 副檔名）。</span><span class="sxs-lookup"><span data-stu-id="371d0-128">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="371d0-129">將以下的 TypeScript 程式程式碼複製到檔案中（當您儲存時，JavaScript 來源會出現新的*Tastes.js*檔案）。</span><span class="sxs-lookup"><span data-stu-id="371d0-129">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. <span data-ttu-id="371d0-130">將第二個檔案新增至**TypeScript**目錄，並將它命名為 `Food.ts` 。</span><span class="sxs-lookup"><span data-stu-id="371d0-130">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="371d0-131">將下列程式碼複製到檔案中。</span><span class="sxs-lookup"><span data-stu-id="371d0-131">Copy the code below into the file.</span></span>

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

## <a name="configuring-npm"></a><span data-ttu-id="371d0-132">設定 NPM</span><span class="sxs-lookup"><span data-stu-id="371d0-132">Configuring NPM</span></span>

<span data-ttu-id="371d0-133">接下來，設定 NPM 以下載 grunt 和 grunt 工作。</span><span class="sxs-lookup"><span data-stu-id="371d0-133">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="371d0-134">在 [方案總管中，以滑鼠右鍵按一下專案，然後從內容功能表中選取 [**加入 > 新專案**]。</span><span class="sxs-lookup"><span data-stu-id="371d0-134">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="371d0-135">選取 [ **NPM 設定檔**] 專案，保留預設名稱*package.js*[]，然後按一下 [**新增**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="371d0-135">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="371d0-136">在 [ *package.json* ] 檔案的 [ `devDependencies` 物件括弧] 中，輸入 "grunt"。</span><span class="sxs-lookup"><span data-stu-id="371d0-136">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="371d0-137">`grunt`從 Intellisense 清單中選取，然後按 enter 鍵。</span><span class="sxs-lookup"><span data-stu-id="371d0-137">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="371d0-138">Visual Studio 會將 grunt 套件名稱加上引號，並新增一個冒號。</span><span class="sxs-lookup"><span data-stu-id="371d0-138">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="371d0-139">在冒號右邊，從 Intellisense 清單的頂端選取最新穩定版本的封裝（ `Ctrl-Space` 如果 intellisense 未出現，請按下）。</span><span class="sxs-lookup"><span data-stu-id="371d0-139">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > <span data-ttu-id="371d0-141">NPM 會使用[語義版本](https://semver.org/)設定來組織相依性。</span><span class="sxs-lookup"><span data-stu-id="371d0-141">NPM uses [semantic versioning](https://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="371d0-142">語義版本控制（也稱為 SemVer）會使用編號配置來識別 \<major> 封裝 \<minor> ... \<patch>Intellisense 只會顯示一些常用的選項，以簡化語義版本設定。</span><span class="sxs-lookup"><span data-stu-id="371d0-142">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="371d0-143">Intellisense 清單中的最上層專案（在上述範例中為0.4.5）會被視為套件的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="371d0-143">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="371d0-144">插入號（^）符號符合最新的主要版本，而波狀符號（~）符合最新的次要版本。</span><span class="sxs-lookup"><span data-stu-id="371d0-144">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="371d0-145">如需 SemVer 所提供完整表現度的指南，請參閱[NPM semver version parser reference](https://www.npmjs.com/package/semver) 。</span><span class="sxs-lookup"><span data-stu-id="371d0-145">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="371d0-146">新增更多相依性以載入 grunt-contrib- \* 適用于*clean*、 *jshint*、 *concat*、 *uglify*和*watch*的封裝，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="371d0-146">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="371d0-147">版本不需要符合範例。</span><span class="sxs-lookup"><span data-stu-id="371d0-147">The versions don't need to match the example.</span></span>

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

4. <span data-ttu-id="371d0-148">儲存 package.json  檔案。</span><span class="sxs-lookup"><span data-stu-id="371d0-148">Save the *package.json* file.</span></span>

<span data-ttu-id="371d0-149">每個專案的套件 `devDependencies` 將會下載，以及每個套件所需的任何檔案。</span><span class="sxs-lookup"><span data-stu-id="371d0-149">The packages for each `devDependencies` item will download, along with any files that each package requires.</span></span> <span data-ttu-id="371d0-150">您可以在**方案總管**中啟用 [**顯示所有**檔案] 按鈕，以在*node_modules*目錄中尋找封裝檔案。</span><span class="sxs-lookup"><span data-stu-id="371d0-150">You can find the package files in the *node_modules* directory by enabling the **Show All Files** button in **Solution Explorer**.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="371d0-152">如有需要，您可以在**方案總管**中手動還原相依性，方法是以滑鼠右鍵按一下 `Dependencies\NPM` ，然後選取 [**還原封裝**] 功能表選項。</span><span class="sxs-lookup"><span data-stu-id="371d0-152">If you need to, you can manually restore dependencies in **Solution Explorer** by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![還原套件](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="371d0-154">設定 Grunt</span><span class="sxs-lookup"><span data-stu-id="371d0-154">Configuring Grunt</span></span>

<span data-ttu-id="371d0-155">Grunt 是使用名為*Gruntfile.js*的資訊清單來設定，其會定義、載入及註冊可以手動執行或設定為根據 Visual Studio 中的事件自動執行的工作。</span><span class="sxs-lookup"><span data-stu-id="371d0-155">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="371d0-156">以滑鼠右鍵按一下專案，然後選取 [**加入**  >  **新專案**]。</span><span class="sxs-lookup"><span data-stu-id="371d0-156">Right-click the project and select **Add** > **New Item**.</span></span> <span data-ttu-id="371d0-157">選取 [ **JavaScript**檔案專案] 範本，將名稱變更為*Gruntfile.js*，然後按一下 [**新增**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="371d0-157">Select the **JavaScript File** item template, change the name to *Gruntfile.js*, and click the **Add** button.</span></span>

1. <span data-ttu-id="371d0-158">將下列程式碼新增至*Gruntfile.js*。</span><span class="sxs-lookup"><span data-stu-id="371d0-158">Add the following code to *Gruntfile.js*.</span></span> <span data-ttu-id="371d0-159">函式會 `initConfig` 設定每個封裝的選項，而模組的其餘部分則會載入和註冊工作。</span><span class="sxs-lookup"><span data-stu-id="371d0-159">The `initConfig` function sets options for each package, and the remainder of the module loads and register tasks.</span></span>

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. <span data-ttu-id="371d0-160">在函式內 `initConfig` ，新增工作的選項， `clean` 如下列範例所示*Gruntfile.js* 。</span><span class="sxs-lookup"><span data-stu-id="371d0-160">Inside the `initConfig` function, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="371d0-161">此工作會 `clean` 接受目錄字串陣列。</span><span class="sxs-lookup"><span data-stu-id="371d0-161">The `clean` task accepts an array of directory strings.</span></span> <span data-ttu-id="371d0-162">此工作會從*wwwroot/lib*中移除檔案，並移除整個 */temp*目錄。</span><span class="sxs-lookup"><span data-stu-id="371d0-162">This task removes files from *wwwroot/lib* and removes the entire */temp* directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. <span data-ttu-id="371d0-163">在函式底下 `initConfig` ，新增對的呼叫 `grunt.loadNpmTasks` 。</span><span class="sxs-lookup"><span data-stu-id="371d0-163">Below the `initConfig` function, add a call to `grunt.loadNpmTasks`.</span></span> <span data-ttu-id="371d0-164">這會讓工作從 Visual Studio 執行。</span><span class="sxs-lookup"><span data-stu-id="371d0-164">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. <span data-ttu-id="371d0-165">儲存*Gruntfile.js*。</span><span class="sxs-lookup"><span data-stu-id="371d0-165">Save *Gruntfile.js*.</span></span> <span data-ttu-id="371d0-166">檔案看起來應該類似下面的螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="371d0-166">The file should look something like the screenshot below.</span></span>

    ![初始 gruntfile.js](using-grunt/_static/gruntfile-js-initial.png)

1. <span data-ttu-id="371d0-168">以滑鼠右鍵按一下*Gruntfile.js* ，然後從內容功能表中選取 [工作執行**器 Explorer** ]。</span><span class="sxs-lookup"><span data-stu-id="371d0-168">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="371d0-169">[工作執行**器 Explorer** ] 視窗隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="371d0-169">The **Task Runner Explorer** window will open.</span></span>

    ![工作執行器 explorer 功能表](using-grunt/_static/task-runner-explorer-menu.png)

1. <span data-ttu-id="371d0-171">確認 `clean` 顯示在工作**Tasks**執行**器 Explorer**的 [工作] 底下。</span><span class="sxs-lookup"><span data-stu-id="371d0-171">Verify that `clean` shows under **Tasks** in the **Task Runner Explorer**.</span></span>

    ![工作執行器 explorer 工作清單](using-grunt/_static/task-runner-explorer-tasks.png)

1. <span data-ttu-id="371d0-173">以滑鼠右鍵按一下清除工作，然後從內容功能表中選取 [**執行**]。</span><span class="sxs-lookup"><span data-stu-id="371d0-173">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="371d0-174">命令視窗會顯示工作的進度。</span><span class="sxs-lookup"><span data-stu-id="371d0-174">A command window displays progress of the task.</span></span>

    ![工作執行器 explorer 執行清除工作](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > <span data-ttu-id="371d0-176">尚無任何要清除的檔案或目錄。</span><span class="sxs-lookup"><span data-stu-id="371d0-176">There are no files or directories to clean yet.</span></span> <span data-ttu-id="371d0-177">如果您想要的話，可以在方案總管中手動建立它們，然後以測試的形式執行清理工作。</span><span class="sxs-lookup"><span data-stu-id="371d0-177">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>

1. <span data-ttu-id="371d0-178">在函 `initConfig` 式中，使用下列程式碼新增的專案 `concat` 。</span><span class="sxs-lookup"><span data-stu-id="371d0-178">In the `initConfig` function, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="371d0-179">`src`屬性陣列會列出要合併的檔案，以其應合併的順序排列。</span><span class="sxs-lookup"><span data-stu-id="371d0-179">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="371d0-180">`dest`屬性會指派所產生之合併檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="371d0-180">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="371d0-181">`all`上述程式碼中的屬性是目標的名稱。</span><span class="sxs-lookup"><span data-stu-id="371d0-181">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="371d0-182">目標會在某些 Grunt 工作中用來允許多個組建環境。</span><span class="sxs-lookup"><span data-stu-id="371d0-182">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="371d0-183">您可以使用 IntelliSense 來查看內建目標，或指派自己的目標。</span><span class="sxs-lookup"><span data-stu-id="371d0-183">You can view the built-in targets using IntelliSense or assign your own.</span></span>

1. <span data-ttu-id="371d0-184">`jshint`使用下列程式碼新增工作。</span><span class="sxs-lookup"><span data-stu-id="371d0-184">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="371d0-185">Jshint `code-quality` 公用程式會針對在*暫存*目錄中找到的每個 JavaScript 檔案執行。</span><span class="sxs-lookup"><span data-stu-id="371d0-185">The jshint `code-quality` utility is run against every JavaScript file found in the *temp* directory.</span></span>

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="371d0-186">當 JavaScript 使用括弧語法來指派屬性而非點標記法（例如，而不是）時，選項 "-W069" 是 jshint 所產生的錯誤。 `Tastes["Sweet"]` `Tastes.Sweet`</span><span class="sxs-lookup"><span data-stu-id="371d0-186">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="371d0-187">選項會關閉警告，讓程式的其餘部分繼續進行。</span><span class="sxs-lookup"><span data-stu-id="371d0-187">The option turns off the warning to allow the rest of the process to continue.</span></span>

1. <span data-ttu-id="371d0-188">`uglify`使用下列程式碼新增工作。</span><span class="sxs-lookup"><span data-stu-id="371d0-188">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="371d0-189">此工作會縮短在臨時目錄中找到的*combined.js*檔案，並遵循標準命名慣例\* \<file name\>.min.js\*在 wwwroot/lib 中建立結果檔案。</span><span class="sxs-lookup"><span data-stu-id="371d0-189">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. <span data-ttu-id="371d0-190">在該負載的呼叫 `grunt.loadNpmTasks` `grunt-contrib-clean` 底下，請使用下列程式碼，針對 jshint、concat 和 uglify 加入相同的呼叫。</span><span class="sxs-lookup"><span data-stu-id="371d0-190">Under the call to `grunt.loadNpmTasks` that loads `grunt-contrib-clean`, include the same call for jshint, concat, and uglify using the code below.</span></span>

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. <span data-ttu-id="371d0-191">儲存*Gruntfile.js*。</span><span class="sxs-lookup"><span data-stu-id="371d0-191">Save *Gruntfile.js*.</span></span> <span data-ttu-id="371d0-192">檔案看起來應該類似下列範例。</span><span class="sxs-lookup"><span data-stu-id="371d0-192">The file should look something like the example below.</span></span>

    ![完整的 grunt 檔案範例](using-grunt/_static/gruntfile-js-complete.png)

1. <span data-ttu-id="371d0-194">請注意，[工作執行**器**] 的 [工作清單] 會包含 `clean` 、 `concat` 和工作 `jshint` `uglify` 。</span><span class="sxs-lookup"><span data-stu-id="371d0-194">Notice that the **Task Runner Explorer** Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="371d0-195">依序執行每個工作，並觀察**方案總管**中的結果。</span><span class="sxs-lookup"><span data-stu-id="371d0-195">Run each task in order and observe the results in **Solution Explorer**.</span></span> <span data-ttu-id="371d0-196">每項工作都應該執行，而不會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="371d0-196">Each task should run without errors.</span></span>

    ![工作執行器 explorer 執行每項工作](using-grunt/_static/task-runner-explorer-run-each-task.png)

    <span data-ttu-id="371d0-198">Concat 工作會建立新的*combined.js*檔案，並將它放入 temp 目錄中。</span><span class="sxs-lookup"><span data-stu-id="371d0-198">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="371d0-199">此工作 `jshint` 只會執行，而且不會產生輸出。</span><span class="sxs-lookup"><span data-stu-id="371d0-199">The `jshint` task simply runs and doesn't produce output.</span></span> <span data-ttu-id="371d0-200">此工作 `uglify` 會建立新的*combined.min.js*檔案，並將其放入*wwwroot/lib*中。</span><span class="sxs-lookup"><span data-stu-id="371d0-200">The `uglify` task creates a new *combined.min.js* file and places it into *wwwroot/lib*.</span></span> <span data-ttu-id="371d0-201">完成時，解決方案看起來應該像下面的螢幕擷取畫面：</span><span class="sxs-lookup"><span data-stu-id="371d0-201">On completion, the solution should look something like the screenshot below:</span></span>

    ![所有工作之後的方案瀏覽器](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > <span data-ttu-id="371d0-203">如需每個套件選項的詳細資訊，請造訪 [https://www.npmjs.com/](https://www.npmjs.com/) 主頁面上 [搜尋] 方塊中的套件名稱並加以查閱。</span><span class="sxs-lookup"><span data-stu-id="371d0-203">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="371d0-204">例如，您可以查詢 grunt-contrib-clean 封裝，以取得說明其所有參數的檔連結。</span><span class="sxs-lookup"><span data-stu-id="371d0-204">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="371d0-205">齊聚一堂</span><span class="sxs-lookup"><span data-stu-id="371d0-205">All together now</span></span>

<span data-ttu-id="371d0-206">使用 Grunt `registerTask()` 方法，以特定循序執行一系列的工作。</span><span class="sxs-lookup"><span data-stu-id="371d0-206">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="371d0-207">例如，若要執行上述的範例步驟，請 > concat-> jshint-> uglify 中，將下列程式碼新增至模組。</span><span class="sxs-lookup"><span data-stu-id="371d0-207">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="371d0-208">在 initConfig 以外，程式碼應該加入與 loadNpmTasks （）呼叫相同的層級。</span><span class="sxs-lookup"><span data-stu-id="371d0-208">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="371d0-209">新工作會顯示在 [工作執行器] 中的 [別名工作] 底下。</span><span class="sxs-lookup"><span data-stu-id="371d0-209">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="371d0-210">您可以用滑鼠右鍵按一下並執行，就如同其他工作一樣。</span><span class="sxs-lookup"><span data-stu-id="371d0-210">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="371d0-211">工作 `all` 會依 `clean` 序執行 `concat` 、 `jshint` 和 `uglify` 。</span><span class="sxs-lookup"><span data-stu-id="371d0-211">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![別名 grunt 工作](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="371d0-213">監看變更</span><span class="sxs-lookup"><span data-stu-id="371d0-213">Watching for changes</span></span>

<span data-ttu-id="371d0-214">工作 `watch` 可讓您留意檔案和目錄。</span><span class="sxs-lookup"><span data-stu-id="371d0-214">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="371d0-215">[監看式] 會在偵測到變更時自動觸發工作。</span><span class="sxs-lookup"><span data-stu-id="371d0-215">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="371d0-216">將下列程式碼新增至 initConfig，以監看 \* TypeScript 目錄中的 .js 檔案是否有變更。</span><span class="sxs-lookup"><span data-stu-id="371d0-216">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="371d0-217">如果 JavaScript 檔案已變更， `watch` 將會執行工作 `all` 。</span><span class="sxs-lookup"><span data-stu-id="371d0-217">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="371d0-218">新增對的呼叫 `loadNpmTasks()` ，以在 [工作執行 `watch` 器] Explorer 中顯示工作。</span><span class="sxs-lookup"><span data-stu-id="371d0-218">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="371d0-219">以滑鼠右鍵按一下工作執行器 Explorer 中的 [監看式] 工作，然後從內容功能表中選取 [執行]。</span><span class="sxs-lookup"><span data-stu-id="371d0-219">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="371d0-220">顯示執行中監看工作的命令視窗會顯示「正在等候 ...」消息。</span><span class="sxs-lookup"><span data-stu-id="371d0-220">The command window that shows the watch task running will display a "Waiting…" message.</span></span> <span data-ttu-id="371d0-221">開啟其中一個 TypeScript 檔案，加入一個空格，然後儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="371d0-221">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="371d0-222">這會觸發監看式工作，並觸發其他工作依序執行。</span><span class="sxs-lookup"><span data-stu-id="371d0-222">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="371d0-223">下列螢幕擷取畫面顯示範例執行。</span><span class="sxs-lookup"><span data-stu-id="371d0-223">The screenshot below shows a sample run.</span></span>

![執行工作輸出](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="371d0-225">系結至 Visual Studio 事件</span><span class="sxs-lookup"><span data-stu-id="371d0-225">Binding to Visual Studio events</span></span>

<span data-ttu-id="371d0-226">除非您想要在每次使用 Visual Studio 時手動啟動工作，否則請**在組建、\*\*\*\*清除**和**專案開啟**事件**之後**，將工作系結至。</span><span class="sxs-lookup"><span data-stu-id="371d0-226">Unless you want to manually start your tasks every time you work in Visual Studio, bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="371d0-227">[系結]， `watch` 讓它在每次開啟時執行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="371d0-227">Bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="371d0-228">在 [工作執行器] 中，以滑鼠右鍵按一下 [ **Bindings**監看式] 工作，然後  >  從內容功能表中選取 [系**結專案**]。</span><span class="sxs-lookup"><span data-stu-id="371d0-228">In Task Runner Explorer, right-click the watch task and select **Bindings** > **Project Open** from the context menu.</span></span>

![將工作系結至專案開啟](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="371d0-230">卸載並重載專案。</span><span class="sxs-lookup"><span data-stu-id="371d0-230">Unload and reload the project.</span></span> <span data-ttu-id="371d0-231">當專案再次載入時，[監看式] 工作會自動開始執行。</span><span class="sxs-lookup"><span data-stu-id="371d0-231">When the project loads again, the watch task starts running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="371d0-232">摘要</span><span class="sxs-lookup"><span data-stu-id="371d0-232">Summary</span></span>

<span data-ttu-id="371d0-233">Grunt 是功能強大的工作執行器，可以用來將大部分的用戶端組建作業自動化。</span><span class="sxs-lookup"><span data-stu-id="371d0-233">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="371d0-234">Grunt 會利用 NPM 來傳遞其套件，並使用與 Visual Studio 的功能工具整合。</span><span class="sxs-lookup"><span data-stu-id="371d0-234">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="371d0-235">Visual Studio 的工作執行器 Explorer 會偵測設定檔案的變更，並提供便利的介面來執行工作、查看執行中的工作，以及將工作系結至 Visual Studio 的事件。</span><span class="sxs-lookup"><span data-stu-id="371d0-235">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>
