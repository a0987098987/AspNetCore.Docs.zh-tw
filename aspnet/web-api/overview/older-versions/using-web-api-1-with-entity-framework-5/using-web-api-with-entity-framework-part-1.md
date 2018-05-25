---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 第 1 部分： 概觀和建立專案 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-creating-the-project"></a>第 1 部分： 概觀和建立專案
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework 是/物件關聯式對應架構。 則會對應到關聯式資料庫中的實體的程式碼中的網域物件。 大部分的情況下，您不必擔心資料庫層級，因為 Entity Framework 會為您處理它。 您的程式碼會操作物件，並變更會保存至資料庫。

## <a name="about-the-tutorial"></a>關於教學課程

在本教學課程中，您將建立簡單的市集應用程式。 有兩個主要部分應用程式。 一般使用者可以檢視產品，並建立訂單：

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

系統管理員可以建立、 刪除或編輯產品：

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>您將學習到的技術

以下是您將學習：

- 如何使用 Entity Framework 透過 ASP.NET Web API。
- 如何使用解 knockout.js 來建立動態的用戶端 UI。
- 如何透過 Web API 中使用表單驗證來驗證使用者。

雖然本教學課程是各自獨立，但是您可能想要先閱讀下列教學課程：

- [第一個 ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [建立 Web 應用程式開發介面支援 CRUD 作業](../creating-a-web-api-that-supports-crud-operations.md)

一些知識[ASP.NET MVC](../../../../mvc/index.md)也很有用。

## <a name="overview"></a>總覽

概括而言，以下是應用程式架構：

- ASP.NET MVC 會為用戶端產生的 HTML 頁面。
- ASP.NET Web API 會公開 CRUD 作業 （「 產品 」 和 「 訂單 」） 的資料。
- Entity Framework 會將轉譯成資料庫實體的 Web API 所使用的 C# 模型。

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

下圖顯示如何將網域物件表示應用程式的各層級： 資料庫層級、 物件模型中，以及最後的 wire 格式，用來傳輸資料到用戶端透過 HTTP。

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>建立 Visual Studio 專案

您可以建立使用 Visual Web Developer Express 或完整版本的 Visual Studio 的教學課程專案。

從**啟動**頁面上，按一下**新專案**。

在**範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 在下**Visual C#**，選取**Web**。 在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名為"ProductStore 」，然後按一下**確定**。

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**按一下**確定**。

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

「 網際網路應用程式 」 範本會建立支援表單驗證的 ASP.NET MVC 應用程式。 如果您執行應用程式現在，已有一些功能：

- 新的使用者可以註冊按一下右上角的"Register"連結。
- 已註冊的使用者可以登入，請按一下 「 登入 」 的連結。

成員資格資訊會保存在資料庫中自動建立。 如需 ASP.NET MVC 中的表單驗證的詳細資訊，請參閱[逐步解說： 在 ASP.NET MVC 中使用表單驗證](https://msdn.microsoft.com/library/ff398049(VS.98).aspx)。

## <a name="update-the-css-file"></a>更新 CSS 檔案

是表面，在此步驟中，但將會呈現較早的螢幕擷取畫面類似的頁面。

在方案總管 中，展開 內容 資料夾，然後開啟名為 Site.css 檔案。 新增下列 CSS 樣式：

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [下一步](using-web-api-with-entity-framework-part-2.md)
