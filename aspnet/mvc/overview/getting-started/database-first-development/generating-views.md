---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 教學課程：產生檢視 EF Database First 與 ASP.NET MVC 應用程式
description: 本文著重於使用的 ASP.NET Scaffold 產生控制器和檢視。
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e1f6646cdf10d293268b92f44b018709e70c0f86
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889778"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>教學課程：產生檢視 EF Database First 與 ASP.NET MVC 應用程式

您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。

本文著重於使用的 ASP.NET Scaffold 產生控制器和檢視。

在本教學課程中，您已：

> [!div class="checklist"]
> * 新增 scaffold
> * 將連結新增至新的檢視
> * 顯示學生檢視
> * 顯示註冊檢視

## <a name="prerequisite"></a>必要條件

* [建立 web 應用程式和資料模型](creating-the-web-application.md)

## <a name="add-scaffold"></a>新增 scaffold

您已準備好產生程式碼，將會提供標準資料作業，模型類別。 您可以加入 scaffold 項目，以將程式碼。 有許多類型的樣板，您可以新增;在本教學課程中，控制器和檢視對應至您在上一節中建立的 Student 和註冊模型，會包含 scaffold。

若要維護您的專案中的一致性，您會將新的控制站加入現有**控制器**資料夾。 以滑鼠右鍵按一下**控制器**資料夾，然後選取**新增** > **新增 Scaffold 項目**。

選取  **MVC 5 控制器與檢視，使用 Entity Framework**選項。 此選項會產生控制器和檢視更新、 刪除、 建立和顯示您的模型中的資料。

![新增 mvc 控制器](generating-views/_static/image2.png)

選取 **學生 (ContosoSite.Models)** 模型類別，然後選取**ContosoUniversityDataEntities (ContosoSite.Models)** 內容類別。 保留做為控制器名稱**StudentsController**。

按一下 [加入] 。

如果您收到錯誤，可能是因為您並未建置在上一節中的專案。 如果是的話，請嘗試建置專案，，然後再次新增 scaffold 項目。

程式碼產生程序完成之後，您會在您的專案中看到新的控制器和檢視**控制器**並**檢視** > **學生**資料夾.


同樣地，執行相同的步驟，但將的 scaffold**註冊**類別。 完成後，您會有**EnrollmentsController.cs**檔案和資料夾之下**檢視**名為**註冊**與 Create、 Delete、 詳細資料、 編輯和索引檢視。

## <a name="add-links-to-new-views"></a>將連結新增至新的檢視

為了讓您更輕鬆地瀏覽至您新的檢視，您可以為學生和註冊幾個超連結新增至索引檢視。 開啟的檔案**檢視** > **家用** > *Index.cshtml*，這是您網站的 [首頁] 頁面。 下列程式碼下方新增 jumbotron。

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

ActionLink 方法中，第一個參數是要顯示在連結的文字。 第二個參數是動作，第三個參數是控制器的名稱。 比方說，第一個連結會指向 StudentsController 中的索引動作。 實際的超連結會從這些值來建構。 最終的第一個連結會帶領使用者前往**Index.cshtml**檔案內**檢視/學生**資料夾。

## <a name="display-student-views"></a>顯示學生檢視

您將驗證新增至您的專案正確的程式碼顯示的學生，清單，並可讓使用者編輯、 建立或刪除資料庫中的學生記錄。

以滑鼠右鍵按一下**檢視** > **首頁** > *Index.cshtml*檔案，然後選取**瀏覽器中的檢視**。 在應用程式的首頁上，選取**學生清單**。

![](generating-views/_static/image6.png)

在  **Index**頁面上，注意清單中的學生與連結，以修改此資料。 選取 **新建**連結，並提供一些值給新的學生。 按一下 **建立**，並注意新的學生新增至您的清單。

回到**Index**頁面上，選取**編輯**連結，然後變更的某些值的學生。 按一下 **儲存**，並注意學生資料錄已變更。

最後，選取**刪除**連結，並確認您想要刪除此記錄，依序按一下**刪除** 按鈕。

不需要撰寫任何程式碼，您已新增一般對資料執行作業的 Student 資料表中的檢視。

您可能已經注意到，欄位的文字標籤為基礎之資料庫屬性 (例如**LastName**) 不一定是您想要顯示在網頁上。 比方說，您可能會想要的標籤**姓氏**。 稍後在本教學課程中，您將會修正這個顯示問題。

## <a name="display-enrollment-views"></a>顯示註冊檢視

您的資料庫都包含學生和註冊的資料表，與 Course 與 Enrollment 資料表之間的一對多關聯性之間的一對多關聯性。 註冊的檢視正確處理這些關聯性。 瀏覽至您的網站，並選取首頁**註冊清單**連結，然後**建立新**連結。

檢視會顯示用來建立新的註冊記錄的表單。 特別是，請注意，表單就會包含**CourseID**下拉式清單並**StudentID**下拉式清單。 兩者會填入相關的資料表中的值。

此外，驗證提供的值會自動套用根據欄位的資料類型。 **級**需要數字，因此如果您嘗試使用不相容的價值，就會顯示一則錯誤訊息：*[等級] 欄位必須是數字。*

您已驗證的自動產生的檢視，可讓使用者可以使用資料庫中的資料。 在本系列中下一個教學課程中，您會更新資料庫，並在 web 應用程式中進行對應的變更。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 新增的 scaffold
> * 已新增新的檢視連結
> * 顯示的學生檢視
> * 顯示的註冊檢視

請前往下一篇文章，以了解如何將資料庫變更。
> [!div class="nextstepaction"]
> [變更資料庫](changing-the-database.md)