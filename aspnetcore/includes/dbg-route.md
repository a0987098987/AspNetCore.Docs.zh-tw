## <a name="debug-diagnostics"></a>偵錯工具診斷

如需詳細的路由診斷輸出`Logging:LogLevel:Microsoft` ， `Debug`請將設定為。 例如，在開發環境中，設定*appsettings。開發. json*：

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