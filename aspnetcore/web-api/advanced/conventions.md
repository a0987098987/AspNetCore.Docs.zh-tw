---
title: 使用 Web API 慣例
author: pranavkm
description: 深入了解 ASP.NET Core 中的 Web API 慣例。
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 023b8d09511aa42966e2a7d1c85e407bb6e79b0f
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635383"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="b53b3-103">使用 Web API 慣例</span><span class="sxs-lookup"><span data-stu-id="b53b3-103">Use web API conventions</span></span>

<span data-ttu-id="b53b3-104">ASP.NET Core 2.2 引進擷取常見 [API 文件](xref:tutorials/web-api-help-pages-using-swagger)的方式，並將其套用至多個動作、控制器，或組件內的所有控制器。</span><span class="sxs-lookup"><span data-stu-id="b53b3-104">ASP.NET Core 2.2 introduces a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="b53b3-105">Web API 慣例為使用 [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) 裝飾個別動作的替代品。</span><span class="sxs-lookup"><span data-stu-id="b53b3-105">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span> <span data-ttu-id="b53b3-106">它讓您可以定義最常見的「傳統」傳回型別及狀態碼，這兩者都是您選取套用至動作的慣例方法這個動作所傳回的。</span><span class="sxs-lookup"><span data-stu-id="b53b3-106">It allows you to define the most common "conventional" return types and status codes that you return from your action with a way to select the convention method that applies to an action.</span></span>

<span data-ttu-id="b53b3-107">根據預設，ASP.NET Core MVC 2.2 會隨附一組預設慣例，`Microsoft.AspNetCore.Mvc.DefaultApiConventions`。</span><span class="sxs-lookup"><span data-stu-id="b53b3-107">By default, ASP.NET Core MVC 2.2 ships with a set of default conventions, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="b53b3-108">慣例以 ASP.NET Core 建立的控制器為基礎。</span><span class="sxs-lookup"><span data-stu-id="b53b3-108">The conventions are based on the controller that ASP.NET Core scaffolds.</span></span> <span data-ttu-id="b53b3-109">若動作遵循 Scaffolding 產生的模式，就應該可以成功使用預設慣例。</span><span class="sxs-lookup"><span data-stu-id="b53b3-109">If your actions follow the pattern that scaffolding produces, you should be successful using the default conventions.</span></span>

<span data-ttu-id="b53b3-110">在執行階段，<xref:Microsoft.AspNetCore.Mvc.ApiExplorer> 了解慣例。</span><span class="sxs-lookup"><span data-stu-id="b53b3-110">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="b53b3-111">`ApiExplorer` 是與 Open API 文件產生器通訊之 MVC 的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="b53b3-111">`ApiExplorer` is MVC's abstraction to communicate with Open API document generators.</span></span> <span data-ttu-id="b53b3-112">來自套用慣例的屬性會與動作建立關聯，且包含在動作的 Swagger 文件中。</span><span class="sxs-lookup"><span data-stu-id="b53b3-112">Attributes from the applied convention get associated with an action and are included in the action's Swagger documentation.</span></span> <span data-ttu-id="b53b3-113">API 分析器也了解慣例。</span><span class="sxs-lookup"><span data-stu-id="b53b3-113">API analyzers also understand conventions.</span></span> <span data-ttu-id="b53b3-114">若您的動作為非傳統的 (例如，其傳回未由套用慣例所記載的狀態碼)，就會產生建議您將其記載的警告。</span><span class="sxs-lookup"><span data-stu-id="b53b3-114">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), it produces a warning, encouraging you to document it.</span></span>

<span data-ttu-id="b53b3-115">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b53b3-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="b53b3-116">套用 Web API 慣例</span><span class="sxs-lookup"><span data-stu-id="b53b3-116">Apply web API conventions</span></span>

