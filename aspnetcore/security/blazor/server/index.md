---
title: 保護 ASP.NET Core Blazor伺服器應用程式
author: guardrex
description: 瞭解如何以 ASP.NET Core Blazor的應用程式來保護伺服器應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/server/index
ms.openlocfilehash: 0021911b731e57bc6eabf857c27a13462e7400ae
ms.sourcegitcommit: 56861af66bb364a5d60c3c72d133d854b4cf292d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82206365"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a><span data-ttu-id="fee87-103">保護 ASP.NET Core Blazor 伺服器應用程式</span><span class="sxs-lookup"><span data-stu-id="fee87-103">Secure ASP.NET Core Blazor Server apps</span></span>

<span data-ttu-id="fee87-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fee87-104">By [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="blazor-server-project-template"></a><span data-ttu-id="fee87-105">Blazor 伺服器專案範本</span><span class="sxs-lookup"><span data-stu-id="fee87-105">Blazor Server project template</span></span>

<span data-ttu-id="fee87-106">建立專案時，可以設定 Blazor 伺服器專案範本進行驗證。</span><span class="sxs-lookup"><span data-stu-id="fee87-106">The Blazor Server project template can be configured for authentication when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="fee87-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fee87-107">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fee87-108">遵循<xref:blazor/get-started>文章中的 Visual Studio 指導方針，使用驗證機制建立新的 Blazor 伺服器專案。</span><span class="sxs-lookup"><span data-stu-id="fee87-108">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="fee87-109">在 [建立新的 ASP.NET Core Web 應用程式]\*\*\*\* 對話方塊中選擇 [Blazor 伺服器應用程式]\*\*\*\* 範本之後，請選取 [驗證]\*\*\*\* 下的 [變更]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fee87-109">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="fee87-110">對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：</span><span class="sxs-lookup"><span data-stu-id="fee87-110">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="fee87-111">**無驗證**</span><span class="sxs-lookup"><span data-stu-id="fee87-111">**No Authentication**</span></span>
* <span data-ttu-id="fee87-112">**個別使用者帳戶** &ndash; 使用者帳戶能以下列方式儲存：</span><span class="sxs-lookup"><span data-stu-id="fee87-112">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="fee87-113">使用 ASP.NET Core 的[身分識別](xref:security/authentication/identity)系統儲存在應用程式內。</span><span class="sxs-lookup"><span data-stu-id="fee87-113">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="fee87-114">使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。</span><span class="sxs-lookup"><span data-stu-id="fee87-114">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="fee87-115">**工作或學校帳戶**</span><span class="sxs-lookup"><span data-stu-id="fee87-115">**Work or School Accounts**</span></span>
* <span data-ttu-id="fee87-116">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="fee87-116">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="fee87-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fee87-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fee87-118">請遵循<xref:blazor/get-started>本文中的 Visual Studio Code 指導方針，使用驗證機制建立新的 Blazor 伺服器專案：</span><span class="sxs-lookup"><span data-stu-id="fee87-118">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="fee87-119">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="fee87-119">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="fee87-120">驗證機制</span><span class="sxs-lookup"><span data-stu-id="fee87-120">Authentication mechanism</span></span> | <span data-ttu-id="fee87-121">描述</span><span class="sxs-lookup"><span data-stu-id="fee87-121">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="fee87-122">`None` (預設值)</span><span class="sxs-lookup"><span data-stu-id="fee87-122">`None` (default)</span></span>         | <span data-ttu-id="fee87-123">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="fee87-123">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="fee87-124">以 ASP.NET Core 身分識別儲存在應用程式中的使用者</span><span class="sxs-lookup"><span data-stu-id="fee87-124">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="fee87-125">儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者</span><span class="sxs-lookup"><span data-stu-id="fee87-125">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="fee87-126">單一租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="fee87-126">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="fee87-127">多個租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="fee87-127">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="fee87-128">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fee87-128">Windows Authentication</span></span> |

<span data-ttu-id="fee87-129">使用`-o|--output`選項時，此命令會使用提供給`{APP NAME}`預留位置的值來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="fee87-129">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="fee87-130">建立專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="fee87-130">Create a folder for the project.</span></span>
* <span data-ttu-id="fee87-131">命名專案。</span><span class="sxs-lookup"><span data-stu-id="fee87-131">Name the project.</span></span>

<span data-ttu-id="fee87-132">如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。</span><span class="sxs-lookup"><span data-stu-id="fee87-132">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="fee87-133">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fee87-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="fee87-134">遵循<xref:blazor/get-started>文章中的 Visual Studio for Mac 指導方針。</span><span class="sxs-lookup"><span data-stu-id="fee87-134">Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.</span></span>

1. <span data-ttu-id="fee87-135">在 [**設定您的新 Blazor 伺服器應用程式**] 步驟上，從 [**驗證**] 下拉式選單選取 [**個別驗證（應用程式內）** ]。</span><span class="sxs-lookup"><span data-stu-id="fee87-135">On the **Configure your new Blazor Server App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="fee87-136">應用程式是針對儲存在應用程式中 ASP.NET Core 身分識別的個別使用者所建立。</span><span class="sxs-lookup"><span data-stu-id="fee87-136">The app is created for individual users stored in the app with ASP.NET Core Identity.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="fee87-137">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fee87-137">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="fee87-138">請遵循<xref:blazor/get-started>本文中的 .NET Core CLI 指導方針，使用驗證機制建立新的 Blazor 伺服器專案：</span><span class="sxs-lookup"><span data-stu-id="fee87-138">Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="fee87-139">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="fee87-139">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="fee87-140">驗證機制</span><span class="sxs-lookup"><span data-stu-id="fee87-140">Authentication mechanism</span></span> | <span data-ttu-id="fee87-141">描述</span><span class="sxs-lookup"><span data-stu-id="fee87-141">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="fee87-142">`None` (預設值)</span><span class="sxs-lookup"><span data-stu-id="fee87-142">`None` (default)</span></span>         | <span data-ttu-id="fee87-143">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="fee87-143">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="fee87-144">以 ASP.NET Core 身分識別儲存在應用程式中的使用者</span><span class="sxs-lookup"><span data-stu-id="fee87-144">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="fee87-145">儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者</span><span class="sxs-lookup"><span data-stu-id="fee87-145">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="fee87-146">單一租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="fee87-146">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="fee87-147">多個租使用者的組織驗證</span><span class="sxs-lookup"><span data-stu-id="fee87-147">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="fee87-148">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fee87-148">Windows Authentication</span></span> |

<span data-ttu-id="fee87-149">使用`-o|--output`選項時，此命令會使用提供給`{APP NAME}`預留位置的值來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="fee87-149">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="fee87-150">建立專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="fee87-150">Create a folder for the project.</span></span>
* <span data-ttu-id="fee87-151">命名專案。</span><span class="sxs-lookup"><span data-stu-id="fee87-151">Name the project.</span></span>

<span data-ttu-id="fee87-152">如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。</span><span class="sxs-lookup"><span data-stu-id="fee87-152">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

---
