---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 教學課程：建立 Web 應用程式和 ef 資料模型資料庫的第一個使用 ASP.NET MVC
description: 本教學課程著重於建立 web 應用程式，並產生您的資料庫資料表為基礎的資料模型。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: dced55386c3f810e406c5c2b3f0071b45e3b2dbd
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236363"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>教學課程：建立 Web 應用程式和 ef 資料模型資料庫的第一個使用 ASP.NET MVC

 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。

本教學課程著重於建立 web 應用程式，並產生您的資料庫資料表為基礎的資料模型。

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 ASP.NET Web 應用程式
> * 產生模型

## <a name="prerequisites"></a>必要條件

* [開始使用 Entity Framework 6 Database First 使用 MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>建立 ASP.NET Web 應用程式

在新的解決方案或與資料庫專案相同的方案中，建立新的專案在 Visual Studio 中，並選取**ASP.NET Web 應用程式**範本。 將專案命名為**ContosoSite**。

![建立專案](creating-the-web-application/_static/image1.png)

按一下 [確定] 。

在 [新增 ASP.NET 專案] 視窗中，選取**MVC**範本。 您可以清除**雲端中的主機**選項現在，因為您將部署更新版本的雲端應用程式。 按一下 **確定**建立應用程式。

建立專案的預設檔案和資料夾。

在本教學課程中，您將使用 Entity Framework 6。 您可以在您的專案，透過 [管理 NuGet 封裝] 視窗中，仔細查看 Entity Framework 的版本。 如有必要，請更新您的 Entity Framework 的版本。

![顯示版本](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>產生模型

您現在會從資料庫資料表建立 Entity Framework 模型。 這些模型是您將使用的資料搭配使用的類別。 每個模型鏡像資料庫中的資料表，並包含對應到資料表中資料行的屬性。

以滑鼠右鍵按一下**模型**資料夾，然後選取**新增**並**新項目**。

在 [加入新項目] 視窗中，選取**資料**的左窗格中並**ADO.NET 實體資料模型**從中間窗格中的選項。 將新的模型檔案**ContosoModel**。

按一下 [加入] 。

在 [實體資料模型精靈] 中，選取**資料庫的 EF Designer**。

按 [ **下一步**]。

如果您有在您的開發環境內定義的資料庫連接，您可能會看到其中一個預先選取這些連線。 不過，您會想要建立新的連接到您在本教學課程的第一個部分中建立的資料庫。 按一下 [**新的連接**] 按鈕。

在 [連接屬性] 視窗中，提供您的資料庫建立所在的本機伺服器的名稱 (在此情況下 **(localdb) \Projects13**)。 提供伺服器名稱之後, 請從可用的資料庫選取 ContosoUniversityData。

![設定連接屬性](creating-the-web-application/_static/image8.png)

按一下 [確定] 。

現在會顯示正確的連接屬性。 您可以在 Web.Config 檔案中使用連接的預設名稱。

按 [ **下一步**]。

選取最新版的 Entity Framework。

按 [ **下一步**]。

選取 **資料表**來產生所有的三個資料表的模型。

按一下 [ **完成**]。

如果您收到安全性警告，請選取**確定**繼續執行範本。

模型從資料庫資料表中，產生，並會顯示一個圖表，顯示的屬性和資料表之間的關聯性。

![模型的圖表](creating-the-web-application/_static/image11.png)

[模型] 資料夾現在包含已從資料庫產生模型與相關的許多新檔案。

**ContosoModel.Context.cs**檔案包含的類別衍生自**DbContext**類別，並提供屬性，對應至資料庫資料表每個模型類別。 **Course.cs**， **Enrollment.cs**，並**Student.cs**檔案包含代表資料庫資料表的模型類別。 使用樣板時，您將使用模型類別和內容類別。

繼續進行本教學課程之前，建置專案。 在下一步 區段中，您會產生資料模型為基礎的程式碼，但如果尚未建置專案，將無法運作一節。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 ASP.NET web 應用程式
> * 產生模型

請前進到下一個教學課程，以了解如何建立會產生資料模型為基礎的程式碼。
> [!div class="nextstepaction"]
> [產生檢視](generating-views.md)