---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: EF Database First 與 ASP.NET MVC： 自訂檢視 |Microsoft Docs
author: tfitzmac
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: ce450af93459f2a69557b3fe0d1ead813ae99986
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825785"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>EF Database First 與 ASP.NET MVC： 自訂檢視
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。
> 
> 系列的這個部分會著重於變更自動產生的檢視，以增強其呈現方式。


## <a name="add-enrolled-courses-to-student-details"></a>將已註冊的課程新增至學生詳細資料

產生的程式碼應用程式提供很好的起點，但它不一定是提供的所有功能，您需要在您的應用程式中。 您可以自訂程式碼，以符合您的應用程式的特定需求。 目前，您的應用程式不會顯示所選取學生的已註冊的課程。 在本節中，您將新增的已註冊的課程針對至每個學生**詳細資料**學生的檢視。

開啟**Students/Details.cshtml**，和最後一個以下&lt;/dl&gt;索引標籤上，但在關閉前&lt;/div&gt;標記中加入下列程式碼。

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

此程式碼會建立 Enrollment 資料表中的每一筆記錄的資料列顯示所選取學生的資料表。 **顯示**方法將 HTML 呈現物件 (modelItem) 表示的運算式。 使用 Display 方法 （而非直接內嵌在程式碼中的屬性值） 以確定此值的格式正確根據其型別和該類型的範本。 在此範例中，每個運算式會從目前的記錄，在迴圈中，來傳回單一屬性且值為基本型別，其呈現為文字。

再次瀏覽至 / Students 索引 檢視，然後選取**詳細資料**學生的其中一個。 您會看到已註冊的課程已包含在檢視中。

![註冊的學生](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [上一頁](changing-the-database.md)
> [下一頁](enhancing-data-validation.md)