<span data-ttu-id="b53b3-117">可透過三種方式套用慣例。</span><span class="sxs-lookup"><span data-stu-id="b53b3-117">There are three ways to apply a convention.</span></span> <span data-ttu-id="b53b3-118">慣例不會進行撰寫，各個動作可能僅會與一個慣例建立關聯。</span><span class="sxs-lookup"><span data-stu-id="b53b3-118">Conventions don't compose, each action may be associated with exactly one convention.</span></span> <span data-ttu-id="b53b3-119">相較於較不特定的慣例，更特定的慣例 (詳述如下) 擁有較高的優先順序。</span><span class="sxs-lookup"><span data-stu-id="b53b3-119">More specific conventions (detailed below) take precedence over less specific ones.</span></span> <span data-ttu-id="b53b3-120">當兩個以上相同屬性的慣例套用至動作時，慣例就具有不確定性。</span><span class="sxs-lookup"><span data-stu-id="b53b3-120">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="b53b3-121">下列選項的作用為將慣例套用至動作，其順序為從最特定到最不特定：</span><span class="sxs-lookup"><span data-stu-id="b53b3-121">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="b53b3-122">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; 會套用至個別動作，並會指定慣例型別及其套用的慣例方法。</span><span class="sxs-lookup"><span data-stu-id="b53b3-122">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span> <span data-ttu-id="b53b3-123">在下列範例中，慣例方法 `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` 會套用至 `Update` 動作：</span><span class="sxs-lookup"><span data-stu-id="b53b3-123">In the following sample, the convention method `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. <span data-ttu-id="b53b3-124">套用至控制器的 `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` &mdash; 會將慣例型別套用至控制器上的所有動作。</span><span class="sxs-lookup"><span data-stu-id="b53b3-124">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the convention type to all actions on the controller.</span></span> <span data-ttu-id="b53b3-125">慣例方法由提示所裝飾，且該提示會決定要套用的動作 (詳細資料於撰寫慣例中)。</span><span class="sxs-lookup"><span data-stu-id="b53b3-125">Convention methods are decorated with hints that determine which actions it would apply to (details as part of authoring conventions).</span></span> <span data-ttu-id="b53b3-126">例如: </span><span class="sxs-lookup"><span data-stu-id="b53b3-126">For example:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. <span data-ttu-id="b53b3-127">套用至組件的 `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` &mdash; 會將慣例型別套用至目前組件中的所有控制器。</span><span class="sxs-lookup"><span data-stu-id="b53b3-127">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="b53b3-128">例如: </span><span class="sxs-lookup"><span data-stu-id="b53b3-128">For example:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="b53b3-129">建立 Web API 慣例</span><span class="sxs-lookup"><span data-stu-id="b53b3-129">Create web API conventions</span></span>

<span data-ttu-id="b53b3-130">慣例是具有方法的靜態型別。</span><span class="sxs-lookup"><span data-stu-id="b53b3-130">A convention is a static type with methods.</span></span> <span data-ttu-id="b53b3-131">使用 `[ProducesResponseType]` 或 `[ProducesDefaultResponseType]` 屬性標註這些方法。</span><span class="sxs-lookup"><span data-stu-id="b53b3-131">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="b53b3-132">若慣例沒有其他更特定的中繼資料屬性，則將此慣例套用至組件會導致慣例方法套用至名稱為 `Find` 且僅有名為 `id` 之參數的任何動作。</span><span class="sxs-lookup"><span data-stu-id="b53b3-132">Applying this convention to an assembly results in the convention method applying to any action with the name `Find` and having exactly one parameter named `id`, as long as they don't have other more specific metadata attributes.</span></span>

<span data-ttu-id="b53b3-133">除了 `[ProducesResponseType]` 和 `[ProducesDefaultResponseType]` 之外，可以將 `[ApiConventionNameMatch]` 和 `[ApiConventionTypeMatch]` 套用至會決定其套用方法的慣例方法。</span><span class="sxs-lookup"><span data-stu-id="b53b3-133">In addition to `[ProducesResponseType]` and `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` can be applied to the convention method that determines the methods they apply to.</span></span> <span data-ttu-id="b53b3-134">例如: </span><span class="sxs-lookup"><span data-stu-id="b53b3-134">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* <span data-ttu-id="b53b3-135">套用至方法的 `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` 選項，會表示只要其前置詞為 "Find"，慣例就可與任何動作相符。</span><span class="sxs-lookup"><span data-stu-id="b53b3-135">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method, indicates that the convention can match any action as long as it's prefixed with "Find".</span></span> <span data-ttu-id="b53b3-136">這包含例如 `Find`、`FindPet` 或是 `FindById` 等方法。</span><span class="sxs-lookup"><span data-stu-id="b53b3-136">This includes methods such as `Find`, `FindPet`, or `FindById`.</span></span>
* <span data-ttu-id="b53b3-137">套用至參數的 `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix`，會表示慣例可與僅有一個以 Suffix 識別碼作為結尾之參數的方法相符。</span><span class="sxs-lookup"><span data-stu-id="b53b3-137">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention can match methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="b53b3-138">這包含例如 `id` 或 `petId` 等參數。</span><span class="sxs-lookup"><span data-stu-id="b53b3-138">This includes parameters such as `id` or `petId`.</span></span> <span data-ttu-id="b53b3-139">`ApiConventionTypeMatch` 可以相似地套用至型別，以限制參數的型別。</span><span class="sxs-lookup"><span data-stu-id="b53b3-139">`ApiConventionTypeMatch` can be similarly applied to types to constrain the type of the parameter.</span></span> <span data-ttu-id="b53b3-140">`params[]` 引數可用以表示不需要明確相符的剩餘參數。</span><span class="sxs-lookup"><span data-stu-id="b53b3-140">A `params[]` argument can be used to indicate remaining parameters that don't need to be explicitly matched.</span></span>
