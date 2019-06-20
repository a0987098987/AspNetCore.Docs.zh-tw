---
title: 在 ASP.NET Core 中使用 Grunt
author: rick-anderson
description: 在 ASP.NET Core 中使用 Grunt
ms.author: riande
ms.date: 06/18/2019
uid: client-side/using-grunt
ms.openlocfilehash: 851ce3b50e88fee597518aef23276800f4b50f06
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207756"
---
# <a name="use-grunt-in-aspnet-core"></a>在 ASP.NET Core 中使用 Grunt

Grunt 是一個 JavaScript 工作執行器，它會會自動化指令碼縮製、TypeScript 編譯、程式碼品質 "lint" 工具、CSS 前置處理器，以及支援用戶端開發所需的任何重複性工作。 Grunt 完全支援 Visual Studio 中。

這個範例會使用空的 ASP.NET Core 專案做為起點，展示如何從頭開始自動化用戶端建置程序。

完成的範例會清除目標部署目錄、 組合 JavaScript 檔案、 檢查程式碼品質、 壓縮 JavaScript 文件內容和部署到您的網頁應用程式根目錄。 我們將使用下列套件：

* **grunt**:Grunt 工作執行器套件。

* **grunt contrib 清除**:移除檔案或目錄的外掛程式。

* **grunt-contrib-jshint**:檢閱 JavaScript 程式碼品質的外掛程式。

* **grunt-contrib-concat**:外掛程式聯結成單一檔案的檔案。

* **grunt contrib uglify**:縮短 JavaScript，以減少大小的外掛程式。

* **grunt-contrib-watch**:監看檔案 」 活動的外掛程式。

## <a name="preparing-the-application"></a>準備應用程式

若要開始，設定新的空白 web 應用程式並新增 TypeScript 範例檔案。 TypeScript 檔案會自動編譯成 JavaScript 使用 Visual Studio 的預設設定，因此我們處理使用 Grunt 的原始資料。

1. 在 Visual Studio 中，建立新`ASP.NET Web Application`。

2. 在 [新增 ASP.NET 專案]  對話方塊中，選取 [ASP.NET Core] **空白**範本，然後按一下 [確定] 按鈕。

3. 在 [方案總管] 中檢閱專案結構。 `\src`資料夾包含空`wwwroot`和`Dependencies`節點。

    ![空的 web 方案](using-grunt/_static/grunt-solution-explorer.png)

4. 加入新的資料夾，名為`TypeScript`到您的專案目錄。

5. 之前新增任何檔案，請確定 Visual Studio 有選項 '編譯儲存' 檢查的 TypeScript 檔案。 瀏覽至**工具** > **選項** > **文字編輯器** > **Typescript**  > **專案**:

    ![設定自動編譯 TypeScript 檔案的選項](using-grunt/_static/typescript-options.png)

6. 以滑鼠右鍵按一下 `TypeScript` 目錄，然後從快顯功能表選取 [新增 > 新增項目]  。 選取 **JavaScript 檔案**項目，並將檔案命名為 *Tastes.ts* (請注意 \*.ts 副檔名)。 將下面 TypeScript 程式碼行複製到檔案中 (當您儲存時，新 *Tastes.js* 檔案將會隨 JavaScript 原始程式碼出現)。

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. 第二個將檔案加入至**TypeScript** directory 並將它命名`Food.ts`。 將下列程式碼複製到檔案。

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

## <a name="configuring-npm"></a>設定 NPM

接著，設定 NPM 以便下載 grunt 與 grunt-tasks。

1. 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後從快顯功能表選取 [新增 > 新增項目]  。 選取 **NPM 設定檔**項目，保留預設名稱  *package.json*，然後按一下 [新增]  按鈕。

