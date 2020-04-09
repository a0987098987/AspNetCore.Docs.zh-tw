## <a name="debug-diagnostics"></a>除錯診斷

對於詳細的路由診斷輸出,設定為`Logging:LogLevel:Microsoft``Debug`。 例如,在開發環境中,設置*應用設置。發展.json*:

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