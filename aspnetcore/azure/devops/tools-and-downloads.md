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
# <a name="tools-and-downloads"></a>工具及下載

Azure 具有多個用於預配和管理資源的介面,例如 Azure[門戶](https://portal.azure.com)[、Azure CLI、Azure](/cli/azure/) [PowerShell、Azure](/powershell/azure/overview)[雲外殼](https://shell.azure.com/bash)和可視化工作室。 本指南採用簡約的方法,並盡可能使用 Azure 雲外殼來減少所需的步驟。 但是,Azure 門戶必須用於某些部分。

## <a name="prerequisites"></a>Prerequisites

需要以下訂閱:

* Azure&mdash;如果您沒有帳戶,[則取得免費試用](https://azure.microsoft.com/free/)。
* Azure DevOps&mdash;服務將創建 Azure DevOps 訂閱和組織,在第 4 章中創建。
* GitHub&mdash;如果您沒有帳戶,請[免費註冊](https://github.com/join)。

需要以下工具:

* [本指南](https://git-scm.com/downloads)&mdash;建議對 Git 的基本理解。 檢視[Git 文件](https://git-scm.com/doc),特別是[git 遠端](https://git-scm.com/docs/git-remote)與[git 推送](https://git-scm.com/docs/git-push)。
* [.NET 核心 SDK](https://dotnet.microsoft.com/download/)&mdash;版本 2.1.300 或更高版本需要生成和運行範例應用。 如果可視化工作室安裝了 **.NET Core 跨平台開發**工作負載,則.NET 核心 SDK 已安裝。

    驗證 .NET 核心 SDK 安裝。 開啟命令外殼,並執行以下命令:

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>建議工具(只有限制)

* [Visual Studio](https://visualstudio.microsoft.com)強大的 Azure 工具為本指南中描述的大多數功能提供了 GUI。 任何版本的視覺工作室將工作,包括免費的視覺工作室社區版。 這些教學旨在展示開發、部署和 DevOps,包括 Visual Studio 和非 Visual Studio。

  確認 Visual Studio 已安裝以下[工作負載](/visualstudio/install/modify-visual-studio):

  * ASP.NET 和 Web 開發
  * Azure 開發
  * .NET Core 跨平台開發
