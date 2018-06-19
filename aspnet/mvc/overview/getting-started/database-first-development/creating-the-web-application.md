---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 第一個使用 ASP.NET MVC 的 EF 資料庫： 建立 Web 應用程式和資料模型 |Microsoft 文件
author: tfitzmac
description: 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 04ccc00fa48702608fdc7b5b00d73778985852f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878772"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>第一個使用 ASP.NET MVC 的 EF 資料庫： 建立 Web 應用程式和資料模型
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。
> 
> 數列的這個部分著重於建立 web 應用程式，並產生您的資料庫資料表為基礎的資料模型。


## <a name="create-a-new-aspnet-web-application"></a>建立新的 ASP.NET Web 應用程式

在新的方案或資料庫專案與相同的方案中，建立新的專案，Visual Studio 中，並選取**ASP.NET Web 應用程式**範本。 將專案命名**ContosoSite**。

![建立專案](creating-the-web-application/_static/image1.png)

按一下 [確定 **Deploying Office Solutions**]。

在 [新增 ASP.NET 專案] 視窗中，選取**MVC**範本。 您可以清除**雲端中的主機**現在選項，因為您將部署之後到雲端應用程式。 按一下**確定**建立應用程式。

![選取 mvc 範本](creating-the-web-application/_static/image2.png)

在建立專案的預設檔案和資料夾。

在本教學課程中，您將使用 Entity Framework 6。 您可以在您的專案，透過 [管理 NuGet 封裝] 視窗中檢查 Entity Framework 的版本。 必要時，更新您的 Entity Framework 版本。

![顯示的版本](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>產生模型

您現在會從資料庫資料表建立 Entity Framework 模型。 這些模型是您將用於處理資料的類別。 每一個模型會反映資料庫中的資料表，並包含對應到資料表中資料行的屬性。

以滑鼠右鍵按一下**模型**資料夾，然後選取**新增**和**新項目**。

![加入新項目](creating-the-web-application/_static/image4.png)

在 [加入新項目] 視窗中，選取**資料**的左窗格中， **ADO.NET 實體資料模型**從中間窗格內的選項。 將新的模型檔案**ContosoModel**。

![建立模型](creating-the-web-application/_static/image5.png)

按一下 [加入] 。

在 實體資料模型精靈中，選取 **資料庫的 EF Designer**。

![從資料庫產生](creating-the-web-application/_static/image6.png)

按 [ **下一步**]。

如果您有在您的開發環境中定義的資料庫連接，您可能會看到其中一個預先選取這些連線。 不過，您會想要建立新的連接到您在本教學課程的第一個部分中建立的資料庫。 按一下**新連線** 按鈕。

![連接到資料庫](creating-the-web-application/_static/image7.png)

在 [連接屬性] 視窗中，提供您的資料庫建立所在的本機伺服器的名稱 (在此情況下 **(localdb) \ProjectsV12**)。 提供伺服器名稱之後, 請從可用的資料庫選取 ContosoUniversityData。

![設定連接屬性](creating-the-web-application/_static/image8.png)

按一下 [確定 **Deploying Office Solutions**]。

現在會顯示正確的連接屬性。 您可以在 Web.Config 檔案中使用連接的預設名稱

![連線設定](creating-the-web-application/_static/image9.png)

按 [ **下一步**]。

選取**資料表**產生三個資料表的模型。

![選取的資料表](creating-the-web-application/_static/image10.png)

按一下 [ **完成**]。

如果您收到安全性警告，請選取**確定**繼續執行範本。

模型產生資料庫資料表和圖表會顯示，並列出的屬性和資料表之間的關聯性。

![模型的圖表](creating-the-web-application/_static/image11.png)

[模型] 資料夾現在會包含從資料庫產生的模型相關的許多新檔案。

![顯示新的模型檔案](creating-the-web-application/_static/image12.png)

**ContosoModel.Context.cs**檔案包含的類別，衍生自**DbContext**類別，並提供每個模型類別對應至資料庫資料表的屬性。 **Course.cs**， **Enrollment.cs**，和**Student.cs**檔案包含代表資料庫資料表的模型類別。 使用 scaffolding 時，您將使用內容類別和模型類別。

進行本教學課程前，先建置專案。 在下一步 區段中，將會產生資料模型為基礎的程式碼，但如果無法建置專案，將無法運作 > 一節。

> [!div class="step-by-step"]
> [上一頁](setting-up-database.md)
> [下一頁](generating-views.md)
