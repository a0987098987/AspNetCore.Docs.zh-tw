---
title: "檢視的相依性插入"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: ff3ded36a04fdbba0628dc5f223bfd865d58612a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="a28cb-103">檢視的相依性插入</span><span class="sxs-lookup"><span data-stu-id="a28cb-103">Dependency injection into views</span></span>

<span data-ttu-id="a28cb-104">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a28cb-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a28cb-105">ASP.NET Core 支援[相依性插入](xref:fundamentals/dependency-injection)到檢視表。</span><span class="sxs-lookup"><span data-stu-id="a28cb-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="a28cb-106">這可用於檢視特定的服務，例如當地語系化或只適用於填入檢視項目所需的資料。</span><span class="sxs-lookup"><span data-stu-id="a28cb-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="a28cb-107">您應該嘗試維護[的重要性分離](http://deviq.com/separation-of-concerns/)您控制器和檢視之間。</span><span class="sxs-lookup"><span data-stu-id="a28cb-107">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="a28cb-108">大部分的檢視所顯示的資料應該來自控制器中傳遞。</span><span class="sxs-lookup"><span data-stu-id="a28cb-108">Most of the data your views display should be passed in from the controller.</span></span>

[<span data-ttu-id="a28cb-109">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="a28cb-109">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)

## <a name="a-simple-example"></a><span data-ttu-id="a28cb-110">簡單的範例</span><span class="sxs-lookup"><span data-stu-id="a28cb-110">A Simple Example</span></span>

