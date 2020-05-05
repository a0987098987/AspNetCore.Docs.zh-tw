---
title: ASP.NET Core MVC 中以視圖為基礎的授權
author: rick-anderson
description: 本檔示範如何在 ASP.NET Core Razor視圖內插入和使用授權服務。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/views
ms.openlocfilehash: 1a3b4a92d270844fa99fc9fb0283226e94edb22d
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775618"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="0b521-103">ASP.NET Core MVC 中以視圖為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="0b521-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="0b521-104">開發人員通常會想要根據目前的使用者識別來顯示、隱藏或修改 UI。</span><span class="sxs-lookup"><span data-stu-id="0b521-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="0b521-105">您可以透過相依性[插入](xref:fundamentals/dependency-injection)來存取 MVC views 內的授權服務。</span><span class="sxs-lookup"><span data-stu-id="0b521-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0b521-106">若要將授權服務插入至Razor視圖中，請`@inject`使用指示詞：</span><span class="sxs-lookup"><span data-stu-id="0b521-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="0b521-107">如果您想要在每個視圖中都要有`@inject`授權服務，請將指示詞放入*Views*目錄的 *_ViewImports. cshtml*檔案中。</span><span class="sxs-lookup"><span data-stu-id="0b521-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="0b521-108">如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="0b521-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="0b521-109">使用插入的授權服務，以`AuthorizeAsync`與以[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)時所檢查的完全相同方式來叫用：</span><span class="sxs-lookup"><span data-stu-id="0b521-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

<span data-ttu-id="0b521-110">在某些情況下，資源會是您的視圖模型。</span><span class="sxs-lookup"><span data-stu-id="0b521-110">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="0b521-111">`AuthorizeAsync`以您在以[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)期間所檢查的相同方式來叫用：</span><span class="sxs-lookup"><span data-stu-id="0b521-111">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

<span data-ttu-id="0b521-112">在上述程式碼中，模型會以資源的形式傳遞，原則評估應該會納入考慮。</span><span class="sxs-lookup"><span data-stu-id="0b521-112">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="0b521-113">請勿依賴切換應用程式 UI 專案的可見度，做為唯一的授權檢查。</span><span class="sxs-lookup"><span data-stu-id="0b521-113">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="0b521-114">隱藏 UI 元素可能無法完全防止存取其相關聯的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="0b521-114">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="0b521-115">例如，請考慮上述程式碼片段中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="0b521-115">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="0b521-116">如果使用者知道相對資源`Edit` URL 是 */Document/Edit/1*，就可以叫用動作方法。</span><span class="sxs-lookup"><span data-stu-id="0b521-116">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="0b521-117">基於這個理由， `Edit`動作方法應該執行自己的授權檢查。</span><span class="sxs-lookup"><span data-stu-id="0b521-117">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
