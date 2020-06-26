---
title: 保護 ASP.NET Core Blazor Server 應用程式
author: guardrex
description: 瞭解如何以 Blazor Server ASP.NET Core 的應用程式來保護應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/server/index
ms.openlocfilehash: ab3baad30f78c5d5e2f969b3292d4886fcd0406d
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85402308"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a><span data-ttu-id="04f68-103">保護 ASP.NET Core Blazor Server 應用程式</span><span class="sxs-lookup"><span data-stu-id="04f68-103">Secure ASP.NET Core Blazor Server apps</span></span>

<span data-ttu-id="04f68-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="04f68-104">By [Luke Latham](https://github.com/guardrex)</span></span>

Blazor Server<span data-ttu-id="04f68-105">應用程式的安全性設定方式與 ASP.NET Core 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="04f68-105"> apps are configured for security in the same manner as ASP.NET Core apps.</span></span> <span data-ttu-id="04f68-106">如需詳細資訊，請參閱底下的文章 <xref:security/index> 。</span><span class="sxs-lookup"><span data-stu-id="04f68-106">For more information, see the articles under <xref:security/index>.</span></span> <span data-ttu-id="04f68-107">此總覽下的主題特別適用于 Blazor Server 。</span><span class="sxs-lookup"><span data-stu-id="04f68-107">Topics under this overview apply specifically to Blazor Server.</span></span> 

## <a name="blazor-server-project-template"></a>Blazor Server<span data-ttu-id="04f68-108">專案範本</span><span class="sxs-lookup"><span data-stu-id="04f68-108"> project template</span></span>

<span data-ttu-id="04f68-109">Blazor Server專案範本可以設定為在建立專案時進行驗證。</span><span class="sxs-lookup"><span data-stu-id="04f68-109">The Blazor Server project template can be configured for authentication when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="04f68-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04f68-110">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="04f68-111">遵循文章中的 Visual Studio 指導方針 <xref:blazor/get-started> ，使用驗證機制來建立新的 Blazor Server 專案。</span><span class="sxs-lookup"><span data-stu-id="04f68-111">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="04f68-112">選擇 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中的\*\* Blazor Server 應用程式**範本之後，請選取 [**驗證**] 底下的 [**變更\*\*]。</span><span class="sxs-lookup"><span data-stu-id="04f68-112">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="04f68-113">對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：</span><span class="sxs-lookup"><span data-stu-id="04f68-113">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="04f68-114">**無驗證**</span><span class="sxs-lookup"><span data-stu-id="04f68-114">**No Authentication**</span></span>
* <span data-ttu-id="04f68-115">**個別使用者帳戶**：可以儲存使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="04f68-115">**Individual User Accounts**: User accounts can be stored:</span></span>
  * <span data-ttu-id="04f68-116">在應用程式中使用 ASP.NET Core 的 [Identity](xref:security/authentication/identity) 系統。</span><span class="sxs-lookup"><span data-stu-id="04f68-116">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="04f68-117">使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。</span><span class="sxs-lookup"><span data-stu-id="04f68-117">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="04f68-118">**工作或學校帳戶**</span><span class="sxs-lookup"><span data-stu-id="04f68-118">**Work or School Accounts**</span></span>
* <span data-ttu-id="04f68-119">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="04f68-119">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="04f68-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="04f68-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="04f68-121">遵循文章中的 Visual Studio Code 指導方針 <xref:blazor/get-started> ，使用驗證機制來建立新的 Blazor Server 專案：</span><span class="sxs-lookup"><span data-stu-id="04f68-121">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="04f68-122">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="04f68-122">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="04f68-123">驗證機制</span><span class="sxs-lookup"><span data-stu-id="04f68-123">Authentication mechanism</span></span> | <span data-ttu-id="04f68-124">說明</span><span class="sxs-lookup"><span data-stu-id="04f68-124">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="04f68-125">`None` (預設)</span><span class="sxs-lookup"><span data-stu-id="04f68-125">`None` (default)</span></span>         | <span data-ttu-id="04f68-126">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="04f68-126">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="04f68-127">使用 ASP.NET Core 儲存在應用程式中的使用者Identity</span><span class="sxs-lookup"><span data-stu-id="04f68-127">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="04f68-128">儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者</span><span class="sxs-lookup"><span data-stu-id="04f68-128">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="04f68-129">單一租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="04f68-129">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="04f68-130">多個租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="04f68-130">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="04f68-131">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="04f68-131">Windows Authentication</span></span> |

