---
title: 發行 ASP.NET Core Azure Web 應用程式的 SignalR 應用程式
author: rachelappel
description: 發行 ASP.NET Core Azure Web 應用程式的 SignalR 應用程式
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 0d98c6b24b9695c0af0170173f13902bac5f55ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271914"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>發行 ASP.NET Core SignalR 應用程式至 Azure Web 應用程式

[Azure Web 應用程式](/azure/app-service/app-service-web-overview)是[Microsoft 雲端運算](https://azure.microsoft.com/)裝載 web 應用程式，包括 ASP.NET Core 平台服務。

> [!NOTE]
> 這篇文章是指從 Visual Studio 發行 ASP.NET Core SignalR 應用程式。 請瀏覽[Azure SignalR 服務](https://azure.microsoft.com/en-gb/services/signalr-service?)如需有關在 Azure 上使用 SignalR。

## <a name="publish-the-app"></a>發行應用程式

Visual Studio 會提供內建的工具發行至 Azure Web 應用程式。 Visual Studio 程式碼的使用者可以使用[Azure CLI](/cli/azure)發行至 Azure 的應用程式的命令。 本文涵蓋發行 Visual Studio 中使用的工具。 若要發行應用程式使用 Azure CLI，請參閱[及命令列工具，將 ASP.NET Core 應用程式發行至 Azure](xref:tutorials/publish-to-azure-webapp-using-cli)。

中的專案上按一下滑鼠右鍵**方案總管 中**選取**發行**。 確認**建立新**簽入**挑選發行目標** 對話方塊中，然後選取**發行**。

![選取發行目標](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

輸入中的下列資訊**建立 App Service**對話方塊並選取**建立**。

| 項目 | 描述 |
| ---- | ----------- |
| **應用程式名稱** | 應用程式的唯一名稱。 |
| **訂用帳戶** | 應用程式使用 Azure 訂閱。 |
| **資源群組** | 相關資源的應用程式所屬的群組。  |
| **裝載計劃** | Web 應用程式定價方案。 |

![建立應用程式服務](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio 會完成下列工作：

* 建立發行設定檔包含發行設定。
* 建立或使用現有*Azure Web 應用程式*提供詳細資料。
* 發行應用程式。
* 啟動瀏覽器中，載入已發佈的 web 應用程式。

請注意 URL 的格式，應用程式是 *{應用程式名稱}.azurewebsites.net*。 例如，應用程式命名`SignalRChattR`具有一個 URL，看起來像`https://signalrchattr.azurewebsites.net`。

如果 HTTP 502.2 錯誤發生，請參閱[部署 ASP.NET Core 預覽發行至 Azure App Service](xref:host-and-deploy/azure-apps/index)加以解決。

## <a name="configure-signalr-web-app"></a>設定 SignalR web 應用程式

ASP.NET Core SignalR 應用程式必須具有 Azure Web 應用程式發行[ARR 親和性](https://en.wikipedia.org/wiki/Application_Request_Routing)啟用。 [WebSockets](xref:fundamentals/websockets)必須啟用，以允許 WebSockets 傳輸函式。

在 Azure 入口網站中，瀏覽至**應用程式設定**web 應用程式。 設定**WebSockets**至**上**，並確認**ARR 親和性**是**上**。

![在 Azure 入口網站的 azure Web 應用程式設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets 和其他傳輸[受到為基礎的 App Service 方案](/azure/azure-subscription-service-limits#app-service-limits)。

## <a name="related-resources"></a>相關資源

* [將 ASP.NET Core 應用程式發行至 Azure 中，使用命令列工具](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [將 ASP.NET Core 應用程式發行至 Azure，使用 Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [主機和部署在 Azure 上的 ASP.NET Core 預覽應用程式](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
