---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: "第一個使用 ASP.NET MVC 的 EF 資料庫： 產生檢視 |Microsoft 文件"
author: tfitzmac
description: "使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 5fccb3c56af0945ec448becff777a3e92dc160d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>第一個使用 ASP.NET MVC 的 EF 資料庫： 產生檢視表
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。
> 
> 數列的這個部分著重於使用 ASP.NET Scaffolding 產生控制器和檢視。


## <a name="add-scaffold"></a>加入 scaffold

您已準備好產生程式碼將會提供標準的資料作業模型類別。 您可以加入 scaffold 項目將程式碼。 有許多選項，您可以加入; scaffolding 類型在本教學課程中，控制器和檢視對應至您在上一節中建立的學生和註冊模型，將包含 scaffold。

為了維持一致性，您的專案中，您會將新的控制站加入現有**控制器**資料夾。 以滑鼠右鍵按一下**控制器**資料夾，然後選取**新增**–**新的 Scaffold 項目**。

![加入 scaffold](generating-views/_static/image1.png)

選取**的 MVC 5 控制器與檢視，使用 Entity Framework**選項。 此選項將會產生控制器和檢視更新、 刪除、 建立和顯示您的模型中的資料。

![新增 mvc 控制器](generating-views/_static/image2.png)

選取**學生**模型類別，並選取**ContosoUniversityEntities**內容類別。 保留相同的控制器名稱**StudentsController**，

![指定控制器](generating-views/_static/image3.png)

按一下 [加入] 。

如果您收到錯誤，它可能是因為您未建立在上一節中的專案。 如果是這樣，建置專案，再試一次，然後再次新增 scaffold 項目。

程式碼產生程序完成之後，您會看到新的控制器和檢視您的專案中。

![顯示檢視](generating-views/_static/image4.png)

同樣地，執行相同的步驟，但是加入 scaffold 註冊類別。 完成時，您應該有**EnrollmentsController.cs**檔和資料夾之下的**檢視**名為**註冊**以 Create、 Delete、 詳細資料、 編輯和索引檢視。

![顯示檢視](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>將連結加入至新的檢視

若要讓您更輕鬆地瀏覽至新的檢視，您可以將超連結數加入索引檢視表學生版和註冊項目。 開啟位於檔案**Views/Home/Index.cshtml**，這是您的網站的首頁。 新增 jumbotron 以下程式碼。

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

ActionLink 方法中，第一個參數是要顯示在連結的文字。 第二個參數是動作，而第三個參數是控制器的名稱。 例如，第一個連結會指向 StudentsController 中的索引動作。 實際的超連結是從這些值所建構。 第一個連結最終會採用使用者**Index.cshtml**檔案內**檢視/學生**資料夾。

## <a name="display-student-views"></a>顯示學生檢視

您將驗證加入至您的專案正確的程式碼顯示學生的清單，並可讓使用者編輯、 建立或刪除資料庫中的學生記錄。

以滑鼠右鍵按一下**Views/Home/Index.cshtml**檔案，然後選取**瀏覽器中的檢視**。 在此頁面上，按一下 學員清單的連結。

![](generating-views/_static/image6.png)

這個頁面上，請注意學生與連結，以修改此資料的清單。

![學員清單](generating-views/_static/image7.png)

按一下**新建**連結，並提供新的學生的某些值。

![建立新的學生](generating-views/_static/image8.png)

按一下**建立**，並注意新的學生新增到清單。

![與新的學生的清單](generating-views/_static/image9.png)

選取**編輯**連結，並變更部分的值為一位學生。

![編輯學生](generating-views/_static/image10.png)

按一下**儲存**，並注意學生記錄已變更。

最後，選取**刪除**連結，並確認您想要刪除記錄，方法是按一下**刪除** 按鈕。

![刪除學生](generating-views/_static/image11.png)

不需要撰寫任何程式碼，您已加入執行常見的作業資料學生表中的檢視。

您可能已注意到欄位文字標籤為基礎之資料庫屬性 (例如**LastName**) 但不一定要在網頁上顯示。 例如，您可能會想要標籤**姓氏**。 稍後在本教學課程中，您會修正此顯示問題。

## <a name="display-enrollment-views"></a>顯示註冊檢視

您的資料庫包含 Student 與註冊的資料表，與 「 課程 」 和 「 註冊資料表之間的一對多關聯性之間的一對多關聯性。 註冊的檢視會正確地處理這些關聯性。 瀏覽的網站，並選取 [首頁]**的註冊項目清單**連結，然後**新建**連結。 檢視會顯示表單，以建立新的註冊記錄。 特別是，請注意，表單會包含兩個下拉式清單，其中會填入相關資料表中的值。

![建立註冊](generating-views/_static/image12.png)

此外，驗證提供的值會自動套用根據欄位的資料類型。 等級需要數字，因此如果您嘗試以提供不相容的值，會顯示錯誤訊息。

![驗證訊息](generating-views/_static/image13.png)

您已驗證自動產生的檢視，讓使用者使用的資料庫中的資料。 在此系列的下一個教學課程，您將會更新資料庫和對應變更 web 應用程式中。

>[!div class="step-by-step"]
[上一頁](creating-the-web-application.md)
[下一頁](changing-the-database.md)
