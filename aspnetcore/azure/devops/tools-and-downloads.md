---
title: 工具和下載-使用 ASP.NET Core 和 Azure DevOps
author: CamSoper
description: 使用 ASP.NET Core 和 Azure 進行 DevOps 所需的工具和下載。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: ed8aee214ff9b9e941aeea01887882c3bdfc56a7
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85400293"
---
# <a name="tools-and-downloads"></a>工具及下載

Azure 提供數個介面來布建和管理資源，例如[Azure 入口網站](https://portal.azure.com)、 [Azure CLI](/cli/azure/)、 [Azure PowerShell](/powershell/azure/overview)、 [Azure Cloud Shell](https://shell.azure.com/bash)和 Visual Studio。 本指南會採用極簡方法，並盡可能使用 Azure Cloud Shell，以減少所需的步驟。 不過，Azure 入口網站必須用於某些部分。

## <a name="prerequisites"></a>必要條件

需要下列訂用帳戶：

* Azure &mdash; 如果您沒有帳戶，請[取得免費試用版](https://azure.microsoft.com/free/dotnet/)。
* &mdash;您的 Azure DevOps 訂用帳戶和組織 Azure DevOps Services 在第4章中建立。
* GitHub &mdash; 如果您沒有帳戶，請[免費註冊](https://github.com/join)。

需要下列工具：

* [Git](https://git-scm.com/downloads) &mdash;建議您在本指南中瞭解 Git 的基本概念。 請參閱[git 檔](https://git-scm.com/doc)，特別是[git 遠端](https://git-scm.com/docs/git-remote)和[git push](https://git-scm.com/docs/git-push)。
* [.NET Core SDK](https://dotnet.microsoft.com/download/) &mdash;需要2.1.300 或更新版本，才能建立並執行範例應用程式。 如果 Visual Studio 與 **.Net Core 跨平臺開發**工作負載一起安裝，則已安裝 .NET Core SDK。

    確認您的 .NET Core SDK 安裝。 開啟命令 shell，然後執行下列命令：

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>建議的工具（僅限 Windows）

* [Visual Studio](https://visualstudio.microsoft.com)的健全 Azure 工具提供 GUI，可用於本指南中所述的大部分功能。 任何版本的 Visual Studio 都可以使用，包括免費的 Visual Studio Community 版本。 這些教學課程的撰寫是為了示範開發、部署和 DevOps，以及是否 Visual Studio。

  確認 Visual Studio 已安裝下列[工作負載](/visualstudio/install/modify-visual-studio)：

  * ASP.NET 和 Web 開發
  * Azure 開發
  * .NET Core 跨平台開發
