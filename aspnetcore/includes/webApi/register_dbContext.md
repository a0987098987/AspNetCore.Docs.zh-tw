## <a name="register-the-database-context"></a><span data-ttu-id="ad166-101">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="ad166-101">Register the database context</span></span>

<span data-ttu-id="ad166-102">為了將資料庫內容插入至控制器中，我們需要向[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="ad166-102">In order to inject the database context into the controller, we need to register it with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="ad166-103">使用內建的[相依性插入](xref:fundamentals/dependency-injection)支援，向服務容器登錄資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="ad166-103">Register the database context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ad166-104">以下列內容取代 *Startup.cs* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="ad166-104">Replace the contents of the *Startup.cs* file with the following:</span></span>

<span data-ttu-id="ad166-105">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]</span><span class="sxs-lookup"><span data-stu-id="ad166-105">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]</span></span>

<span data-ttu-id="ad166-106">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="ad166-106">The preceding code:</span></span>

* <span data-ttu-id="ad166-107">移除我們不會使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ad166-107">Removes the code we're not using.</span></span>
* <span data-ttu-id="ad166-108">指定將記憶體內部資料庫插入服務容器中。</span><span class="sxs-lookup"><span data-stu-id="ad166-108">Specifies an in-memory database is injected into the service container.</span></span>
