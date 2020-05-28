---
<span data-ttu-id="b992c-101">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="b992c-101">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="b992c-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="b992c-102">'Blazor'</span></span>
- <span data-ttu-id="b992c-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="b992c-103">'Identity'</span></span>
- <span data-ttu-id="b992c-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="b992c-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="b992c-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="b992c-105">'Razor'</span></span>
- <span data-ttu-id="b992c-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="b992c-106">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="b992c-107">ASP.NET Core 負載/壓力測試</span><span class="sxs-lookup"><span data-stu-id="b992c-107">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="b992c-108">負載測試和壓力測試非常重要，可確保 web 應用程式的效能和可擴充性。</span><span class="sxs-lookup"><span data-stu-id="b992c-108">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="b992c-109">它們的目標會不同，即使它們經常共用類似的測試也一樣。</span><span class="sxs-lookup"><span data-stu-id="b992c-109">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="b992c-110">**負載測試**：測試應用程式是否可以在特定案例中處理指定的使用者負載，同時仍然滿足回應目標。</span><span class="sxs-lookup"><span data-stu-id="b992c-110">**Load tests**: Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="b992c-111">應用程式會在正常情況下執行。</span><span class="sxs-lookup"><span data-stu-id="b992c-111">The app is run under normal conditions.</span></span>

<span data-ttu-id="b992c-112">**壓力測試**：在極端情況下執行時，測試應用程式穩定性通常長時間執行。</span><span class="sxs-lookup"><span data-stu-id="b992c-112">**Stress tests**: Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="b992c-113">測試會在應用程式上放置高使用者負載、尖峰或逐漸增加的負載，或限制應用程式的運算資源。</span><span class="sxs-lookup"><span data-stu-id="b992c-113">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="b992c-114">壓力測試會判斷壓力不足的應用程式是否可以從失敗中復原，並正常地回到預期的行為。</span><span class="sxs-lookup"><span data-stu-id="b992c-114">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="b992c-115">在壓力之下，應用程式不會在正常情況下執行。</span><span class="sxs-lookup"><span data-stu-id="b992c-115">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="b992c-116">Visual Studio 2019 是具有負載測試功能 Visual Studio 的最後一個版本。</span><span class="sxs-lookup"><span data-stu-id="b992c-116">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="b992c-117">對於未來需要負載測試工具的客戶，我們建議替代工具，例如 Apache JMeter、Akamai CloudTest 和 BlazeMeter。</span><span class="sxs-lookup"><span data-stu-id="b992c-117">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="b992c-118">如需詳細資訊，請參閱[Visual Studio 2019 版本](/visualstudio/releases/2019/release-notes-v16.0#test-tools)資訊。</span><span class="sxs-lookup"><span data-stu-id="b992c-118">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="b992c-119">Visual Studio 工具</span><span class="sxs-lookup"><span data-stu-id="b992c-119">Visual Studio tools</span></span>

<span data-ttu-id="b992c-120">Visual Studio 可讓使用者建立、開發和調試 web 效能和負載測試。</span><span class="sxs-lookup"><span data-stu-id="b992c-120">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="b992c-121">有一個選項可用來在網頁瀏覽器中錄製動作，以建立測試。</span><span class="sxs-lookup"><span data-stu-id="b992c-121">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="b992c-122">如需如何使用 Visual Studio 2017 建立、設定及執行負載測試專案的詳細資訊，請參閱[快速入門：建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)。</span><span class="sxs-lookup"><span data-stu-id="b992c-122">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="b992c-123">負載測試可以設定為在內部部署執行，或使用 Azure DevOps 在雲端中執行。</span><span class="sxs-lookup"><span data-stu-id="b992c-123">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="b992c-124">協力廠商工具</span><span class="sxs-lookup"><span data-stu-id="b992c-124">Third-party tools</span></span>

<span data-ttu-id="b992c-125">下列清單包含具有各種功能集的協力廠商 web 效能工具：</span><span class="sxs-lookup"><span data-stu-id="b992c-125">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="b992c-126">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="b992c-126">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="b992c-127">ApacheBench （ab）</span><span class="sxs-lookup"><span data-stu-id="b992c-127">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="b992c-128">Gatling</span><span class="sxs-lookup"><span data-stu-id="b992c-128">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="b992c-129">k6</span><span class="sxs-lookup"><span data-stu-id="b992c-129">k6</span></span>](https://k6.io)
* [<span data-ttu-id="b992c-130">Locust</span><span class="sxs-lookup"><span data-stu-id="b992c-130">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="b992c-131">West 風 WebSurge</span><span class="sxs-lookup"><span data-stu-id="b992c-131">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="b992c-132">Netling</span><span class="sxs-lookup"><span data-stu-id="b992c-132">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="b992c-133">Vegeta</span><span class="sxs-lookup"><span data-stu-id="b992c-133">Vegeta</span></span>](https://github.com/tsenart/vegeta)