2. 在  *package.json*檔案，內部`devDependencies`物件大括號中，輸入 「 grunt"。 選取`grunt`從 Intellisense 清單，然後按 Enter 鍵。 Visual Studio 會加上引號 grunt 封裝名稱，並加入冒號。 冒號右邊，請從頂端的 [Intellisense] 清單中選取封裝的最新穩定版本 (按`Ctrl-Space`Intellisense 不會出現)。

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > 使用 NPM[語意版本設定](http://semver.org/)組織相依性。 語意版本設定，也就是 SemVer 識別套件的編號配置\<主要 >。\<次要 >。\<修補程式 >。 Intellisense 會顯示只有幾個常見的選擇，以簡化語意版本設定。 在 [Intellisense] 清單 (在上述範例中的 0.4.5) 頂端的項目會被視為封裝的最新穩定版本。 插入號 (^) 符號符合最新的主要版本和波狀符號 （~） 比對的最新的次要版本。 請參閱[NPM semver 版本剖析器參考](https://www.npmjs.com/package/semver)做為 SemVer 提供的完整表現度的指南。

3. 如下列範例所示，新增更多的相依性以針對 *clean*、*jshint*、*concat*、*uglify* 與 *watch* 載入 grunt-contrib-\* 套件。 版本不需要與範例相符。

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

4. 儲存*package.json*檔案。

每個套件`devDependencies`將下載的項目，以及每個封裝所需的任何檔案。 您可以找到套件檔案中的*node_modules*藉由啟用目錄**顯示所有檔案**按鈕**方案總管 中**。

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> 如果您需要您可以手動還原中的相依性**方案總管**上按一下滑鼠右鍵`Dependencies\NPM`，然後選取**還原套件**功能表選項。

![還原套件](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>設定 Grunt

Grunt 已設定使用名為資訊清單*Gruntfile.js* ，定義、 載入和註冊可以手動執行或執行 Visual Studio 中的事件中的 自動根據設定的工作。

1. 以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。 選取 [ **JavaScript 檔案**項目範本，將名稱變更為*Gruntfile.js*，然後按一下**新增**] 按鈕。

1. 將下列程式碼加入*Gruntfile.js*。 `initConfig`函式會將每一個封裝的選項和模組的其餘部分會載入並註冊工作。

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. 內部`initConfig`函式中，新增的選項`clean`工作中的範例所示*Gruntfile.js*如下。 `clean`工作會接受目錄字串的陣列。 這項工作會從檔案中移除*wwwroot/lib* ，並移除整個 */暫存*目錄。

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. 下面`initConfig`函式中，將呼叫加入`grunt.loadNpmTasks`。 這將使工作可以從 Visual Studio 執行。

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. 儲存*Gruntfile.js*。 檔案看起來應該像下列螢幕擷取畫面。

    ![初始的 gruntfile](using-grunt/_static/gruntfile-js-initial.png)

1. 以滑鼠右鍵按一下*Gruntfile.js* ，然後選取**Task Runner Explorer**從內容功能表。 **Task Runner Explorer**視窗隨即開啟。

    ![工作執行器總管功能表](using-grunt/_static/task-runner-explorer-menu.png)

1. 確認`clean`會顯示在下方**任務**中**Task Runner Explorer**。

    ![工作執行器總管工作清單](using-grunt/_static/task-runner-explorer-tasks.png)

1. 以滑鼠右鍵按一下 「 清除 」 工作，然後選取**執行**從內容功能表。 命令視窗會顯示作業進度。

    ![工作執行器總管 中執行清理工作](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > 沒有任何檔案或目錄將尚未清除。 如有需要，您可以在 方案總管 中手動建立它們，然後再執行測試的 「 清除 」 工作。

1. 在 `initConfig`函式中，新增項目`concat`使用下列程式碼。

    `src`屬性陣列會列出檔案，若要結合，它們應該要結合的順序。 `dest`屬性指派給所產生的合併檔案的路徑。

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > `all`上述程式碼中的屬性為目標的名稱。 目標用於某些 Grunt 工作，以允許多個組建環境。 您可以檢視使用 IntelliSense 的內建的目標，或指派自己。

1. 使用下列程式碼新增`jshint`工作。

    Jshint`code-quality`公用程式執行中找到的每個 JavaScript 檔案*temp*目錄。

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > 選項 "-W069" 錯誤是由 jshint 在 JavaScript 使用括號語法來指派屬性而非點標記法 (亦即 `Tastes["Sweet"]`，而非 `Tastes.Sweet`) 時所產生。 此選項會關閉警告，以允許處理序的剩餘部分。

1. 使用下列程式碼新增`uglify`工作。

    工作縮短*combined.js*檔案找到暫存目錄中，並會將結果檔案建立標準的命名慣例的 wwwroot/lib *\<檔案名稱\>。 min.js*.

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. 若要呼叫下`grunt.loadNpmTasks`可載入`grunt-contrib-clean`、 包含 jshint，concat、 的相同呼叫和 uglify 使用下列程式碼。

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. 儲存*Gruntfile.js*。 檔案看起來應該像下面範例。

    ![完整的 grunt 檔案範例](using-grunt/_static/gruntfile-js-complete.png)

1. 請注意， **Task Runner Explorer**工作清單包括`clean`， `concat`，`jshint`和`uglify`工作。 順序執行每項工作，並觀察結果**方案總管 中**。 每個工作應該執行無誤。

    ![執行每項工作的工作執行器總管](using-grunt/_static/task-runner-explorer-run-each-task.png)

    Concat 工作建立新*combined.js*檔案，並將它放入暫存目錄。 `jshint`工作只在執行，並不會產生輸出。 `uglify`工作會建立新*combined.min.js*檔案，並將它放*wwwroot/lib*。 完成時，方案應該看起來像下列螢幕擷取畫面：

    ![方案總管 中的所有工作](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > 如需有關每個套件的選項的詳細資訊，請瀏覽[ https://www.npmjs.com/ ](https://www.npmjs.com/)和查閱的主頁面上的 [搜尋] 方塊中的封裝名稱。 例如，您可以查閱 grunt contrib 清除封裝，以取得說明的所有參數的文件連結。

### <a name="all-together-now"></a>整合所有工作

使用 Grunt`registerTask()`方法，以特定順序執行一系列的工作。 例如，若要執行上述步驟中全新的順序]-> [範例 concat]-> [jshint]-> [uglify、 將下列程式碼新增至模組。 程式碼應該將外部 initConfig loadNpmTasks() 呼叫相同的層級。

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

在 [別名工作] 下的 [Task Runner explorer] 中出現新的工作。 您可以以滑鼠右鍵按一下，並執行它，就像其他工作一樣。 `all`工作會執行`clean`， `concat`，`jshint`和`uglify`，順序。

![別名 grunt 工作](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>監看變更

A`watch`工作會監看檔案和目錄上的。 監看式自動觸發的工作，偵測到變更時。 將以下的程式碼新增至監看變更 initConfig \*TypeScript 目錄中的.js 檔案。 如果 JavaScript 檔案變更時，`watch`將會執行`all`工作。

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

將呼叫加入`loadNpmTasks()`以顯示`watch`Task Runner explorer 中的工作。

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

以滑鼠右鍵按一下 Task Runner explorer 中的監看式項工作，並從操作功能表中選取 執行。 顯示 監看式工作執行的命令視窗會顯示 等候訊息。 開啟 TypeScript 檔案的其中一個，加上空格，並儲存檔案。 這會觸發監看式工作，並觸發其他工作順序執行。 以下螢幕擷取畫面顯示執行範例。

![執行工作輸出](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>繫結至 Visual Studio 事件

除非您想要以手動方式啟動您的工作，每次您在 Visual Studio 中工作時，您可以繫結到工作**再建置**，**之後建置**，**清除**，和**專案開啟**事件。

讓我們繫結`watch`使其執行每次 Visual Studio 隨即開啟。 在 Task Runner explorer 中，以滑鼠右鍵按一下 監看式工作，然後選取**繫結 > 專案開啟**從內容功能表。

![繫結至專案開啟的工作](using-grunt/_static/bindings-project-open.png)

卸載並重新載入專案。 當專案重新載入時，監看式工作就會開始自動執行。

## <a name="summary"></a>總結

Grunt 是功能強大的工作執行器，可用來將大部分的用戶端組建工作自動化。 Grunt 運用 NPM 提供其套件和工具與 Visual Studio 整合的功能。 Visual Studio 的 Task Runner explorer 會偵測到組態檔的變更，並提供方便的介面，以執行工作、 檢視執行中的工作，並將工作繫結至 Visual Studio 事件。
