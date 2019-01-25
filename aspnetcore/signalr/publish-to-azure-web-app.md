---
title: 發佈 ASP.NET Core SignalR 應用程式至 Azure Web 應用程式
author: bradygaster
description: 發佈 ASP.NET Core SignalR 應用程式至 Azure Web 應用程式
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 66fa855590c49c4284e4b42cae57f3d4d81dd0fc
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837672"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>發佈 ASP.NET Core SignalR 應用程式，為 Azure Web 應用程式

[Azure Web 應用程式](/azure/app-service/app-service-web-overview)已[Microsoft 雲端運算](https://azure.microsoft.com/)裝載 web 應用程式，包括 ASP.NET Core 的平台服務。

> [!NOTE]
> 這篇文章是指從 Visual Studio 發佈的 ASP.NET Core SignalR 應用程式。 請瀏覽[適用於 Azure SignalR 服務](https://azure.microsoft.com/en-gb/services/signalr-service?)如需有關在 Azure 上使用 SignalR。

## <a name="publish-the-app"></a>發行應用程式

Visual Studio 會提供內建工具發行至 Azure Web 應用程式。 Visual Studio Code 使用者可以使用[Azure CLI](/cli/azure)命令，以將應用程式發佈至 Azure。 本文涵蓋發行 Visual Studio 中使用的工具。 若要發行應用程式使用 Azure CLI，請參閱[使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure](/azure/app-service/app-service-web-get-started-dotnet)。

中的專案上按一下滑鼠右鍵**方案總管**，然後選取**發佈**。 確認**新建**簽入**挑選發行目標**對話方塊中，然後選取**發行**。

![挑選發佈目標](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

輸入中的下列資訊**建立 App Service**對話方塊，然後選取**建立**。

| 項目 | 描述 |
| ---- | ----------- |
| **應用程式名稱** | 應用程式的唯一名稱。 |
| **訂用帳戶** | Azure 訂用帳戶，應用程式使用。 |
| **資源群組** | 應用程式所屬的相關資源的群組。  |
| **主控方案** | Web 應用程式的定價方案。 |

![建立 app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio 中完成下列工作：

* 建立發行設定檔包含發佈設定。
* 建立或使用現有*Azure Web 應用程式*提供詳細資料。
* 發行應用程式。
* 啟動瀏覽器中，使用已發佈的 web 應用程式載入。

請注意 URL 的格式，為應用程式 *{應用程式名稱}.azurewebsites.net.net*。 例如，名為應用程式`SignalRChattR`具有一個 URL 看起來像 `https://signalrchattr.azurewebsites.net`。

如果 HTTP 502.2 錯誤發生，請參閱[至 Azure App Service 部署 ASP.NET Core 預覽版本](xref:host-and-deploy/azure-apps/index)加以解決。

## <a name="configure-signalr-web-app"></a>SignalR web 應用程式設定

ASP.NET Core SignalR 應用程式的已發行的 Azure Web 應用程式就必須擁有[ARR 同質性](https://en.wikipedia.org/wiki/Application_Request_Routing)啟用。 [Websocket](xref:fundamentals/websockets)應該啟用，以允許函式的 Websocket 傳輸。

在 Azure 入口網站中，瀏覽至**應用程式設定**web 應用程式。 設定**WebSockets**要**上**，並確認**ARR 同質性**是**上**。

![在 Azure 入口網站中的 azure Web 應用程式設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 Websocket 和其他傳輸[會限制為基礎的 App Service 方案](/azure/azure-subscription-service-limits#app-service-limits)。

## <a name="related-resources"></a>相關資源

* [使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure](/azure/app-service/app-service-web-get-started-dotnet)
* [將 ASP.NET Core 應用程式發佈至 Azure 與 Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [裝載和部署在 Azure 上的 ASP.NET Core 預覽應用程式](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
