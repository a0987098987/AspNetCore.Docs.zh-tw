## <a name="register-the-database-context"></a><span data-ttu-id="66f24-101">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="66f24-101">Register the database context</span></span>

<span data-ttu-id="66f24-102">在此步驟中，資料庫內容是使用[相依性插入](xref:fundamentals/dependency-injection)容器來註冊。</span><span class="sxs-lookup"><span data-stu-id="66f24-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="66f24-103">以相依性插入 (DI) 容器註冊的服務 (如 DB 內容) 可供控制器使用。</span><span class="sxs-lookup"><span data-stu-id="66f24-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="66f24-104">使用內建的[相依性插入](xref:fundamentals/dependency-injection)支援，向服務容器註冊 DB 內容。</span><span class="sxs-lookup"><span data-stu-id="66f24-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="66f24-105">以下列程式碼取代 *Startup.cs* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="66f24-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]

::: moniker-end  

<span data-ttu-id="66f24-106">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="66f24-106">The preceding code:</span></span>

* <span data-ttu-id="66f24-107">移除未使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="66f24-107">Removes the unused code.</span></span>
* <span data-ttu-id="66f24-108">指定將記憶體內部資料庫插入服務容器中。</span><span class="sxs-lookup"><span data-stu-id="66f24-108">Specifies an in-memory database is injected into the service container.</span></span>
