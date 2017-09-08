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
# <a name="limiting-identity-by-scheme"></a>配置，以限制身分識別

<a name=security-authorization-limiting-by-scheme></a>

在某些情況下，例如單一頁面應用程式很可能得到多個驗證方法。 比方說，您的應用程式可能會使用 cookie 基本驗證來登入和承載驗證 JavaScript 要求。 在某些情況下，您可能需要多個執行個體的驗證中介軟體。 例如，兩個 cookie middlewares 其中一個含有基本的身分識別，因為使用者要求的作業需要額外的安全性，已觸發多因素驗證時便會建立一個。

驗證配置時，例如設定在驗證期間，驗證中介軟體的名稱

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

在此組態中兩個驗證 middlewares 已經加入，一個 cookie，一個用於持有者。

>[!NOTE]
>加入多個驗證中介軟體時，您應該確定任何中介軟體設定成自動執行。 您可以設定`AutomaticAuthenticate`選項屬性設定為 false。 如果您無法執行這項篩選由配置無法運作。

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>選取配置和授權屬性

做為沒有驗證中介軟體會設定為自動執行，並建立您必須在授權選擇哪個中介軟體將會使用身分識別。 選取您想要使用授權的中介軟體的最簡單方式是使用`ActiveAuthenticationSchemes`屬性。 這個屬性可以接受以逗號分隔清單，若要使用的驗證配置。 例如，

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

在上述 cookie 和承載範例 middlewares 將會執行，並有機會建立並附加目前使用者的身分識別。 藉由指定單一配置只有指定的中介軟體會執行;

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

僅使用 Bearer 配置的中介軟體會執行在此情況下，並以 cookie 為基礎的所有識別會遭到都忽略。

## <a name="selecting-the-scheme-with-policies"></a>選取原則的配置

如果您想要指定在所需的配置[原則](policies.md#security-authorization-policies-based)您可以設定`AuthenticationSchemes`加入您的原則時的集合。

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

在此範例中 Over18 原則只會執行比對所建立的識別`Bearer`中介軟體。
