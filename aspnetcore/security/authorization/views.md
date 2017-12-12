---
title: "在 ASP.NET Core MVC 檢視為基礎的授權"
author: rick-anderson
description: "本文件將示範如何插入，並利用授權服務的 ASP.NET Core Razor 檢視內。"
keywords: "ASP.NET Core，授權 IAuthorizationService，Razor 授權"
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="a3f61-104">檢視為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="a3f61-104">View-based authorization</span></span>

<span data-ttu-id="a3f61-105">開發人員通常會想要顯示、 隱藏或修改目前的使用者識別為基礎的 UI。</span><span class="sxs-lookup"><span data-stu-id="a3f61-105">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="a3f61-106">您可以存取授權服務會在透過 MVC 檢視內[相依性插入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="a3f61-106">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="a3f61-107">若要插入 Razor 檢視授權服務，使用`@inject`指示詞：</span><span class="sxs-lookup"><span data-stu-id="a3f61-107">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="a3f61-108">如果您想要授權服務的每個檢視中，放入`@inject`到指示詞*_ViewImports.cshtml*檔案*檢視*目錄。</span><span class="sxs-lookup"><span data-stu-id="a3f61-108">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="a3f61-109">如需詳細資訊，請參閱[檢視的相依性插入](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="a3f61-109">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="a3f61-110">使用插入的授權服務叫用`AuthorizeAsync`方式完全相同時，您會檢查[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="a3f61-110">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3f61-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3f61-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3f61-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3f61-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="a3f61-113">在某些情況下，資源會是您的檢視模型。</span><span class="sxs-lookup"><span data-stu-id="a3f61-113">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="a3f61-114">叫用`AuthorizeAsync`方式完全相同時，您會檢查[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="a3f61-114">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3f61-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3f61-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3f61-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3f61-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="a3f61-117">在上述程式碼中，模型會傳遞做為原則評估應該採取的資源納入考量。</span><span class="sxs-lookup"><span data-stu-id="a3f61-117">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="a3f61-118">不要只依賴切換可見性的應用程式的 UI 元素的唯一授權檢查。</span><span class="sxs-lookup"><span data-stu-id="a3f61-118">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="a3f61-119">隱藏 UI 項目可能無法完全防止存取至其關聯之的控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="a3f61-119">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="a3f61-120">例如，請考慮前面的程式碼片段中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="a3f61-120">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="a3f61-121">使用者可以叫用`Edit`動作方法如果他或她知道之相對資源 URL 是*/Document/Edit/1*。</span><span class="sxs-lookup"><span data-stu-id="a3f61-121">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="a3f61-122">基於這個理由，`Edit`動作方法執行它自己的授權檢查。</span><span class="sxs-lookup"><span data-stu-id="a3f61-122">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
