---
title: 工具和下載-使用 ASP.NET Core 和 Azure DevOps
author: CamSoper
description: 使用 ASP.NET Core 和 Azure 進行 DevOps 所需的工具和下載。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 1917a329a5dcbe60542541cfcdc746799307e3d5
ms.sourcegitcommit: 4a9321db7ca4e69074fa08a678dcc91e16215b1e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850405"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="9f50d-103">工具及下載</span><span class="sxs-lookup"><span data-stu-id="9f50d-103">Tools and downloads</span></span>

<span data-ttu-id="9f50d-104">Azure 提供數個介面來布建和管理資源，例如[Azure 入口網站](https://portal.azure.com)、 [Azure CLI](/cli/azure/)、 [Azure PowerShell](/powershell/azure/overview)、 [Azure Cloud Shell](https://shell.azure.com/bash)和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="9f50d-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="9f50d-105">本指南會採用極簡方法，並盡可能使用 Azure Cloud Shell，以減少所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="9f50d-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="9f50d-106">不過，Azure 入口網站必須用於某些部分。</span><span class="sxs-lookup"><span data-stu-id="9f50d-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f50d-107">先決條件</span><span class="sxs-lookup"><span data-stu-id="9f50d-107">Prerequisites</span></span>

<span data-ttu-id="9f50d-108">需要下列訂用帳戶：</span><span class="sxs-lookup"><span data-stu-id="9f50d-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="9f50d-109">Azure &mdash;如果您沒有帳戶，請[取得免費試用版](https://azure.microsoft.com/free/dotnet/)。</span><span class="sxs-lookup"><span data-stu-id="9f50d-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/dotnet/).</span></span>
* <span data-ttu-id="9f50d-110">您&mdash;的 Azure DevOps 訂用帳戶和組織 Azure DevOps Services 在第4章中建立。</span><span class="sxs-lookup"><span data-stu-id="9f50d-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="9f50d-111">GitHub &mdash;如果您沒有帳戶，請[免費註冊](https://github.com/join)。</span><span class="sxs-lookup"><span data-stu-id="9f50d-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="9f50d-112">需要下列工具：</span><span class="sxs-lookup"><span data-stu-id="9f50d-112">The following tools are required:</span></span>

* <span data-ttu-id="9f50d-113">[Git](https://git-scm.com/downloads) &mdash;建議您在本指南中瞭解 git 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9f50d-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="9f50d-114">請參閱[git 檔](https://git-scm.com/doc)，特別是[git 遠端](https://git-scm.com/docs/git-remote)和[git push](https://git-scm.com/docs/git-push)。</span><span class="sxs-lookup"><span data-stu-id="9f50d-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="9f50d-115">需要[.NET Core SDK](https://dotnet.microsoft.com/download/) &mdash;版本2.1.300 或更新版本，才能建立並執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f50d-115">[.NET Core SDK](https://dotnet.microsoft.com/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="9f50d-116">如果 Visual Studio 與 **.Net Core 跨平臺開發**工作負載一起安裝，則已安裝 .NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="9f50d-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="9f50d-117">確認您的 .NET Core SDK 安裝。</span><span class="sxs-lookup"><span data-stu-id="9f50d-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="9f50d-118">開啟命令 shell，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9f50d-118">Open a command shell, and run the following command:</span></span>

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="9f50d-119">建議的工具（僅限 Windows）</span><span class="sxs-lookup"><span data-stu-id="9f50d-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="9f50d-120">[Visual Studio](https://visualstudio.microsoft.com)的健全 Azure 工具提供 GUI，可用於本指南中所述的大部分功能。</span><span class="sxs-lookup"><span data-stu-id="9f50d-120">[Visual Studio](https://visualstudio.microsoft.com)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="9f50d-121">任何版本的 Visual Studio 都可以使用，包括免費的 Visual Studio Community 版本。</span><span class="sxs-lookup"><span data-stu-id="9f50d-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="9f50d-122">這些教學課程的撰寫是為了示範開發、部署和 DevOps，以及是否 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="9f50d-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="9f50d-123">確認 Visual Studio 已安裝下列[工作負載](/visualstudio/install/modify-visual-studio)：</span><span class="sxs-lookup"><span data-stu-id="9f50d-123">Confirm that Visual Studio has the following [workloads](/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="9f50d-124">ASP.NET 和 Web 開發</span><span class="sxs-lookup"><span data-stu-id="9f50d-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="9f50d-125">Azure 開發</span><span class="sxs-lookup"><span data-stu-id="9f50d-125">Azure development</span></span>
  * <span data-ttu-id="9f50d-126">.NET Core 跨平台開發</span><span class="sxs-lookup"><span data-stu-id="9f50d-126">.NET Core cross-platform development</span></span>
