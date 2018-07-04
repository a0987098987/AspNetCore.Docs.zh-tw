---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: 將模型新增 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 98b6693b07e4da318a649494649d9da83fe8a7d3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365132"
---
<a name="adding-a-model"></a>新增模型
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

在本節中，您將新增一些類別來管理資料庫中的電影。 這些類別是&quot;模型&quot;ASP.NET MVC 應用程式的一部分。

您將使用的.NET Framework 資料存取技術，稱為[Entity Framework](https://docs.microsoft.com/ef/)來定義和使用這些模型類別。 Entity Framework （通常稱為 EF） 支援，開發架構稱為*Code First*。 程式碼第一次可讓您撰寫簡單的類別來建立模型物件。 (這些是也稱為 POCO 類別，來自&quot;純舊 CLR 物件。&quot;)然後您可以在即時從您的類別，可讓非常精簡且快速的開發工作流程所建立的資料庫。 如果您需要先建立資料庫時，您仍然可以依照本教學課程來了解 MVC 和 EF 應用程式開發。 您可以接著依照 Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview)教學課程中，其中涵蓋了資料庫的第一種方法。

## <a name="adding-model-classes"></a>新增模型類別

在 [**方案總管] 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。

![](adding-a-model/_static/image1.png)

請輸入*類別*名稱&quot;電影&quot;。

加入下列五個屬性，以`Movie`類別：

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

我們將使用`Movie`類別來代表資料庫中的電影。 每個執行個體`Movie`物件會對應至資料庫資料表，以及每個屬性中的資料列`Movie`類別會對應至資料表的資料行。

注意： 若要使用 System.Data.Entity 和相關的類別，您必須安裝[Entity Framework NuGet 套件](https://www.nuget.org/packages/EntityFramework/)。 請依照下列連結，取得進一步指示。

在相同的檔案中，新增下列`MovieDBContext`類別：

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext`類別代表會處理擷取、 儲存及更新的 Entity Framework 電影資料庫內容`Movie`類別執行個體在資料庫中的。 `MovieDBContext`衍生自`DbContext`基底 Entity Framework 所提供的類別。

為了能夠參考`DbContext`並`DbSet`，您需要新增下列`using`陳述式在檔案頂端：

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

您可以將手動新增 using 陳述式，或您可以暫留在紅色曲線，請按一下`Show potential fixes`按一下 `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

注意： 數個未使用`using`陳述式已移除。 Visual Studio 會顯示為灰色的未使用的相依性。 您可以藉由將滑鼠停留的灰色的相依性移除未使用的相依性，請按一下`Show potential fixes`，按一下 **移除未使用的 Using。**

![](adding-a-model/_static/image3.png)

最後，我們已新增模型 (MVC 中的 M)。 在下一節中您將使用的資料庫連接字串。

> [!div class="step-by-step"]
> [上一頁](adding-a-view.md)
> [下一頁](creating-a-connection-string.md)