<span data-ttu-id="a28cb-111">您可以將服務插入檢視使用`@inject`指示詞。</span><span class="sxs-lookup"><span data-stu-id="a28cb-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="a28cb-112">您可以將`@inject`做為將屬性加入至您的檢視，並填入使用 DI 的屬性。</span><span class="sxs-lookup"><span data-stu-id="a28cb-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="a28cb-113">語法`@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="a28cb-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="a28cb-114">舉例來說，`@inject`動作：</span><span class="sxs-lookup"><span data-stu-id="a28cb-114">An example of `@inject` in action:</span></span>

<span data-ttu-id="a28cb-115">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]</span><span class="sxs-lookup"><span data-stu-id="a28cb-115">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]</span></span>

<span data-ttu-id="a28cb-116">此檢視會顯示一份`ToDoItem`執行個體，以及顯示整體統計資料摘要。</span><span class="sxs-lookup"><span data-stu-id="a28cb-116">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="a28cb-117">摘要已填入從插入`StatisticsService`。</span><span class="sxs-lookup"><span data-stu-id="a28cb-117">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="a28cb-118">此服務會註冊為中的相依性插入`ConfigureServices`中*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a28cb-118">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

<span data-ttu-id="a28cb-119">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]</span><span class="sxs-lookup"><span data-stu-id="a28cb-119">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]</span></span>

<span data-ttu-id="a28cb-120">`StatisticsService`執行一組上的一些計算`ToDoItem`執行個體，它會存取透過儲存機制：</span><span class="sxs-lookup"><span data-stu-id="a28cb-120">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

<span data-ttu-id="a28cb-121">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]</span><span class="sxs-lookup"><span data-stu-id="a28cb-121">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]</span></span>

<span data-ttu-id="a28cb-122">範例儲存機制會使用記憶體中的集合。</span><span class="sxs-lookup"><span data-stu-id="a28cb-122">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="a28cb-123">如上所示的實作 （它對所有記憶體中的資料） 都建議您不要針對大型、 可從遠端存取的資料集。</span><span class="sxs-lookup"><span data-stu-id="a28cb-123">The implementation shown above (which operates on all of the data in memory) is not recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="a28cb-124">此範例會顯示資料繫結至檢視的模型和檢視所插入的服務：</span><span class="sxs-lookup"><span data-stu-id="a28cb-124">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![若要執行檢視列出的項目總數，完成項目、 平均的優先順序和優先順序層級與布林值，指出完成的工作清單。](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="a28cb-126">填入查閱資料</span><span class="sxs-lookup"><span data-stu-id="a28cb-126">Populating Lookup Data</span></span>

<span data-ttu-id="a28cb-127">檢視資料隱碼可用來填入 UI 項目，例如下拉式清單中的選項。</span><span class="sxs-lookup"><span data-stu-id="a28cb-127">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="a28cb-128">請考慮使用者設定檔表格，其中包含指定性別、 狀態和其他喜好設定的選項。</span><span class="sxs-lookup"><span data-stu-id="a28cb-128">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="a28cb-129">呈現這類使用標準的 MVC 方法的表單，則會需要控制站要求的選項，這些集合的每個資料存取服務，然後再來擴展模型或`ViewBag`與每一組繫結的選項。</span><span class="sxs-lookup"><span data-stu-id="a28cb-129">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="a28cb-130">另一個方法會將服務直接插入檢視來取得選項。</span><span class="sxs-lookup"><span data-stu-id="a28cb-130">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="a28cb-131">這會將控制站，將這個檢視項目建構邏輯移至檢視本身所需的程式碼數量降到最低。</span><span class="sxs-lookup"><span data-stu-id="a28cb-131">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="a28cb-132">控制器的動作，以顯示 編輯表單只需要將表單的設定檔執行個體的設定檔：</span><span class="sxs-lookup"><span data-stu-id="a28cb-132">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

<span data-ttu-id="a28cb-133">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]</span><span class="sxs-lookup"><span data-stu-id="a28cb-133">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]</span></span>

<span data-ttu-id="a28cb-134">用來更新這些喜好設定的 HTML 表單包含了三個屬性的下拉式清單中：</span><span class="sxs-lookup"><span data-stu-id="a28cb-134">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![允許的名稱、 性別、 狀態和偏愛顏色的項目表單更新設定檔 檢視。](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="a28cb-136">這些清單會填入插入檢視服務：</span><span class="sxs-lookup"><span data-stu-id="a28cb-136">These lists are populated by a service that has been injected into the view:</span></span>

<span data-ttu-id="a28cb-137">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]</span><span class="sxs-lookup"><span data-stu-id="a28cb-137">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]</span></span>

<span data-ttu-id="a28cb-138">`ProfileOptionsService`是設計來提供此表單所需資料的 UI 層級服務：</span><span class="sxs-lookup"><span data-stu-id="a28cb-138">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

<span data-ttu-id="a28cb-139">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]</span><span class="sxs-lookup"><span data-stu-id="a28cb-139">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]</span></span>

>[!TIP]
> <span data-ttu-id="a28cb-140">別忘了註冊將要求中的相依性插入透過型別`ConfigureServices`方法中的*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="a28cb-140">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="a28cb-141">覆寫服務</span><span class="sxs-lookup"><span data-stu-id="a28cb-141">Overriding Services</span></span>

<span data-ttu-id="a28cb-142">除了插入新的服務，這項技術也可以用來覆寫先前插入的服務 頁面上。</span><span class="sxs-lookup"><span data-stu-id="a28cb-142">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="a28cb-143">下圖會顯示所有可用的欄位在第一個範例中使用 頁面上：</span><span class="sxs-lookup"><span data-stu-id="a28cb-143">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Intellisense 的內容功能表上的型別 @ 符號列出 Html、 元件、 StatsService 和 Url 的欄位](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="a28cb-145">如您所見，預設欄位包含`Html`， `Component`，和`Url`(以及`StatsService`我們插入)。</span><span class="sxs-lookup"><span data-stu-id="a28cb-145">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="a28cb-146">如果您要取代您自己的預設 HTML Helper 執行個體，無法輕鬆地，您使用`@inject`:</span><span class="sxs-lookup"><span data-stu-id="a28cb-146">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

<span data-ttu-id="a28cb-147">[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]</span><span class="sxs-lookup"><span data-stu-id="a28cb-147">[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]</span></span>

<span data-ttu-id="a28cb-148">如果您想要擴充現有的服務，您只可以在繼承自或換行以您自己的現有實作時，使用這項技術。</span><span class="sxs-lookup"><span data-stu-id="a28cb-148">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="a28cb-149">另請參閱</span><span class="sxs-lookup"><span data-stu-id="a28cb-149">See Also</span></span>

* <span data-ttu-id="a28cb-150">Simon Timms 部落格：[查閱資料放入您的檢視](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="a28cb-150">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
