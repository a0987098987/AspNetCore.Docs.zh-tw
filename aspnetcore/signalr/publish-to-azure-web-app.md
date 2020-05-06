---
title: 將 ASP.NET Core SignalR應用程式發佈至 Azure App Service
author: bradygaster
description: 瞭解如何將 ASP.NET Core SignalR應用程式發佈至 Azure App Service。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: a5d19c1519c69351605e8da1d8fa70bff784efd4
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777185"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>將 ASP.NET Core SignalR 應用程式發佈至 Azure App Service

依[Brady Gaster](https://twitter.com/bradygaster)

[Azure App Service](/azure/app-service/app-service-web-overview)是用來裝載 web 應用程式的[Microsoft 雲端](https://azure.microsoft.com/)運算平臺服務，包括 ASP.NET Core。

> [!NOTE]
> 本文是指從 Visual Studio 發佈 ASP.NET Core SignalR 應用程式。 如需詳細資訊，請參閱[SignalR service For Azure](https://azure.microsoft.com/services/signalr-service)。

## <a name="publish-the-app"></a>發佈應用程式

本文說明如何使用 Visual Studio 中的工具來發行。 Visual Studio Code 使用者可以使用[Azure CLI](/cli/azure)命令將應用程式發佈到 Azure。 如需詳細資訊，請參閱[使用命令列工具將 ASP.NET Core 應用程式發佈到 Azure](/azure/app-service/app-service-web-get-started-dotnet)。

1. 以滑鼠右鍵按一下**方案總管**中的專案，然後選取 [**發佈**]。

1. 確認在 [**挑選發行目標**] 對話方塊中選取了 [ **App Service** ] 和 [**建立新**的]。

1. 從 [**發佈**] 按鈕下拉式選選取 [**建立設定檔**]。

   在 [**建立 App Service** ] 對話方塊中，輸入下表所述的資訊，然後選取 [**建立**]。

   | 項目               | 描述 |
   | ------------------ | ----------- |
   | **名稱**           | 應用程式的唯一名稱。 |
   | **訂閱帳戶**   | 應用程式使用的 Azure 訂用帳戶。 |
   | **資源群組** | 應用程式所屬的相關資源群組。 |
   | **主控方案**   | Web 應用程式的定價方案。 |

1. 在 [相依性] [ ** **  > **新增**] 下拉式清單中選取 [ **Azure SignalR服務**]：

   ![[相依性] 區域顯示 [ SignalR新增] 下拉式清單中的 [Azure 服務] 的選取專案](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. 在 [ **Azure SignalR服務**] 對話方塊中，選取 [**建立SignalR新的 Azure 服務實例**]。

1. 提供 [**名稱**]、[**資源群組**] 和 [**位置**]。 返回 [ ** SignalR Azure 服務**] 對話方塊，然後選取 [**新增**]。

Visual Studio 完成下列工作：

* 建立包含發行設定的發行設定檔。
* 建立具有所提供詳細資料的*Azure Web 應用程式*。
* 發佈應用程式。
* 啟動瀏覽器，它會載入 web 應用程式。

應用程式 URL 的格式為`{APP SERVICE NAME}.azurewebsites.net`。 例如，名為`SignalRChatApp`的應用程式具有的 URL `https://signalrchatapp.azurewebsites.net`。

如果部署以預覽 .NET Core 版本為目標的應用程式時，發生 HTTP *502.2-不正確的閘道*錯誤，請參閱將[ASP.NET Core preview 版本部署至 Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)加以解決。

## <a name="configure-the-app-in-azure-app-service"></a>在 Azure App Service 中設定應用程式

> [!NOTE]
> *本節僅適用于不使用 Azure SignalR服務的應用程式。*
>
> 如果應用程式使用 Azure SignalR服務，則 App Service 不需要設定本章節所述的應用程式要求路由（ARR）親和性和 Web 通訊端。 用戶端會將其 Web 通訊端SignalR連線至 Azure 服務，而不是直接連接至應用程式。

若為不含 Azure SignalR服務的託管應用程式，請啟用：

* [ARR 親和性](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html)，可將來自使用者的要求路由回到相同的 App Service 實例。 預設設定為 [**開啟**]。
* [Web 通訊端](xref:fundamentals/websockets)，允許 Web 通訊端傳輸運作。 預設設定為 [**關閉**]。

1. 在 Azure 入口網站中，流覽至**應用程式服務**中的 web 應用程式。
1.  > 開啟**Configuration****[設定] [一般設定**]。
1. 將 [ **Web 通訊端**] 設為 [**開啟**]。
1. 確認 [ **ARR 親和性**] 已設定為 [**開啟**]。

## <a name="app-service-plan-limits"></a>App Service 計畫限制

Web 通訊端和其他傳輸會根據選取的 App Service 方案而受到限制。 如需詳細資訊，請參閱[azure 訂用帳戶和服務限制、配額和限制](/azure/azure-subscription-service-limits#app-service-limits)一文中的*azure 雲端服務限制*和*App Service 限制*一節。

## <a name="additional-resources"></a>其他資源

* [什麼是 Azure SignalR服務？](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [使用命令列工具將 ASP.NET Core 應用程式發行到 Azure](/azure/app-service/app-service-web-get-started-dotnet)
* [在 Azure 上裝載和部署 ASP.NET Core 預覽應用程式](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
