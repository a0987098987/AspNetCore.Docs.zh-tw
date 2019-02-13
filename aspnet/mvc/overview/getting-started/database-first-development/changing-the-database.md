---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 教學課程：變更資料庫的 EF Database First 與 ASP.NET MVC 應用程式
description: 本教學課程著重於對資料庫結構的更新和傳播該項變更整個 web 應用程式。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667631"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>教學課程：變更資料庫的 EF Database First 與 ASP.NET MVC 應用程式

您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。

本教學課程著重於對資料庫結構的更新和傳播該項變更整個 web 應用程式。

在本教學課程中，您已：

> [!div class="checklist"]
> * 加入資料行
> * 將屬性加入至檢視

## <a name="prerequisites"></a>必要條件

* [產生檢視](generating-views.md)

## <a name="add-a-column"></a>加入資料行

如果您在資料庫中更新資料表的結構，您需要確保您的變更會傳播到資料模型、 檢視和控制器。

本教學課程中，要將新的資料行加入記錄中間名學生的 Student 資料表中。 若要新增此資料行，請開啟資料庫專案，並開啟 Student.sql 檔案。 透過設計工具或 T-SQL 程式碼中，新增名為的資料行**MiddleName** NVARCHAR(50) 且允許 NULL 值。

啟動您的資料庫專案 （或 F5），這項變更部署到您的本機資料庫。 新的欄位加入至資料表。 如果您看不到 SQL Server 物件總管 中，按一下窗格中的 重新整理 按鈕。

![顯示新的資料行](changing-the-database/_static/image2.png)

新的資料行存在於資料庫資料表中，但它目前不存在的資料模型類別中。 您必須更新模型，以包含新的資料行。 在 **模型**資料夾中，開啟**ContosoModel.edmx**檔案來顯示模型圖表。 請注意，Student 模型不包含 [MiddleName] 屬性。 以滑鼠右鍵按一下設計介面上的任何位置，然後選取**從資料庫更新模型**。

在 [更新精靈] 中，選取**重新整理**索引標籤，然後選取**資料表** > **dbo** > **學生**。 按一下 [ **完成**]。

在更新程序完成之後，資料庫圖表包含新**MiddleName**屬性。 儲存**ContosoModel.edmx**檔案。 您必須儲存此檔案才能傳播至新的屬性**Student.cs**類別。 您現在已更新的資料庫和模型。

建置方案。

## <a name="add-the-property-to-the-views"></a>將屬性加入至檢視

不幸的是，在檢視仍然不包含新的屬性。 若要更新的檢視有兩個選項-您可以重新產生檢視藉由再次新增 scaffold Student 類別，或您可以手動將新屬性新增至您現有的檢視。 在本教學課程中，您會新增 scaffolding 再次因為您無法對任何自訂的變更自動產生的檢視。 您可以考慮以手動方式加入的屬性，當您檢視已變更，並不想要捨棄這些變更。

若要確保檢視時都會重新建立**學生**下方的資料夾**檢視**，並刪除**StudentsController**。 然後，以滑鼠右鍵按一下**控制器**資料夾，並新增樣板**學生**模型。 同樣地，將控制器命名**StudentsController**。 選取 [新增]。

重新建置方案。 檢視現在會包含 [MiddleName] 屬性。

![顯示中間名](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 加入資料行
> * 加入檢視中的屬性

請前進到下一個教學課程，以了解如何自訂用於顯示學生記錄的詳細檢視。
> [!div class="nextstepaction"]
> [自訂檢視](customizing-a-view.md)