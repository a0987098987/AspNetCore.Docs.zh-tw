---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: "使用 Entity Framework 6 與 Web API 2 |Microsoft 文件"
author: MikeWasson
description: "本教學課程將告訴您建立以 ASP.NET Web API 的 web 應用程式的基本概念後端。 教學課程會使用 Entity Framework 6 資料配置..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 42139f8c158dd84cfc30f23c013343348b0c008a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-web-api-2-with-entity-framework-6"></a>使用 Entity Framework 6 與 Web API 2
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](https://github.com/MikeWasson/BookService)

> 本教學課程將告訴您建立以 ASP.NET Web API 的 web 應用程式的基本概念後端。 教學課程會使用資料層和解 Knockout.js Entity Framework 6，用戶端 JavaScript 應用程式。 教學課程也會示範如何將應用程式部署至 Azure App Service Web 應用程式。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [解 Knockout.js](http://knockoutjs.com/) 3.1


本教學課程會使用 Entity Framework 6 與 ASP.NET Web API 2，若要建立 web 應用程式來管理後端資料庫。 以下是您將建立的應用程式的螢幕擷取畫面。

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

應用程式會使用單一頁面應用程式 (SPA) 設計。 「 單一頁面應用程式 」 是載入單一 HTML 頁面，並以動態方式，而不必載入新的頁面更新頁面的 web 應用程式的一般詞彙。 之後的初始頁面負載，應用程式會示範透過 AJAX 要求的伺服器。 AJAX 要求傳回的 JSON 資料，才能更新 UI 的應用程式使用。

AJAX 不是新的但現在有更輕鬆地建立及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。 本教學課程使用[解 Knockout.js](http://knockoutjs.com/)，但是您可以使用任何廠商的 JavaScript 用戶端架構。

以下是主要的建置組塊，此應用程式：

- ASP.NET MVC 建立 HTML 網頁。
- ASP.NET Web API 處理 AJAX 要求，並傳回 JSON 資料。
- 解 Knockout.js 資料繫結的 HTML 項目為 JSON 資料。
- Entity Framework 交談的資料庫。

## <a name="see-this-app-running-on-azure"></a>請參閱 < 在 Azure 上執行此應用程式

若要查看即時 web 應用程式執行完成站台嗎？ 您可以部署到 Azure 帳戶的完整版本的應用程式，只要按一下下列按鈕。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

您需要 Azure 帳戶，以將此方案部署到 Azure。 如果您沒有帳戶，您會有下列選項：

- [開啟免費的 Azure 帳戶](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度您可以使用試用付費型 Azure 服務，而且即使他們用於之後可以使帳戶保持最多並使用免費的 Azure 服務。
- [啟用 MSDN 訂閱者權益](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶可讓您信用額度付費型 Azure 服務，您可以使用每個月。

## <a name="create-the-project"></a>建立專案

開啟 Visual Studio。 從**檔案**功能表上，選取**新增**，然後選取**專案**。 (或按一下**新專案**[開始] 頁面上。)

在**新專案**] 對話方塊中，按一下 [ **Web**的左窗格中和**ASP.NET Web 應用程式**中間窗格內。 BookService 為專案名稱，然後按一下**確定**。

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

在**新增 ASP.NET 專案**對話方塊中，選取**Web API**範本。

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

如果您想要裝載在 Azure App Service 中的專案，將保留**雲端中的主機**核取方塊。

按一下**確定**建立專案。

## <a name="configure-azure-settings-optional"></a>設定 Azure （選擇性）

如果您保留**雲端中的主機**核取選項，Visual Studio 會提示您登入 Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

登入 Azure 之後，Visual Studio 會提示您設定 web 應用程式。 輸入網站的名稱，選取您的 Azure 訂閱，然後選取地理區域。 在下**資料庫伺服器**，選取**建立新的伺服器**。 輸入系統管理員使用者名稱和密碼。

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

>[!div class="step-by-step"]
[下一步](part-2.md)
