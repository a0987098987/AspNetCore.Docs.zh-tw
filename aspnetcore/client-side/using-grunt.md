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
# <a name="use-grunt-in-aspnet-core"></a>在ASP.NET核心中使用"格魯特"

Grunt 是 JavaScript 工作執行者,自動執行文稿小化、TypeScript 編譯、程式碼品質"lint"工具、CSS 預處理器,以及支援客戶端開發所需的任何重複性工作。 在視覺工作室中完全支援 Grunt。

本示例使用空ASP.NET Core 專案作為起點,以演示如何從頭開始自動執行用戶端生成過程。

完成的範例清理目標部署目錄,合併 JavaScript 檔,檢查代碼品質,壓縮 JavaScript 檔內容並部署到 Web 應用程式的根目錄。 我們使用以下套件:

* **咕咕**:咕咕任務運行程式包。

* **放大縮小字型功能 放大縮小字型**功能

* **咕咕-contrib-jshint:** 一個外掛程式,它回顧了JavaScript代碼的品質。

* **咕咕-contrib-concat:** 將檔案合併到單個檔案中的外掛程式。

* **咕咕-連續-醜陋**:一個外掛程式,它使JAVAScript小數減小。

* **咕咕-連續觀察**:一個外掛程式,監視文件活動。

## <a name="preparing-the-application"></a>準備應用程式

首先,設置一個新的空 Web 應用程式並添加 TypeScript 範例檔。 TypeScript 檔案使用預設的 Visual Studio 設置自動編譯到 JavaScript 中,並且將成為我們使用 Grunt 處理的原材料。

1. 在視覺化工作室中,建立新`ASP.NET Web Application`。

2. 在 **"新建ASP.NET專案"** 對話框中,選擇"核心**空**"範本ASP.NET,然後單擊"確定"按鈕。

3. 在解決方案資源管理器中,查看專案結構。 該`\src`資料夾包括`wwwroot`空`Dependencies`節點和節點。

    ![空 Web 解決方案](using-grunt/_static/grunt-solution-explorer.png)

4. 向項目目錄添加名為`TypeScript`的新資料夾。

5. 在添加任何檔之前,請確保 Visual Studio 具有選中的 TypeScript 檔「儲存編譯」選項。 瀏覽到**工具** > **選項** > **文字編輯器** > **型態文稿** > **專案**:

    ![選項設定自動編譯 TypeScript 檔案](using-grunt/_static/typescript-options.png)

