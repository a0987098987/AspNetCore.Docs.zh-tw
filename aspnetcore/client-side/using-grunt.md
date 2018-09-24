---
title: 在 ASP.NET Core 中使用 Grunt
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2016
uid: client-side/using-grunt
ms.openlocfilehash: 21fa565c930563bbc819c2a02ea71655193513d0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272970"
---
# <a name="use-grunt-in-aspnet-core"></a>在 ASP.NET Core 中使用 Grunt

作者：[Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt 是一個 JavaScript 工作執行器，它會會自動化指令碼縮製、TypeScript 編譯、程式碼品質 "lint" 工具、CSS 前置處理器，以及支援用戶端開發所需的任何重複性工作。 Grunt 在 Visual Studio 中受到完整的支援，雖然 ASP.NET 專案範本預設使用 Gulp (請參閱[使用 Gulp](using-gulp.md))。

這個範例會使用空的 ASP.NET Core 專案做為起點，展示如何從頭開始自動化用戶端建置程序。

完成的範例會清除目標部署目錄、 組合 JavaScript 檔案、 檢查程式碼品質、 壓縮 JavaScript 文件內容和部署到您的網頁應用程式根目錄。 我們將使用下列套件：

* **grunt**：Grunt 工作執行器套件。

* **grunt-contrib-clean**：移除檔案或目錄的外掛程式。

* **grunt-contrib-jshint**： 檢閱 JavaScript 程式碼品質的外掛程式。

* **grunt-contrib-concat**：將檔案聯結為單一檔案的外掛程式。

* **grunt-contrib-uglify**：最小化 JavaScript 檔案以減少大小的外掛程式。

* **grunt-contrib-watch**：監看檔案活動的外掛程式。

## <a name="preparing-the-application"></a>準備應用程式

若要開始，設定新的空白 web 應用程式並新增 TypeScript 範例檔案。 TypeScript 檔案會自動編譯成 JavaScript 使用 Visual Studio 的預設設定，且將我們原料處理使用 grunt 所完成。

1.  在 Visual Studio 中，建立新`ASP.NET Web Application`。

2.  在**新增 ASP.NET 專案**] 對話方塊中，選取 [ASP.NET Core**空**範本，然後按一下 [確定] 按鈕。

3.  在 [方案總管] 中，檢閱專案結構。 `\src`資料夾包含空白`wwwroot`和`Dependencies`節點。

    ![空的 web 方案](using-grunt/_static/grunt-solution-explorer.png)

4.  加入新的資料夾，名為`TypeScript`至專案目錄。

5.  之前新增任何檔案，請確定 Visual Studio 中的選項 '編譯儲存' 的簽入 TypeScript 檔案。 瀏覽至**工具** > **選項** > **文字編輯器** > **Typescript**  > **專案**:

    ![設定自動 compliation TypeScript 檔案的選項](using-grunt/_static/typescript-options.png)

6.  以滑鼠右鍵按一下`TypeScript`目錄中，而且選取**新增 > 新的項目**從內容功能表。 選取**JavaScript 檔案**項目，並將檔案命名*Tastes.ts* (請注意\*.ts 副檔名)。 將下面 TypeScript 程式碼行複製到檔案 (當您儲存時，新*Tastes.js* JavaScript 來源檔案中將會出現)。
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  加入第二個檔案**TypeScript**目錄並將其命名`Food.ts`。 將下列程式碼複製到檔案。

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

接著，設定 NPM 以便下載 grunt grunt 所完成工作。

1. 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增 > 新的項目**從內容功能表。 選取**NPM 組態檔**項目，保留預設名稱， *package.json*，然後按一下**新增** 按鈕。

2. 在*package.json*內部檔案`devDependencies`物件大括號中，輸入 「 grunt"。 選取`grunt`從 Intellisense 清單，然後按 Enter 鍵。 Visual Studio 會加上引號 grunt 所完成的封裝名稱，並加入冒號。 右邊的冒號，請從 Intellisense 清單頂端選取封裝的最新穩定版本 (按`Ctrl-Space`Intellisense 不會出現)。

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > NPM 使用[語意版本設定](http://semver.org/)將組織的相依性。 語意版本設定，也稱為 SemVer 編號方式識別封裝<major>。<minor>。<patch>.Intellisense 會顯示只有幾個常見的選項，以簡化語意版本設定。 Intellisense 清單 (在上述範例 0.4.5) 中的最上層項目會被視為封裝的最新穩定版本。 插入號 (^) 符號符合最新的主要版本和波狀符號 （~） 符合最新的次要版本。 請參閱[NPM semver 版本剖析器參考](https://www.npmjs.com/package/semver)做為 SemVer 提供的完整表現的指南。

3. 新增更多的相依性，以便載入 grunt-contrib-\*封裝的*全新*， *jshint*， *concat*， *uglify*，和*監看式*如下列範例所示。 版本不需要符合範例。

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

將下載之套件的每個 devDependencies 項目，以及每個封裝所需的任何檔案。 您可以找到套件檔案中的`node_modules`目錄啟用**顯示所有檔案**[方案總管] 中的按鈕。

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> 如果需要您可以以滑鼠右鍵按一下，以手動方式還原方案總管 中的相依性`Dependencies\NPM`，然後選取**還原封裝**功能表選項。

![還原封裝](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>設定 Grunt

Grunt 已設定為使用名為資訊清單*Gruntfile.js*定義、 載入和註冊工作，可以手動執行或設定為基礎而執行自動 Visual Studio 中的事件。

1. 以滑鼠右鍵按一下專案，然後選取**新增 > 新的項目**。 選取**Grunt 組態檔**選項，請保留預設名稱， *Gruntfile.js*，然後按一下**新增** 按鈕。

   初始程式碼包含模組定義和`grunt.initConfig()`方法。 `initConfig()`用來設定每一個封裝的選項和模組的其餘部分將會載入並註冊工作。
    
   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. 內部`initConfig()`方法，加入選項`clean`工作範例所示*Gruntfile.js*下方。 「 清除 」 工作會接受目錄字串的陣列。 此工作從 wwwroot/程式庫移除檔案，並移除整個/暫存目錄。

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. 以下 initConfig() 方法中，將呼叫加入`grunt.loadNpmTasks()`。 這會讓工作可從 Visual Studio。

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. 儲存*Gruntfile.js*。 檔案應看起來像下列螢幕擷取畫面。

    ![初始 gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. 以滑鼠右鍵按一下*Gruntfile.js*選取**工作執行器總管**從內容功能表。 工作執行器總管視窗隨即開啟。

    ![工作執行器總管功能表](using-grunt/_static/task-runner-explorer-menu.png)

6. 確認`clean`會顯示在下方**工作**工作執行器總管中。

    ![工作執行器總管工作清單](using-grunt/_static/task-runner-explorer-tasks.png)

7. 「 清除 」 工作上按一下滑鼠右鍵，然後選取**執行**從內容功能表。 命令視窗會顯示工作的進度。

    ![工作執行器總管 中執行清理工作](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > 沒有任何檔案或目錄尚未清除。 如有需要，您可以在 [方案總管] 中手動建立它們，並再做為測試執行 「 清除 」 工作。
    
8. 在 initConfig() 方法中，新增的項目`concat`使用下列程式碼。

    `src`屬性陣列會列出檔案，以結合，它們應該要結合的順序。 `dest`屬性指派給所產生的合併檔案的路徑。

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > `all`上述程式碼中的屬性為目標的名稱。 目標用於某些 grunt 所完成的工作，以允許多個組建環境。 您可以檢視的內建的目標，使用 Intellisense，或指派自己。
    
9. 新增`jshint`工作使用下列程式碼。

    針對每個找到暫存目錄中的 JavaScript 檔案，執行 jshint 程式碼品質公用程式。
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > 選項"-W069 」 錯誤所產生 jshint JavaScript 使用括號語法來指定的屬性，而不是點標記法，也就是當`Tastes["Sweet"]`而不是`Tastes.Sweet`。 選項會關閉警告，以允許其他處理序繼續進行。

10. 新增`uglify`工作使用下列程式碼。

    工作縮短*combined.js*檔案找到暫存目錄中，並會將結果檔案建立 wwwroot/lib 遵循標準命名慣例*\<檔案名稱\>。 min.js*.
    
    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. 在載入 grunt contrib 清除呼叫 grunt.loadNpmTasks()，包含相同的呼叫，如 jshint，concat，uglify 使用下列程式碼。
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. 儲存*Gruntfile.js*。 檔案看起來應該像下面範例。

    ![完成 grunt 檔案範例](using-grunt/_static/gruntfile-js-complete.png)

13. 請注意，工作執行器總管工作清單包括`clean`， `concat`，`jshint`和`uglify`工作。 執行每項工作順序，並觀察 [方案總管] 中的結果。 每個工作應該執行無誤。
    
    ![執行每項工作的工作執行器總管](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    Concat 工作建立新*combined.js*檔案，並將它放入暫存目錄。 只要 jshint 工作執行，並不會產生輸出。 Uglify 工作建立新*combined.min.js*檔案，並將它放入 wwwroot/lib。 完成時，方案看起來應該類似下面的螢幕擷取畫面：
    
    ![方案總管 在所有工作](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > 如需有關每個封裝的選項的詳細資訊，請瀏覽[ https://www.npmjs.com/ ](https://www.npmjs.com/)和查閱的主頁面上的 [搜尋] 方塊中的封裝名稱。 例如，您可以查閱 grunt contrib 清除封裝，以取得文件的連結，說明及其所有的參數。

### <a name="all-together-now"></a>一堂

使用 Grunt`registerTask()`方法，以特定順序執行一系列的工作。 例如，若要執行上述步驟，在初始狀態的順序]-> [範例 concat]-> [jshint]-> [uglify、 將下列程式碼加入至模組。 程式碼應加入至外部 initConfig loadNpmTasks() 呼叫，相同的層級。

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

別名 [工作] 下的工作執行器總管中出現新的工作。 您可以以滑鼠右鍵按一下，然後執行就如同其他工作。 `all`工作將會執行`clean`， `concat`，`jshint`和`uglify`，順序。

![別名 grunt 所完成的工作](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>監看變更

A`watch`工作會監看檔案及目錄上的。 監看式自動觸發工作，如果它偵測到變更。 下列程式碼加入監看的變更 initConfig \*TypeScript 目錄中的.js 檔案。 如果變更的 JavaScript 檔案，`watch`將執行`all`工作。

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

將呼叫加入`loadNpmTasks()`顯示`watch`工作執行器總管中的工作。

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

以滑鼠右鍵按一下 監看式中的工作工作執行器總管，並從操作功能表選取 執行。 顯示 監看式工作執行此命令視窗會顯示 等候 訊息。 開啟其中一個 TypeScript 檔案，加入空格，並儲存檔案。 這將會觸發監看式工作，並觸發其他工作，以執行順序。 下列螢幕擷取畫面顯示範例執行。

![執行工作的輸出](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>繫結至 Visual Studio 事件

除非您想要以手動方式啟動您的工作，每次您使用 Visual Studio 中，您可以繫結工作**之前建置**，**後建置**，**清除**，和**專案開啟**事件。

讓我們來繫結`watch`使其執行每次開啟 Visual Studio。 在工作執行器總管中，以滑鼠右鍵按一下 監看式工作，然後選取**繫結 > 開啟專案**從內容功能表。

![繫結至專案開啟的工作](using-grunt/_static/bindings-project-open.png)

卸載並重新載入專案。 當專案重新載入時，監看式工作會開始自動執行。

## <a name="summary"></a>總結

Grunt 是功能強大的工作執行器，可以讓大部分的用戶端組建工作自動執行。 Grunt 會利用 NPM 傳遞其封裝和工具與 Visual Studio 整合的功能。 Visual Studio 工作執行器總管會偵測到組態檔變更，並提供方便的介面來執行工作、 檢視正在執行的工作，以及將工作繫結至 Visual Studio 事件。

## <a name="additional-resources"></a>其他資源

   * [使用 Gulp](using-gulp.md)
