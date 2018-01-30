---
title: "個別使用者帳戶建立的專案為基礎的發行項"
author: rick-anderson
description: "本文件列出個別的使用者帳戶建立的專案為基礎的發行項。"
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: aee18fa08fbc5c8452ca2b401d32858edaf55e7c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="0c029-103">個別使用者帳戶建立的專案為基礎的發行項</span><span class="sxs-lookup"><span data-stu-id="0c029-103">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="0c029-104">ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。</span><span class="sxs-lookup"><span data-stu-id="0c029-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="0c029-105">驗證範本可用於使用.NET 核心 CLI `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="0c029-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="0c029-106">下列文件顯示如何使用 ASP.NET Core 範本使用個別的使用者帳戶中產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="0c029-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="0c029-107">使用 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="0c029-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="0c029-108">ASP.NET Core 中的帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="0c029-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="0c029-109">建立 ASP.NET Core 應用程式與受保護的授權的使用者資料</span><span class="sxs-lookup"><span data-stu-id="0c029-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
