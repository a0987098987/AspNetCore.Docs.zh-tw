> [!WARNING]
> <span data-ttu-id="72f06-101">**Catch-all**參數可能會因為路由中的[錯誤](https://github.com/dotnet/aspnetcore/issues/18677)而不正確地符合路由。</span><span class="sxs-lookup"><span data-stu-id="72f06-101">A **catch-all** parameter may match routes incorrectly due to a [bug](https://github.com/dotnet/aspnetcore/issues/18677) in routing.</span></span> <span data-ttu-id="72f06-102">受此錯誤影響的應用程式具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="72f06-102">Apps impacted by this bug have the following characteristics:</span></span>
>
> * <span data-ttu-id="72f06-103">全部捕捉路由，例如`{**slug}"`</span><span class="sxs-lookup"><span data-stu-id="72f06-103">A catch-all route, for example, `{**slug}"`</span></span>
> * <span data-ttu-id="72f06-104">Catch-all 路由無法符合它應該符合的要求。</span><span class="sxs-lookup"><span data-stu-id="72f06-104">The catch-all route fails to match requests it should match.</span></span>
> * <span data-ttu-id="72f06-105">移除其他路由會使「全部攔截」路由開始運作。</span><span class="sxs-lookup"><span data-stu-id="72f06-105">Removing other routes makes catch-all route start working.</span></span>
>
> <span data-ttu-id="72f06-106">如需遇到此 bug 的範例案例，請參閱 GitHub bug [18677](https://github.com/dotnet/aspnetcore/issues/18677)和[16579](https://github.com/dotnet/aspnetcore/issues/16579) 。</span><span class="sxs-lookup"><span data-stu-id="72f06-106">See GitHub bugs [18677](https://github.com/dotnet/aspnetcore/issues/18677) and [16579](https://github.com/dotnet/aspnetcore/issues/16579) for example cases that hit this bug.</span></span>
>
> <span data-ttu-id="72f06-107">已規劃此 bug 的加入宣告修正。</span><span class="sxs-lookup"><span data-stu-id="72f06-107">An opt-in fix for this bug is planned.</span></span> <span data-ttu-id="72f06-108">發行修補程式時，將會更新此檔。</span><span class="sxs-lookup"><span data-stu-id="72f06-108">This doc will be updated when the patch is released.</span></span> <span data-ttu-id="72f06-109">發行修補程式後，下列程式碼會設定內部交換器來修正此 bug：</span><span class="sxs-lookup"><span data-stu-id="72f06-109">When the patch is released, the following code will set an internal switch that fixes this bug:</span></span>
>
>```
>public static void Main(string[] args)
>{
>    AppContext.SetSwitch("Microsoft.AspNetCore.Routing.UseCorrectCatchAllBehavior", true);
>    CreateHostBuilder(args).Build().Run();
>}
>// Remaining code removed for brevity.
>```