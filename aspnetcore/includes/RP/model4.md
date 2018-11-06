下表詳細列出 ASP.NET Core 程式碼產生器參數：

| 參數               | 描述|
| ----------------- | ------------ |
| -m  | 模型的名稱。 |
| -dc  | 資料內容。 |
| -udl | 使用預設的配置。 |
| -outDir | 要建立檢視的相對輸出資料夾路徑。 |
| --referenceScriptLibraries | 將 `_ValidationScriptsPartial` 新增至 Edit 和 Create 頁面 |

使用 `h` 參數取得 `aspnet-codegenerator razorpage` 命令的說明：

```console
dotnet aspnet-codegenerator razorpage -h
```

<a name="test"></a>

### <a name="test-the-app"></a>測試應用程式

* 執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL (`http://localhost:port/Movies`)。
* 測試 **Create** 連結。

  ![建立頁面](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* 測試 **Edit**、**Details** 和 **Delete** 連結。

如果您收到類似下列的錯誤，請確認您已執行移轉並更新資料庫：

`An unhandled exception occurred while processing the request. 'no such table: Movie'.`
