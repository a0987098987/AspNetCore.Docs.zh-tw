---
title: 個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項
author: rick-anderson
description: 探索與個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f9c1be16386da935382275815bb5fd5c72894b1c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892525"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="d03f9-103">個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項</span><span class="sxs-lookup"><span data-stu-id="d03f9-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="d03f9-104">ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。</span><span class="sxs-lookup"><span data-stu-id="d03f9-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="d03f9-105">驗證範本可用於使用.NET Core CLI `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="d03f9-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="d03f9-106">請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/5833)web API 驗證。</span><span class="sxs-lookup"><span data-stu-id="d03f9-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="d03f9-107">沒有驗證</span><span class="sxs-lookup"><span data-stu-id="d03f9-107">No Authentication</span></span>

<span data-ttu-id="d03f9-108">在.NET Core CLI，以指定驗證`-au`選項。</span><span class="sxs-lookup"><span data-stu-id="d03f9-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="d03f9-109">在 Visual Studio 中，**變更驗證**對話是適用於新的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d03f9-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="d03f9-110">在 Visual Studio 中的新 web 應用程式的預設值是**不需要驗證**。</span><span class="sxs-lookup"><span data-stu-id="d03f9-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="d03f9-111">沒有驗證所建立的專案：</span><span class="sxs-lookup"><span data-stu-id="d03f9-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="d03f9-112">不包含 web 網頁和登入和登出的 UI。</span><span class="sxs-lookup"><span data-stu-id="d03f9-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="d03f9-113">不包含驗證碼。</span><span class="sxs-lookup"><span data-stu-id="d03f9-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="d03f9-114">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="d03f9-114">Windows Authentication</span></span>

<span data-ttu-id="d03f9-115">指定 Windows 驗證用於新的 web 應用程式中使用.NET Core CLI`-au Windows`選項。</span><span class="sxs-lookup"><span data-stu-id="d03f9-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="d03f9-116">在 Visual Studio 中，**變更驗證** 對話方塊提供**Windows 驗證**選項。</span><span class="sxs-lookup"><span data-stu-id="d03f9-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="d03f9-117">如果已選取 Windows 驗證，應用程式已設定為使用[Windows 驗證的 IIS 模組](xref:host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="d03f9-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="d03f9-118">Windows 驗證適用於內部網路網站。</span><span class="sxs-lookup"><span data-stu-id="d03f9-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d03f9-119">其他資源</span><span class="sxs-lookup"><span data-stu-id="d03f9-119">Additional resources</span></span>

<span data-ttu-id="d03f9-120">下列文件顯示如何使用 ASP.NET Core 範本使用個別的使用者帳戶中產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d03f9-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="d03f9-121">使用 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="d03f9-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="d03f9-122">ASP.NET Core 中的帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="d03f9-122">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d03f9-123">建立 ASP.NET Core 應用程式與受保護的授權的使用者資料</span><span class="sxs-lookup"><span data-stu-id="d03f9-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
