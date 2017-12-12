---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: "加入模型 (VB) |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a>加入模型 (VB)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 開始之前，請確定您已安裝下面所列的必要條件。 您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。 [下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 C#，切換至[C# 版本](../cs/adding-a-model.md)本教學課程。


## <a name="adding-a-model"></a>加入模型

本節中您要加入一些類別，來管理資料庫中的影片。 這些類別會在 ASP.NET MVC 應用程式的 「 模型 」 一部分。

若要定義及使用這些模型類別，您將使用稱為 Entity Framework 的.NET Framework 資料存取技術。 呼叫開發架構 （通常稱為 EF） 的 Entity Framework 支援*Code First*。 程式碼第一次可讓您撰寫簡單的類別來建立模型物件。 （也稱為是 POCO 類別，從 「 純舊 CLR 物件 」。）然後您可以立即從您的類別，可讓非常全新且更快速的開發工作流程所建立的資料庫。

## <a name="adding-model-classes"></a>加入模型類別

在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。

![](adding-a-model/_static/image1.png)

「 電影 」 的類別命名。

加入下列五個屬性，以`Movie`類別：

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

我們將使用`Movie`類別來代表在資料庫中的影片。 每個執行個體`Movie`物件將會對應到資料庫資料表，以及每個內容中的資料列`Movie`類別會對應到資料表中的資料行。

在同一個檔案中，加入下列`MovieDBContext`類別：

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

`MovieDBContext`類別代表 Entity Framework 影片的資料庫內容，用來處理擷取、 儲存及更新`Movie`類別執行個體在資料庫中的。 `MovieDBContext`衍生自`DbContext`基底類別由 Entity Framework 所提供。 如需有關`DbContext`和`DbSet`，請參閱[Entity Framework 的產能改善功能](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)。

若要能夠參考`DbContext`和`DbSet`，您需要新增下列`imports`在檔案最上方的陳述式：

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

完整*Movie.vb*檔案如下所示。

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>建立的連接字串和使用 SQL Server Compact

`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。 您可以詢問一個問題，是如何指定將會連線到哪一個資料庫。 您將會執行，藉由新增中的連接資訊*Web.config*應用程式檔案。

開啟應用程式根目錄*Web.config*檔案。 (不*Web.config*檔案*檢視*資料夾。)下圖顯示兩者*Web.config*檔案; 開啟*Web.config*以紅色圈出的檔案。

![](adding-a-model/_static/image2.png)

## 

加入下列連接字串至`<connectionStrings>`中的項目*Web.config*檔案。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

下列範例顯示的某一部分*Web.config*檔案與新加入的連接字串：

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

少量的程式碼和 XML 是您需要撰寫以代表及電影資料儲存在資料庫中的所有項目。

接下來，您將建置新`MoviesController`類別可用來顯示電影，並允許使用者建立新的電影清單。

>[!div class="step-by-step"]
[上一頁](adding-a-view.md)
[下一頁](accessing-your-models-data-from-a-controller.md)