6. 右鍵按下`TypeScript`目錄,然後從上下文選單**中選擇「新增>新專案**。 選擇**JavaScript 檔案**項目並命名檔案 *「品味.ts」(* 請注意\*.ts 副檔名)。 將下面的 TypeScript 程式複製到檔案中(當您儲存時,一個新的*品味.js*檔將顯示與 JavaScript 源。

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. 將第二個檔案添加到**TypeScript**`Food.ts`目錄並命名它。 將下面的代碼複製到檔中。

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

接下來,將 NPM 配置為下載咕咕和咆哮的任務。

1. 在「解決方案資源管理器」中,右鍵單擊項目,然後從上下文菜單**中選擇「添加>新專案**」。。 選擇**NPM 設定檔**項目,保留預設名稱 *「包.json」,* 然後單擊 **「添加**」按鈕。

2. 在*包.json*檔中`devDependencies`,在 物件大括弧內輸入"grunt"。 從`grunt`"感知"清單中選擇,然後按 Enter 鍵。 Visual Studio 將引用咆哮的包名稱,並添加冒號。 在冒號清單的右側,從"感知"列表的頂部選擇包的最新穩定版本(如果未顯示 Intellisense,`Ctrl-Space`請按)。

    ![咕咕的理智](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > NPM 使用[語義版本控制](https://semver.org/)來組織依賴項。 語義版本控制(也稱為 SemVer)標識具有編號\<方案 主要>的包。\<次要>。\<補丁>。 Intellisense 僅顯示幾個常見選項,從而簡化了語義版本控制。 Intellisense 清單中的頂部專案(如上例中的 0.4.5)被視為包的最新穩定版本。 加斯特 (*) 符號與最新主要版本匹配,波浪線 (*) 與最新的次要版本匹配。 請參閱[NPM semver 版本解析器引用](https://www.npmjs.com/package/semver),作為 SemVer 提供的完整運算式指南。

3. 添加更多依賴\*項來載入咕咕-contrib-包,用於*清潔**、jshint、concat、ugly*和*手錶*,如下例所示*concat**uglify*。 版本不需要與示例匹配。

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

4. 保存*包.json*檔。

每個專案`devDependencies`的包將下載,以及每個包所需的任何檔。 通過在**解決方案資源管理器**中啟用 **「顯示所有檔」** 按鈕,可以在*node_modules*目錄中找到套件檔。

![咕咕node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> 如果需要,可以通過右鍵單`Dependencies\NPM`擊 並選擇「**還原包」** 功能表選項,在**解決方案資源管理器**中手動還原依賴項。

![還原包](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>設定葛籣特

Grunt 使用名為*Gruntfile.js*的清單進行配置,該清單定義、載入和註冊可手動執行的任務,或配置為根據 Visual Studio 中的事件自動執行。

1. 右鍵按一下項目並選擇「**新增新** > **專案**」。 選擇**JavaScript 檔**項目樣本,將名稱更改為*Gruntfile.js*,然後按下「**添加**」按鈕。

1. 將以下代碼加入*Gruntfile.js*。 該`initConfig`函數為每個包設置選項,模組的其餘部分載入和註冊任務。

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. 在`initConfig`函數內,添加任務的選項,`clean`如下例*Gruntfile.js*所示。 任務`clean`接受目錄字串陣列。 此任務從*wwwroot/lib*中刪除檔案,並刪除整個 */temp*目錄。

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. 在函數`initConfig`下方,`grunt.loadNpmTasks`向 添加調用。 這將使任務可以從可視化工作室運行。

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. 儲存*格倫特檔案.js*. 該檔應類似於下面的屏幕截圖。

    ![初始咕咕檔案](using-grunt/_static/gruntfile-js-initial.png)

1. 右鍵按*下 Gruntfile.js*並從上下文選單中選擇**工作執行器資源管理員**。 任務**運行者資源管理員**視窗將打開。

    ![工作執行器資源管理員選單](using-grunt/_static/task-runner-explorer-menu.png)

1. 驗證在`clean`**任務運行器資源管理器**中**的任務**下顯示的顯示。

    ![工作執行程式資源管理員工作清單](using-grunt/_static/task-runner-explorer-tasks.png)

1. 右鍵單擊乾淨任務,然後從上下文菜單中選擇 **「運行**」。。 命令視窗顯示任務的進度。

    ![工作執行器資源管理員執行乾淨工作](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > 尚沒有要清理的檔或目錄。 如果您願意,可以在解決方案資源管理器中手動創建它們,然後作為測試運行乾淨任務。

1. 在`initConfig`函數中,添加用於`concat`使用以下代碼的條目。

    屬性`src`陣列按應合併的順序出要合併的檔。 屬性`dest`將路徑分配給生成的組合檔。

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > 上述`all`代碼中的屬性是目標的名稱。 目標用於某些 Grunt 任務,以允許多個生成環境。 您可以使用 IntelliSense 查看內建目標或分配您自己的目標。

1. 使用下面的`jshint`代碼添加任務。

    jshint`code-quality`實用程式針對*暫存*目錄中找到的每個 JavaScript 檔執行。

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > 選項「-W069」是 jshint 在 JavaScript 使用括弧語法分配屬性而不是點`Tastes["Sweet"]`表示`Tastes.Sweet`法(即 而不是時)生成的錯誤。 該選項將關閉警告,以允許進程的其餘部分繼續。

1. 使用下面的`uglify`代碼添加任務。

    該任務對在臨時目錄中找到的*合併.js*檔進行分明,並在 wwwroot/lib 中按照標準命名約定*\<\>檔名 .min.js*創建結果檔。

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. 在調用`grunt.loadNpmTasks`該負`grunt-contrib-clean`載 下,使用下面的代碼包括對 jshint、concat 和 ugly 的相同調用。

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. 儲存*格倫特檔案.js*. 該檔應類似於下面的範例。

    ![完整的咕咕檔案範例](using-grunt/_static/gruntfile-js-complete.png)

1. 請注意,**工作執行者資源管理員**工作`clean``concat`清單`jshint``uglify`包括和 工作。 按順序運行每個任務,並在**解決方案資源管理器**中觀察結果。 每個任務都應運行,而不會出錯。

    ![工作執行器資源管理員執行每個工作](using-grunt/_static/task-runner-explorer-run-each-task.png)

    concat 工作將創建一個新的*組合.js*檔,並將其放入臨時目錄中。 任務`jshint`只是運行,不生成輸出。 任務`uglify`創建一個新的*組合.min.js*檔,並將其放入*wwwroot/lib*中。 完成後,解決方案應如下所示的螢幕截圖:

    ![所有工作後的解決方案資源管理員](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > 有關每個包的選項的詳細資訊,請造[https://www.npmjs.com/](https://www.npmjs.com/)訪並查找主頁搜索框中的包名稱。 例如,您可以查找咕咕-contrib-clean 包,以獲得解釋其所有參數的文檔連結。

### <a name="all-together-now"></a>齊聚一堂

使用 Grunt`registerTask()`方法按特定順序運行一系列任務。 例如,要按順序運行上述示例步驟,請按> -> jhint ->丑陋,請向模組添加下面的代碼。 代碼應添加到與 loadNpmTasks() 調用相同的級別,在 initConfig 外部。

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

新任務顯示在別名任務下的任務運行者資源管理器中。 您可以像在其他任務一樣右鍵按一下並運行它。 工作`all`將依順序`clean`執行`concat``jshint` `uglify` 。

![別名咕咕任務](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>監看變更

任務`watch`監視檔和目錄。 監視在檢測到更改時自動觸發任務。 將下面的代碼添加到 initConfig 以監視 TypeScript\*目錄中對 .js 檔的更改。 如果更改了 JavaScript`watch`檔`all`,將運行 該任務。

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

新增呼叫以`loadNpmTasks()`在工作執行`watch`器 資源管理員中顯示任務。

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

右鍵單擊任務運行器資源管理器中的監視任務,然後從上下文菜單中選擇"運行"。 顯示正在運行的監視任務的命令視窗將顯示"等待..."消息。 打開其中一個 TypeScript 檔,添加空格,然後儲存該檔。 這將觸發監視任務,並觸發其他任務按順序運行。 下面的屏幕截圖顯示了示例運行。

![執行工作輸出](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>繫結到視覺化工作室事件

除非希望在每次在 Visual Studio 中工作時手動啟動任務,否則請將任務綁定到**生成之前**、**生成後**,**清理**和**專案打開**事件。

綁定`watch`,以便每次打開 Visual Studio 時都會運行。 在任務運行者資源管理器中,右鍵單擊監視任務並從上下文菜單中選擇 **「綁定** > **專案打開**」。。

![將工作繫結到項目開啟](using-grunt/_static/bindings-project-open.png)

卸載並重新載入專案。 當專案再次載入時,監視任務將自動開始運行。

## <a name="summary"></a>摘要

Grunt 是一個強大的任務運行者,可用於自動執行大多數用戶端生成任務。 Grunt 利用 NPM 來交付其包,並具有與 Visual Studio 的工具集成功能。 Visual Studio 的任務執行者資源管理員可檢測對設定檔的更改,並提供一個方便的介面來執行任務、查看正在運行的任務並將任務綁定到 Visual Studio 事件。
