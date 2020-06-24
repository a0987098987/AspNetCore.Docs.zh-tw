---
title: 保護 ASP.NET Core Blazor 伺服器應用程式
author: guardrex
description: 瞭解如何以 Blazor ASP.NET Core 的應用程式來保護伺服器應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/server/index
ms.openlocfilehash: 2811e08fd2f6c66112ffa0bb40f474158f4c7a59
ms.sourcegitcommit: 5e462c3328c70f95969d02adce9c71592049f54c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85292681"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a><span data-ttu-id="72ffa-103">保護 ASP.NET Core Blazor 伺服器應用程式</span><span class="sxs-lookup"><span data-stu-id="72ffa-103">Secure ASP.NET Core Blazor Server apps</span></span>

<span data-ttu-id="72ffa-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="72ffa-104">By [Luke Latham](https://github.com/guardrex)</span></span>

Blazor<span data-ttu-id="72ffa-105">伺服器應用程式的安全性設定方式與 ASP.NET Core 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="72ffa-105"> Server apps are configured for security in the same manner as ASP.NET Core apps.</span></span> <span data-ttu-id="72ffa-106">如需詳細資訊，請參閱底下的文章 <xref:security/index> 。</span><span class="sxs-lookup"><span data-stu-id="72ffa-106">For more information, see the articles under <xref:security/index>.</span></span> <span data-ttu-id="72ffa-107">本總覽底下的主題特別適用于 Blazor 伺服器。</span><span class="sxs-lookup"><span data-stu-id="72ffa-107">Topics under this overview apply specifically to Blazor Server.</span></span> 

## <a name="blazor-server-project-template"></a>Blazor<span data-ttu-id="72ffa-108">伺服器專案範本</span><span class="sxs-lookup"><span data-stu-id="72ffa-108"> Server project template</span></span>

<span data-ttu-id="72ffa-109">Blazor建立專案時，可以設定伺服器專案範本進行驗證。</span><span class="sxs-lookup"><span data-stu-id="72ffa-109">The Blazor Server project template can be configured for authentication when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="72ffa-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72ffa-110">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="72ffa-111">遵循文章中的 Visual Studio 指導方針 <xref:blazor/get-started> ， Blazor 使用驗證機制建立新的伺服器專案。</span><span class="sxs-lookup"><span data-stu-id="72ffa-111">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="72ffa-112">在 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中選擇\*\* Blazor 伺服器應用程式**範本之後，請選取 [**驗證**] 底下的 [**變更\*\*]。</span><span class="sxs-lookup"><span data-stu-id="72ffa-112">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="72ffa-113">對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：</span><span class="sxs-lookup"><span data-stu-id="72ffa-113">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="72ffa-114">**無驗證**</span><span class="sxs-lookup"><span data-stu-id="72ffa-114">**No Authentication**</span></span>
* <span data-ttu-id="72ffa-115">**個別使用者帳戶**：可以儲存使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="72ffa-115">**Individual User Accounts**: User accounts can be stored:</span></span>
  * <span data-ttu-id="72ffa-116">在應用程式中使用 ASP.NET Core 的 [Identity](xref:security/authentication/identity) 系統。</span><span class="sxs-lookup"><span data-stu-id="72ffa-116">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="72ffa-117">使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。</span><span class="sxs-lookup"><span data-stu-id="72ffa-117">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="72ffa-118">**工作或學校帳戶**</span><span class="sxs-lookup"><span data-stu-id="72ffa-118">**Work or School Accounts**</span></span>
* <span data-ttu-id="72ffa-119">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="72ffa-119">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="72ffa-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="72ffa-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="72ffa-121">請遵循本文中的 Visual Studio Code 指導方針 <xref:blazor/get-started> ， Blazor 使用驗證機制建立新的伺服器專案：</span><span class="sxs-lookup"><span data-stu-id="72ffa-121">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="72ffa-122">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="72ffa-122">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="72ffa-123">驗證機制</span><span class="sxs-lookup"><span data-stu-id="72ffa-123">Authentication mechanism</span></span> | <span data-ttu-id="72ffa-124">Description</span><span class="sxs-lookup"><span data-stu-id="72ffa-124">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="72ffa-125">`None` (預設)</span><span class="sxs-lookup"><span data-stu-id="72ffa-125">`None` (default)</span></span>         | <span data-ttu-id="72ffa-126">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="72ffa-126">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="72ffa-127">使用 ASP.NET Core 儲存在應用程式中的使用者Identity</span><span class="sxs-lookup"><span data-stu-id="72ffa-127">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="72ffa-128">儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者</span><span class="sxs-lookup"><span data-stu-id="72ffa-128">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="72ffa-129">單一租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="72ffa-129">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="72ffa-130">多個租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="72ffa-130">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="72ffa-131">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="72ffa-131">Windows Authentication</span></span> |

