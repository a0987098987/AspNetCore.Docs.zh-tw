---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: 將模型新增 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7e732da3b056980c87660f6b80366438b5823c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823674"
---
<a name="adding-a-model"></a>新增模型
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。


在本節中，您將新增一些類別來管理資料庫中的電影。 這些類別是&quot;模型&quot;ASP.NET MVC 應用程式的一部分。

您將使用的.NET Framework 資料存取技術，稱為[Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx)來定義和使用這些模型類別。 Entity Framework （通常稱為 EF） 支援，開發架構稱為*Code First*。 程式碼第一次可讓您撰寫簡單的類別來建立模型物件。 (這些是也稱為 POCO 類別，來自&quot;純舊 CLR 物件。&quot;)然後您可以在即時從您的類別，可讓非常精簡且快速的開發工作流程所建立的資料庫。

## <a name="adding-model-classes"></a>新增模型類別

在 [**方案總管] 中**，以滑鼠右鍵按一下 *模型* 資料夾中，選取**新增**，然後選取**類別**。

![](adding-a-model/_static/image1.png)

請輸入*類別*名稱&quot;電影&quot;。

加入下列五個屬性，以`Movie`類別：

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

我們將使用`Movie`類別來代表資料庫中的電影。 每個執行個體`Movie`物件會對應至資料庫資料表，以及每個屬性中的資料列`Movie`類別會對應至資料表的資料行。

在相同的檔案中，新增下列`MovieDBContext`類別：

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext`類別代表會處理擷取、 儲存及更新的 Entity Framework 電影資料庫內容`Movie`類別執行個體在資料庫中的。 `MovieDBContext`衍生自`DbContext`基底 Entity Framework 所提供的類別。

為了能夠參考`DbContext`並`DbSet`，您需要新增下列`using`陳述式在檔案頂端：

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

完整*Movie.cs*檔案如下所示。 (數個 using 陳述式不需要已移除。)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>建立連接字串和使用 SQL Server LocalDB

`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。 您可能會問的一個問題，是如何指定要連線到哪一個資料庫。 您將的做法是將連接資訊*Web.config*應用程式檔案。

開啟應用程式根目錄*Web.config*檔案。 (沒有*Web.config*中的檔案*檢視*資料夾。)開啟*Web.config*紅色外框的檔案。

![](adding-a-model/_static/image2.png)

加入下列連接字串`<connectionStrings>`中的項目*Web.config*檔案。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

下列範例示範的一部分*Web.config*檔案以加入新的連接字串：

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

此少量的程式碼和 XML 是您要撰寫以代表，並將電影資料儲存在資料庫中的所有項目。

接下來，您將建立新`MoviesController`類別可用來顯示電影資料，並允許使用者建立新的電影清單。

> [!div class="step-by-step"]
> [上一頁](adding-a-view.md)
> [下一頁](accessing-your-models-data-from-a-controller.md)
