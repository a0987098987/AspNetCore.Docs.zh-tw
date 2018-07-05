---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: 在 Visual Studio 2013 的 ASP.NET Scaffold |Microsoft Docs
author: tfitzmac
description: ASP.NET Scaffold 是隨附於 Visual Studio 2013 中的新功能。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: f014ef1439376349f49f10f1f4063945ab81e7c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369088"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>在 Visual Studio 2013 的 ASP.NET Scaffold
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Scaffold 是隨附於 Visual Studio 2013 中的新功能。


## <a name="overview"></a>總覽

ASP.NET Scaffold 是 ASP.NET Web 應用程式的程式碼產生架構。 Visual Studio 2013 包含預先安裝的程式碼產生器，適用於 MVC 和 Web API 專案。 當您想要快速新增資料模型進行互動的程式碼時，您可以新增至您專案的 scaffolding。 使用 scaffolding 可減少開發您的專案中的標準資料作業的時間量。

根據預設，Visual Studio 2013 不支援的 Web Form 專案，產生的程式碼，但您可以藉由將 MVC 相依性新增至專案，或安裝延伸模組 Web form 使用 scaffolding。 這兩種方法如下所示。

Visual Studio 2013 Update 2 (目前 RC) 提供功能來擴充 ASP.NET Scaffolding，以符合您案例的需求。 透過這項功能，您可以建立自訂的 scaffolding 範本，並將它新增至加入新的 Scaffold 對話方塊。 在自訂的範本中，您可以指定加入 scaffold 項目時，會產生的程式碼。 如需詳細資訊，請參閱 <<c0> [ 適用於 Visual Studio 中建立自訂 Scaffolder](https://go.microsoft.com/fwlink/p/?LinkId=395029)。

## <a name="prerequisites"></a>必要條件

若要使用的 ASP.NET Scaffold，您必須具備：

- Microsoft Visual Studio 2013
- Web 開發人員工具 （預設 Visual Studio 2013 安裝的一部分）
- ASP.NET Web 架構與工具的 2013 （預設 Visual Studio 2013 安裝的一部分）

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>將 scaffold 項目新增至 MVC 或 Web API

若要加入 scaffold，以滑鼠右鍵按一下專案或資料夾，以在專案中，然後選取**新增**–**新增 Scaffold 項目**，如下圖所示。

![加入 scaffold 項目](aspnet-scaffolding-overview/_static/image1.png)

從**新增 Scaffold** ] 視窗中，選取 [新增 scaffold 的型別。

![選取的 scaffold 的類型](aspnet-scaffolding-overview/_static/image2.png)

**新增控制器**視窗可讓您選取選項來產生控制器，包括您是否想要使用 Entity Framework 6 的新非同步功能。

![新增控制器](aspnet-scaffolding-overview/_static/image3.png)

頁面與相關的類別會針對您的案例。 例如下, 圖顯示的 MVC 控制器和透過名為電影模型類別的 scaffolding 所建立的檢視。

![建立的檔案](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>加入 Web Form 中的 scaffold 項目

產生 Web Form 的程式碼的樣板加入，您必須安裝 Visual studio 擴充功能，或新增 MVC 相依性。 這兩種方法如下所示，但您只需要執行其中一種方法。

### <a name="web-forms-scaffolding-extension"></a>Web Forms Scaffolding 擴充功能

您可以安裝 Visual Studio 擴充功能可讓您使用 Web Form 專案中使用 scaffolding。 在 Visual Studio 中，選取**工具**，然後**擴充功能和更新**。 在此對話方塊中搜尋 Visual Studio 元件庫**Web Forms Scaffolding**。

![安裝 web forms scaffolding](aspnet-scaffolding-overview/_static/image5.png)

如需詳細資訊，請參閱 < [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478)。

### <a name="mvc-dependencies"></a>MVC 相依性

若要新增 MVC 相依性，請選取**新增** - **新增 Scaffold 項目**。 在 [新增 Scaffold] 視窗中，選取**MVC 相依性**，如下所示。

![新增 MVC 相依性](aspnet-scaffolding-overview/_static/image6.png)

有兩個選項，scaffolding MVC;最少且完整。 如果您選取最小，只有 NuGet 套件和 ASP.NET mvc 的參考會加入到專案中。 如果您選取 [完整] 選項中，加入基本的相依性，以及 MVC 專案所需的內容檔案。 若要輕鬆地使用 scaffolding，選取 完整相依性。

![選取 完整相依性](aspnet-scaffolding-overview/_static/image7.png)

新增相依性之後, 您會看到**readme.txt**檔案。 請仔細依照此檔案中的指示，以確保您的專案可以正確運作。

當您完成 readme.txt 檔案中的步驟時，您可以加入新的 scaffold 項目，有關 MVC 和 Web API 上一節中所示。 自動產生的檢視和控制器，則會在專案中正確運作。

## <a name="tutorials"></a>教學課程

若要建立自訂的 scaffolder，請參閱[適用於 Visual Studio 中建立自訂 Scaffolder](https://go.microsoft.com/fwlink/p/?LinkId=395029)。

若要自訂產生的檔案，請參閱[如何自訂產生的檔案，從 [新增 Scaffold 項目] 對話方塊](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)。

如需範例使用 scaffolding **Database First 開發**，請參閱[EF Database First 與 ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md)。

如需範例的使用中的 scaffolding **MVC**專案，請參閱[Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。

如需範例的使用中的 scaffolding **Web API**專案，請參閱[建立 REST API 與 Web API 2 中的屬性路由](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md)。
