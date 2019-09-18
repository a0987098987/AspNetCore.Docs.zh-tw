---
title: 個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項
author: rick-anderson
description: 探索與個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: cf548417268a8587787471b9ed91c0ed109fbee9
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080695"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="d1895-103">個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項</span><span class="sxs-lookup"><span data-stu-id="d1895-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="d1895-104">ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。</span><span class="sxs-lookup"><span data-stu-id="d1895-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="d1895-105">驗證範本可用於使用.NET Core CLI `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="d1895-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

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

<span data-ttu-id="d1895-106">請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/5833)以取得 Web API 驗證。</span><span class="sxs-lookup"><span data-stu-id="d1895-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="d1895-107">無驗證</span><span class="sxs-lookup"><span data-stu-id="d1895-107">No Authentication</span></span>

<span data-ttu-id="d1895-108">使用`-au`選項在 .NET Core CLI 中指定驗證。</span><span class="sxs-lookup"><span data-stu-id="d1895-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="d1895-109">在 Visual Studio 中，新的 web 應用程式可以使用 [**變更驗證**] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="d1895-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="d1895-110">Visual Studio 中新 web 應用程式的預設值為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="d1895-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="d1895-111">建立不含驗證的專案：</span><span class="sxs-lookup"><span data-stu-id="d1895-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="d1895-112">請勿包含網頁和 UI 以進行登入和登出。</span><span class="sxs-lookup"><span data-stu-id="d1895-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="d1895-113">不包含驗證碼。</span><span class="sxs-lookup"><span data-stu-id="d1895-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="d1895-114">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="d1895-114">Windows Authentication</span></span>

<span data-ttu-id="d1895-115">在 .NET Core CLI 中使用`-au Windows`選項，為新的 web 應用程式指定 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="d1895-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="d1895-116">在 Visual Studio 中，[**變更驗證**] 對話方塊會提供**Windows 驗證**選項。</span><span class="sxs-lookup"><span data-stu-id="d1895-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="d1895-117">如果已選取 [Windows 驗證]，則會將應用程式設定為使用[Windows 驗證 IIS 模組](xref:host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="d1895-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="d1895-118">Windows 驗證適用于內部網路網站。</span><span class="sxs-lookup"><span data-stu-id="d1895-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d1895-119">其他資源</span><span class="sxs-lookup"><span data-stu-id="d1895-119">Additional resources</span></span>

<span data-ttu-id="d1895-120">下列文件顯示如何使用 ASP.NET Core 範本使用個別的使用者帳戶中產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d1895-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="d1895-121">使用 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="d1895-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="d1895-122">ASP.NET Core 中的帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="d1895-122">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d1895-123">建立 ASP.NET Core 應用程式與受保護的授權的使用者資料</span><span class="sxs-lookup"><span data-stu-id="d1895-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
