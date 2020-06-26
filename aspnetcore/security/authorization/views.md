---
title: ASP.NET Core MVC 中以視圖為基礎的授權
author: rick-anderson
description: 本檔示範如何在 ASP.NET Core 視圖內插入和使用授權服務 Razor 。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/views
ms.openlocfilehash: ce2365dbdcffc8c44d0d9d7cc6c9d29d0fe1895c
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408925"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="ef4b5-103">ASP.NET Core MVC 中以視圖為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="ef4b5-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="ef4b5-104">開發人員通常會想要根據目前的使用者識別來顯示、隱藏或修改 UI。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="ef4b5-105">您可以透過相依性[插入](xref:fundamentals/dependency-injection)來存取 MVC views 內的授權服務。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ef4b5-106">若要將授權服務插入至 Razor 視圖中，請使用指示詞 `@inject` ：</span><span class="sxs-lookup"><span data-stu-id="ef4b5-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="ef4b5-107">如果您想要在每個視圖中都要有授權服務，請將指示詞放 `@inject` 入*Views*目錄的 *_ViewImports. cshtml*檔案中。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="ef4b5-108">如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="ef4b5-109">使用插入的授權服務，以與以 `AuthorizeAsync` [資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)時所檢查的完全相同方式來叫用：</span><span class="sxs-lookup"><span data-stu-id="ef4b5-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

<span data-ttu-id="ef4b5-110">在某些情況下，資源會是您的視圖模型。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-110">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="ef4b5-111">`AuthorizeAsync`以您在以[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)期間所檢查的相同方式來叫用：</span><span class="sxs-lookup"><span data-stu-id="ef4b5-111">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

<span data-ttu-id="ef4b5-112">在上述程式碼中，模型會以資源的形式傳遞，原則評估應該會納入考慮。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-112">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="ef4b5-113">請勿依賴切換應用程式 UI 專案的可見度，做為唯一的授權檢查。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-113">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="ef4b5-114">隱藏 UI 元素可能無法完全防止存取其相關聯的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-114">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="ef4b5-115">例如，請考慮上述程式碼片段中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-115">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="ef4b5-116">`Edit`如果使用者知道相對資源 URL 是 */Document/Edit/1*，就可以叫用動作方法。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-116">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="ef4b5-117">基於這個理由， `Edit` 動作方法應該執行自己的授權檢查。</span><span class="sxs-lookup"><span data-stu-id="ef4b5-117">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
