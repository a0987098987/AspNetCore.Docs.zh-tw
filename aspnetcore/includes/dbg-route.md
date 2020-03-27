## <a name="debug-diagnostics"></a>偵錯工具診斷

如需詳細的路由診斷輸出，請將 `Logging:LogLevel:Microsoft` 設定為 `Debug`。 例如，在開發環境中，設定*appsettings。開發. json*：

```JSON
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Debug",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}