---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 教學課程：自訂檢視的 EF Database First 與 ASP.NET MVC 應用程式
description: 本教學課程著重於變更自動產生的檢視，以增強其呈現方式。
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236493"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>教學課程：自訂檢視的 EF Database First 與 ASP.NET MVC 應用程式

您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。

本教學課程著重於變更自動產生的檢視，以增強其呈現方式。

在本教學課程中，您已：

> [!div class="checklist"]
> * 將課程新增至學生詳細資料頁面
> * 確認課程會新增至頁面

## <a name="prerequisites"></a>必要條件

* [變更資料庫](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>將課程新增至學生詳細資料

產生的程式碼應用程式提供很好的起點，但它不一定是提供的所有功能，您需要在您的應用程式中。 您可以自訂程式碼，以符合您的應用程式的特定需求。 目前，您的應用程式不會顯示所選取學生的已註冊的課程。 在本節中，您將新增的已註冊的課程針對至每個學生**詳細資料**學生的檢視。

開啟**檢視** > **學生** > *Details.cshtml*。 最後一個以下&lt;/dl&gt;標記，但在關閉前&lt;/div&gt;標記中加入下列程式碼。

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

此程式碼會建立 Enrollment 資料表中的每一筆記錄的資料列顯示所選取學生的資料表。 **顯示**方法將 HTML 呈現物件 (modelItem) 表示的運算式。 使用 Display 方法 （而非直接內嵌在程式碼中的屬性值） 以確定此值的格式正確根據其型別和該類型的範本。 在此範例中，每個運算式會從目前的記錄，在迴圈中，來傳回單一屬性且值為基本型別，其呈現為文字。

## <a name="confirm-courses-are-added"></a>確認課程也會新增

執行方案。 按一下 **學生的清單**，然後選取**詳細資料**學生的其中一個。 您會看到已註冊的課程已包含在檢視中。

![註冊的學生](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>後續步驟
在本教學課程中，您已：

> [!div class="checklist"]
> * 已加入的課程，學生詳細資料頁面
> * 確認課程會新增至頁面

請前進到下一個教學課程，以了解如何新增資料註解來指定驗證需求，並顯示格式。
> [!div class="nextstepaction"]
> [增強資料驗證](enhancing-data-validation.md)