<span data-ttu-id="04f68-132">使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="04f68-132">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="04f68-133">建立專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="04f68-133">Create a folder for the project.</span></span>
* <span data-ttu-id="04f68-134">命名專案。</span><span class="sxs-lookup"><span data-stu-id="04f68-134">Name the project.</span></span>

<span data-ttu-id="04f68-135">如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。</span><span class="sxs-lookup"><span data-stu-id="04f68-135">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="04f68-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="04f68-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="04f68-137">遵循文章中的 Visual Studio for Mac 指導方針 <xref:blazor/get-started> 。</span><span class="sxs-lookup"><span data-stu-id="04f68-137">Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.</span></span>

1. <span data-ttu-id="04f68-138">在 [**設定新的 Blazor Server 應用程式**] 步驟上，從 [**驗證**] 下拉式選單選取 [**個別驗證（應用程式內）** ]。</span><span class="sxs-lookup"><span data-stu-id="04f68-138">On the **Configure your new Blazor Server App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="04f68-139">應用程式會針對以 ASP.NET Core 儲存在應用程式中的個別使用者而建立 Identity 。</span><span class="sxs-lookup"><span data-stu-id="04f68-139">The app is created for individual users stored in the app with ASP.NET Core Identity.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="04f68-140">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="04f68-140">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="04f68-141">遵循文章中的 .NET Core CLI 指導方針 <xref:blazor/get-started> ，使用驗證機制來建立新的 Blazor Server 專案：</span><span class="sxs-lookup"><span data-stu-id="04f68-141">Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="04f68-142">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="04f68-142">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="04f68-143">驗證機制</span><span class="sxs-lookup"><span data-stu-id="04f68-143">Authentication mechanism</span></span> | <span data-ttu-id="04f68-144">說明</span><span class="sxs-lookup"><span data-stu-id="04f68-144">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="04f68-145">`None` (預設)</span><span class="sxs-lookup"><span data-stu-id="04f68-145">`None` (default)</span></span>         | <span data-ttu-id="04f68-146">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="04f68-146">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="04f68-147">使用 ASP.NET Core 儲存在應用程式中的使用者Identity</span><span class="sxs-lookup"><span data-stu-id="04f68-147">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="04f68-148">儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者</span><span class="sxs-lookup"><span data-stu-id="04f68-148">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="04f68-149">單一租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="04f68-149">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="04f68-150">多個租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="04f68-150">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="04f68-151">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="04f68-151">Windows Authentication</span></span> |

<span data-ttu-id="04f68-152">使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="04f68-152">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="04f68-153">建立專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="04f68-153">Create a folder for the project.</span></span>
* <span data-ttu-id="04f68-154">命名專案。</span><span class="sxs-lookup"><span data-stu-id="04f68-154">Name the project.</span></span>

<span data-ttu-id="04f68-155">如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。</span><span class="sxs-lookup"><span data-stu-id="04f68-155">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

---

## <a name="scaffold-identity"></a><span data-ttu-id="04f68-156">ScaffoldIdentity</span><span class="sxs-lookup"><span data-stu-id="04f68-156">Scaffold Identity</span></span>

<span data-ttu-id="04f68-157">Scaffold Identity 至 Blazor Server 專案：</span><span class="sxs-lookup"><span data-stu-id="04f68-157">Scaffold Identity into a Blazor Server project:</span></span>

* <span data-ttu-id="04f68-158">[沒有現有的授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization)。</span><span class="sxs-lookup"><span data-stu-id="04f68-158">[Without existing authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization).</span></span>
* <span data-ttu-id="04f68-159">[具有授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization)。</span><span class="sxs-lookup"><span data-stu-id="04f68-159">[With authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization).</span></span>
