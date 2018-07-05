---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 第 1 部分： 概觀與建立專案 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0540f3142d73fef616e30544bb1130b75c0bb436
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816301"
---
<a name="part-1-overview-and-creating-the-project"></a>第 1 部分： 概觀與建立專案
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework 是物件/關聯式對應架構。 它會在您的程式碼中的網域物件對應到關聯式資料庫中的實體。 大部分的情況下，您不必擔心資料庫層級，因為 Entity Framework 會為您處理它。 您的程式碼會操作物件，並變更已保存至資料庫。

## <a name="about-the-tutorial"></a>針對本教學課程

在本教學課程中，您將建立一個簡單的市集應用程式。 有兩個應用程式的主要部分。 一般使用者可以檢視產品，並建立訂單：

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

系統管理員可以建立、 刪除或編輯產品：

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>您將學習到的技能

以下是您將學到什麼：

- 如何使用 Entity Framework 搭配 ASP.NET Web API。
- 如何使用 knockout.js 建立動態的用戶端 UI。
- 如何使用 Web API 中的表單驗證，來驗證使用者。

雖然本教學課程都各自獨立的您可能想要先閱讀下列教學課程：

- [第一個 ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [建立 Web API 支援 CRUD 作業](../creating-a-web-api-that-supports-crud-operations.md)

一些知識[ASP.NET MVC](../../../../mvc/index.md)也很有用。

## <a name="overview"></a>總覽

概括而言，以下是應用程式的架構：

- ASP.NET MVC 會為用戶端產生的 HTML 頁面。
- ASP.NET Web API 會公開資料 （「 產品 」 和 「 訂單 」） 上的 CRUD 作業。
- Entity Framework 會轉譯成資料庫實體的 Web API 所使用的 C# 模型。

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

下圖顯示如何在應用程式的各種層級表示網域物件： 資料庫層級、 物件模型中，和最後電傳格式，這會將資料以用戶端透過 HTTP 傳輸。

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>建立 Visual Studio 專案

您可以建立使用 Visual Web Developer Express 或 Visual Studio 的完整版本的教學課程專案。

從**開始**頁面上，按一下**新的專案**。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名為"ProductStore 」，然後按一下**確定**。

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**，按一下 **確定**。

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

「 網際網路應用程式 」 範本會建立支援表單驗證的 ASP.NET MVC 應用程式。 如果您執行應用程式現在，它已經具有一些功能：

- 新的使用者可以按一下右上角的 [註冊] 連結進行註冊。
- 已註冊的使用者可以登入，按一下 「 登入 」 的連結。

成員資格資訊會保存在資料庫中自動建立。 如需在 ASP.NET MVC 中的表單驗證的詳細資訊，請參閱[逐步解說： 在 ASP.NET MVC 中使用表單驗證](https://msdn.microsoft.com/library/ff398049(VS.98).aspx)。

## <a name="update-the-css-file"></a>更新 CSS 檔案

是表面，在此步驟中，但它會讓轉譯先前的螢幕擷取畫面如下的頁面。

在方案總管 中，展開 Content 資料夾，然後開啟名為 Site.css 檔案。 新增下列 CSS 樣式：

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [下一步](using-web-api-with-entity-framework-part-2.md)
