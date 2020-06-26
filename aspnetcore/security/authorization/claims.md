---
title: ASP.NET Core 中以宣告為基礎的授權
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 應用程式中新增授權的宣告檢查。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/claims
ms.openlocfilehash: 404e26f0fb5e71dbc22b1c08a2f8caf8461ad7e1
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406377"
---
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="b000e-103">ASP.NET Core 中以宣告為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="b000e-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="b000e-104">建立身分識別時，可能會被指派一個或多個由信任方發行的宣告。</span><span class="sxs-lookup"><span data-stu-id="b000e-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="b000e-105">宣告是一組名稱值組，代表主旨，而不是主體可執行檔動作。</span><span class="sxs-lookup"><span data-stu-id="b000e-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="b000e-106">例如，您可能有一個駕駛者授權，由當地駕駛授權單位發行。</span><span class="sxs-lookup"><span data-stu-id="b000e-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="b000e-107">您的驅動程式授權已在此提供您的出生日期。</span><span class="sxs-lookup"><span data-stu-id="b000e-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="b000e-108">在此情況下，宣告名稱會是 `DateOfBirth` ，宣告值會是您的出生日期，例如， `8th June 1970` 而簽發者會是駕駛授權授權單位。</span><span class="sxs-lookup"><span data-stu-id="b000e-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="b000e-109">以宣告為基礎的授權，其最簡單的會檢查宣告的值，並允許根據該值來存取資源。</span><span class="sxs-lookup"><span data-stu-id="b000e-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="b000e-110">例如，如果您想要存取深夜俱樂部，授權程式可能如下：</span><span class="sxs-lookup"><span data-stu-id="b000e-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="b000e-111">門安全主管會評估您的出生日期值，以及他們是否信任簽發者（駕駛授權授權單位），再授與您存取權。</span><span class="sxs-lookup"><span data-stu-id="b000e-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="b000e-112">身分識別可以包含多個具有多個值的宣告，而且可以包含相同類型的多個宣告。</span><span class="sxs-lookup"><span data-stu-id="b000e-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="b000e-113">新增宣告檢查</span><span class="sxs-lookup"><span data-stu-id="b000e-113">Adding claims checks</span></span>

<span data-ttu-id="b000e-114">以宣告為基礎的授權檢查是宣告式的-開發人員會將其內嵌在其程式碼內、針對控制器中的控制器或動作，指定目前使用者必須擁有的宣告，並選擇性地將宣告必須保留以存取要求的資源的值。</span><span class="sxs-lookup"><span data-stu-id="b000e-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="b000e-115">宣告需求是以原則為基礎，開發人員必須建立並註冊表示宣告需求的原則。</span><span class="sxs-lookup"><span data-stu-id="b000e-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="b000e-116">最簡單的宣告原則類型會尋找宣告是否存在，且不會檢查值。</span><span class="sxs-lookup"><span data-stu-id="b000e-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="b000e-117">首先，您必須建立並註冊原則。</span><span class="sxs-lookup"><span data-stu-id="b000e-117">First you need to build and register the policy.</span></span> <span data-ttu-id="b000e-118">這會在授權服務設定過程中發生，這通常會在您的 `ConfigureServices()` *Startup.cs*檔案中納入。</span><span class="sxs-lookup"><span data-stu-id="b000e-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

::: moniker-end

<span data-ttu-id="b000e-119">在此情況下， `EmployeeOnly` 原則會檢查目前身分識別上是否存在宣告 `EmployeeNumber` 。</span><span class="sxs-lookup"><span data-stu-id="b000e-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="b000e-120">然後，您可以在 `Policy` 屬性上使用屬性來套用原則 `AuthorizeAttribute` ，以指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="b000e-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="b000e-121">`AuthorizeAttribute`屬性可以套用至整個控制器，在此實例中，只有符合原則的識別才允許存取控制器上的任何動作。</span><span class="sxs-lookup"><span data-stu-id="b000e-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="b000e-122">如果您的控制器受到 `AuthorizeAttribute` 屬性保護，但想要允許匿名存取特定動作，請套用 `AllowAnonymousAttribute` 屬性。</span><span class="sxs-lookup"><span data-stu-id="b000e-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

<span data-ttu-id="b000e-123">大部分的宣告都隨附一個值。</span><span class="sxs-lookup"><span data-stu-id="b000e-123">Most claims come with a value.</span></span> <span data-ttu-id="b000e-124">建立原則時，您可以指定允許的值清單。</span><span class="sxs-lookup"><span data-stu-id="b000e-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="b000e-125">下列範例只會針對員工編號為1、2、3、4或5的員工成功。</span><span class="sxs-lookup"><span data-stu-id="b000e-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

::: moniker-end
### <a name="add-a-generic-claim-check"></a><span data-ttu-id="b000e-126">新增一般宣告檢查</span><span class="sxs-lookup"><span data-stu-id="b000e-126">Add a generic claim check</span></span>

<span data-ttu-id="b000e-127">如果宣告值不是單一值或需要轉換，請使用[RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion)。</span><span class="sxs-lookup"><span data-stu-id="b000e-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="b000e-128">如需詳細資訊，請參閱[使用 func 來完成原則](xref:security/authorization/policies#use-a-func-to-fulfill-a-policy)。</span><span class="sxs-lookup"><span data-stu-id="b000e-128">For more information, see [Use a func to fulfill a policy](xref:security/authorization/policies#use-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="b000e-129">多重原則評估</span><span class="sxs-lookup"><span data-stu-id="b000e-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="b000e-130">如果您將多個原則套用至控制器或動作，則在授與存取權之前，所有原則都必須通過。</span><span class="sxs-lookup"><span data-stu-id="b000e-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="b000e-131">例如：</span><span class="sxs-lookup"><span data-stu-id="b000e-131">For example:</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

<span data-ttu-id="b000e-132">在上述範例中，任何符合原則的身分識別 `EmployeeOnly` 都可以存取 `Payslip` 動作，因為該原則會在控制器上強制執行。</span><span class="sxs-lookup"><span data-stu-id="b000e-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="b000e-133">不過，為了呼叫動作，身分 `UpdateSalary` 識別必須*同時*滿足 `EmployeeOnly` 原則和 `HumanResources` 原則。</span><span class="sxs-lookup"><span data-stu-id="b000e-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="b000e-134">如果您想要更複雜的原則，例如取得出生日期、計算其存留期，然後檢查年齡是21或更久，則您需要撰寫[自訂原則處理常式](xref:security/authorization/policies)。</span><span class="sxs-lookup"><span data-stu-id="b000e-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
