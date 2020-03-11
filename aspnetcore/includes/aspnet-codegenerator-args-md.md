<!-- Options common to Razor Pages and Controller -->
| 選項               | 描述|
| ----------------- | ------------ |
| --model 或 -m  | 要使用的模型類別。 |
| --dataContext 或 -dc  | 要使用的 `DbContext` 類別。 |
| --bootstrapVersion 或 -b  | 指定啟動程序版本。 有效值為 `3` 或 `4`。 預設值為 `4`。 如果需要但不存在，則會建立 *wwwroot* 目錄，其中包含指定版本的啟動程序檔案。 |
| --referenceScriptLibraries 或 -scripts |  所產生檢視中的參考指令碼程式庫。 將 `_ValidationScriptsPartial` 新增至 [編輯] 和 [建立] 頁面。 |
| --layout 或 -l | 要使用的自訂 [配置] 頁面。 |
| --useDefaultLayout 或 -udl | 針對檢視使用預設的配置。 |
| --force 或 -f | 覆寫現有檔案。 |
| --relativeFolderPath 或 -outDir | 專案的相對輸出資料夾路徑，其為產生檔案的位置。 若未指定，則會在專案資料夾中產生檔案。 |