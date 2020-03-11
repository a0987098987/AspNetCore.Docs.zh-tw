<a name="codegenerator"></a> 下表詳細列出 ASP.NET Core 程式碼產生器參數：

| 參數               | 描述|
| ----------------- | ------------ |
| -M  | 模型的名稱。 |
| -dc  | 要使用的 `DbContext` 類別。 |
| -udl | 使用預設的配置。 |
| -outDir | 要建立檢視的相對輸出資料夾路徑。 |
| --referenceScriptLibraries | 將 `_ValidationScriptsPartial` 新增至 Edit 和 Create 頁面 |

使用 `h` 參數取得 `aspnet-codegenerator razorpage` 命令的說明：

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

如需詳細資訊，請參閱[dotnet aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator)。
