---
title: "宣告型授權"
author: rick-anderson
description: "本文件說明如何加入 ASP.NET Core 應用程式中的宣告授權檢查。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: dd8f42684f9e58b9329602aa9b70d2c0ab950892
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="claims-based-authorization"></a><span data-ttu-id="3d1ef-103">宣告型授權</span><span class="sxs-lookup"><span data-stu-id="3d1ef-103">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="3d1ef-104">建立身分識別時它可能會指派一個或多個受信任的合作對象所發出的宣告。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="3d1ef-105">宣告是名稱值組，表示何種主體，則為，沒有什麼主體可以執行。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-105">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="3d1ef-106">例如，您可能駕駛執照，推動授權本機的授權單位所核發。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="3d1ef-107">您的驅動程式授權上有您的出生日期。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="3d1ef-108">在此情況下會宣告名稱`DateOfBirth`，宣告值會是您的日期的生日，例如`8th June 1970`簽發者就會推動授權授權單位。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="3d1ef-109">在最簡單的宣告式授權會檢查宣告的值，並允許值為基礎的資源存取。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="3d1ef-110">例如，如果您想要存取晚上社團授權程序可能會是：</span><span class="sxs-lookup"><span data-stu-id="3d1ef-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="3d1ef-111">媒體櫃門的安全主管有關會評估您的生日的宣告，以及其是否彼此信任的簽發者 （推動 「 授權 」 授權） 授與您存取之前的日期值。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="3d1ef-112">身分識別可包含具有多個值的多個宣告，且可以包含多個相同類型的宣告。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="3d1ef-113">新增宣告檢查</span><span class="sxs-lookup"><span data-stu-id="3d1ef-113">Adding claims checks</span></span>

<span data-ttu-id="3d1ef-114">宣告型的授權檢查是宣告式-開發人員會內嵌它們對控制器或動作中控制站，其程式碼內指定的宣告，而目前的使用者必須擁有，按住選擇性地宣告的值必須可存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="3d1ef-115">需求會原則為基礎的宣告，開發人員必須建置和註冊表示宣告需求的原則。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="3d1ef-116">最簡單的類型宣告原則尋找宣告存在，但不會檢查此值。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-116">The simplest type of claim policy looks for the presence of a claim and does not check the value.</span></span>

<span data-ttu-id="3d1ef-117">首先您必須建置和註冊的原則。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-117">First you need to build and register the policy.</span></span> <span data-ttu-id="3d1ef-118">這會以授權服務組態的一部分，其通常參與方式進行`ConfigureServices()`中您*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="3d1ef-119">在此情況下`EmployeeOnly`原則會檢查是否有`EmployeeNumber`上目前的身分識別宣告。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="3d1ef-120">接著，您套用原則使用`Policy`屬性`AuthorizeAttribute`屬性來指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="3d1ef-121">`AuthorizeAttribute`屬性可以套用至整個控制站，這個執行個體中，只識別比對原則會控制站上允許的存取權的任何動作。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="3d1ef-122">如果您有受保護的控制器`AuthorizeAttribute`屬性，但是想要允許匿名存取您所套用的特定動作`AllowAnonymousAttribute`屬性。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-122">If you have a controller that is protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="3d1ef-123">大部分的宣告會隨附的值。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-123">Most claims come with a value.</span></span> <span data-ttu-id="3d1ef-124">建立原則時，您可以指定允許值的清單。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="3d1ef-125">下列範例會只有成功的員工的員工數目是 1、 2、 3、 4 或 5。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="3d1ef-126">多個原則評估</span><span class="sxs-lookup"><span data-stu-id="3d1ef-126">Multiple Policy Evaluation</span></span>

<span data-ttu-id="3d1ef-127">如果您將多個原則套用至控制器或動作，然後必須通過所有原則，才可取得存取。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-127">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="3d1ef-128">例如: </span><span class="sxs-lookup"><span data-stu-id="3d1ef-128">For example:</span></span>

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

<span data-ttu-id="3d1ef-129">在上述範例中，可滿足任何身分識別`EmployeeOnly`原則可以存取`Payslip`控制站上的動作與該原則會強制執行。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-129">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="3d1ef-130">不過若要呼叫`UpdateSalary`身分識別必須達到的動作*兩者*`EmployeeOnly`原則和`HumanResources`原則。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-130">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="3d1ef-131">如果您想更複雜的原則，例如使日期的生日的宣告，計算年齡從它，然後檢查年齡 21 或更舊版本則需要撰寫[自訂原則的處理常式](policies.md)。</span><span class="sxs-lookup"><span data-stu-id="3d1ef-131">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md).</span></span>
