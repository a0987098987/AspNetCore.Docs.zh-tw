---
title: "配置，以限制身分識別"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 2483c441da317a5c29b611b3a4910eae3c01fd7a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-identity-by-scheme"></a><span data-ttu-id="2ee2e-103">配置，以限制身分識別</span><span class="sxs-lookup"><span data-stu-id="2ee2e-103">Limiting identity by scheme</span></span>

<a name=security-authorization-limiting-by-scheme></a>

<span data-ttu-id="2ee2e-104">在某些情況下，例如單一頁面應用程式很可能得到多個驗證方法。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-104">In some scenarios, such as Single Page Applications it is possible to end up with multiple authentication methods.</span></span> <span data-ttu-id="2ee2e-105">比方說，您的應用程式可能會使用 cookie 基本驗證來登入和承載驗證 JavaScript 要求。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-105">For example, your application may use cookie-based authentication to log in and bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="2ee2e-106">在某些情況下，您可能需要多個執行個體的驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-106">In some cases you may have multiple instances of an authentication middleware.</span></span> <span data-ttu-id="2ee2e-107">例如，兩個 cookie middlewares 其中一個含有基本的身分識別，因為使用者要求的作業需要額外的安全性，已觸發多因素驗證時便會建立一個。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-107">For example, two cookie middlewares where one contains a basic identity and one is created when a multi-factor authentication has triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="2ee2e-108">驗證配置時，例如設定在驗證期間，驗證中介軟體的名稱</span><span class="sxs-lookup"><span data-stu-id="2ee2e-108">Authentication schemes are named when authentication middleware is configured during authentication, for example</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AuthenticationScheme = "Cookie",
    LoginPath = new PathString("/Account/Unauthorized/"),
    AccessDeniedPath = new PathString("/Account/Forbidden/"),
    AutomaticAuthenticate = false
});

app.UseBearerAuthentication(options =>
{
    options.AuthenticationScheme = "Bearer";
    options.AutomaticAuthenticate = false;
});
```

<span data-ttu-id="2ee2e-109">在此組態中兩個驗證 middlewares 已經加入，一個 cookie，一個用於持有者。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-109">In this configuration two authentication middlewares have been added, one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="2ee2e-110">加入多個驗證中介軟體時，您應該確定任何中介軟體設定成自動執行。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-110">When adding multiple authentication middleware you should ensure that no middleware is configured to run automatically.</span></span> <span data-ttu-id="2ee2e-111">您可以設定`AutomaticAuthenticate`選項屬性設定為 false。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-111">You do this by setting the `AutomaticAuthenticate` options property to false.</span></span> <span data-ttu-id="2ee2e-112">如果您無法執行這項篩選由配置無法運作。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-112">If you fail to do this filtering by scheme will not work.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="2ee2e-113">選取配置和授權屬性</span><span class="sxs-lookup"><span data-stu-id="2ee2e-113">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="2ee2e-114">做為沒有驗證中介軟體會設定為自動執行，並建立您必須在授權選擇哪個中介軟體將會使用身分識別。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-114">As no authentication middleware is configured to automatically run and create an identity you must, at the point of authorization choose which middleware will be used.</span></span> <span data-ttu-id="2ee2e-115">選取您想要使用授權的中介軟體的最簡單方式是使用`ActiveAuthenticationSchemes`屬性。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-115">The simplest way to select the middleware you wish to authorize with is to use the `ActiveAuthenticationSchemes` property.</span></span> <span data-ttu-id="2ee2e-116">這個屬性可以接受以逗號分隔清單，若要使用的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-116">This property accepts a comma delimited list of Authentication Schemes to use.</span></span> <span data-ttu-id="2ee2e-117">例如，</span><span class="sxs-lookup"><span data-stu-id="2ee2e-117">For example;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

<span data-ttu-id="2ee2e-118">在上述 cookie 和承載範例 middlewares 將會執行，並有機會建立並附加目前使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-118">In the example above both the cookie and bearer middlewares will run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="2ee2e-119">藉由指定單一配置只有指定的中介軟體會執行;</span><span class="sxs-lookup"><span data-stu-id="2ee2e-119">By specifying a single scheme only the specified middleware will run;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

<span data-ttu-id="2ee2e-120">僅使用 Bearer 配置的中介軟體會執行在此情況下，並以 cookie 為基礎的所有識別會遭到都忽略。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-120">In this case only the middleware with the Bearer scheme would run, and any cookie based identities would be ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="2ee2e-121">選取原則的配置</span><span class="sxs-lookup"><span data-stu-id="2ee2e-121">Selecting the scheme with policies</span></span>

<span data-ttu-id="2ee2e-122">如果您想要指定在所需的配置[原則](policies.md#security-authorization-policies-based)您可以設定`AuthenticationSchemes`加入您的原則時的集合。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-122">If you prefer to specify the desired schemes in [policy](policies.md#security-authorization-policies-based) you can set the `AuthenticationSchemes` collection when adding your policy.</span></span>

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

<span data-ttu-id="2ee2e-123">在此範例中 Over18 原則只會執行比對所建立的識別`Bearer`中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2ee2e-123">In this example the Over18 policy will only run against the identity created by the `Bearer` middleware.</span></span>
