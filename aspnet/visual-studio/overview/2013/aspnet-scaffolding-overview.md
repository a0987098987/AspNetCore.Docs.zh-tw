---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: "Visual Studio 2013 中的 ASP.NET Scaffolding |Microsoft 文件"
author: tfitzmac
description: "ASP.NET Scaffolding 是隨附於 Visual Studio 2013 的新功能。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Visual Studio 2013 中的 ASP.NET Scaffolding
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Scaffolding 是隨附於 Visual Studio 2013 的新功能。


## <a name="overview"></a>概觀

ASP.NET Scaffolding 是 ASP.NET Web 應用程式的程式碼產生架構。 Visual Studio 2013 包含預先安裝的程式碼產生器適用於 MVC 和 Web API 專案。 當您想要快速加入資料模型互動的程式碼時，您可以加入至您的專案的 scaffolding。 使用 scaffolding 可減少開發您的專案中的標準資料作業的時間量。

根據預設，Visual Studio 2013 不支援的 Web Form 專案，產生程式碼，但您可以使用 scaffolding Web 表單將 MVC 相依性加入專案，或安裝擴充功能。 這兩種方法如下所示。

Visual Studio 2013 Update 2 (目前 RC) 提供功能延長 ASP.NET Scaffolding，以符合您案例的需求。 透過這項功能，您可以建立自訂的 scaffolding 範本，並將它加入至加入新的 Scaffold 對話方塊。 在自訂的範本，您可以指定可以加入 scaffold 項目時，會產生的程式碼。 如需詳細資訊，請參閱[for Visual Studio 中建立自訂 Scaffolder](https://go.microsoft.com/fwlink/p/?LinkId=395029)。

## <a name="prerequisites"></a>必要條件

若要使用 ASP.NET Scaffolding，您必須：

- Microsoft Visual Studio 2013
- Web 開發人員工具 （預設 Visual Studio 2013 安裝的一部分）
- ASP.NET 網頁架構和工具 2013 （預設 Visual Studio 2013 安裝的一部分）

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>將 scaffold 項目加入至 MVC 或 Web API

若要加入 scaffold，以滑鼠右鍵按一下專案或專案內的資料夾，然後選取**新增**–**新的 Scaffold 項目**，如下列影像所示。

![加入 scaffold 項目](aspnet-scaffolding-overview/_static/image1.png)

從**新增 Scaffold**視窗中，選取要加入 scaffold 的類型。

![選取 scaffold 類型](aspnet-scaffolding-overview/_static/image2.png)

**加入控制器**視窗讓您選取選項來產生控制器，包括您是否想要使用新的非同步功能，從 Entity Framework 6 的機會。

![加入控制器](aspnet-scaffolding-overview/_static/image3.png)

相關類別和頁面會建立您的案例。 例如下, 圖顯示的 MVC 控制器和透過模型類別，名為影片的 scaffolding 所建立的檢視。

![建立的檔案](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Web Form 中加入 scaffold 項目

Web Form 程式碼會產生的樣板加入，您必須安裝 Visual studio 擴充功能，或將 MVC 相依性。 這兩種方法如下所示，但您只需要執行其中一種方法。

### <a name="web-forms-scaffolding-extension"></a>Web Form 的 Scaffolding 擴充功能

您可以安裝 Visual Studio 擴充功能可讓您使用 Web Form 專案中使用 scaffolding。 在 Visual Studio 中，選取**工具**然後**擴充功能和更新**。 在此對話方塊中搜尋 Visual Studio 組件庫的**Web Form Scaffolding**。

![安裝 web form 的 scaffolding](aspnet-scaffolding-overview/_static/image5.png)

如需詳細資訊，請參閱[Web Form Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478)。

### <a name="mvc-dependencies"></a>MVC 相依性

若要將 MVC 相依性，請選取**新增** - **新的 Scaffold 項目**。 在 [新增 Scaffold] 視窗中，選取**MVC 相依性**，如下所示。

![將 MVC 相依性](aspnet-scaffolding-overview/_static/image6.png)

有兩個選項的 scaffolding MVC;最少且完整。 如果您選取最少，只有 NuGet 封裝和 ASP.NET MVC 的參考會加入至您的專案。 如果您選取 [完整] 選項，加入最少的相依性，以及所需的內容檔案的 MVC 專案。 若要輕鬆地使用 scaffolding，選取 完整的相依性。

![選取完整的相依性](aspnet-scaffolding-overview/_static/image7.png)

在之後新增相依性，您會看到**readme.txt**檔案。 小心遵循此檔案中的指示，以確保您的專案可以正確運作。

當您完成的 readme.txt 檔案中的步驟時，您可以加入新的 scaffold 項目，如前一節有關 MVC 和 Web API 中所示。 自動產生的檢視和控制器，則將您的專案中正確運作。

## <a name="tutorials"></a>教學課程

若要建立自訂的 scaffolder，請參閱[for Visual Studio 中建立自訂 Scaffolder](https://go.microsoft.com/fwlink/p/?LinkId=395029)。

若要自訂產生的檔案，請參閱[如何自訂產生的檔案從新的 Scaffold 項目對話方塊](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)。

如需使用 scaffolding **Database First 開發**，請參閱[EF Database First 搭配 ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md)。

如需範例的使用中的 scaffolding **MVC**專案，請參閱[開始使用 ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。

如需範例的使用中的 scaffolding **Web API**專案，請參閱[屬由路由的 Web API 2 中建立 REST API](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md)。
