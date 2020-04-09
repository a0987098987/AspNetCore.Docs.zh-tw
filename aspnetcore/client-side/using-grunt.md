---
title: 在ASP.NET核心中使用"格魯特"
author: rick-anderson
description: 在ASP.NET核心中使用"格魯特"
ms.author: riande
ms.date: 12/05/2019
uid: client-side/using-grunt
ms.openlocfilehash: e516b85da7e94d0c93be642086fede0a11fea3c2
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78657588"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="78b7c-103">在ASP.NET核心中使用"格魯特"</span><span class="sxs-lookup"><span data-stu-id="78b7c-103">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="78b7c-104">Grunt 是 JavaScript 工作執行者,自動執行文稿小化、TypeScript 編譯、程式碼品質"lint"工具、CSS 預處理器,以及支援客戶端開發所需的任何重複性工作。</span><span class="sxs-lookup"><span data-stu-id="78b7c-104">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="78b7c-105">在視覺工作室中完全支援 Grunt。</span><span class="sxs-lookup"><span data-stu-id="78b7c-105">Grunt is fully supported in Visual Studio.</span></span>

<span data-ttu-id="78b7c-106">本示例使用空ASP.NET Core 專案作為起點,以演示如何從頭開始自動執行用戶端生成過程。</span><span class="sxs-lookup"><span data-stu-id="78b7c-106">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="78b7c-107">完成的範例清理目標部署目錄,合併 JavaScript 檔,檢查代碼品質,壓縮 JavaScript 檔內容並部署到 Web 應用程式的根目錄。</span><span class="sxs-lookup"><span data-stu-id="78b7c-107">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="78b7c-108">我們使用以下套件:</span><span class="sxs-lookup"><span data-stu-id="78b7c-108">We will use the following packages:</span></span>

* <span data-ttu-id="78b7c-109">**咕咕**:咕咕任務運行程式包。</span><span class="sxs-lookup"><span data-stu-id="78b7c-109">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="78b7c-110">**放大縮小字型功能 放大縮小字型**功能</span><span class="sxs-lookup"><span data-stu-id="78b7c-110">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="78b7c-111">**咕咕-contrib-jshint:** 一個外掛程式,它回顧了JavaScript代碼的品質。</span><span class="sxs-lookup"><span data-stu-id="78b7c-111">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="78b7c-112">**咕咕-contrib-concat:** 將檔案合併到單個檔案中的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="78b7c-112">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="78b7c-113">**咕咕-連續-醜陋**:一個外掛程式,它使JAVAScript小數減小。</span><span class="sxs-lookup"><span data-stu-id="78b7c-113">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="78b7c-114">**咕咕-連續觀察**:一個外掛程式,監視文件活動。</span><span class="sxs-lookup"><span data-stu-id="78b7c-114">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="78b7c-115">準備應用程式</span><span class="sxs-lookup"><span data-stu-id="78b7c-115">Preparing the application</span></span>

<span data-ttu-id="78b7c-116">首先,設置一個新的空 Web 應用程式並添加 TypeScript 範例檔。</span><span class="sxs-lookup"><span data-stu-id="78b7c-116">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="78b7c-117">TypeScript 檔案使用預設的 Visual Studio 設置自動編譯到 JavaScript 中,並且將成為我們使用 Grunt 處理的原材料。</span><span class="sxs-lookup"><span data-stu-id="78b7c-117">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1. <span data-ttu-id="78b7c-118">在視覺化工作室中,建立新`ASP.NET Web Application`。</span><span class="sxs-lookup"><span data-stu-id="78b7c-118">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2. <span data-ttu-id="78b7c-119">在 **"新建ASP.NET專案"** 對話框中,選擇"核心**空**"範本ASP.NET,然後單擊"確定"按鈕。</span><span class="sxs-lookup"><span data-stu-id="78b7c-119">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3. <span data-ttu-id="78b7c-120">在解決方案資源管理器中,查看專案結構。</span><span class="sxs-lookup"><span data-stu-id="78b7c-120">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="78b7c-121">該`\src`資料夾包括`wwwroot`空`Dependencies`節點和節點。</span><span class="sxs-lookup"><span data-stu-id="78b7c-121">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![空 Web 解決方案](using-grunt/_static/grunt-solution-explorer.png)

