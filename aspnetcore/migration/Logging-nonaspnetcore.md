---
<span data-ttu-id="ab060-101">標題：從 Microsoft Extensions 進行遷移。記錄2.1 到2.2 或 3.0 author： pakrym 描述：瞭解如何遷移使用 Microsoft. Extensions 的 non-ASP.NET Core 應用程式。從2.1 到2.2 或3.0 進行記錄。</span><span class="sxs-lookup"><span data-stu-id="ab060-101">title: Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0 author: pakrym description: Learn how to migrate a non-ASP.NET Core application that uses Microsoft.Extensions.Logging from 2.1 to 2.2 or 3.0.</span></span>
<span data-ttu-id="ab060-102">pakrym ms. custom： mvc ms. date： 01/04/2019 no-loc：</span><span class="sxs-lookup"><span data-stu-id="ab060-102">ms.author: pakrym ms.custom: mvc ms.date: 01/04/2019 no-loc:</span></span>
- <span data-ttu-id="ab060-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ab060-103">'Blazor'</span></span>
- <span data-ttu-id="ab060-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ab060-104">'Identity'</span></span>
- <span data-ttu-id="ab060-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ab060-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="ab060-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ab060-106">'Razor'</span></span>
- <span data-ttu-id="ab060-107">' SignalR ' uid：遷移/記錄-nonaspnetcore</span><span class="sxs-lookup"><span data-stu-id="ab060-107">'SignalR' uid: migration/logging-nonaspnetcore</span></span>

---

# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="ab060-108">從 Microsoft 的副檔名遷移。記錄2.1 到2.2 或3。0</span><span class="sxs-lookup"><span data-stu-id="ab060-108">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="ab060-109">本文概述將使用 `Microsoft.Extensions.Logging` 從2.1 到2.2 或3.0 的 Non-ASP.NET Core 應用程式遷移的一般步驟。</span><span class="sxs-lookup"><span data-stu-id="ab060-109">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="ab060-110">2.1 至 2.2</span><span class="sxs-lookup"><span data-stu-id="ab060-110">2.1 to 2.2</span></span>

<span data-ttu-id="ab060-111">手動建立 `ServiceCollection` 並呼叫 `AddLogging` 。</span><span class="sxs-lookup"><span data-stu-id="ab060-111">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="ab060-112">2.1 範例：</span><span class="sxs-lookup"><span data-stu-id="ab060-112">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="ab060-113">2.2 範例：</span><span class="sxs-lookup"><span data-stu-id="ab060-113">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="ab060-114">2.1 到3。0</span><span class="sxs-lookup"><span data-stu-id="ab060-114">2.1 to 3.0</span></span>

<span data-ttu-id="ab060-115">在3.0 中，請使用 `LoggingFactory.Create` 。</span><span class="sxs-lookup"><span data-stu-id="ab060-115">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="ab060-116">2.1 範例：</span><span class="sxs-lookup"><span data-stu-id="ab060-116">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="ab060-117">3.0 範例：</span><span class="sxs-lookup"><span data-stu-id="ab060-117">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="ab060-118">其他資源</span><span class="sxs-lookup"><span data-stu-id="ab060-118">Additional resources</span></span>

* <span data-ttu-id="ab060-119">[Microsoft Extensions]： [[主控台] NuGet 套件](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)。</span><span class="sxs-lookup"><span data-stu-id="ab060-119">[Microsoft.Extensions.Logging.Console NuGet package](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/).</span></span>
* <xref:fundamentals/logging/index>
