---
title: 工具和下載-使用 ASP.NET Core 和 Azure 的 DevOps
author: CamSoper
description: 工具和下載所需的 ASP.NET Core 與 Azure 的 DevOps。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 0c64e723f1b912323103f201a66c1edaeccdcc2d
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284341"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="1eabb-103">工具和下載</span><span class="sxs-lookup"><span data-stu-id="1eabb-103">Tools and downloads</span></span>

<span data-ttu-id="1eabb-104">Azure 有數個介面，來佈建和管理資源，例如[Azure 入口網站](https://portal.azure.com)， [Azure CLI](/cli/azure/)， [Azure PowerShell](/powershell/azure/overview)， [Azure 雲端Shell](https://shell.azure.com/bash)，和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="1eabb-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="1eabb-105">本指南會採用最簡單的方法，並使用 Azure Cloud Shell 中盡可能減少所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="1eabb-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="1eabb-106">不過，必須使用 Azure 入口網站的某些部分。</span><span class="sxs-lookup"><span data-stu-id="1eabb-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1eabb-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="1eabb-107">Prerequisites</span></span>

<span data-ttu-id="1eabb-108">以下的訂用帳戶是必要的：</span><span class="sxs-lookup"><span data-stu-id="1eabb-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="1eabb-109">Azure&mdash;如果您沒有帳戶，請[取得免費試用](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="1eabb-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="1eabb-110">Azure 的 DevOps 服務&mdash;第 4 章中建立您的 Azure DevOps 的訂用帳戶和組織。</span><span class="sxs-lookup"><span data-stu-id="1eabb-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="1eabb-111">GitHub&mdash;如果您沒有帳戶，請[免費註冊](https://github.com/join)。</span><span class="sxs-lookup"><span data-stu-id="1eabb-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="1eabb-112">下列工具是必要的：</span><span class="sxs-lookup"><span data-stu-id="1eabb-112">The following tools are required:</span></span>

* <span data-ttu-id="1eabb-113">[Git](https://git-scm.com/downloads) &mdash; Git 有基本了解建議您使用本指南中。</span><span class="sxs-lookup"><span data-stu-id="1eabb-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="1eabb-114">檢閱[Git 文件](https://git-scm.com/doc)，特別[git 遠端](https://git-scm.com/docs/git-remote)並[git 推送](https://git-scm.com/docs/git-push)。</span><span class="sxs-lookup"><span data-stu-id="1eabb-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="1eabb-115">[.NET core SDK](https://www.microsoft.com/net/download/) &mdash; 2.1.300 版或更新版本才可建置並執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="1eabb-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="1eabb-116">如果 Visual Studio 會隨 **.NET Core 跨平台開發**工作負載中，.NET Core SDK 已安裝。</span><span class="sxs-lookup"><span data-stu-id="1eabb-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="1eabb-117">確認您的.NET Core SDK 安裝。</span><span class="sxs-lookup"><span data-stu-id="1eabb-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="1eabb-118">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1eabb-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="1eabb-119">建議的工具 (僅 Windows)</span><span class="sxs-lookup"><span data-stu-id="1eabb-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="1eabb-120">[Visual Studio](https://www.visualstudio.com/)的功能強大的 Azure 工具提供 GUI 來執行大部分的本指南中所述的功能。</span><span class="sxs-lookup"><span data-stu-id="1eabb-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="1eabb-121">任何版本的 Visual Studio 將會運作，包括免費的 Visual Studio Community Edition。</span><span class="sxs-lookup"><span data-stu-id="1eabb-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="1eabb-122">教學課程會示範使用和不使用 Visual Studio 的開發、 部署及 DevOps 寫入。</span><span class="sxs-lookup"><span data-stu-id="1eabb-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="1eabb-123">確認 Visual Studio 具有下列[工作負載](/visualstudio/install/modify-visual-studio)安裝：</span><span class="sxs-lookup"><span data-stu-id="1eabb-123">Confirm that Visual Studio has the following [workloads](/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="1eabb-124">ASP.NET 與網頁程式開發</span><span class="sxs-lookup"><span data-stu-id="1eabb-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="1eabb-125">Azure 開發</span><span class="sxs-lookup"><span data-stu-id="1eabb-125">Azure development</span></span>
  * <span data-ttu-id="1eabb-126">.NET Core 跨平台開發</span><span class="sxs-lookup"><span data-stu-id="1eabb-126">.NET Core cross-platform development</span></span>
