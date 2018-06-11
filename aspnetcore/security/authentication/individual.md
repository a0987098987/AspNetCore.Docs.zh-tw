---
title: 個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項
author: rick-anderson
description: 探索與個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項。
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 6d05cd8c0ee6c9029eb9bbafc15d9b19ee7633de
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252018"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="a3ffe-103">個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項</span><span class="sxs-lookup"><span data-stu-id="a3ffe-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="a3ffe-104">ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。</span><span class="sxs-lookup"><span data-stu-id="a3ffe-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="a3ffe-105">驗證範本可用於使用.NET 核心 CLI `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="a3ffe-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="a3ffe-107">下列文件顯示如何使用 ASP.NET Core 範本使用個別的使用者帳戶中產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="a3ffe-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="a3ffe-108">使用 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="a3ffe-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="a3ffe-109">ASP.NET Core 中的帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="a3ffe-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="a3ffe-110">建立 ASP.NET Core 應用程式與受保護的授權的使用者資料</span><span class="sxs-lookup"><span data-stu-id="a3ffe-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
