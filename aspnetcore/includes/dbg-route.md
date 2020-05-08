## <a name="debug-diagnostics"></a><span data-ttu-id="a7efd-101">偵錯工具診斷</span><span class="sxs-lookup"><span data-stu-id="a7efd-101">Debug diagnostics</span></span>

<span data-ttu-id="a7efd-102">如需詳細的路由診斷輸出`Logging:LogLevel:Microsoft` ， `Debug`請將設定為。</span><span class="sxs-lookup"><span data-stu-id="a7efd-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="a7efd-103">例如，在開發環境中，設定*appsettings。開發. json*：</span><span class="sxs-lookup"><span data-stu-id="a7efd-103">For example, in the development environment, set *appsettings.Development.json*:</span></span>

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