4. <span data-ttu-id="78b7c-123">向項目目錄添加名為`TypeScript`的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="78b7c-123">Add a new folder named `TypeScript` to your project directory.</span></span>

5. <span data-ttu-id="78b7c-124">在添加任何檔之前,請確保 Visual Studio 具有選中的 TypeScript 檔「儲存編譯」選項。</span><span class="sxs-lookup"><span data-stu-id="78b7c-124">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="78b7c-125">瀏覽到**工具** > **選項** > **文字編輯器** > **型態文稿** > **專案**:</span><span class="sxs-lookup"><span data-stu-id="78b7c-125">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![選項設定自動編譯 TypeScript 檔案](using-grunt/_static/typescript-options.png)

6. <span data-ttu-id="78b7c-127">右鍵按下`TypeScript`目錄,然後從上下文選單**中選擇「新增>新專案**。</span><span class="sxs-lookup"><span data-stu-id="78b7c-127">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="78b7c-128">選擇**JavaScript 檔案**項目並命名檔案 *「品味.ts」(* 請注意\*.ts 副檔名)。</span><span class="sxs-lookup"><span data-stu-id="78b7c-128">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="78b7c-129">將下面的 TypeScript 程式複製到檔案中(當您儲存時,一個新的*品味.js*檔將顯示與 JavaScript 源。</span><span class="sxs-lookup"><span data-stu-id="78b7c-129">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. <span data-ttu-id="78b7c-130">將第二個檔案添加到**TypeScript**`Food.ts`目錄並命名它。</span><span class="sxs-lookup"><span data-stu-id="78b7c-130">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="78b7c-131">將下面的代碼複製到檔中。</span><span class="sxs-lookup"><span data-stu-id="78b7c-131">Copy the code below into the file.</span></span>

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

## <a name="configuring-npm"></a><span data-ttu-id="78b7c-132">設定 NPM</span><span class="sxs-lookup"><span data-stu-id="78b7c-132">Configuring NPM</span></span>

<span data-ttu-id="78b7c-133">接下來,將 NPM 配置為下載咕咕和咆哮的任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-133">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="78b7c-134">在「解決方案資源管理器」中,右鍵單擊項目,然後從上下文菜單**中選擇「添加>新專案**」。。</span><span class="sxs-lookup"><span data-stu-id="78b7c-134">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="78b7c-135">選擇**NPM 設定檔**項目,保留預設名稱 *「包.json」,* 然後單擊 **「添加**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="78b7c-135">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="78b7c-136">在*包.json*檔中`devDependencies`,在 物件大括弧內輸入"grunt"。</span><span class="sxs-lookup"><span data-stu-id="78b7c-136">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="78b7c-137">從`grunt`"感知"清單中選擇,然後按 Enter 鍵。</span><span class="sxs-lookup"><span data-stu-id="78b7c-137">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="78b7c-138">Visual Studio 將引用咆哮的包名稱,並添加冒號。</span><span class="sxs-lookup"><span data-stu-id="78b7c-138">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="78b7c-139">在冒號清單的右側,從"感知"列表的頂部選擇包的最新穩定版本(如果未顯示 Intellisense,`Ctrl-Space`請按)。</span><span class="sxs-lookup"><span data-stu-id="78b7c-139">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![咕咕的理智](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > <span data-ttu-id="78b7c-141">NPM 使用[語義版本控制](https://semver.org/)來組織依賴項。</span><span class="sxs-lookup"><span data-stu-id="78b7c-141">NPM uses [semantic versioning](https://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="78b7c-142">語義版本控制(也稱為 SemVer)標識具有編號\<方案 主要>的包。\<次要>。\<補丁>。</span><span class="sxs-lookup"><span data-stu-id="78b7c-142">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="78b7c-143">Intellisense 僅顯示幾個常見選項,從而簡化了語義版本控制。</span><span class="sxs-lookup"><span data-stu-id="78b7c-143">Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="78b7c-144">Intellisense 清單中的頂部專案(如上例中的 0.4.5)被視為包的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="78b7c-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="78b7c-145">加斯特 (*) 符號與最新主要版本匹配,波浪線 (*) 與最新的次要版本匹配。</span><span class="sxs-lookup"><span data-stu-id="78b7c-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="78b7c-146">請參閱[NPM semver 版本解析器引用](https://www.npmjs.com/package/semver),作為 SemVer 提供的完整運算式指南。</span><span class="sxs-lookup"><span data-stu-id="78b7c-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="78b7c-147">添加更多依賴\*項來載入咕咕-contrib-包,用於*清潔\*\*、jshint、concat、ugly*和*手錶*,如下例所示*concat\*\*uglify*。</span><span class="sxs-lookup"><span data-stu-id="78b7c-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="78b7c-148">版本不需要與示例匹配。</span><span class="sxs-lookup"><span data-stu-id="78b7c-148">The versions don't need to match the example.</span></span>

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

4. <span data-ttu-id="78b7c-149">保存*包.json*檔。</span><span class="sxs-lookup"><span data-stu-id="78b7c-149">Save the *package.json* file.</span></span>

<span data-ttu-id="78b7c-150">每個專案`devDependencies`的包將下載,以及每個包所需的任何檔。</span><span class="sxs-lookup"><span data-stu-id="78b7c-150">The packages for each `devDependencies` item will download, along with any files that each package requires.</span></span> <span data-ttu-id="78b7c-151">通過在**解決方案資源管理器**中啟用 **「顯示所有檔」** 按鈕,可以在*node_modules*目錄中找到套件檔。</span><span class="sxs-lookup"><span data-stu-id="78b7c-151">You can find the package files in the *node_modules* directory by enabling the **Show All Files** button in **Solution Explorer**.</span></span>

![咕咕node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="78b7c-153">如果需要,可以通過右鍵單`Dependencies\NPM`擊 並選擇「**還原包」** 功能表選項,在**解決方案資源管理器**中手動還原依賴項。</span><span class="sxs-lookup"><span data-stu-id="78b7c-153">If you need to, you can manually restore dependencies in **Solution Explorer** by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![還原包](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="78b7c-155">設定葛籣特</span><span class="sxs-lookup"><span data-stu-id="78b7c-155">Configuring Grunt</span></span>

<span data-ttu-id="78b7c-156">Grunt 使用名為*Gruntfile.js*的清單進行配置,該清單定義、載入和註冊可手動執行的任務,或配置為根據 Visual Studio 中的事件自動執行。</span><span class="sxs-lookup"><span data-stu-id="78b7c-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="78b7c-157">右鍵按一下項目並選擇「**新增新** > **專案**」。</span><span class="sxs-lookup"><span data-stu-id="78b7c-157">Right-click the project and select **Add** > **New Item**.</span></span> <span data-ttu-id="78b7c-158">選擇**JavaScript 檔**項目樣本,將名稱更改為*Gruntfile.js*,然後按下「**添加**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="78b7c-158">Select the **JavaScript File** item template, change the name to *Gruntfile.js*, and click the **Add** button.</span></span>

1. <span data-ttu-id="78b7c-159">將以下代碼加入*Gruntfile.js*。</span><span class="sxs-lookup"><span data-stu-id="78b7c-159">Add the following code to *Gruntfile.js*.</span></span> <span data-ttu-id="78b7c-160">該`initConfig`函數為每個包設置選項,模組的其餘部分載入和註冊任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-160">The `initConfig` function sets options for each package, and the remainder of the module loads and register tasks.</span></span>

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. <span data-ttu-id="78b7c-161">在`initConfig`函數內,添加任務的選項,`clean`如下例*Gruntfile.js*所示。</span><span class="sxs-lookup"><span data-stu-id="78b7c-161">Inside the `initConfig` function, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="78b7c-162">任務`clean`接受目錄字串陣列。</span><span class="sxs-lookup"><span data-stu-id="78b7c-162">The `clean` task accepts an array of directory strings.</span></span> <span data-ttu-id="78b7c-163">此任務從*wwwroot/lib*中刪除檔案,並刪除整個 */temp*目錄。</span><span class="sxs-lookup"><span data-stu-id="78b7c-163">This task removes files from *wwwroot/lib* and removes the entire */temp* directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. <span data-ttu-id="78b7c-164">在函數`initConfig`下方,`grunt.loadNpmTasks`向 添加調用。</span><span class="sxs-lookup"><span data-stu-id="78b7c-164">Below the `initConfig` function, add a call to `grunt.loadNpmTasks`.</span></span> <span data-ttu-id="78b7c-165">這將使任務可以從可視化工作室運行。</span><span class="sxs-lookup"><span data-stu-id="78b7c-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. <span data-ttu-id="78b7c-166">儲存*格倫特檔案.js*.</span><span class="sxs-lookup"><span data-stu-id="78b7c-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="78b7c-167">該檔應類似於下面的屏幕截圖。</span><span class="sxs-lookup"><span data-stu-id="78b7c-167">The file should look something like the screenshot below.</span></span>

    ![初始咕咕檔案](using-grunt/_static/gruntfile-js-initial.png)

1. <span data-ttu-id="78b7c-169">右鍵按*下 Gruntfile.js*並從上下文選單中選擇**工作執行器資源管理員**。</span><span class="sxs-lookup"><span data-stu-id="78b7c-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="78b7c-170">任務**運行者資源管理員**視窗將打開。</span><span class="sxs-lookup"><span data-stu-id="78b7c-170">The **Task Runner Explorer** window will open.</span></span>

    ![工作執行器資源管理員選單](using-grunt/_static/task-runner-explorer-menu.png)

1. <span data-ttu-id="78b7c-172">驗證在`clean`**任務運行器資源管理器**中**的任務**下顯示的顯示。</span><span class="sxs-lookup"><span data-stu-id="78b7c-172">Verify that `clean` shows under **Tasks** in the **Task Runner Explorer**.</span></span>

    ![工作執行程式資源管理員工作清單](using-grunt/_static/task-runner-explorer-tasks.png)

1. <span data-ttu-id="78b7c-174">右鍵單擊乾淨任務,然後從上下文菜單中選擇 **「運行**」。。</span><span class="sxs-lookup"><span data-stu-id="78b7c-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="78b7c-175">命令視窗顯示任務的進度。</span><span class="sxs-lookup"><span data-stu-id="78b7c-175">A command window displays progress of the task.</span></span>

    ![工作執行器資源管理員執行乾淨工作](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > <span data-ttu-id="78b7c-177">尚沒有要清理的檔或目錄。</span><span class="sxs-lookup"><span data-stu-id="78b7c-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="78b7c-178">如果您願意,可以在解決方案資源管理器中手動創建它們,然後作為測試運行乾淨任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>

1. <span data-ttu-id="78b7c-179">在`initConfig`函數中,添加用於`concat`使用以下代碼的條目。</span><span class="sxs-lookup"><span data-stu-id="78b7c-179">In the `initConfig` function, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="78b7c-180">屬性`src`陣列按應合併的順序出要合併的檔。</span><span class="sxs-lookup"><span data-stu-id="78b7c-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="78b7c-181">屬性`dest`將路徑分配給生成的組合檔。</span><span class="sxs-lookup"><span data-stu-id="78b7c-181">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="78b7c-182">上述`all`代碼中的屬性是目標的名稱。</span><span class="sxs-lookup"><span data-stu-id="78b7c-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="78b7c-183">目標用於某些 Grunt 任務,以允許多個生成環境。</span><span class="sxs-lookup"><span data-stu-id="78b7c-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="78b7c-184">您可以使用 IntelliSense 查看內建目標或分配您自己的目標。</span><span class="sxs-lookup"><span data-stu-id="78b7c-184">You can view the built-in targets using IntelliSense or assign your own.</span></span>

1. <span data-ttu-id="78b7c-185">使用下面的`jshint`代碼添加任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="78b7c-186">jshint`code-quality`實用程式針對*暫存*目錄中找到的每個 JavaScript 檔執行。</span><span class="sxs-lookup"><span data-stu-id="78b7c-186">The jshint `code-quality` utility is run against every JavaScript file found in the *temp* directory.</span></span>

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="78b7c-187">選項「-W069」是 jshint 在 JavaScript 使用括弧語法分配屬性而不是點`Tastes["Sweet"]`表示`Tastes.Sweet`法(即 而不是時)生成的錯誤。</span><span class="sxs-lookup"><span data-stu-id="78b7c-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="78b7c-188">該選項將關閉警告,以允許進程的其餘部分繼續。</span><span class="sxs-lookup"><span data-stu-id="78b7c-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

1. <span data-ttu-id="78b7c-189">使用下面的`uglify`代碼添加任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="78b7c-190">該任務對在臨時目錄中找到的*合併.js*檔進行分明,並在 wwwroot/lib 中按照標準命名約定*\<\>檔名 .min.js*創建結果檔。</span><span class="sxs-lookup"><span data-stu-id="78b7c-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. <span data-ttu-id="78b7c-191">在調用`grunt.loadNpmTasks`該負`grunt-contrib-clean`載 下,使用下面的代碼包括對 jshint、concat 和 ugly 的相同調用。</span><span class="sxs-lookup"><span data-stu-id="78b7c-191">Under the call to `grunt.loadNpmTasks` that loads `grunt-contrib-clean`, include the same call for jshint, concat, and uglify using the code below.</span></span>

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. <span data-ttu-id="78b7c-192">儲存*格倫特檔案.js*.</span><span class="sxs-lookup"><span data-stu-id="78b7c-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="78b7c-193">該檔應類似於下面的範例。</span><span class="sxs-lookup"><span data-stu-id="78b7c-193">The file should look something like the example below.</span></span>

    ![完整的咕咕檔案範例](using-grunt/_static/gruntfile-js-complete.png)

1. <span data-ttu-id="78b7c-195">請注意,**工作執行者資源管理員**工作`clean``concat`清單`jshint``uglify`包括和 工作。</span><span class="sxs-lookup"><span data-stu-id="78b7c-195">Notice that the **Task Runner Explorer** Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="78b7c-196">按順序運行每個任務,並在**解決方案資源管理器**中觀察結果。</span><span class="sxs-lookup"><span data-stu-id="78b7c-196">Run each task in order and observe the results in **Solution Explorer**.</span></span> <span data-ttu-id="78b7c-197">每個任務都應運行,而不會出錯。</span><span class="sxs-lookup"><span data-stu-id="78b7c-197">Each task should run without errors.</span></span>

    ![工作執行器資源管理員執行每個工作](using-grunt/_static/task-runner-explorer-run-each-task.png)

    <span data-ttu-id="78b7c-199">concat 工作將創建一個新的*組合.js*檔,並將其放入臨時目錄中。</span><span class="sxs-lookup"><span data-stu-id="78b7c-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="78b7c-200">任務`jshint`只是運行,不生成輸出。</span><span class="sxs-lookup"><span data-stu-id="78b7c-200">The `jshint` task simply runs and doesn't produce output.</span></span> <span data-ttu-id="78b7c-201">任務`uglify`創建一個新的*組合.min.js*檔,並將其放入*wwwroot/lib*中。</span><span class="sxs-lookup"><span data-stu-id="78b7c-201">The `uglify` task creates a new *combined.min.js* file and places it into *wwwroot/lib*.</span></span> <span data-ttu-id="78b7c-202">完成後,解決方案應如下所示的螢幕截圖:</span><span class="sxs-lookup"><span data-stu-id="78b7c-202">On completion, the solution should look something like the screenshot below:</span></span>

    ![所有工作後的解決方案資源管理員](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > <span data-ttu-id="78b7c-204">有關每個包的選項的詳細資訊,請造[https://www.npmjs.com/](https://www.npmjs.com/)訪並查找主頁搜索框中的包名稱。</span><span class="sxs-lookup"><span data-stu-id="78b7c-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="78b7c-205">例如,您可以查找咕咕-contrib-clean 包,以獲得解釋其所有參數的文檔連結。</span><span class="sxs-lookup"><span data-stu-id="78b7c-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="78b7c-206">齊聚一堂</span><span class="sxs-lookup"><span data-stu-id="78b7c-206">All together now</span></span>

<span data-ttu-id="78b7c-207">使用 Grunt`registerTask()`方法按特定順序運行一系列任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="78b7c-208">例如,要按順序運行上述示例步驟,請按> -> jhint ->丑陋,請向模組添加下面的代碼。</span><span class="sxs-lookup"><span data-stu-id="78b7c-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="78b7c-209">代碼應添加到與 loadNpmTasks() 調用相同的級別,在 initConfig 外部。</span><span class="sxs-lookup"><span data-stu-id="78b7c-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="78b7c-210">新任務顯示在別名任務下的任務運行者資源管理器中。</span><span class="sxs-lookup"><span data-stu-id="78b7c-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="78b7c-211">您可以像在其他任務一樣右鍵按一下並運行它。</span><span class="sxs-lookup"><span data-stu-id="78b7c-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="78b7c-212">工作`all`將依順序`clean`執行`concat``jshint` `uglify` 。</span><span class="sxs-lookup"><span data-stu-id="78b7c-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![別名咕咕任務](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="78b7c-214">監看變更</span><span class="sxs-lookup"><span data-stu-id="78b7c-214">Watching for changes</span></span>

<span data-ttu-id="78b7c-215">任務`watch`監視檔和目錄。</span><span class="sxs-lookup"><span data-stu-id="78b7c-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="78b7c-216">監視在檢測到更改時自動觸發任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="78b7c-217">將下面的代碼添加到 initConfig 以監視 TypeScript\*目錄中對 .js 檔的更改。</span><span class="sxs-lookup"><span data-stu-id="78b7c-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="78b7c-218">如果更改了 JavaScript`watch`檔`all`,將運行 該任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="78b7c-219">新增呼叫以`loadNpmTasks()`在工作執行`watch`器 資源管理員中顯示任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="78b7c-220">右鍵單擊任務運行器資源管理器中的監視任務,然後從上下文菜單中選擇"運行"。</span><span class="sxs-lookup"><span data-stu-id="78b7c-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="78b7c-221">顯示正在運行的監視任務的命令視窗將顯示"等待..."消息。</span><span class="sxs-lookup"><span data-stu-id="78b7c-221">The command window that shows the watch task running will display a "Waiting…" message.</span></span> <span data-ttu-id="78b7c-222">打開其中一個 TypeScript 檔,添加空格,然後儲存該檔。</span><span class="sxs-lookup"><span data-stu-id="78b7c-222">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="78b7c-223">這將觸發監視任務,並觸發其他任務按順序運行。</span><span class="sxs-lookup"><span data-stu-id="78b7c-223">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="78b7c-224">下面的屏幕截圖顯示了示例運行。</span><span class="sxs-lookup"><span data-stu-id="78b7c-224">The screenshot below shows a sample run.</span></span>

![執行工作輸出](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="78b7c-226">繫結到視覺化工作室事件</span><span class="sxs-lookup"><span data-stu-id="78b7c-226">Binding to Visual Studio events</span></span>

<span data-ttu-id="78b7c-227">除非希望在每次在 Visual Studio 中工作時手動啟動任務,否則請將任務綁定到**生成之前**、**生成後**,**清理**和**專案打開**事件。</span><span class="sxs-lookup"><span data-stu-id="78b7c-227">Unless you want to manually start your tasks every time you work in Visual Studio, bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="78b7c-228">綁定`watch`,以便每次打開 Visual Studio 時都會運行。</span><span class="sxs-lookup"><span data-stu-id="78b7c-228">Bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="78b7c-229">在任務運行者資源管理器中,右鍵單擊監視任務並從上下文菜單中選擇 **「綁定** > **專案打開**」。。</span><span class="sxs-lookup"><span data-stu-id="78b7c-229">In Task Runner Explorer, right-click the watch task and select **Bindings** > **Project Open** from the context menu.</span></span>

![將工作繫結到項目開啟](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="78b7c-231">卸載並重新載入專案。</span><span class="sxs-lookup"><span data-stu-id="78b7c-231">Unload and reload the project.</span></span> <span data-ttu-id="78b7c-232">當專案再次載入時,監視任務將自動開始運行。</span><span class="sxs-lookup"><span data-stu-id="78b7c-232">When the project loads again, the watch task starts running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="78b7c-233">摘要</span><span class="sxs-lookup"><span data-stu-id="78b7c-233">Summary</span></span>

<span data-ttu-id="78b7c-234">Grunt 是一個強大的任務運行者,可用於自動執行大多數用戶端生成任務。</span><span class="sxs-lookup"><span data-stu-id="78b7c-234">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="78b7c-235">Grunt 利用 NPM 來交付其包,並具有與 Visual Studio 的工具集成功能。</span><span class="sxs-lookup"><span data-stu-id="78b7c-235">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="78b7c-236">Visual Studio 的任務執行者資源管理員可檢測對設定檔的更改,並提供一個方便的介面來執行任務、查看正在運行的任務並將任務綁定到 Visual Studio 事件。</span><span class="sxs-lookup"><span data-stu-id="78b7c-236">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>
