---
title: 工具並下載 - 具有 ASP.NET 核心與 Azure 的 DevOps
author: CamSoper
description: 使用ASP.NET核心和Azure的DevOps所需的工具和下載。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 9c1042dd48b9167209b46e97a09e011b80e2609c
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79511141"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="d6f2a-103">工具及下載</span><span class="sxs-lookup"><span data-stu-id="d6f2a-103">Tools and downloads</span></span>

<span data-ttu-id="d6f2a-104">Azure 具有多個用於預配和管理資源的介面,例如 Azure[門戶](https://portal.azure.com)[、Azure CLI、Azure](/cli/azure/) [PowerShell、Azure](/powershell/azure/overview)[雲外殼](https://shell.azure.com/bash)和可視化工作室。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="d6f2a-105">本指南採用簡約的方法,並盡可能使用 Azure 雲外殼來減少所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="d6f2a-106">但是,Azure 門戶必須用於某些部分。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6f2a-107">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="d6f2a-107">Prerequisites</span></span>

<span data-ttu-id="d6f2a-108">需要以下訂閱:</span><span class="sxs-lookup"><span data-stu-id="d6f2a-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="d6f2a-109">Azure&mdash;如果您沒有帳戶,[則取得免費試用](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="d6f2a-110">Azure DevOps&mdash;服務將創建 Azure DevOps 訂閱和組織,在第 4 章中創建。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="d6f2a-111">GitHub&mdash;如果您沒有帳戶,請[免費註冊](https://github.com/join)。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="d6f2a-112">需要以下工具:</span><span class="sxs-lookup"><span data-stu-id="d6f2a-112">The following tools are required:</span></span>

* <span data-ttu-id="d6f2a-113">[本指南](https://git-scm.com/downloads)&mdash;建議對 Git 的基本理解。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="d6f2a-114">檢視[Git 文件](https://git-scm.com/doc),特別是[git 遠端](https://git-scm.com/docs/git-remote)與[git 推送](https://git-scm.com/docs/git-push)。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="d6f2a-115">[.NET 核心 SDK](https://dotnet.microsoft.com/download/)&mdash;版本 2.1.300 或更高版本需要生成和運行範例應用。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-115">[.NET Core SDK](https://dotnet.microsoft.com/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="d6f2a-116">如果可視化工作室安裝了 **.NET Core 跨平台開發**工作負載,則.NET 核心 SDK 已安裝。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="d6f2a-117">驗證 .NET 核心 SDK 安裝。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="d6f2a-118">開啟命令外殼,並執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="d6f2a-118">Open a command shell, and run the following command:</span></span>

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="d6f2a-119">建議工具(只有限制)</span><span class="sxs-lookup"><span data-stu-id="d6f2a-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="d6f2a-120">[Visual Studio](https://visualstudio.microsoft.com)強大的 Azure 工具為本指南中描述的大多數功能提供了 GUI。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-120">[Visual Studio](https://visualstudio.microsoft.com)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="d6f2a-121">任何版本的視覺工作室將工作,包括免費的視覺工作室社區版。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="d6f2a-122">這些教學旨在展示開發、部署和 DevOps,包括 Visual Studio 和非 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="d6f2a-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="d6f2a-123">確認 Visual Studio 已安裝以下[工作負載](/visualstudio/install/modify-visual-studio):</span><span class="sxs-lookup"><span data-stu-id="d6f2a-123">Confirm that Visual Studio has the following [workloads](/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="d6f2a-124">ASP.NET 和 Web 開發</span><span class="sxs-lookup"><span data-stu-id="d6f2a-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="d6f2a-125">Azure 開發</span><span class="sxs-lookup"><span data-stu-id="d6f2a-125">Azure development</span></span>
  * <span data-ttu-id="d6f2a-126">.NET Core 跨平台開發</span><span class="sxs-lookup"><span data-stu-id="d6f2a-126">.NET Core cross-platform development</span></span>
