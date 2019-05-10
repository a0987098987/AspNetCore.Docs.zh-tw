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
# <a name="claims-based-authorization-in-aspnet-core"></a>ASP.NET Core 中的宣告型授權

<a name="security-authorization-claims-based"></a>

建立身分識別時可能會將它指派一個或多個受信任的合作對象所發出的宣告。 宣告會表示哪些主體名稱值組，沒有什麼主體可以。 例如，您可能必須駕照，由本機駕駛授權單位發出。 您的驅動程式授權上有出生日期。 在此情況下會宣告名稱`DateOfBirth`，宣告值會是您生日，例如`8th June 1970`簽發者就是推動的 「 授權 」 授權單位。 宣告型授權，簡單來說，會檢查宣告的值，並可讓根據該值資源的存取權。 例如，如果您想要存取晚上 club 授權程序可能是：

門安全性主管會評估您的生日的宣告，以及其是否信任簽發者 （駕駛 「 授權 」 授權） 授與您存取之前的日期值。

身分識別可以包含具有多個值的多個宣告，而且可以包含多個宣告型別相同。

## <a name="adding-claims-checks"></a>新增宣告檢查

宣告型的授權檢查都是宣告式-開發人員將其內嵌控制器或動作的控制器內的程式碼內指定宣告也必須擁有目前的使用者，並選擇性地宣告的值必須維持一致的存取要求的資源。 宣告需求為基礎的原則、 開發人員必須建置和註冊原則，表示宣告需求。

最簡單的類型宣告的宣告存在的原則看起來並不會檢查值。

首先您要建置和註冊原則。 這會以授權服務組態的一部分，其通常會參與方式進行`ConfigureServices()`在您*Startup.cs*檔案。

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

在此情況下`EmployeeOnly`原則會檢查是否有`EmployeeNumber`上目前的身分識別宣告。

您接著套用原則使用`Policy`屬性上的`AuthorizeAttribute`屬性來指定原則名稱;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute`屬性可以套用至整個控制器，這個執行個體中，只比對原則的身分識別會在控制站上允許任何動作的存取權。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

如果您有受保護的控制器`AuthorizeAttribute`屬性，但想要允許匿名存取您所套用的特定動作`AllowAnonymousAttribute`屬性。

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

大部分的宣告會隨附的值。 建立原則時，您可以指定一份允許的值。 下列範例會只有成功的員工的員工編號是 1、 2、 3、 4 或 5。

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

### <a name="add-a-generic-claim-check"></a>新增泛型宣告檢查

如果宣告值不是單一值，或需要轉換，請使用[RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion)。 如需詳細資訊，請參閱 <<c0> [ 滿足原則使用 func](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy)。

## <a name="multiple-policy-evaluation"></a>多個原則評估

如果您將多個原則套用至控制器或動作，然後必須通過所有原則，才能在授與存取權。 例如: 

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

在上述範例中，可滿足任何身分識別`EmployeeOnly`原則可以存取`Payslip`控制器上強制執行與該原則的動作。 不過若要呼叫`UpdateSalary`動作的身分識別必須滿足*兩者*`EmployeeOnly`原則和`HumanResources`原則。

如果您想更複雜的原則，例如使生日的宣告的日期，計算從該年齡，然後檢查年齡是 21 歲，則您需要撰寫[自訂原則的處理常式](xref:security/authorization/policies)。
