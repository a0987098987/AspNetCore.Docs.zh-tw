---
title: ASP.NET Core 檢視中的相依性插入
author: ardalis
description: 了解 ASP.NET Core 如何支援 MVC 檢視中的相依性插入。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: 9b437d27a8d391db4533596674d144628a0c10b1
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207053"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a><span data-ttu-id="6a336-103">ASP.NET Core 檢視中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="6a336-103">Dependency injection into views in ASP.NET Core</span></span>

<span data-ttu-id="6a336-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6a336-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6a336-105">ASP.NET Core 支援檢視中的[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="6a336-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="6a336-106">這可用於檢視特定服務，例如僅適用於填入檢視項目所需的當地語系化或資料。</span><span class="sxs-lookup"><span data-stu-id="6a336-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="6a336-107">您應該嘗試維護控制器與檢視之間的 [Separation of Concerns](http://deviq.com/separation-of-concerns/) (關注點分離)。</span><span class="sxs-lookup"><span data-stu-id="6a336-107">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="6a336-108">您檢視所顯示的大部分資料應該都是從控制器傳入。</span><span class="sxs-lookup"><span data-stu-id="6a336-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="6a336-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6a336-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="6a336-110">簡單範例</span><span class="sxs-lookup"><span data-stu-id="6a336-110">A Simple Example</span></span>

<span data-ttu-id="6a336-111">您可以使用 `@inject` 指示詞，將服務插入至檢視。</span><span class="sxs-lookup"><span data-stu-id="6a336-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="6a336-112">您可以將 `@inject` 視為將屬性新增至檢視，並使用 DI 來填入屬性。</span><span class="sxs-lookup"><span data-stu-id="6a336-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="6a336-113">`@inject` 的語法：`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="6a336-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="6a336-114">運作中 `@inject` 範例：</span><span class="sxs-lookup"><span data-stu-id="6a336-114">An example of `@inject` in action:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="6a336-115">此檢視會顯示 `ToDoItem` 執行個體清單，以及顯示整體統計資料的摘要。</span><span class="sxs-lookup"><span data-stu-id="6a336-115">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="6a336-116">摘要是從插入的 `StatisticsService` 中填入。</span><span class="sxs-lookup"><span data-stu-id="6a336-116">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="6a336-117">在 *Startup.cs* 的 `ConfigureServices` 中，註冊此服務以進行相依性插入：</span><span class="sxs-lookup"><span data-stu-id="6a336-117">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="6a336-118">`StatisticsService` 會對一組 `ToDoItem` 執行個體執行一些計算，以透過存放庫進行存取：</span><span class="sxs-lookup"><span data-stu-id="6a336-118">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

<span data-ttu-id="6a336-119">範例存放庫會使用記憶體內部集合。</span><span class="sxs-lookup"><span data-stu-id="6a336-119">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="6a336-120">上面顯示的實作 (作用於記憶體中的所有資料) 不建議用於大型可從遠端存取的資料集。</span><span class="sxs-lookup"><span data-stu-id="6a336-120">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="6a336-121">此範例所顯示的資料來自繫結至檢視的模型，以及插入至檢視的服務：</span><span class="sxs-lookup"><span data-stu-id="6a336-121">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![To Do 檢視，列出項目總數、已完成項目、平均優先順序 ，以及具有其優先順序層級和指出完成之布林值的工作清單。](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="6a336-123">填入查閱資料</span><span class="sxs-lookup"><span data-stu-id="6a336-123">Populating Lookup Data</span></span>

<span data-ttu-id="6a336-124">檢視插入可以用來填入 UI 項目中的選項，例如下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="6a336-124">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="6a336-125">請考慮使用者設定檔表單，其中包含指定性別、狀態和其他喜好設定的選項。</span><span class="sxs-lookup"><span data-stu-id="6a336-125">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="6a336-126">使用標準 MVC 方法轉譯這類表單時，需要控制器要求所有這些選項集的資料存取服務，然後將已繫結的每個選項集填入模型或 `ViewBag` 中。</span><span class="sxs-lookup"><span data-stu-id="6a336-126">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="6a336-127">另一種方法會將服務直接插入至檢視，以取得選項。</span><span class="sxs-lookup"><span data-stu-id="6a336-127">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="6a336-128">這會將控制器所需的程式碼數量降到最低，並將此檢視項目建構邏輯移至檢視本身。</span><span class="sxs-lookup"><span data-stu-id="6a336-128">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="6a336-129">要顯示設定檔編輯表單的控制器動作，只需要將設定檔執行個體傳遞給表單：</span><span class="sxs-lookup"><span data-stu-id="6a336-129">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="6a336-130">用來更新這些喜好設定的 HTML 表單包括三個屬性的下拉式清單：</span><span class="sxs-lookup"><span data-stu-id="6a336-130">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![[更新設定檔] 檢視，內含允許輸入名稱、性別、狀態和偏愛顏色的表單。](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="6a336-132">這些清單會填入已插入至檢視的服務：</span><span class="sxs-lookup"><span data-stu-id="6a336-132">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="6a336-133">`ProfileOptionsService` 是設計成只提供此表單所需資料的 UI 層級服務：</span><span class="sxs-lookup"><span data-stu-id="6a336-133">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> <span data-ttu-id="6a336-134">請不要忘記在 `Startup.ConfigureServices` 中註冊您透過相依性插入所要求的類型。</span><span class="sxs-lookup"><span data-stu-id="6a336-134">Don't forget to register types you request through dependency injection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6a336-135">未註冊的類型會在執行階段擲回例外狀況，因為服務提供者是透過 [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice) 內部查詢。</span><span class="sxs-lookup"><span data-stu-id="6a336-135">An unregistered type throws an exception at runtime because the service provider is internally queried via [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span></span>

## <a name="overriding-services"></a><span data-ttu-id="6a336-136">覆寫服務</span><span class="sxs-lookup"><span data-stu-id="6a336-136">Overriding Services</span></span>

<span data-ttu-id="6a336-137">除了插入新的服務之外，這項技術也可以用來覆寫頁面上先前插入的服務。</span><span class="sxs-lookup"><span data-stu-id="6a336-137">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="6a336-138">下圖顯示第一個範例中所使用頁面上的所有可用欄位：</span><span class="sxs-lookup"><span data-stu-id="6a336-138">The figure below shows all of the fields available on the page used in the first example:</span></span>

![已鍵入 @ symbol listing Html、Component、StatsService 和 URL 欄位上的 Intellisense 操作功能表](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="6a336-140">如您所見，預設欄位包括 `Html`、`Component` 和 `Url` (以及我們插入的 `StatsService`)。</span><span class="sxs-lookup"><span data-stu-id="6a336-140">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="6a336-141">例如，如果您要將預設 HTML 協助程式取代為您自己的協助程式，則使用 `@inject` 就可以輕鬆地達成：</span><span class="sxs-lookup"><span data-stu-id="6a336-141">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="6a336-142">如果您想要擴充現有服務，則只需要在繼承自現有實作或自行包裝現有實作時使用這項技術。</span><span class="sxs-lookup"><span data-stu-id="6a336-142">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="6a336-143">請參閱</span><span class="sxs-lookup"><span data-stu-id="6a336-143">See Also</span></span>

* <span data-ttu-id="6a336-144">Simon Timms 部落格：[將查閱資料放入檢視中](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="6a336-144">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
