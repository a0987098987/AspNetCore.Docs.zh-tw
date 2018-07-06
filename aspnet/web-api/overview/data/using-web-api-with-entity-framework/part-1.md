---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: 使用 Entity Framework 6 的 Web API 2 |Microsoft Docs
author: MikeWasson
description: 本教學課程將教導您使用 ASP.NET Web API 建立 web 應用程式的基本概念後端。 本教學課程會使用 Entity Framework 6 的資料配置...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: bc853413e814e6ef1a44841d114853003d7328f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827229"
---
<a name="using-web-api-2-with-entity-framework-6"></a>使用 Web API 2 和 Entity Framework 6
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](https://github.com/MikeWasson/BookService)

> 本教學課程將教導您使用 ASP.NET Web API 建立 web 應用程式的基本概念後端。 本教學課程會使用資料層和 Knockout.js Entity Framework 6，用戶端 JavaScript 應用程式。 本教學課程也會示範如何將應用程式部署至 Azure App Service Web Apps。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


本教學課程會使用 ASP.NET Web API 2 與 Entity Framework 6 建立 web 應用程式管理後端資料庫。 以下是您將建立的應用程式的螢幕擷取畫面。

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

應用程式會使用單一頁面應用程式 (SPA) 設計。 「 單一頁面應用程式 」 是載入單一 HTML 頁面，並以動態方式，而不是載入新的頁面更新頁面的 web 應用程式的一般詞彙。 在初始網頁載入後，應用程式會與透過 AJAX 要求的伺服器。 AJAX 要求傳回的 JSON 資料，應用程式用來更新 UI。

AJAX 不是新的但是今天很輕鬆地建置及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。 本教學課程會使用[Knockout.js](http://knockoutjs.com/)，但是您可以使用任何的 JavaScript 用戶端架構。

以下是此應用程式主要建置組塊：

- ASP.NET MVC 建立 HTML 網頁。
- ASP.NET Web API 處理 AJAX 要求，並傳回 JSON 資料。
- Knockout.js 資料繫結的 HTML 項目至 JSON 資料。
- Entity Framework 與資料庫交談。

## <a name="see-this-app-running-on-azure"></a>請參閱在 Azure 上執行此應用程式

若要查看為即時 web 應用程式正在執行的完成的網站嗎？ 您可以部署您的 Azure 帳戶的應用程式的完整版本，只要按一下下面的按鈕。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

您需要有 Azure 帳戶才能將此解決方案部署至 Azure。 如果您還沒有帳戶，您會有下列選項：

- [免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。
- [啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。

## <a name="create-the-project"></a>建立專案

開啟 Visual Studio。 從**檔案**功能表上，選取**新增**，然後選取**專案**。 (或者按一下**新的專案**[開始] 頁面上。)

在**新的專案** 對話方塊中，按一下**Web**的左窗格中並**ASP.NET Web 應用程式**在中間窗格中。 專案 BookService 命名，然後按一下**確定**。

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

在 **新的 ASP.NET 專案**對話方塊中，選取**Web API**範本。

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

如果您想要裝載在 Azure App Service 中的專案，將保留**雲端中的主機**核取方塊。

按一下 [確定] 建立專案。

## <a name="configure-azure-settings-optional"></a>設定 Azure 設定 （選擇性）

如果您離開**雲端中的主機**核取選項，Visual Studio 會提示您登入 Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

登入 Azure 之後，Visual Studio 會提示您設定 web 應用程式。 輸入網站的名稱，選取您 Azure 訂用帳戶，然後選取地理區域。 底下**資料庫伺服器**，選取**建立新的伺服器**。 輸入系統管理員使用者名稱和密碼。

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [下一步](part-2.md)
