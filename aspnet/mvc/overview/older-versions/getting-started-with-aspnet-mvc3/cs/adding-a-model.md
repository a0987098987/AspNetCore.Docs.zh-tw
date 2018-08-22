---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: 新增模型 (C#) |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b68f857777b1b69ca401c29261211f40df253e7a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826436"
---
<a name="adding-a-model-c"></a>新增模型 (C#)
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。 [下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/adding-a-model.md)本教學課程。


## <a name="adding-a-model"></a>新增模型

在本節中，您將新增一些類別來管理資料庫中的電影。 這些類別是 ASP.NET MVC 應用程式的 「 模型 」 部分。

您將使用稱為 Entity Framework 的.NET Framework 資料存取技術，來定義和使用這些模型類別。 Entity Framework （通常稱為 EF） 支援，開發架構稱為*Code First*。 程式碼第一次可讓您撰寫簡單的類別來建立模型物件。 （這些也稱為是 POCO 類別，來自 「 純舊 CLR 物件 」。）然後您可以在即時從您的類別，可讓非常精簡且快速的開發工作流程所建立的資料庫。

## <a name="adding-model-classes"></a>新增模型類別

在 [**方案總管] 中**，以滑鼠右鍵按一下 *模型* 資料夾中，選取**新增**，然後選取**類別**。

![](adding-a-model/_static/image1.png)

名稱*類別*「 電影 」。

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

加入下列五個屬性，以`Movie`類別：

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

我們將使用`Movie`類別來代表資料庫中的電影。 每個執行個體`Movie`物件會對應至資料庫資料表，以及每個屬性中的資料列`Movie`類別會對應至資料表的資料行。

在相同的檔案中，新增下列`MovieDBContext`類別：

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext`類別代表會處理擷取、 儲存及更新的 Entity Framework 電影資料庫內容`Movie`類別執行個體在資料庫中的。 `MovieDBContext`衍生自`DbContext`基底 Entity Framework 所提供的類別。 如需詳細資訊`DbContext`並`DbSet`，請參閱[Entity Framework 的產能改進](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)。

為了能夠參考`DbContext`並`DbSet`，您需要新增下列`using`陳述式在檔案頂端：

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

完整*Movie.cs*檔案如下所示。

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>建立連接字串和使用 SQL Server Compact

`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。 您可能會問的一個問題，是如何指定要連線到哪一個資料庫。 您將的做法是將連接資訊*Web.config*應用程式檔案。

開啟應用程式根目錄*Web.config*檔案。 (沒有*Web.config*中的檔案*檢視*資料夾。)下圖顯示兩者*Web.config*檔案; 開啟*Web.config*以紅色圈起的檔案。

![](adding-a-model/_static/image4.png)

### 

加入下列連接字串`<connectionStrings>`中的項目*Web.config*檔案。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

下列範例示範的一部分*Web.config*檔案以加入新的連接字串：

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

此少量的程式碼和 XML 是您要撰寫以代表，並將電影資料儲存在資料庫中的所有項目。

接下來，您將建立新`MoviesController`類別可用來顯示電影資料，並允許使用者建立新的電影清單。

> [!div class="step-by-step"]
> [上一頁](adding-a-view.md)
> [下一頁](accessing-your-models-data-from-a-controller.md)
