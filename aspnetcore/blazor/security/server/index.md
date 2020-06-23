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
ms.openlocfilehash: 2bdd2f256f456cbf474181021fafc6830bfd68f4
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85242923"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a><span data-ttu-id="fdefe-103">保護 ASP.NET Core Blazor 伺服器應用程式</span><span class="sxs-lookup"><span data-stu-id="fdefe-103">Secure ASP.NET Core Blazor Server apps</span></span>

<span data-ttu-id="fdefe-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fdefe-104">By [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="blazor-server-project-template"></a>Blazor<span data-ttu-id="fdefe-105">伺服器專案範本</span><span class="sxs-lookup"><span data-stu-id="fdefe-105"> Server project template</span></span>

<span data-ttu-id="fdefe-106">Blazor建立專案時，可以設定伺服器專案範本進行驗證。</span><span class="sxs-lookup"><span data-stu-id="fdefe-106">The Blazor Server project template can be configured for authentication when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="fdefe-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdefe-107">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fdefe-108">遵循文章中的 Visual Studio 指導方針 <xref:blazor/get-started> ， Blazor 使用驗證機制建立新的伺服器專案。</span><span class="sxs-lookup"><span data-stu-id="fdefe-108">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="fdefe-109">在 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中選擇\*\* Blazor 伺服器應用程式**範本之後，請選取 [**驗證**] 底下的 [**變更\*\*]。</span><span class="sxs-lookup"><span data-stu-id="fdefe-109">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="fdefe-110">對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：</span><span class="sxs-lookup"><span data-stu-id="fdefe-110">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="fdefe-111">**無驗證**</span><span class="sxs-lookup"><span data-stu-id="fdefe-111">**No Authentication**</span></span>
* <span data-ttu-id="fdefe-112">**個別使用者帳戶**：可以儲存使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="fdefe-112">**Individual User Accounts**: User accounts can be stored:</span></span>
  * <span data-ttu-id="fdefe-113">在應用程式中使用 ASP.NET Core 的 [Identity](xref:security/authentication/identity) 系統。</span><span class="sxs-lookup"><span data-stu-id="fdefe-113">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="fdefe-114">使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。</span><span class="sxs-lookup"><span data-stu-id="fdefe-114">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="fdefe-115">**工作或學校帳戶**</span><span class="sxs-lookup"><span data-stu-id="fdefe-115">**Work or School Accounts**</span></span>
* <span data-ttu-id="fdefe-116">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="fdefe-116">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="fdefe-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fdefe-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fdefe-118">請遵循本文中的 Visual Studio Code 指導方針 <xref:blazor/get-started> ， Blazor 使用驗證機制建立新的伺服器專案：</span><span class="sxs-lookup"><span data-stu-id="fdefe-118">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="fdefe-119">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="fdefe-119">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="fdefe-120">驗證機制</span><span class="sxs-lookup"><span data-stu-id="fdefe-120">Authentication mechanism</span></span> | <span data-ttu-id="fdefe-121">描述</span><span class="sxs-lookup"><span data-stu-id="fdefe-121">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="fdefe-122">`None` (預設)</span><span class="sxs-lookup"><span data-stu-id="fdefe-122">`None` (default)</span></span>         | <span data-ttu-id="fdefe-123">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="fdefe-123">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="fdefe-124">使用 ASP.NET Core 儲存在應用程式中的使用者Identity</span><span class="sxs-lookup"><span data-stu-id="fdefe-124">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="fdefe-125">儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者</span><span class="sxs-lookup"><span data-stu-id="fdefe-125">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="fdefe-126">單一租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="fdefe-126">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="fdefe-127">多個租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="fdefe-127">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="fdefe-128">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fdefe-128">Windows Authentication</span></span> |

<span data-ttu-id="fdefe-129">使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="fdefe-129">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="fdefe-130">建立專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="fdefe-130">Create a folder for the project.</span></span>
* <span data-ttu-id="fdefe-131">命名專案。</span><span class="sxs-lookup"><span data-stu-id="fdefe-131">Name the project.</span></span>

<span data-ttu-id="fdefe-132">如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。</span><span class="sxs-lookup"><span data-stu-id="fdefe-132">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="fdefe-133">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fdefe-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="fdefe-134">遵循文章中的 Visual Studio for Mac 指導方針 <xref:blazor/get-started> 。</span><span class="sxs-lookup"><span data-stu-id="fdefe-134">Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.</span></span>

1. <span data-ttu-id="fdefe-135">在 [**設定新的 Blazor 伺服器應用程式**] 步驟上，從 [**驗證**] 下拉式選單選取 [**個別驗證（應用程式內）** ]。</span><span class="sxs-lookup"><span data-stu-id="fdefe-135">On the **Configure your new Blazor Server App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="fdefe-136">應用程式會針對以 ASP.NET Core 儲存在應用程式中的個別使用者而建立 Identity 。</span><span class="sxs-lookup"><span data-stu-id="fdefe-136">The app is created for individual users stored in the app with ASP.NET Core Identity.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="fdefe-137">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fdefe-137">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="fdefe-138">請遵循本文中的 .NET Core CLI 指導方針 <xref:blazor/get-started> ， Blazor 使用驗證機制建立新的伺服器專案：</span><span class="sxs-lookup"><span data-stu-id="fdefe-138">Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="fdefe-139">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="fdefe-139">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="fdefe-140">驗證機制</span><span class="sxs-lookup"><span data-stu-id="fdefe-140">Authentication mechanism</span></span> | <span data-ttu-id="fdefe-141">描述</span><span class="sxs-lookup"><span data-stu-id="fdefe-141">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="fdefe-142">`None` (預設)</span><span class="sxs-lookup"><span data-stu-id="fdefe-142">`None` (default)</span></span>         | <span data-ttu-id="fdefe-143">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="fdefe-143">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="fdefe-144">使用 ASP.NET Core 儲存在應用程式中的使用者Identity</span><span class="sxs-lookup"><span data-stu-id="fdefe-144">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="fdefe-145">儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者</span><span class="sxs-lookup"><span data-stu-id="fdefe-145">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="fdefe-146">單一租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="fdefe-146">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="fdefe-147">多個租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="fdefe-147">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="fdefe-148">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fdefe-148">Windows Authentication</span></span> |

<span data-ttu-id="fdefe-149">使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="fdefe-149">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="fdefe-150">建立專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="fdefe-150">Create a folder for the project.</span></span>
* <span data-ttu-id="fdefe-151">命名專案。</span><span class="sxs-lookup"><span data-stu-id="fdefe-151">Name the project.</span></span>

<span data-ttu-id="fdefe-152">如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。</span><span class="sxs-lookup"><span data-stu-id="fdefe-152">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

---

## <a name="secure-an-existing-app"></a><span data-ttu-id="fdefe-153">保護現有的應用程式</span><span class="sxs-lookup"><span data-stu-id="fdefe-153">Secure an existing app</span></span>

Blazor<span data-ttu-id="fdefe-154">伺服器應用程式的安全性設定方式與 ASP.NET Core 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="fdefe-154"> Server apps are configured for security in the same manner as ASP.NET Core apps.</span></span> <span data-ttu-id="fdefe-155">如需詳細資訊，請參閱底下的文章 <xref:security/index> 。</span><span class="sxs-lookup"><span data-stu-id="fdefe-155">For more information, see the articles under <xref:security/index>.</span></span>

## <a name="scaffold-identity"></a><span data-ttu-id="fdefe-156">ScaffoldIdentity</span><span class="sxs-lookup"><span data-stu-id="fdefe-156">Scaffold Identity</span></span>

<span data-ttu-id="fdefe-157">Scaffold Identity 至 Blazor 伺服器專案：</span><span class="sxs-lookup"><span data-stu-id="fdefe-157">Scaffold Identity into a Blazor Server project:</span></span>

* <span data-ttu-id="fdefe-158">[沒有現有的授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization)。</span><span class="sxs-lookup"><span data-stu-id="fdefe-158">[Without existing authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization).</span></span>
* <span data-ttu-id="fdefe-159">[具有授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization)。</span><span class="sxs-lookup"><span data-stu-id="fdefe-159">[With authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization).</span></span>
