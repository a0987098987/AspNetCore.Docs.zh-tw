---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: "將模型加入 |Microsoft 文件"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 13aab58e86829a8d4accd1d304420dcb34ffa472
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2017
---
<a name="adding-a-model"></a>加入模型
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

本節中您要加入一些類別，來管理資料庫中的影片。 這些類別會&quot;模型&quot;ASP.NET MVC 應用程式的一部分。

您將使用的.NET Framework 資料存取技術稱為[Entity Framework](https://docs.microsoft.com/ef/)來定義及使用這些模型類別。 呼叫開發架構 （通常稱為 EF） 的 Entity Framework 支援*Code First*。 程式碼第一次可讓您撰寫簡單的類別來建立模型物件。 (這些是也稱為 POCO 類別，從&quot;純舊 CLR 物件。&quot;)然後您可以立即從您的類別，可讓非常全新且更快速的開發工作流程所建立的資料庫。 如果您需要先建立資料庫時，您仍然可以遵循本教學課程來了解 MVC 和 EF 應用程式開發。 您可以遵循 Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview)教學課程中，其中涵蓋了資料庫的第一種方法。

## <a name="adding-model-classes"></a>加入模型類別

在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。

![](adding-a-model/_static/image1.png)

輸入*類別*名稱&quot;影片&quot;。

加入下列五個屬性，以`Movie`類別：

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

我們將使用`Movie`類別來代表在資料庫中的影片。 每個執行個體`Movie`物件將會對應到資料庫資料表，以及每個內容中的資料列`Movie`類別會對應到資料表中的資料行。

注意： 若要使用 System.Data.Entity 和相關的類別，您必須安裝[Entity Framework 的 NuGet 套件](https://www.nuget.org/packages/EntityFramework/)。 請以取得進一步指示的連結。

在同一個檔案中，加入下列`MovieDBContext`類別：

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext`類別代表 Entity Framework 影片的資料庫內容，用來處理擷取、 儲存及更新`Movie`類別執行個體在資料庫中的。 `MovieDBContext`衍生自`DbContext`基底類別由 Entity Framework 所提供。

若要能夠參考`DbContext`和`DbSet`，您需要新增下列`using`在檔案最上方的陳述式：

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

您可以透過手動方式新增 using 陳述式，或您可以將滑鼠停留在紅色曲線，請按一下`Show potential fixes`按一下`using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

注意： 幾個未使用`using`陳述式已被移除。 Visual Studio 會顯示為灰色的未使用的相依性。 您可以移除 unnused 相依性，將滑鼠游標停留於灰色的相依性，請按一下`Show potential fixes`按一下**移除未使用的 using。**

![](adding-a-model/_static/image3.png)

最後，我們已加入模型 (MVC 中 M)。 下一節將使用的資料庫連接字串。

>[!div class="step-by-step"]
[上一頁](adding-a-view.md)
[下一頁](creating-a-connection-string.md)
