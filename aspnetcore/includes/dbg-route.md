## <a name="debug-diagnostics"></a><span data-ttu-id="de9b0-101">偵錯工具診斷</span><span class="sxs-lookup"><span data-stu-id="de9b0-101">Debug diagnostics</span></span>

<span data-ttu-id="de9b0-102">如需詳細的路由診斷輸出，請將 `Logging:LogLevel:Microsoft` 設定為 `Debug`。</span><span class="sxs-lookup"><span data-stu-id="de9b0-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="de9b0-103">例如，在開發環境中，設定*appsettings。開發. json*：</span><span class="sxs-lookup"><span data-stu-id="de9b0-103">For example, in the development environment, set *appsettings.Development.json*:</span></span>

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