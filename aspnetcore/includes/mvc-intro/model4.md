下表詳細列出 ASP.NET Core 程式碼產生器參數：

| 參數               | 說明|
| ----------------- | ------------ |
| -m  | 模型的名稱。 |
| -dc  | 資料內容。 |
| -udl | 使用預設的配置。 |
| --relativeFolderPath | 要建立檔案的相對輸出資料夾路徑。 |
| --useDefaultLayout | 應針對檢視使用預設的配置。 |
| --referenceScriptLibraries | 將 `_ValidationScriptsPartial` 新增至 Edit 和 Create 頁面 |

使用 `h` 參數取得 `aspnet-codegenerator controller` 命令的說明：

```console
dotnet aspnet-codegenerator controller -h
```

如需詳細資訊，請參閱 [dotnet aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator)
