## <a name="debug-diagnostics"></a><span data-ttu-id="be013-101">偵錯工具診斷</span><span class="sxs-lookup"><span data-stu-id="be013-101">Debug diagnostics</span></span>

<span data-ttu-id="be013-102">如需詳細的路由診斷輸出，請將設定 `Logging:LogLevel:Microsoft` 為 `Debug` 。</span><span class="sxs-lookup"><span data-stu-id="be013-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="be013-103">在開發環境中，將appsettings.Development.js中的記錄層級設定為*on*：</span><span class="sxs-lookup"><span data-stu-id="be013-103">In the development environment, set the log level in *appsettings.Development.json*:</span></span>

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