<span data-ttu-id="72ffa-132">使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="72ffa-132">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="72ffa-133">建立專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="72ffa-133">Create a folder for the project.</span></span>
* <span data-ttu-id="72ffa-134">命名專案。</span><span class="sxs-lookup"><span data-stu-id="72ffa-134">Name the project.</span></span>

<span data-ttu-id="72ffa-135">如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。</span><span class="sxs-lookup"><span data-stu-id="72ffa-135">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="72ffa-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="72ffa-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="72ffa-137">遵循文章中的 Visual Studio for Mac 指導方針 <xref:blazor/get-started> 。</span><span class="sxs-lookup"><span data-stu-id="72ffa-137">Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.</span></span>

1. <span data-ttu-id="72ffa-138">在 [**設定新的 Blazor 伺服器應用程式**] 步驟上，從 [**驗證**] 下拉式選單選取 [**個別驗證（應用程式內）** ]。</span><span class="sxs-lookup"><span data-stu-id="72ffa-138">On the **Configure your new Blazor Server App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="72ffa-139">應用程式會針對以 ASP.NET Core 儲存在應用程式中的個別使用者而建立 Identity 。</span><span class="sxs-lookup"><span data-stu-id="72ffa-139">The app is created for individual users stored in the app with ASP.NET Core Identity.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="72ffa-140">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="72ffa-140">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="72ffa-141">請遵循本文中的 .NET Core CLI 指導方針 <xref:blazor/get-started> ， Blazor 使用驗證機制建立新的伺服器專案：</span><span class="sxs-lookup"><span data-stu-id="72ffa-141">Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="72ffa-142">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="72ffa-142">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="72ffa-143">驗證機制</span><span class="sxs-lookup"><span data-stu-id="72ffa-143">Authentication mechanism</span></span> | <span data-ttu-id="72ffa-144">Description</span><span class="sxs-lookup"><span data-stu-id="72ffa-144">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="72ffa-145">`None` (預設)</span><span class="sxs-lookup"><span data-stu-id="72ffa-145">`None` (default)</span></span>         | <span data-ttu-id="72ffa-146">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="72ffa-146">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="72ffa-147">使用 ASP.NET Core 儲存在應用程式中的使用者Identity</span><span class="sxs-lookup"><span data-stu-id="72ffa-147">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="72ffa-148">儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者</span><span class="sxs-lookup"><span data-stu-id="72ffa-148">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="72ffa-149">單一租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="72ffa-149">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="72ffa-150">多個租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="72ffa-150">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="72ffa-151">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="72ffa-151">Windows Authentication</span></span> |

<span data-ttu-id="72ffa-152">使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="72ffa-152">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="72ffa-153">建立專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="72ffa-153">Create a folder for the project.</span></span>
* <span data-ttu-id="72ffa-154">命名專案。</span><span class="sxs-lookup"><span data-stu-id="72ffa-154">Name the project.</span></span>

<span data-ttu-id="72ffa-155">如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。</span><span class="sxs-lookup"><span data-stu-id="72ffa-155">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

---

## <a name="scaffold-identity"></a><span data-ttu-id="72ffa-156">ScaffoldIdentity</span><span class="sxs-lookup"><span data-stu-id="72ffa-156">Scaffold Identity</span></span>

<span data-ttu-id="72ffa-157">Scaffold Identity 至 Blazor 伺服器專案：</span><span class="sxs-lookup"><span data-stu-id="72ffa-157">Scaffold Identity into a Blazor Server project:</span></span>

* <span data-ttu-id="72ffa-158">[沒有現有的授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization)。</span><span class="sxs-lookup"><span data-stu-id="72ffa-158">[Without existing authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization).</span></span>
* <span data-ttu-id="72ffa-159">[具有授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization)。</span><span class="sxs-lookup"><span data-stu-id="72ffa-159">[With authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization).</span></span>
