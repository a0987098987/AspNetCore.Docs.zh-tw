## <a name="debug-diagnostics"></a><span data-ttu-id="ac385-101">除錯診斷</span><span class="sxs-lookup"><span data-stu-id="ac385-101">Debug diagnostics</span></span>

<span data-ttu-id="ac385-102">對於詳細的路由診斷輸出,設定為`Logging:LogLevel:Microsoft``Debug`。</span><span class="sxs-lookup"><span data-stu-id="ac385-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="ac385-103">例如,在開發環境中,設置*應用設置。發展.json*:</span><span class="sxs-lookup"><span data-stu-id="ac385-103">For example, in the development environment, set *appsettings.Development.json*:</span></span>

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