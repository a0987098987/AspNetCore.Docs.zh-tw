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
# <a name="claims-based-authorization"></a>宣告型授權

<a name="security-authorization-claims-based"></a>

建立身分識別時它可能會指派一個或多個受信任的合作對象所發出的宣告。 宣告是名稱值組，表示何種主體，則為，沒有什麼主體可以執行。 例如，您可能駕駛執照，推動授權本機的授權單位所核發。 您的驅動程式授權上有您的出生日期。 在此情況下會宣告名稱`DateOfBirth`，宣告值會是您的日期的生日，例如`8th June 1970`簽發者就會推動授權授權單位。 在最簡單的宣告式授權會檢查宣告的值，並允許值為基礎的資源存取。 例如，如果您想要存取晚上社團授權程序可能會是：

媒體櫃門的安全主管有關會評估您的生日的宣告，以及其是否彼此信任的簽發者 （推動 「 授權 」 授權） 授與您存取之前的日期值。

身分識別可包含具有多個值的多個宣告，且可以包含多個相同類型的宣告。

## <a name="adding-claims-checks"></a>新增宣告檢查

宣告型的授權檢查是宣告式-開發人員會內嵌它們對控制器或動作中控制站，其程式碼內指定的宣告，而目前的使用者必須擁有，按住選擇性地宣告的值必須可存取要求的資源。 需求會原則為基礎的宣告，開發人員必須建置和註冊表示宣告需求的原則。

最簡單的類型宣告原則尋找宣告存在，但不會檢查此值。

首先您必須建置和註冊的原則。 這會以授權服務組態的一部分，其通常參與方式進行`ConfigureServices()`中您*Startup.cs*檔案。

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

接著，您套用原則使用`Policy`屬性`AuthorizeAttribute`屬性來指定原則名稱。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute`屬性可以套用至整個控制站，這個執行個體中，只識別比對原則會控制站上允許的存取權的任何動作。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

如果您有受保護的控制器`AuthorizeAttribute`屬性，但是想要允許匿名存取您所套用的特定動作`AllowAnonymousAttribute`屬性。

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

大部分的宣告會隨附的值。 建立原則時，您可以指定允許值的清單。 下列範例會只有成功的員工的員工數目是 1、 2、 3、 4 或 5。

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

## <a name="multiple-policy-evaluation"></a>多個原則評估

如果您將多個原則套用至控制器或動作，然後必須通過所有原則，才可取得存取。 例如: 

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

在上述範例中，可滿足任何身分識別`EmployeeOnly`原則可以存取`Payslip`控制站上的動作與該原則會強制執行。 不過若要呼叫`UpdateSalary`身分識別必須達到的動作*兩者*`EmployeeOnly`原則和`HumanResources`原則。

如果您想更複雜的原則，例如使日期的生日的宣告，計算年齡從它，然後檢查年齡 21 或更舊版本則需要撰寫[自訂原則的處理常式](policies.md)。
