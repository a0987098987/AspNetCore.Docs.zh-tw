---
title: "簡單的授權"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: 013ce0d9ac1e9c1b6bb541b9fa66218c3fd799bb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="simple-authorization"></a>簡單的授權

<a name=security-authorization-simple></a>

在 MVC 中的授權由`AuthorizeAttribute`屬性和各種不同的參數。 在其最簡單的套用`AuthorizeAttribute`屬性到控制器或動作限制存取控制器或動作，任何已驗證的使用者。

例如，下列程式碼會限制存取`AccountController`任何已驗證的使用者。

```csharp
[Authorize]
   public class AccountController : Controller
   {
       public ActionResult Login()
       {
       }

       public ActionResult Logout()
       {
       }
   }
   ```

如果您想要只將授權套用至動作，而不是在控制器套用`AuthorizeAttribute`該項動作; 此屬性

```csharp
public class AccountController : Controller
   {
       public ActionResult Login()
       {
       }

       [Authorize]
       public ActionResult Logout()
       {
       }
   }
   ```

現在已驗證的使用者可以存取登出函式。

您也可以使用`AllowAnonymousAttribute`屬性，以允許存取個別的動作; 未經驗證使用者，例如

```csharp
[Authorize]
   public class AccountController : Controller
   {
       [AllowAnonymous]
       public ActionResult Login()
       {
       }

       public ActionResult Logout()
       {
       }
   }
   ```

這樣可讓只有經過驗證的使用者`AccountController`，除了`Login`動作，都可以存取所有人不論他們已驗證或未驗證 / 匿名的狀態。

>[!WARNING]
> `[AllowAnonymous]`會略過所有授權陳述式。 如果您套用結合`[AllowAnonymous]`和任何`[Authorize]`屬性然後 Authorize 屬性將永遠會被忽略。 例如，如果您套用`[AllowAnonymous]`在控制器層級任何`[Authorize]`屬性相同的控制站，或在其中任何動作將會被忽略。
