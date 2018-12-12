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
# <a name="tools-and-downloads"></a>工具和下載

Azure 有數個介面，來佈建和管理資源，例如[Azure 入口網站](https://portal.azure.com)， [Azure CLI](/cli/azure/)， [Azure PowerShell](/powershell/azure/overview)， [Azure 雲端Shell](https://shell.azure.com/bash)，和 Visual Studio。 本指南會採用最簡單的方法，並使用 Azure Cloud Shell 中盡可能減少所需的步驟。 不過，必須使用 Azure 入口網站的某些部分。

## <a name="prerequisites"></a>必要條件

以下的訂用帳戶是必要的：

* Azure&mdash;如果您沒有帳戶，請[取得免費試用](https://azure.microsoft.com/free/)。
* Azure 的 DevOps 服務&mdash;第 4 章中建立您的 Azure DevOps 的訂用帳戶和組織。
* GitHub&mdash;如果您沒有帳戶，請[免費註冊](https://github.com/join)。

下列工具是必要的：

* [Git](https://git-scm.com/downloads) &mdash; Git 有基本了解建議您使用本指南中。 檢閱[Git 文件](https://git-scm.com/doc)，特別[git 遠端](https://git-scm.com/docs/git-remote)並[git 推送](https://git-scm.com/docs/git-push)。
* [.NET core SDK](https://www.microsoft.com/net/download/) &mdash; 2.1.300 版或更新版本才可建置並執行範例應用程式。 如果 Visual Studio 會隨 **.NET Core 跨平台開發**工作負載中，.NET Core SDK 已安裝。

    確認您的.NET Core SDK 安裝。 開啟命令殼層中，然後執行下列命令：

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>建議的工具 (僅 Windows)

* [Visual Studio](https://www.visualstudio.com/)的功能強大的 Azure 工具提供 GUI 來執行大部分的本指南中所述的功能。 任何版本的 Visual Studio 將會運作，包括免費的 Visual Studio Community Edition。 教學課程會示範使用和不使用 Visual Studio 的開發、 部署及 DevOps 寫入。

  確認 Visual Studio 具有下列[工作負載](/visualstudio/install/modify-visual-studio)安裝：

  * ASP.NET 與網頁程式開發
  * Azure 開發
  * .NET Core 跨平台開發
