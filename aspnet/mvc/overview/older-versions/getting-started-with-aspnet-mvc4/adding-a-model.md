---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: 將模型加入 |Microsoft 文件
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 562a06e22aad62b6982aca3316a2dfe18a6eba2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871957"
---
<a name="adding-a-model"></a>加入模型
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程的更新的版本時使用[這裡](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 容易遵循，及示範更多的功能。


本節中您要加入一些類別，來管理資料庫中的影片。 這些類別會&quot;模型&quot;ASP.NET MVC 應用程式的一部分。

您將使用的.NET Framework 資料存取技術稱為[Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx)來定義及使用這些模型類別。 呼叫開發架構 （通常稱為 EF） 的 Entity Framework 支援*Code First*。 程式碼第一次可讓您撰寫簡單的類別來建立模型物件。 (這些是也稱為 POCO 類別，從&quot;純舊 CLR 物件。&quot;)然後您可以立即從您的類別，可讓非常全新且更快速的開發工作流程所建立的資料庫。

## <a name="adding-model-classes"></a>加入模型類別

在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。

![](adding-a-model/_static/image1.png)

輸入*類別*名稱&quot;影片&quot;。

加入下列五個屬性，以`Movie`類別：

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

我們將使用`Movie`類別來代表在資料庫中的影片。 每個執行個體`Movie`物件將會對應到資料庫資料表，以及每個內容中的資料列`Movie`類別會對應到資料表中的資料行。

在同一個檔案中，加入下列`MovieDBContext`類別：

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext`類別代表 Entity Framework 影片的資料庫內容，用來處理擷取、 儲存及更新`Movie`類別執行個體在資料庫中的。 `MovieDBContext`衍生自`DbContext`基底類別由 Entity Framework 所提供。

若要能夠參考`DbContext`和`DbSet`，您需要新增下列`using`在檔案最上方的陳述式：

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

完整*Movie.cs*檔案如下所示。 (數個使用陳述式不需要已被移除。)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>建立的連接字串和使用 SQL Server LocalDB

`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。 您可以詢問一個問題，是如何指定將會連線到哪一個資料庫。 您將會執行，藉由新增中的連接資訊*Web.config*應用程式檔案。

開啟應用程式根目錄*Web.config*檔案。 (不*Web.config*檔案*檢視*資料夾。)開啟*Web.config*檔案加上紅框。

![](adding-a-model/_static/image2.png)

加入下列連接字串至`<connectionStrings>`中的項目*Web.config*檔案。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

下列範例顯示的某一部分*Web.config*檔案與新加入的連接字串：

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

少量的程式碼和 XML 是您需要撰寫以代表及電影資料儲存在資料庫中的所有項目。

接下來，您將建置新`MoviesController`類別可用來顯示電影，並允許使用者建立新的電影清單。

> [!div class="step-by-step"]
> [上一頁](adding-a-view.md)
> [下一頁](accessing-your-models-data-from-a-controller.md)
