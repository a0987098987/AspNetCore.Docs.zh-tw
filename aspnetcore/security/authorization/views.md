---
title: "檢視架構的授權"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 3b7fa6025d766da80ba92ee27af20bf9bfe0dcf4
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="be396-103">檢視架構的授權</span><span class="sxs-lookup"><span data-stu-id="be396-103">View Based Authorization</span></span>

<a name=security-authorization-views></a>

<span data-ttu-id="be396-104">通常開發人員會想要顯示、 隱藏或修改目前的使用者識別為基礎的 UI。</span><span class="sxs-lookup"><span data-stu-id="be396-104">Often a developer will want to show, hide or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="be396-105">您可以存取授權服務會在透過 MVC 檢視內[相依性插入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="be396-105">You can access the authorization service within MVC views via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span></span> <span data-ttu-id="be396-106">若要將插入 Razor 檢視使用的授權服務`@inject`指示詞，例如`@inject IAuthorizationService AuthorizationService`(需要`@using Microsoft.AspNetCore.Authorization`)。</span><span class="sxs-lookup"><span data-stu-id="be396-106">To inject the authorization service into a Razor view use the `@inject` directive, for example `@inject IAuthorizationService AuthorizationService` (requires `@using Microsoft.AspNetCore.Authorization`).</span></span> <span data-ttu-id="be396-107">如果您想要授權服務中的每個檢視則放入`@inject`到指示詞`_ViewImports.cshtml`檔案`Views`目錄。</span><span class="sxs-lookup"><span data-stu-id="be396-107">If you want the authorization service in every view then place the `@inject` directive into the `_ViewImports.cshtml` file in the `Views` directory.</span></span> <span data-ttu-id="be396-108">如需有關檢視的相依性插入[檢視的相依性插入](../../mvc/views/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="be396-108">For more information on dependency injection into views see [Dependency injection into views](../../mvc/views/dependency-injection.md).</span></span>

<span data-ttu-id="be396-109">一旦您已插入授權服務就使用它藉由呼叫`AuthorizeAsync`方法中完全相同方式，您會檢查期間[以資源為基礎的授權](resourcebased.md#security-authorization-resource-based-imperative)。</span><span class="sxs-lookup"><span data-stu-id="be396-109">Once you have injected the authorization service you use it by calling the `AuthorizeAsync` method in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative).</span></span>

```csharp
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

<span data-ttu-id="be396-110">在某些情況下，資源會是您的檢視模型，而且您可以呼叫`AuthorizeAsync`中完全相同方式，您會檢查期間[以資源為基礎的授權](resourcebased.md#security-authorization-resource-based-imperative);</span><span class="sxs-lookup"><span data-stu-id="be396-110">In some cases the resource will be your view model, and you can call `AuthorizeAsync` in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative);</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be396-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be396-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be396-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be396-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

<span data-ttu-id="be396-113">您可以查看模型會如資源授權應該納入考量。</span><span class="sxs-lookup"><span data-stu-id="be396-113">Here you can see the model is passed as the resource authorization should take into consideration.</span></span>

>[!WARNING]
><span data-ttu-id="be396-114">請勿依賴顯示或隱藏您的 UI 做為唯一的授權方法的部分。</span><span class="sxs-lookup"><span data-stu-id="be396-114">Do not rely on showing or hiding parts of your UI as your only authorization method.</span></span> <span data-ttu-id="be396-115">隱藏 UI 項目不代表使用者無法存取它。</span><span class="sxs-lookup"><span data-stu-id="be396-115">Hiding a UI element does not mean a user cannot access it.</span></span> <span data-ttu-id="be396-116">您也必須在控制器的程式碼中授權使用者。</span><span class="sxs-lookup"><span data-stu-id="be396-116">You must also authorize the user within your controller code.</span></span>
