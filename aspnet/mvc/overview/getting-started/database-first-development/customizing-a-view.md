---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: "第一個使用 ASP.NET MVC 的 EF 資料庫： 自訂檢視 |Microsoft 文件"
author: tfitzmac
description: "使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: af9609396cff18b08824732731ddb9c5cca578fa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>第一個使用 ASP.NET MVC 的 EF 資料庫： 自訂檢視
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。
> 
> 變更自動產生的檢視，以增強簡報著重於數列的這個部分。


## <a name="add-enrolled-courses-to-student-details"></a>新增已註冊的課程學生詳細資料

產生的程式碼應用程式提供很好的起點，但它不一定會提供所有您需要在您的應用程式中的功能。 您可以自訂程式碼以符合您的應用程式的特定需求。 目前，您的應用程式不會顯示所選的學生已註冊的課程。 在本節中，您會將已註冊的課程的每個學生**詳細資料**學生的檢視。

開啟**Students/Details.cshtml**，和最後一個之下&lt;/dl&gt;索引標籤上，但在關閉前&lt;/div&gt;標記中加入下列程式碼。

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

這段程式碼會建立顯示所選的學生在 Enrollment 資料表中每一筆記錄的資料列的資料表。 **顯示**方法將 HTML 呈現物件 (modelItem)，表示運算式。 使用顯示方法 （而非只內嵌程式碼中的屬性值） 以確定此值的格式正確會根據其類型和該類型的範本。 在此範例中，每個運算式會傳回單一的屬性與目前的記錄，在迴圈中，且值為基本型別也會轉譯為文字。

再次瀏覽至學生/索引檢視，並選取**詳細資料**學生的其中一個。 您會看到已註冊的課程已包含在檢視中。

![學生註冊](customizing-a-view/_static/image1.png)

>[!div class="step-by-step"]
[上一頁](changing-the-database.md)
[下一頁](enhancing-data-validation.md)
