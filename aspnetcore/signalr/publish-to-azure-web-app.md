---
title: 發佈 ASP.NET Core SignalR 應用程式至 Azure App Service
author: bradygaster
description: 了解如何將 ASP.NET Core SignalR 應用程式發行至 Azure App Service。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406105"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>發佈 ASP.NET Core SignalR 應用程式至 Azure App Service

藉由[Brady Gaster](https://twitter.com/bradygaster)

[Azure App Service](/azure/app-service/app-service-web-overview)已[Microsoft 雲端運算](https://azure.microsoft.com/)裝載 web 應用程式，包括 ASP.NET Core 的平台服務。

> [!NOTE]
> 這篇文章是指從 Visual Studio 發佈的 ASP.NET Core SignalR 應用程式。 如需詳細資訊，請參閱 <<c0> [ 適用於 Azure SignalR 服務](https://azure.microsoft.com/services/signalr-service)。

## <a name="publish-the-app"></a>發行應用程式

本文涵蓋發行 Visual Studio 中使用的工具。 Visual Studio Code 使用者可以使用[Azure CLI](/cli/azure)命令，以將應用程式發佈至 Azure。 如需詳細資訊，請參閱 <<c0> [ 使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure](/azure/app-service/app-service-web-get-started-dotnet)。

1. 在 [方案總管]  中，以滑鼠右鍵按一下專案，然後選取 [發佈]  。

1. 確認**App Service**並**新建**中所選**挑選發行目標**對話方塊。

1. 選取 **建立設定檔**從**發佈**向下按鈕拖放。

   輸入下表中所述的資訊**建立 App Service**對話方塊，然後選取**建立**。

   | 項目               | 描述 |
   | ------------------ | ----------- |
   | **名稱**           | 應用程式的唯一名稱。 |
   | **訂用帳戶**   | Azure 訂用帳戶，應用程式使用。 |
   | **資源群組** | 應用程式所屬的相關資源的群組。 |
   | **主控方案**   | Web 應用程式的定價方案。 |

1. 選取  **Azure SignalR 服務**中**相依性** > **新增**下拉式清單：

   ![在 [加入] 下拉式清單中顯示選取的 Azure SignalR 服務的相依性區域](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. 在  **Azure SignalR 服務**對話方塊中，選取**建立新的 Azure SignalR 服務執行個體**。

1. 提供**名稱**，**資源群組**，並**位置**。 返回**Azure SignalR 服務**對話方塊，然後選取**新增**。

Visual Studio 中完成下列工作：

* 建立發行設定檔包含發佈設定。
* 會建立*Azure Web 應用程式*提供詳細資料。
* 發行應用程式。
* 會啟動瀏覽器中，它會載入 web 應用程式。

應用程式的 URL 的格式是`{APP SERVICE NAME}.azurewebsites.net`。 例如，名為應用程式`SignalRChatApp`具有一個 URL 的`https://signalrchatapp.azurewebsites.net`。

如果 HTTP *502.2-閘道不正確*預覽.NET Core 版本為目標的應用程式部署時，就會發生錯誤，請參閱[至 Azure App Service 部署 ASP.NET Core 預覽版本](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)加以解決。

## <a name="configure-the-app-in-azure-app-service"></a>在 Azure App Service 中設定應用程式

> [!NOTE]
> *本節僅適用於未使用 Azure SignalR 服務應用程式。*
>
> 如果應用程式使用 Azure SignalR 服務，App Service 不需要應用程式要求路由 (ARR) 親和性與這一節所述的 Web 通訊端的組態。 Azure SignalR 服務，不是直接以應用程式，用戶端連線其 Web 通訊端。

託管的應用程式而不需 Azure SignalR 服務，啟用：

* [ARR 同質性](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html)回到相同的 App Service 執行個體的路由要求的使用者。 預設值是**上**。
* [Web 通訊端](xref:fundamentals/websockets)以允許 Web 通訊端傳輸至函式。 預設值是**關閉**。

1. 在 Azure 入口網站中，瀏覽至 web 應用程式中**應用程式服務**。
1. 開啟**組態** > **一般設定**。
1. 設定**Web 通訊端**要**上**。
1. 確認**ARR 同質性**設為**上**。

## <a name="app-service-plan-limits"></a>App Service 方案限制

Web 通訊端和其他傳輸數目限制為基礎的 App Service 方案選取。 如需詳細資訊，請參閱 < *Azure 雲端服務限制*並*App Service 限制*區段[Azure 訂用帳戶和服務限制、 配額和條件約束](/azure/azure-subscription-service-limits#app-service-limits)發行項。

## <a name="additional-resources"></a>其他資源

* [什麼是 Azure SignalR 服務？](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure](/azure/app-service/app-service-web-get-started-dotnet)
* [裝載和部署在 Azure 上的 ASP.NET Core 預覽應用程式](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
