<span data-ttu-id="5a744-101">讀取相關設定值的慣用方法是使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="5a744-101">The preferred way to read related configuration values is using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="5a744-102">例如，若要讀取下列設定值：</span><span class="sxs-lookup"><span data-stu-id="5a744-102">For example, to read the following configuration values:</span></span>

```json
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  }
```

<span data-ttu-id="5a744-103">建立下列 `PositionOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="5a744-103">Create the following `PositionOptions` class:</span></span>

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Options/PositionOptions.cs?name=snippet)]

<span data-ttu-id="5a744-104">選項類別：</span><span class="sxs-lookup"><span data-stu-id="5a744-104">An options class:</span></span>

* <span data-ttu-id="5a744-105">必須是非抽象，且具有公用無參數的函式。</span><span class="sxs-lookup"><span data-stu-id="5a744-105">Must be non-abstract with a public parameterless constructor.</span></span>
* <span data-ttu-id="5a744-106">類型的所有公用讀寫屬性都會系結。</span><span class="sxs-lookup"><span data-stu-id="5a744-106">All public read-write properties of the type are bound.</span></span>
* <span data-ttu-id="5a744-107">欄位***未***系結。</span><span class="sxs-lookup"><span data-stu-id="5a744-107">Fields are ***not*** bound.</span></span> <span data-ttu-id="5a744-108">在上述程式碼中， `Position` 不會系結。</span><span class="sxs-lookup"><span data-stu-id="5a744-108">In the preceding code, `Position` is not bound.</span></span> <span data-ttu-id="5a744-109">`Position`使用屬性，因此將類別系結 `"Position"` 至設定提供者時，不需要在應用程式中硬式編碼字串。</span><span class="sxs-lookup"><span data-stu-id="5a744-109">The `Position` property is used so the string `"Position"` doesn't need to be hard coded in the app when binding the class to a configuration provider.</span></span>

<span data-ttu-id="5a744-110">下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5a744-110">The following code:</span></span>

* <span data-ttu-id="5a744-111">呼叫[ConfigurationBinder](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*)將類別系結 `PositionOptions` 至 `Position` 區段。</span><span class="sxs-lookup"><span data-stu-id="5a744-111">Calls [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) to bind the `PositionOptions` class to the `Position` section.</span></span>
* <span data-ttu-id="5a744-112">顯示設定 `Position` 資料。</span><span class="sxs-lookup"><span data-stu-id="5a744-112">Displays the `Position` configuration data.</span></span>

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Pages/Test22.cshtml.cs?name=snippet)]

<span data-ttu-id="5a744-113">在上述程式碼中，根據預設，會讀取應用程式啟動後對 JSON 設定檔進行的變更。</span><span class="sxs-lookup"><span data-stu-id="5a744-113">In the preceding code, by default, changes to the JSON configuration file after the app has started are read.</span></span>

<span data-ttu-id="5a744-114">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)系結並傳回指定的型別。</span><span class="sxs-lookup"><span data-stu-id="5a744-114">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="5a744-115">`ConfigurationBinder.Get<T>`可能比使用更方便 `ConfigurationBinder.Bind` 。</span><span class="sxs-lookup"><span data-stu-id="5a744-115">`ConfigurationBinder.Get<T>` may be more convenient than using `ConfigurationBinder.Bind`.</span></span> <span data-ttu-id="5a744-116">下列程式碼示範如何搭配使用 `ConfigurationBinder.Get<T>` 與 `PositionOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="5a744-116">The following code shows how to use `ConfigurationBinder.Get<T>` with the `PositionOptions` class:</span></span>

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Pages/Test21.cshtml.cs?name=snippet)]

<span data-ttu-id="5a744-117">在上述程式碼中，根據預設，會讀取應用程式啟動後對 JSON 設定檔進行的變更。</span><span class="sxs-lookup"><span data-stu-id="5a744-117">In the preceding code, by default, changes to the JSON configuration file after the app has started are read.</span></span>

<span data-ttu-id="5a744-118">使用 [***選項] 模式***時，另一種方法是系結 `Position` 區段，並將它加入至相依性[插入服務容器](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="5a744-118">An alternative approach when using the ***options pattern*** is to bind the `Position` section and add it to the [dependency injection service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5a744-119">在下列程式碼中， `PositionOptions` 會新增至服務容器， <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 並系結至設定：</span><span class="sxs-lookup"><span data-stu-id="5a744-119">In the following code, `PositionOptions` is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Startup.cs?name=snippet)]

<span data-ttu-id="5a744-120">使用上述程式碼，下列程式碼會讀取位置選項：</span><span class="sxs-lookup"><span data-stu-id="5a744-120">Using the preceding code, the following code reads the position options:</span></span>

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Pages/Test2.cshtml.cs?name=snippet)]

<span data-ttu-id="5a744-121">在上述程式碼中，***不***會讀取在應用程式啟動後對 JSON 設定檔進行的變更。</span><span class="sxs-lookup"><span data-stu-id="5a744-121">In the preceding code, changes to the JSON configuration file after the app has started are ***not*** read.</span></span> <span data-ttu-id="5a744-122">若要在應用程式啟動後讀取變更，請使用[IOptionsSnapshot](xref:fundamentals/configuration/options#ios)。</span><span class="sxs-lookup"><span data-stu-id="5a744-122">To read changes after the app has started, use [IOptionsSnapshot](xref:fundamentals/configuration/options#ios).</span></span>
