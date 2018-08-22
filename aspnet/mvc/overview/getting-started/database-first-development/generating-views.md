---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: EF Database First 與 ASP.NET MVC： 產生檢視 |Microsoft Docs
author: tfitzmac
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 74c7abdc2d0f8fff9ad769d013fb001e2b9e427b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825163"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>EF Database First 與 ASP.NET MVC： 產生檢視
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。
> 
> 系列的這個部分會著重於使用 ASP.NET Scaffolding 產生控制器和檢視。


## <a name="add-scaffold"></a>新增 scaffold

您已準備好產生程式碼，將會提供標準資料作業，模型類別。 您可以加入 scaffold 項目，以將程式碼。 有許多類型的樣板，您可以新增;在本教學課程中，控制器和檢視對應至您在上一節中建立的 Student 和註冊模型，會包含 scaffold。

若要維護您的專案中的一致性，您會將新的控制站加入現有**控制器**資料夾。 以滑鼠右鍵按一下**控制器**資料夾，然後選取**新增**–**新增 Scaffold 項目**。

![新增 scaffold](generating-views/_static/image1.png)

選取  **MVC 5 控制器與檢視，使用 Entity Framework**選項。 此選項會產生控制器和檢視更新、 刪除、 建立和顯示您的模型中的資料。

![新增 mvc 控制器](generating-views/_static/image2.png)

選取 [**學生**模型類別]，然後選取**ContosoUniversityEntities**內容類別。 保留做為控制器名稱**StudentsController**，

![指定控制器](generating-views/_static/image3.png)

按一下 [加入] 。

如果您收到錯誤，可能是因為您並未建置在上一節中的專案。 如果是的話，請嘗試建置專案，，然後再次新增 scaffold 項目。

程式碼產生程序完成之後，您會看到新的控制器和檢視專案中。

![顯示檢視](generating-views/_static/image4.png)

同樣地，執行相同的步驟，但是加入 scaffold，以註冊類別。 完成後，您應該有**EnrollmentsController.cs**檔案和資料夾之下**檢視**名為**註冊**以 Create、 Delete、 詳細資料、 編輯和索引檢視。

![顯示檢視](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>將連結新增至新的檢視

為了讓您更輕鬆地瀏覽至您新的檢視，您可以為學生和註冊幾個超連結新增至索引檢視。 開啟的檔案**Views/Home/Index.cshtml**，這是您網站的 [首頁] 頁面。 下列程式碼下方新增 jumbotron。

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

ActionLink 方法中，第一個參數是要顯示在連結的文字。 第二個參數是動作，第三個參數是控制器的名稱。 比方說，第一個連結會指向 StudentsController 中的索引動作。 實際的超連結會從這些值來建構。 最終的第一個連結會帶領使用者前往**Index.cshtml**檔案內**檢視/學生**資料夾。

## <a name="display-student-views"></a>顯示學生檢視

您將驗證新增至您的專案正確的程式碼顯示的學生，清單，並可讓使用者編輯、 建立或刪除資料庫中的學生記錄。

以滑鼠右鍵按一下**Views/Home/Index.cshtml**檔案，然後選取**瀏覽器中的檢視**。 在此頁面上，按一下連結的學生清單。

![](generating-views/_static/image6.png)

在此頁面上，注意到的學生與連結，以修改此資料的清單。

![學生的清單](generating-views/_static/image7.png)

按一下 **新建**連結，並提供一些值給新的學生。

![建立新的學生](generating-views/_static/image8.png)

按一下 **建立**，並注意新的學生新增至您的清單。

![使用新的學生清單](generating-views/_static/image9.png)

選取 **編輯**連結，然後變更的某些值的學生。

![編輯學生](generating-views/_static/image10.png)

按一下 **儲存**，並注意學生資料錄已變更。

最後，選取**刪除**連結，並確認您想要刪除此記錄，依序按一下**刪除** 按鈕。

![刪除學生](generating-views/_static/image11.png)

不需要撰寫任何程式碼，您已新增一般對資料執行作業的 Student 資料表中的檢視。

您可能已經注意到，欄位的文字標籤為基礎之資料庫屬性 (例如**LastName**) 不一定是您想要顯示在網頁上。 比方說，您可能會想要的標籤**姓氏**。 稍後在本教學課程中，您將會修正這個顯示問題。

## <a name="display-enrollment-views"></a>顯示註冊檢視

您的資料庫都包含學生和註冊的資料表，與 Course 與 Enrollment 資料表之間的一對多關聯性之間的一對多關聯性。 註冊的檢視正確處理這些關聯性。 瀏覽至您的網站，並選取首頁**註冊清單**連結，然後**建立新**連結。 檢視會顯示用來建立新的註冊記錄的表單。 特別是，請注意，表單包含兩個相關資料表中的值會填入的下拉式清單。

![建立註冊](generating-views/_static/image12.png)

此外，驗證提供的值會自動套用根據欄位的資料類型。 級需要的數字，因此如果您嘗試使用不相容的價值，就會顯示一則錯誤訊息。

![驗證訊息](generating-views/_static/image13.png)

您已驗證的自動產生的檢視，可讓使用者可以使用資料庫中的資料。 在本系列中下一個教學課程中，您會更新資料庫，並在 web 應用程式中進行對應的變更。

> [!div class="step-by-step"]
> [上一頁](creating-the-web-application.md)
> [下一頁](changing-the-database.md)
