---
title: 使用 Web API 慣例
author: pranavkm
description: 深入了解 ASP.NET Core 中的 Web API 慣例。
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 892fb8898c78a1645c766544715a8256462207c6
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249382"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="03964-103">使用 Web API 慣例</span><span class="sxs-lookup"><span data-stu-id="03964-103">Use web API conventions</span></span>

<span data-ttu-id="03964-104">作者：[Pranav Krishnamoorthy](https://github.com/pranavkm) 和 [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="03964-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="03964-105">ASP.NET Core 2.2 和更新版本包含擷取常見 [API 文件](xref:tutorials/web-api-help-pages-using-swagger)的方式，並將其套用至多個動作、控制器，或組件內的所有控制器。</span><span class="sxs-lookup"><span data-stu-id="03964-105">ASP.NET Core 2.2 and later includes a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="03964-106">Web API 慣例為使用 [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) 裝飾個別動作的替代品。</span><span class="sxs-lookup"><span data-stu-id="03964-106">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span>

<span data-ttu-id="03964-107">慣例可讓您：</span><span class="sxs-lookup"><span data-stu-id="03964-107">A convention allows you to:</span></span>

* <span data-ttu-id="03964-108">定義從特定動作類型傳回的最常見傳回型別和狀態碼。</span><span class="sxs-lookup"><span data-stu-id="03964-108">Define the most common return types and status codes returned from a specific type of action.</span></span>
* <span data-ttu-id="03964-109">識別偏離已定義標準的動作。</span><span class="sxs-lookup"><span data-stu-id="03964-109">Identify actions that deviate from the defined standard.</span></span>

<span data-ttu-id="03964-110">ASP.NET Core MVC 2.2 和更新版本在 `Microsoft.AspNetCore.Mvc.DefaultApiConventions` 中包含一組預設慣例。</span><span class="sxs-lookup"><span data-stu-id="03964-110">ASP.NET Core MVC 2.2 and later includes a set of default conventions in `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="03964-111">這些慣例是以 ASP.NET Core **API** 專案範本中所提供的控制器 (*ValuesController.cs*) 為基礎。</span><span class="sxs-lookup"><span data-stu-id="03964-111">The conventions are based on the controller (*ValuesController.cs*) provided in the ASP.NET Core **API** project template.</span></span> <span data-ttu-id="03964-112">若動作遵循範本中的模式，就應該可以成功使用預設慣例。</span><span class="sxs-lookup"><span data-stu-id="03964-112">If your actions follow the patterns in the template, you should be successful using the default conventions.</span></span> <span data-ttu-id="03964-113">如果預設慣例不符合您的需求，請參閱[建立 Web API 慣例](#create-web-api-conventions)。</span><span class="sxs-lookup"><span data-stu-id="03964-113">If the default conventions don't meet your needs, see [Create web API conventions](#create-web-api-conventions).</span></span>

<span data-ttu-id="03964-114">在執行階段，<xref:Microsoft.AspNetCore.Mvc.ApiExplorer> 了解慣例。</span><span class="sxs-lookup"><span data-stu-id="03964-114">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="03964-115">`ApiExplorer` 是 MVC 與 [OpenAPI](https://www.openapis.org/) (也稱為 Swagger) 文件產生器通訊的抽象層。</span><span class="sxs-lookup"><span data-stu-id="03964-115">`ApiExplorer` is MVC's abstraction to communicate with [OpenAPI](https://www.openapis.org/) (also known as Swagger) document generators.</span></span> <span data-ttu-id="03964-116">來自套用慣例的屬性會與動作建立關聯，且包含在動作的 OpenAPI 文件中。</span><span class="sxs-lookup"><span data-stu-id="03964-116">Attributes from the applied convention are associated with an action and are included in the action's OpenAPI documentation.</span></span> <span data-ttu-id="03964-117">[API 分析器](xref:web-api/advanced/analyzers)也了解慣例。</span><span class="sxs-lookup"><span data-stu-id="03964-117">[API analyzers](xref:web-api/advanced/analyzers) also understand conventions.</span></span> <span data-ttu-id="03964-118">若您的動作為非傳統的 (例如，其傳回未由套用慣例所記載的狀態碼)，就會出現警告，建議您記載狀態碼。</span><span class="sxs-lookup"><span data-stu-id="03964-118">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), a warning encourages you to document the status code.</span></span>

<span data-ttu-id="03964-119">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="03964-119">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="03964-120">套用 Web API 慣例</span><span class="sxs-lookup"><span data-stu-id="03964-120">Apply web API conventions</span></span>

<span data-ttu-id="03964-121">慣例不會進行撰寫；各個動作可能僅會與一個慣例建立關聯。</span><span class="sxs-lookup"><span data-stu-id="03964-121">Conventions don't compose; each action may be associated with exactly one convention.</span></span> <span data-ttu-id="03964-122">相較於較不特定的慣例，更特定的慣例擁有較高的優先順序。</span><span class="sxs-lookup"><span data-stu-id="03964-122">More specific conventions take precedence over less specific conventions.</span></span> <span data-ttu-id="03964-123">當兩個以上相同屬性的慣例套用至動作時，慣例就具有不確定性。</span><span class="sxs-lookup"><span data-stu-id="03964-123">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="03964-124">下列選項的作用為將慣例套用至動作，其順序為從最特定到最不特定：</span><span class="sxs-lookup"><span data-stu-id="03964-124">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="03964-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; 會套用至個別動作，並會指定慣例型別及其套用的慣例方法。</span><span class="sxs-lookup"><span data-stu-id="03964-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span>

    <span data-ttu-id="03964-126">在下列範例中，預設慣例類型的 `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` 慣例方法會套用至 `Update` 動作：</span><span class="sxs-lookup"><span data-stu-id="03964-126">In the following example, the default convention type's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    <span data-ttu-id="03964-127">`Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` 慣例方法會將下列屬性套用至動作：</span><span class="sxs-lookup"><span data-stu-id="03964-127">The `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method applies the following attributes to the action:</span></span>

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(204)]
    [ProducesResponseType(404)]
    [ProducesResponseType(400)]
    ```

<span data-ttu-id="03964-128">如需有關 `[ProducesDefaultResponseType]`,的詳細資訊，請參閱[預設回應](https://swagger.io/docs/specification/describing-responses/#default) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="03964-128">For more information on `[ProducesDefaultResponseType]`, see [Default Response](https://swagger.io/docs/specification/describing-responses/#default).</span></span>

1. <span data-ttu-id="03964-129">套用到控制器的 `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` &mdash; 會將指定的慣例類型套用至控制器上的所有動作。</span><span class="sxs-lookup"><span data-stu-id="03964-129">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the specified convention type to all actions on the controller.</span></span> <span data-ttu-id="03964-130">慣例方法由提示所裝飾，且該提示會決定要套用慣例方法的動作。</span><span class="sxs-lookup"><span data-stu-id="03964-130">A convention method is decorated with hints that determine the actions to which the convention method applies.</span></span> <span data-ttu-id="03964-131">如需提示的詳細資訊，請參閱[建立 Web API 慣例](#create-web-api-conventions)。</span><span class="sxs-lookup"><span data-stu-id="03964-131">For more information on hints, see [Create web API conventions](#create-web-api-conventions)).</span></span>

    <span data-ttu-id="03964-132">在下列範例中，會將一組預設的慣例套用至 *ContactsConventionController* 中的所有動作：</span><span class="sxs-lookup"><span data-stu-id="03964-132">In the following example, the default set of conventions is applied to all actions in *ContactsConventionController*:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. <span data-ttu-id="03964-133">套用至組件的 `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` &mdash; 會將指定的慣例類型套用至目前組件中的所有控制器。</span><span class="sxs-lookup"><span data-stu-id="03964-133">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the specified convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="03964-134">建議您將組件層級屬性套用至 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="03964-134">As a recommendation, apply assembly-level attributes to the `Startup` class.</span></span>

    <span data-ttu-id="03964-135">在下列範例中，會將一組預設的慣例套用至組件中的所有控制器：</span><span class="sxs-lookup"><span data-stu-id="03964-135">In the following example, the default set of conventions is applied to all controllers in the assembly:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="03964-136">建立 Web API 慣例</span><span class="sxs-lookup"><span data-stu-id="03964-136">Create web API conventions</span></span>

<span data-ttu-id="03964-137">如果預設 API 慣例不符合您的需求，請建立您自己的慣例。</span><span class="sxs-lookup"><span data-stu-id="03964-137">If the default API conventions don't meet your needs, create your own conventions.</span></span> <span data-ttu-id="03964-138">慣例為：</span><span class="sxs-lookup"><span data-stu-id="03964-138">A convention is:</span></span>

* <span data-ttu-id="03964-139">具有方法的靜態類型。</span><span class="sxs-lookup"><span data-stu-id="03964-139">A static type with methods.</span></span>
* <span data-ttu-id="03964-140">能夠定義動作的[回應類型](#response-types)和[命名需求](#naming-requirements)。</span><span class="sxs-lookup"><span data-stu-id="03964-140">Capable of defining [response types](#response-types) and [naming requirements](#naming-requirements) on actions.</span></span>

### <a name="response-types"></a><span data-ttu-id="03964-141">回應類型</span><span class="sxs-lookup"><span data-stu-id="03964-141">Response types</span></span>

<span data-ttu-id="03964-142">使用 `[ProducesResponseType]` 或 `[ProducesDefaultResponseType]` 屬性標註這些方法。</span><span class="sxs-lookup"><span data-stu-id="03964-142">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span> <span data-ttu-id="03964-143">例如：</span><span class="sxs-lookup"><span data-stu-id="03964-143">For example:</span></span>

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

<span data-ttu-id="03964-144">如果沒有更特定的中繼資料屬性，將此慣例套用至組件可確保：</span><span class="sxs-lookup"><span data-stu-id="03964-144">If more specific metadata attributes are absent, applying this convention to an assembly enforces that:</span></span>

* <span data-ttu-id="03964-145">將慣例方法套用至任何名為 `Find` 的動作。</span><span class="sxs-lookup"><span data-stu-id="03964-145">The convention method applies to any action named `Find`.</span></span>
* <span data-ttu-id="03964-146">`Find` 動作中有名為 `id` 的參數。</span><span class="sxs-lookup"><span data-stu-id="03964-146">A parameter named `id` is present on the `Find` action.</span></span>

### <a name="naming-requirements"></a><span data-ttu-id="03964-147">命名需求</span><span class="sxs-lookup"><span data-stu-id="03964-147">Naming requirements</span></span>

<span data-ttu-id="03964-148">您可以將 `[ApiConventionNameMatch]` 和 `[ApiConventionTypeMatch]` 屬性套用至會決定其套用動作的慣例方法。</span><span class="sxs-lookup"><span data-stu-id="03964-148">The `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` attributes can be applied to the convention method that determines the actions to which they apply.</span></span> <span data-ttu-id="03964-149">例如：</span><span class="sxs-lookup"><span data-stu-id="03964-149">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

<span data-ttu-id="03964-150">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="03964-150">In the preceding example:</span></span>

* <span data-ttu-id="03964-151">套用至方法的 `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` 選項表示慣例會比對任何前置詞為 "Find" 的動作。</span><span class="sxs-lookup"><span data-stu-id="03964-151">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method indicates that the convention matches any action prefixed with "Find".</span></span> <span data-ttu-id="03964-152">相符動作範例包括 `Find`、`FindPet` 和 `FindById`。</span><span class="sxs-lookup"><span data-stu-id="03964-152">Examples of matching actions include `Find`, `FindPet`, and `FindById`.</span></span>
* <span data-ttu-id="03964-153">套用至參數的 `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` 表示慣例會比對只有一個以後置詞識別碼結尾之參數的方法。</span><span class="sxs-lookup"><span data-stu-id="03964-153">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention matches methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="03964-154">範例包括 `id` 或 `petId` 等參數。</span><span class="sxs-lookup"><span data-stu-id="03964-154">Examples include parameters such as `id` or `petId`.</span></span> <span data-ttu-id="03964-155">同樣地，`ApiConventionTypeMatch` 可套用至類型來限制參數類型。</span><span class="sxs-lookup"><span data-stu-id="03964-155">`ApiConventionTypeMatch` can be similarly applied to types to constrain the parameter type.</span></span> <span data-ttu-id="03964-156">`params[]` 引數表示其餘參數不需要明確相符。</span><span class="sxs-lookup"><span data-stu-id="03964-156">A `params[]` argument indicates remaining parameters that don't need to be explicitly matched.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="03964-157">其他資源</span><span class="sxs-lookup"><span data-stu-id="03964-157">Additional resources</span></span>

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
