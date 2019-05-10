---
title: ASP.NET Core 中的宣告型授權
author: rick-anderson
description: 了解如何在 ASP.NET Core 應用程式中新增宣告授權檢查。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086158"
---
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="d8bde-103">ASP.NET Core 中的宣告型授權</span><span class="sxs-lookup"><span data-stu-id="d8bde-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="d8bde-104">建立身分識別時可能會將它指派一個或多個受信任的合作對象所發出的宣告。</span><span class="sxs-lookup"><span data-stu-id="d8bde-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="d8bde-105">宣告會表示哪些主體名稱值組，沒有什麼主體可以。</span><span class="sxs-lookup"><span data-stu-id="d8bde-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="d8bde-106">例如，您可能必須駕照，由本機駕駛授權單位發出。</span><span class="sxs-lookup"><span data-stu-id="d8bde-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="d8bde-107">您的驅動程式授權上有出生日期。</span><span class="sxs-lookup"><span data-stu-id="d8bde-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="d8bde-108">在此情況下會宣告名稱`DateOfBirth`，宣告值會是您生日，例如`8th June 1970`簽發者就是推動的 「 授權 」 授權單位。</span><span class="sxs-lookup"><span data-stu-id="d8bde-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="d8bde-109">宣告型授權，簡單來說，會檢查宣告的值，並可讓根據該值資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="d8bde-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="d8bde-110">例如，如果您想要存取晚上 club 授權程序可能是：</span><span class="sxs-lookup"><span data-stu-id="d8bde-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="d8bde-111">門安全性主管會評估您的生日的宣告，以及其是否信任簽發者 （駕駛 「 授權 」 授權） 授與您存取之前的日期值。</span><span class="sxs-lookup"><span data-stu-id="d8bde-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="d8bde-112">身分識別可以包含具有多個值的多個宣告，而且可以包含多個宣告型別相同。</span><span class="sxs-lookup"><span data-stu-id="d8bde-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="d8bde-113">新增宣告檢查</span><span class="sxs-lookup"><span data-stu-id="d8bde-113">Adding claims checks</span></span>

<span data-ttu-id="d8bde-114">宣告型的授權檢查都是宣告式-開發人員將其內嵌控制器或動作的控制器內的程式碼內指定宣告也必須擁有目前的使用者，並選擇性地宣告的值必須維持一致的存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="d8bde-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="d8bde-115">宣告需求為基礎的原則、 開發人員必須建置和註冊原則，表示宣告需求。</span><span class="sxs-lookup"><span data-stu-id="d8bde-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="d8bde-116">最簡單的類型宣告的宣告存在的原則看起來並不會檢查值。</span><span class="sxs-lookup"><span data-stu-id="d8bde-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="d8bde-117">首先您要建置和註冊原則。</span><span class="sxs-lookup"><span data-stu-id="d8bde-117">First you need to build and register the policy.</span></span> <span data-ttu-id="d8bde-118">這會以授權服務組態的一部分，其通常會參與方式進行`ConfigureServices()`在您*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8bde-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="d8bde-119">在此情況下`EmployeeOnly`原則會檢查是否有`EmployeeNumber`上目前的身分識別宣告。</span><span class="sxs-lookup"><span data-stu-id="d8bde-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="d8bde-120">您接著套用原則使用`Policy`屬性上的`AuthorizeAttribute`屬性來指定原則名稱;</span><span class="sxs-lookup"><span data-stu-id="d8bde-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="d8bde-121">`AuthorizeAttribute`屬性可以套用至整個控制器，這個執行個體中，只比對原則的身分識別會在控制站上允許任何動作的存取權。</span><span class="sxs-lookup"><span data-stu-id="d8bde-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="d8bde-122">如果您有受保護的控制器`AuthorizeAttribute`屬性，但想要允許匿名存取您所套用的特定動作`AllowAnonymousAttribute`屬性。</span><span class="sxs-lookup"><span data-stu-id="d8bde-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="d8bde-123">大部分的宣告會隨附的值。</span><span class="sxs-lookup"><span data-stu-id="d8bde-123">Most claims come with a value.</span></span> <span data-ttu-id="d8bde-124">建立原則時，您可以指定一份允許的值。</span><span class="sxs-lookup"><span data-stu-id="d8bde-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="d8bde-125">下列範例會只有成功的員工的員工編號是 1、 2、 3、 4 或 5。</span><span class="sxs-lookup"><span data-stu-id="d8bde-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

### <a name="add-a-generic-claim-check"></a><span data-ttu-id="d8bde-126">新增泛型宣告檢查</span><span class="sxs-lookup"><span data-stu-id="d8bde-126">Add a generic claim check</span></span>

<span data-ttu-id="d8bde-127">如果宣告值不是單一值，或需要轉換，請使用[RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion)。</span><span class="sxs-lookup"><span data-stu-id="d8bde-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="d8bde-128">如需詳細資訊，請參閱 <<c0> [ 滿足原則使用 func](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy)。</span><span class="sxs-lookup"><span data-stu-id="d8bde-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="d8bde-129">多個原則評估</span><span class="sxs-lookup"><span data-stu-id="d8bde-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="d8bde-130">如果您將多個原則套用至控制器或動作，然後必須通過所有原則，才能在授與存取權。</span><span class="sxs-lookup"><span data-stu-id="d8bde-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="d8bde-131">例如: </span><span class="sxs-lookup"><span data-stu-id="d8bde-131">For example:</span></span>

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

<span data-ttu-id="d8bde-132">在上述範例中，可滿足任何身分識別`EmployeeOnly`原則可以存取`Payslip`控制器上強制執行與該原則的動作。</span><span class="sxs-lookup"><span data-stu-id="d8bde-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="d8bde-133">不過若要呼叫`UpdateSalary`動作的身分識別必須滿足*兩者*`EmployeeOnly`原則和`HumanResources`原則。</span><span class="sxs-lookup"><span data-stu-id="d8bde-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="d8bde-134">如果您想更複雜的原則，例如使生日的宣告的日期，計算從該年齡，然後檢查年齡是 21 歲，則您需要撰寫[自訂原則的處理常式](xref:security/authorization/policies)。</span><span class="sxs-lookup"><span data-stu-id="d8bde-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
