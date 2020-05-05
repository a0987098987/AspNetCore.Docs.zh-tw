---
title: ASP.NET Core 中以宣告為基礎的授權
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 應用程式中新增授權的宣告檢查。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/claims
ms.openlocfilehash: de8ab915e6a8529c7401f89fad067ec33d5d0713
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774414"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>ASP.NET Core 中以宣告為基礎的授權

<a name="security-authorization-claims-based"></a>

建立身分識別時，可能會被指派一個或多個由信任方發行的宣告。 宣告是一組名稱值組，代表主旨，而不是主體可執行檔動作。 例如，您可能有一個駕駛者授權，由當地駕駛授權單位發行。 您的驅動程式授權已在此提供您的出生日期。 在此情況下，宣告名稱會`DateOfBirth`是，宣告值會是您的出生日期，例如`8th June 1970` ，而簽發者會是駕駛授權授權單位。 以宣告為基礎的授權，其最簡單的會檢查宣告的值，並允許根據該值來存取資源。 例如，如果您想要存取深夜俱樂部，授權程式可能如下：

門安全主管會評估您的出生日期值，以及他們是否信任簽發者（駕駛授權授權單位），再授與您存取權。

身分識別可以包含多個具有多個值的宣告，而且可以包含相同類型的多個宣告。

## <a name="adding-claims-checks"></a>新增宣告檢查

以宣告為基礎的授權檢查是宣告式的-開發人員會將其內嵌在其程式碼內、針對控制器中的控制器或動作，指定目前使用者必須擁有的宣告，並選擇性地將宣告必須保留以存取要求的資源的值。 宣告需求是以原則為基礎，開發人員必須建立並註冊表示宣告需求的原則。

最簡單的宣告原則類型會尋找宣告是否存在，且不會檢查值。

首先，您必須建立並註冊原則。 這會在授權服務設定過程中發生，這通常會在您的`ConfigureServices()` *Startup.cs*檔案中納入。

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

在此情況下`EmployeeOnly` ，原則會檢查目前身分識別`EmployeeNumber`上是否存在宣告。

然後，您可以在`Policy` `AuthorizeAttribute`屬性上使用屬性來套用原則，以指定原則名稱。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute`屬性可以套用至整個控制器，在此實例中，只有符合原則的識別才允許存取控制器上的任何動作。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

如果您的控制器受到`AuthorizeAttribute`屬性保護，但想要允許匿名存取特定動作，請套用`AllowAnonymousAttribute`屬性。

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

大部分的宣告都隨附一個值。 建立原則時，您可以指定允許的值清單。 下列範例只會針對員工編號為1、2、3、4或5的員工成功。

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
### <a name="add-a-generic-claim-check"></a>新增一般宣告檢查

如果宣告值不是單一值或需要轉換，請使用[RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion)。 如需詳細資訊，請參閱[使用 func 來完成原則](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy)。

## <a name="multiple-policy-evaluation"></a>多重原則評估

如果您將多個原則套用至控制器或動作，則在授與存取權之前，所有原則都必須通過。 例如：

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

在上述範例中，任何符合`EmployeeOnly`原則的身分識別都可以`Payslip`存取動作，因為該原則會在控制器上強制執行。 不過， `UpdateSalary`為了呼叫動作，身分識別必須*同時*滿足`EmployeeOnly`原則和`HumanResources`原則。

如果您想要更複雜的原則，例如取得出生日期、計算其存留期，然後檢查年齡是21或更久，則您需要撰寫[自訂原則處理常式](xref:security/authorization/policies)。
