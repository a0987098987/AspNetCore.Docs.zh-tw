## <a name="debug-diagnostics"></a>偵錯工具診斷

如需詳細的路由診斷輸出，請將設定 `Logging:LogLevel:Microsoft` 為 `Debug` 。 在開發環境中，將appsettings.Development.js中的記錄層級設定為*on*：

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Debug",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```
