## <a name="register-the-database-context"></a><span data-ttu-id="d4649-101">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="d4649-101">Register the database context</span></span>

<span data-ttu-id="d4649-102">在此步驟中，資料庫內容是使用[相依性插入](xref:fundamentals/dependency-injection)容器來註冊。</span><span class="sxs-lookup"><span data-stu-id="d4649-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d4649-103">以相依性插入 (DI) 容器註冊的服務 (如 DB 內容) 可供控制器使用。</span><span class="sxs-lookup"><span data-stu-id="d4649-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="d4649-104">使用內建的[相依性插入](xref:fundamentals/dependency-injection)支援，向服務容器註冊 DB 內容。</span><span class="sxs-lookup"><span data-stu-id="d4649-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d4649-105">以下列程式碼取代 *Startup.cs* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="d4649-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

<span data-ttu-id="d4649-106">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="d4649-106">The preceding code:</span></span>

* <span data-ttu-id="d4649-107">移除未使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d4649-107">Removes the code that is not used.</span></span>
* <span data-ttu-id="d4649-108">指定將記憶體內部資料庫插入服務容器中。</span><span class="sxs-lookup"><span data-stu-id="d4649-108">Specifies an in-memory database is injected into the service container.</span></span>