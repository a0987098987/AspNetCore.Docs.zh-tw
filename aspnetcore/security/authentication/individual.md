---
title: 以個別使用者帳戶所建立 ASP.NET Core 專案為基礎的文章
author: rick-anderson
description: 根據使用個別使用者帳戶所建立 ASP.NET Core 專案來探索文章。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: 91c5665dc50124b3ba09bdcfbf3ba501f684c604
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463029"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="b77bb-103">以個別使用者帳戶所建立 ASP.NET Core 專案為基礎的文章</span><span class="sxs-lookup"><span data-stu-id="b77bb-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="b77bb-104">ASP.NET Core 身分識別包含在具有 [個別使用者帳戶] 選項之 Visual Studio 的專案範本中。</span><span class="sxs-lookup"><span data-stu-id="b77bb-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="b77bb-105">驗證範本可在 .NET Core CLI 中使用 `-au Individual`：</span><span class="sxs-lookup"><span data-stu-id="b77bb-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="b77bb-106">請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/5833)以取得 Web API 驗證。</span><span class="sxs-lookup"><span data-stu-id="b77bb-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="b77bb-107">無驗證</span><span class="sxs-lookup"><span data-stu-id="b77bb-107">No Authentication</span></span>

<span data-ttu-id="b77bb-108">使用 `-au` 選項在 .NET Core CLI 中指定驗證。</span><span class="sxs-lookup"><span data-stu-id="b77bb-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="b77bb-109">在 Visual Studio 中，新的 web 應用程式可以使用 [**變更驗證**] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b77bb-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="b77bb-110">Visual Studio 中新 web 應用程式的預設值為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="b77bb-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="b77bb-111">建立不含驗證的專案：</span><span class="sxs-lookup"><span data-stu-id="b77bb-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="b77bb-112">請勿包含網頁和 UI 以進行登入和登出。</span><span class="sxs-lookup"><span data-stu-id="b77bb-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="b77bb-113">不包含驗證碼。</span><span class="sxs-lookup"><span data-stu-id="b77bb-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="b77bb-114">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="b77bb-114">Windows Authentication</span></span>

<span data-ttu-id="b77bb-115">使用 `-au Windows` 選項，為 .NET Core CLI 中的新 web 應用程式指定 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="b77bb-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="b77bb-116">在 Visual Studio 中，[**變更驗證**] 對話方塊會提供**Windows 驗證**選項。</span><span class="sxs-lookup"><span data-stu-id="b77bb-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="b77bb-117">如果已選取 [Windows 驗證]，則會將應用程式設定為使用[Windows 驗證 IIS 模組](xref:host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="b77bb-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="b77bb-118">Windows 驗證適用于內部網路網站。</span><span class="sxs-lookup"><span data-stu-id="b77bb-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b77bb-119">其他資源</span><span class="sxs-lookup"><span data-stu-id="b77bb-119">Additional resources</span></span>

<span data-ttu-id="b77bb-120">下列文章示範如何使用在 ASP.NET Core 範本中產生的程式碼，以使用個別使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="b77bb-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="b77bb-121">ASP.NET Core 中的帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="b77bb-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b77bb-122">建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="b77bb-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
