---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: "開始使用 Entity Framework 6 資料庫優先使用 MVC 5 |Microsoft 文件"
author: tfitzmac
description: "使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: cb979333131cc6ac87fd640bf7c96931054a1814
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>開始使用 Entity Framework 6 資料庫優先使用 MVC 5
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。 在序列的最後一個部分中，您將部署至 Azure 的站台和資料庫。
> 
> 數列的這個部分著重於建立資料庫和資料填入其中。
> 
> 這一系列從 Tom Dykstra 和 Rick Anderson 寫入的。 它已根據上改善的註解區段中的使用者意見。


## <a name="introduction"></a>簡介

本主題說明如何開始使用的現有資料庫中，快速建立 web 應用程式，可讓使用者與資料互動。 它會使用 Entity Framework 6 與 MVC 5 建置 web 應用程式。 ASP.NET Scaffolding 功能可讓您自動產生程式碼顯示、 更新、 建立和刪除資料。 使用 Visual Studio 內發行的工具，您可以輕鬆地部署網站和資料庫至 Azure。

本主題說明這種情況您有一個資料庫並想要產生程式碼，該資料庫的欄位為基礎的 web 應用程式。 這個方法會呼叫第一個資料庫的開發。 如果您沒有現有的資料庫，您可以改為使用此方法稱為 Code First 開發包含定義資料類別，並產生類別內容的資料庫。

Code First 開發的簡介範例，請參閱[開始使用 ASP.NET MVC 5](../introduction/getting-started.md)。 如需更進階的範例，請參閱[建立 ASP.NET MVC 4 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

如需選取要使用哪一個 Entity Framework 方法的指引，請參閱[Entity Framework 開發方式](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。

## <a name="prerequisites"></a>必要條件

Visual Studio 2013 or Visual Studio Express 2013 for Web

## <a name="set-up-the-database"></a>將資料庫設定

若要模擬的環境需要現有的資料庫，您將第一次使用某些預先填入的資料，建立資料庫，然後再建立 web 應用程式連接至資料庫的。

本教學課程被開發使用 LocalDB Visual Studio 2013 或 Visual Studio Express 2013 for Web。 您可以使用現有的資料庫伺服器，而不是 LocalDB，但根據您的 Visual Studio 和您的資料庫類型的版本，所有的 Visual Studio 中的資料工具可能不支援。 如果工具不適用於您的資料庫，您可能需要執行一些管理套件中的特定資料庫的步驟為您的資料庫。

如果您的 Visual Studio 版本中有資料庫工具的問題，請確定您已安裝資料庫工具的最新版本。 如需更新或安裝資料庫工具的詳細資訊，請參閱[Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027)。

啟動 Visual Studio 並建立**SQL Server 資料庫專案**。 將專案命名**ContosoUniversityData**。

![建立資料庫專案](setting-up-database/_static/image1.png)

您現在有了空的資料庫專案。 您將此將資料庫部署到 Azure 稍後在本教學課程中，因此您必須將 Azure SQL Database 設為專案的目標平台。 設定目標平台不會實際部署資料庫。它僅表示資料庫專案將驗證資料庫設計為與目標平台相容。 若要設定的目標平台，請開啟**屬性**專案並選取**Microsoft Azure SQL Database**目標平台。

![將目標平台設定](setting-up-database/_static/image2.png)

您可以建立本教學課程需要加上定義之資料表的 SQL 指令碼的資料表。 以滑鼠右鍵按一下您的專案，並加入新項目。

![加入新項目](setting-up-database/_static/image3.png)

加入名為學生的新資料表。

![加入學生資料表](setting-up-database/_static/image4.png)

資料表檔中建立資料表的下列程式碼取代 T-SQL 命令。

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

請注意，[設計] 視窗會自動同步處理的程式碼。 您可以使用程式碼或設計工具。

![顯示程式碼和設計](setting-up-database/_static/image5.png)

加入另一個資料表。 此時間課程將它命名，並使用下列 T-SQL 命令。

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

然後，重複一次建立名為註冊的資料表。

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

您可以填入您的資料庫，透過在部署資料庫之後，執行指令碼的資料。 部署後指令碼加入專案。 您可以使用預設名稱。

![加入部署後指令碼](setting-up-database/_static/image6.png)

將下列 T-SQL 程式碼加入至部署後指令碼。 此指令碼僅會將資料加入至資料庫，找到沒有相符的記錄時。 它不覆寫或刪除任何您可能在資料庫中輸入的資料。

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

請務必注意，在每次部署資料庫專案，執行部署後指令碼。 因此，您必須仔細地考量您的需求，撰寫此指令碼時。 在某些情況下，您可能希望每次部署專案時，從頭開始從一組已知的資料。 在其他情況下，您可能不想變更現有的資料，以任何方式。 根據您的需求，您可以決定是否需要部署後指令碼，或您要包含在指令碼中的項目。 如需填入您的部署後指令碼的資料庫的詳細資訊，請參閱[包括 SQL Server 資料庫專案中的資料](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)。

現在您有 4 個 SQL 指令碼檔案，但沒有實際的資料表。 您已準備好部署至 localdb 資料庫專案。 在 Visual Studio 中，按一下 [開始] 按鈕 （或 F5），來建置及部署資料庫專案。 請檢查 [輸出] 索引標籤，以確認建置和部署成功。

![顯示輸出](setting-up-database/_static/image7.png)

若要查看已建立新的資料庫，請開啟**SQL Server 物件總管**和尋找正確的本機資料庫伺服器中的專案名稱 (在此情況下**(localdb) \ProjectsV12**)。

![顯示新的資料庫](setting-up-database/_static/image8.png)

若要查看資料表填入資料，以滑鼠右鍵按一下資料表，然後選取**檢視資料**。

![顯示資料表資料](setting-up-database/_static/image9.png)

資料表資料的可編輯檢視隨即顯示。

![顯示資料表資料的結果](setting-up-database/_static/image10.png)

您的資料庫現在已設定，並填入資料。 在下一個教學課程中，您將建立資料庫的 web 應用程式。

>[!div class="step-by-step"]
[下一步](creating-the-web-application.md)
