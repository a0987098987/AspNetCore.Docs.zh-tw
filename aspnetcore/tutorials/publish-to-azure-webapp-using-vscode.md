---
title: 使用 Visual Studio Code 將 ASP.NET Core 應用程式發佈至 Azure
author: ricardoserradas
description: 了解如何使用 Visual Studio Code 將 ASP.NET Core 應用程式發佈至 Azure App Service
ms.author: riserrad
ms.custom: mvc
ms.date: 07/10/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: eaf9cca61b21d04d127ff15a579f3d8da794f7d9
ms.sourcegitcommit: 40dc9b00131985abcd99bd567647420d798e798a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/09/2020
ms.locfileid: "78935430"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a>使用 Visual Studio Code 將 ASP.NET Core 應用程式發佈至 Azure

作者：[Ricardo Serradas](https://twitter.com/ricardoserradas)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

若要針對 App Service 部署問題進行疑難排解，請參閱 <xref:test/troubleshoot-azure-iis>。

## <a name="intro"></a>簡介

在本教學課程中，您將了解如何建立 ASP.Net Core MVC 應用程式，並將其部署於 Visual Studio Code。

## <a name="set-up"></a>設定

- 如果您沒有帳戶，請開啟[免費的 Azure 帳戶](https://azure.microsoft.com/free/dotnet/)。
- 安裝 [.NET Core SDK](https://dotnet.microsoft.com/download)
- 安裝 [Visual Studio Code](https://code.visualstudio.com/Download)
  - 將 [C# 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)安裝至 Visual Studio Code
  - 安裝[Azure App Service 擴充](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)功能以 Visual Studio Code 並進行設定，然後再繼續

## <a name="create-an-aspnet-core-mvc-project"></a>建立 ASP.NET Core MVC 專案

使用終端機巡覽至您希望在其上建立專案的資料夾，並使用下列命令：

```dotnetcli
dotnet new mvc
```

您會擁有類似如下所示的資料夾結構：

```cmd
      appsettings.Development.json
      appsettings.json
<DIR> Controllers
<DIR> Models
      netcore-vscode.csproj
<DIR> obj
      Program.cs
<DIR> Properties
      Startup.cs
<DIR> Views
<DIR> wwwroot
```

## <a name="open-it-with-visual-studio-code"></a>使用 Visual Studio Code 加以開啟

建立專案後，您可以使用下列其中一個選項來以 Visual Studio Code 將其開啟：

### <a name="through-the-command-line"></a>透過命令列

在您建立專案的資料夾中使用下列命令：

```cmd
> code .
```

如果下列命令無法運作，請確認您的安裝是否已參考[此連結](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform)來正確設定。

### <a name="through-visual-studio-code-interface"></a>透過 Visual Studio Code 介面

- 開啟 Visual Studio Code
- 在功能表中，選取 `File > Open Folder`
- 選取您建立 MVC 專案的資料夾根目錄

開啟專案資料夾時，您會收到訊息指出遺失用於建置與偵錯的必要資產。 接受協助來予以新增。

![已載入專案的 Visual Studio Code 介面](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

`.vscode` 資料夾將建立於專案結構下方。 該資料夾會包含下列檔案：

```cmd
launch.json
tasks.json
```

這些是可協助您建置與偵錯 .NET Core Web 應用程式的公用程式檔案。

## <a name="run-the-app"></a>執行應用程式

在將應用程式部署至 Azure 之前，請確定其可在本機電腦上正常執行。

- 按 F5 以執行專案

您的 Web 應用程式會在預設瀏覽器的新索引標籤上開始執行。 啟動時您可能會看到隱私警告。 這是因為您的應用程式會使用 HTTP 或 HTTPS 啟動，且會根據預設巡覽至 HTTPS 端點。

![在本機對應用程式偵錯時的隱私警告](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

若要保留偵錯工作階段，請按一下 `Advanced`，然後 `Continue to localhost (unsafe)`。

## <a name="generate-the-deployment-package-locally"></a>在本機產生部署套件

- 開啟 Visual Studio Code 終端機
- 使用下列命令，在稱為 `Release` 的子資料夾中產生 `publish` 套件：
  - `dotnet publish -c Release -o ./publish`
- 新的 `publish` 資料夾將建立於專案結構下方

![發佈資料夾結構](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a>發佈到 Azure App Service

運用適用於 Visual Studio Code 的 Azure App Service 延伸模組，遵循下列步驟將該網站直接發佈至 Azure App Service。

### <a name="if-youre-creating-a-new-web-app"></a>您要建立新的 Web 應用程式

- 以滑鼠右鍵按一下 `publish` 資料夾並選取 `Deploy to Web App...`
- 選取您希望建立 Web 應用程式的訂用帳戶
- 選取 `Create New Web App`
- 輸入 Web 應用程式的名稱

延伸模組將會建立新的 Web 應用程式，並會自動開始將套件部署至該應用程式。 部署完成後，按一下 `Browse Website` 以驗證部署。

![部署成功訊息](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

您按一下 `Browse Website` 後，您會使用預設的瀏覽器來巡覽：

![已成功部署新 Web 應用程式](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a>如果您要部署至現有的 Web 應用程式

- 以滑鼠右鍵按一下 `publish` 資料夾並選取 `Deploy to Web App...`
- 選取現有 Web 應用程式所在的訂用帳戶
- 從清單中選取該 Web 應用程式
- Visual Studio Code 會詢問您是否要覆寫現有的內容。 按一下 `Deploy` 以確認

延伸模組會將更新的內容部署至該 Web 應用程式。 完成後，按一下 `Browse Website` 以驗證部署。

![已成功部署現有 Web 應用程式](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a>後續步驟

- [建立您的第一個 Azure DevOps 管線](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a>其他資源

- [Azure App Service](/azure/app-service/app-service-web-overview)
- [Azure 資源群組](/azure/azure-resource-manager/resource-group-overview#resource-groups)