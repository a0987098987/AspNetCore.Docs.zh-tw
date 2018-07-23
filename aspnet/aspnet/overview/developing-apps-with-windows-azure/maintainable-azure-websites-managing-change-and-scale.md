---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 實際操作實驗室： 可維護的 Azure 網站： 管理變更和規模 |Microsoft Docs
author: rick-anderson
description: 在此實驗室中，了解 Microsoft Azure 如何讓您輕鬆建置及部署網站至生產環境。
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 3a118cdd7e3f3878976e4f8480ce2236b8d3ba88
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824792"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>實際操作實驗室： 可維護的 Azure 網站： 管理變更和小數位數
====================
藉由[Web Camp 小組](https://twitter.com/webcamps)

[下載 Web 研討會訓練套件](http://aka.ms/webcamps-training-kit)

> Microsoft Azure 可讓您輕鬆建置及部署網站至生產環境。 但您未完成時即時應用程式時，您剛開始使用 ！ 您必須處理變更的需求、 資料庫更新、 小數位數和更多功能。 幸運的是，Azure App Service 具有您的後盾，並附上豐富的功能，可協助您保護您的站台順利執行。
> 
> Azure 提供安全且彈性的開發、 部署和延展選項為任何大小的 web 應用程式。 運用現有的工具來建立及部署應用程式無須管理基礎結構。
> 
> 自行佈建生產 web 應用程式以分鐘為單位輕鬆地部署您慣用的開發工具所建立的內容。 您可以部署現有的網站，直接從支援的原始檔控制**Git**， **GitHub**， **Bitbucket**， **TFS**，和甚至**DropBox**。 部署直接從您最喜愛的 IDE 或使用指令碼**PowerShell**在 Windows 或**CLI**任何作業系統上執行的工具。 一旦部署之後，掌握您的網站持續進行連續部署的支援。
> 
> Azure 提供可調式、 持久的雲端儲存體、 備份和復原解決方案的任何資料，不論巨量或少。 當部署至生產環境，例如資料表、 Blob 和 SQL 資料庫的儲存體服務的應用程式，協助您調整在雲端中的應用程式。
> 
> 使用 SQL 資料庫時，請務必保持產能的資料庫部署應用程式的新版本時。 要感謝**Entity Framework Code First Migrations**，開發和部署您的資料模型已簡化，以更新您的環境，以分鐘為單位。 這個實際操作實驗室會顯示您的 web 應用程式部署到 Microsoft Azure 中的生產環境時，可能會遇到的不同主題。
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。
> 
> 如需本主題的深入報導，請參閱 <<c0> [ 建置真實世界雲端應用程式與 Azure 的電子書](building-real-world-cloud-apps-with-windows-azure/introduction.md)。


<a id="Overview"></a>
## <a name="overview"></a>總覽

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 讓 使用現有的模型 Entity Framework 移轉
- 更新物件模型並據以使用 Entity Framework 移轉的資料庫
- 部署至 Azure App Service 使用 Git
- 回復成先前的部署使用 Azure 管理入口網站
- 使用 Azure 儲存體來調整 web 應用程式
- 設定自動調整 web 應用程式中使用 Azure 管理入口網站
- 建立和設定 Visual Studio 中的負載測試專案

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

需要下列項目才能完成這個實際操作實驗室：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本
- [Azure SDK for.NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [GIT 版本控制系統](http://git-scm.com/download)
- Microsoft Azure 訂用帳戶 

    - 註冊[免費試用版](http://aka.ms/watk-freetrial)
    - 如果您是 Visual Studio Professional、 Test Professional、 Premium 或 Ultimate with MSDN 或 MSDN Platforms 訂閱者時，啟用您[MSDN 權益](http://aka.ms/watk-msdn)現在要開始開發及測試 Azure 上
    - [BizSpark](http://aka.ms/watk-bizspark)成員會自動獲得 Azure 權益透過其 Visual Studio Ultimate with MSDN 訂用帳戶
    - 成員[Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials 方案收到免費的每月 Azure 點數

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

若要在這個實際操作實驗室中執行的練習，您必須先設定您的環境。

1. 開啟 Windows Explorer 並瀏覽至實驗室**來源**資料夾。
2. 以滑鼠右鍵按一下**Setup.cmd** ，然後選取**系統管理員身分執行**來啟動安裝程序，將會設定您的環境並安裝這個實驗室的 Visual Studio 程式碼片段。
3. 如果會顯示 [使用者帳戶控制] 對話方塊，確認要繼續進行的動作。

> [!NOTE]
> 請確定您執行安裝程式之前檢查這個實驗室的所有相依性。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>使用程式碼片段

整個實驗室文件中，您將會指示要插入程式碼區塊。 為了方便起見，大部分的這段程式碼提供 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，若要避免以手動方式新增內存取。

> [!NOTE]
> 每個練習會伴隨起始方案，位於**開始**練習，可讓您依照每個練習，獨立於其他的資料夾。 請留意練習期間新增的程式碼片段缺少這些啟動解決方案，並可能無法運作，直到您已完成練習。 在練習的原始程式碼，您也可以找到**結束**資料夾包含 Visual Studio 方案，以程式碼所產生的相對應的練習中的步驟。 如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案與指引。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室還包含下列練習：

1. [使用 Entity Framework 移轉](#Exercise1)
2. [Web 應用程式部署至預備環境](#Exercise2)
3. [在生產環境中執行部署復原](#Exercise3)
4. [使用 Azure 儲存體進行調整](#Exercise4)
5. [使用 Web 應用程式的自動調整規模](#Exercise5)（可選擇性地用於 Visual Studio 2013 Ultimate edition）

估計的時間才能完成這個實驗室： **75 分鐘**

> [!NOTE]
> 當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。 每個預先定義的集合以符合特定的開發樣式設計，並決定視窗版面配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。 在這個實驗室中的程序說明完成指定的工作，在 Visual Studio 中使用時所需的動作**一般開發設定**集合。 如果您選擇不同的設定集合，您的開發環境，可能會有在步驟中，您應該考慮到的差異。


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>練習 1： 使用 Entity Framework 移轉

當您開發應用程式時，您的資料模型可能會隨著時間改變。 這些變更可能會影響您的資料庫中現有的模型，（如果您要建立新的版本），並務必要讓資料庫保持在最新狀態，以避免發生錯誤。

若要簡化您的模型，這些變更的追蹤**Entity Framework Code First Migrations**自動偵測比較您的模型資料庫結構描述的變更，並產生特定的程式碼，以更新您的資料庫，建立新*版本*您的資料庫。

這個練習將示範如何啟用**移轉**針對您的應用程式，以及如何輕鬆地偵測和產生的變更來更新您的資料庫。

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>工作 1： 啟用移轉

在這個工作中，您會瀏覽的啟用步驟**Entity Framework Code First Migrations**要**Geek 測驗**資料庫變更的模型，以及了解如何將這些變更反映在資料庫。

1. 開啟 Visual Studio，然後開啟**GeekQuiz.sln**方案檔從**Source\Ex1 UsingEntityFrameworkMigrations\Begin**。
2. 建置方案，以下載和安裝**NuGet**封裝的相依性。 若要這樣做，請以滑鼠右鍵按一下方案，然後按一下**建置方案**或按**Ctrl + Shift + B**。
3. 從**工具**功能表，在 Visual Studio 中，選取**程式庫套件管理員**，然後按一下**Package Manager Console**。
4. 在  **Package Manager Console**，輸入下列命令，然後按**Enter**。 將建立初始移轉現有的模型為基礎。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![啟用移轉](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "啟用移轉")

    *啟用移轉*

    > [!NOTE]
    > 此命令會新增**移轉**Geek 測驗的專案，其中包含檔案的資料夾名**Configuration.cs**。 **組態**類別可讓您設定移轉您的內容的運作方式。
5. 使用啟用的移轉，您需要更新**組態**類別，以填入資料庫的初始資料， **Geek 測驗**需要。 底下**移轉**，取代**Configuration.cs**檔案，與位於**Source\Assets**本實驗室的資料夾。

    > [!NOTE]
    > 由於**移轉**稱之為**種子**與每個資料庫更新的方法，您必須確定資料庫中不會複寫記錄。 **AddOrUpdate**方法將有助於防止重複的資料。
6. 新增初始移轉，請輸入下列命令，然後按**Enter**。

    > [!NOTE]
    > 請確定沒有名為資料庫&quot;GeekQuizProd&quot; LocalDB 執行個體中。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![新增基底結構描述移轉](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "加入基底結構描述移轉")

    *新增基底結構描述移轉*

    > [!NOTE]
    > **新增移轉**會 scaffold 下一步 的移轉，根據您對您的模型建立的最後一個移轉以來的變更。 在此情況下，因為它是第一個移轉專案時，它會新增建立中所定義的所有資料表的指令碼**TriviaContext**類別。
7. 執行移轉至執行下列命令來更新資料庫。 此命令會指定**Verbose**旗標，以檢視套用到目標資料庫的 SQL 陳述式。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![建立初始資料庫](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "建立初始資料庫")

    *建立初始資料庫*

    > [!NOTE]
    > **更新資料庫**會將任何暫止的移轉套用至資料庫。 在此情況下，它會建立使用您的 web.config 檔案中定義的連接字串的資料庫。
8. 移至**檢視**功能表，然後開啟**SQL Server 物件總管**。

    ![在 SQL Server 物件總管 中開啟](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "在 SQL Server 物件總管 中開啟")

    *在 SQL Server 物件總管 中開啟*
9. 在 [ **SQL Server 物件總管**] 視窗中，連接到 LocalDB 執行個體中，以滑鼠右鍵按一下**SQL Server**節點，然後選取**加入 SQL Server...** 選項。

    ![加入 SQL Server 執行個體](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "加入 SQL Server 執行個體")

    *加入 SQL Server 物件總管 中的 SQL Server 執行個體*
10. 設定**伺服器名稱**要 *(localdb) \v11.0* ，並將**Windows 驗證**當做驗證模式。 按一下  **Connect**以繼續。

    ![連接到 LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "連接到 LocalDB")

    *連接到 LocalDB*
11. 開啟**GeekQuizProd**資料庫中，展開**資料表**節點。 如您所見， **Update-database**產生的命令中所定義的所有資料表**TriviaContext**類別。 找出**dbo。TriviaQuestions**資料表，並開啟 [資料行] 節點。 在下一個工作中，您會將新的資料行新增至此資料表，並更新資料庫使用**移轉**。

    ![邏輯問題的資料行](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "邏輯問題的資料行")

    *邏輯問題的資料行*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>工作 2-使用移轉更新資料庫結構描述

在這個工作中，您將使用**Entity Framework Code First Migrations**偵測您的模型中的變更，並產生必要的程式碼，以更新資料庫。 您將會更新**TriviaQuestions**藉由將新屬性的實體。 然後，您將執行命令來建立新的移轉，將新的資料行包含資料表中。

1. 在 **方案總管**，按兩下**TriviaQuestion.cs**檔案位於**模型**資料夾。
2. 新增名為的新屬性**提示**，如下列程式碼片段所示。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. 在  **Package Manager Console**，輸入下列命令，然後按**Enter**。 將反映在我們的模型變更會建立新的移轉。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Add-migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "新增移轉")

    *新增移轉*

    > [!NOTE]
    > 移轉檔案由兩個方法，組成**向上**並**向下**。
    > 
    > - **向上**方法將用來指定哪些變更我們的應用程式需求，要套用至資料庫的目前版本。
    > - **下移**用來回復的變更，我們已新增至**向上**方法。
    > 
    > 當資料庫移轉更新資料庫時，它會執行所有移轉作業中的時間戳記順序，以及只是尚未使用上次更新之後 ( \_MigrationHistory 資料表會追蹤的哪些移轉經套用)。 **向上**所有移轉的方法會呼叫並將進行的變更我們已指定到資料庫。 如果我們要回到上一個移轉時，決定**往下**將重做的變更，以反向順序呼叫方法。
4. 在  **Package Manager Console**，輸入下列命令，然後按**Enter**。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. 輸出**Update-database**產生命令**Alter Table**新增至新的資料行的 SQL 陳述式**TriviaQuestions**資料表，如下圖所示。

    ![新增資料行的 SQL 陳述式產生](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "新增資料行的 SQL 陳述式產生")

    *新增資料行的 SQL 陳述式產生*
6. 在  **SQL Server 物件總管**，重新整理**dbo。TriviaQuestions**資料表，並確認新**提示**都會顯示資料行。

    ![顯示新的 「 提示 」 資料行](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "顯示新的 「 提示 」 資料行")

    *顯示新的 「 提示 」 資料行*
7. 回到**TriviaQuestion.cs**編輯器中，加入**StringLength**條件約束*提示*屬性，如下列程式碼片段所示。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. 在  **Package Manager Console**，輸入下列命令，然後按**Enter**。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. 在  **Package Manager Console**，輸入下列命令，然後按**Enter**。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. 輸出**Update-database**產生命令**Alter Table** SQL 陳述式來更新*提示*類型的資料行**TriviaQuestions**資料表，如下圖所示。

    ![Alter column SQL 陳述式產生](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter 資料行的 SQL 陳述式產生")

    *Alter column SQL 陳述式產生*
11. 在  **SQL Server 物件總管**，重新整理**dbo。TriviaQuestions**資料表，並檢查**提示**資料行類型是**nvarchar(150)**。

    ![顯示新的條件約束](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "顯示新的條件約束")

    *顯示新的條件約束*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>練習 2： 將 Web 應用程式部署至預備環境

**Web 應用程式在 Azure App Service 中的**可讓您執行的預備的發行。 預備的發行會建立一個預備網站位置，為每個預設生產網站，並可讓您交換這些位置不需要停機。 這是實際用於公開釋放之前，先驗證變更，以累加方式整合網站內容，並回復變更無法如預期運作。

在這個練習中，您將部署**Geek 測驗**到預備環境使用 Git 原始檔控制的 web 應用程式的應用程式。 若要這樣做，您會建立 web 應用程式和佈建所需的元件，在管理入口網站，請設定**Git**來源從本機電腦至預備位置的程式碼存放庫和推播應用程式。 您也會更新您的生產環境資料庫**Code First 移轉**您在前一個練習中建立。 您接著將此測試環境，以驗證其作業中執行應用程式。 當您滿意其運作根據您的預期，您將會升級到生產環境應用程式。

> [!NOTE]
> 若要啟用預備的發行，web 應用程式必須處於**標準模式**。 請注意，是否您變更為標準模式的 web 應用程式，將會產生額外費用。 如需有關定價的詳細資訊，請參閱 < [App Service 定價](https://azure.microsoft.com/pricing/details/app-service/)。


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>工作 1-在 Azure App Service 中建立 Web 應用程式

在這個工作中，您將建立的 web 應用程式**Azure App Service**從管理入口網站。 您也會設定**SQL Database**保存應用程式資料，並設定原始檔控制的本機 Git 存放庫。

1. 移至[Azure 管理入口網站](https://manage.windowsazure.com)並使用您的訂用帳戶相關聯的 Microsoft 帳戶登入。

    ![登入 Azure 管理入口網站](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *登入 Azure 管理入口網站*
2. 按一下 **新增**在頁面底部的命令列中。

    ![建立新的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "建立新的 web 應用程式")

    *建立新的 web 應用程式*
3. 按一下 **計算**，**網站**，然後**自訂建立**。

    ![建立新的 web 應用程式，使用 自訂建立](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "建立新的 web 應用程式，使用 自訂建立")

    *建立新的 web 應用程式，使用 自訂建立*
4. 在**新的網站-自訂建立**對話方塊方塊中，提供可用**URL** (例如*geek 測驗*)，選取中的位置**區域**下拉式清單中，然後選取**建立新的 SQL database**中**資料庫**下拉式清單。 最後，選取**從原始檔控制發行**核取方塊，然後按一下**下一步**。

    ![自訂新的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *自訂新的 web 應用程式*
5. 指定資料庫設定的下列資訊：

   - 在 **名稱**文字方塊中，輸入資料庫名稱 (例如*geekquiz\_db*)
   - 在伺服器中**下拉式清單**清單中，選取**新的 SQL database 伺服器**。 或者，您可以選取現有的伺服器。
   - 在 **資料庫使用者名稱**並**資料庫密碼**方塊中，輸入 SQL database 伺服器的系統管理員使用者名稱和密碼。 如果您選取的伺服器已經建立，您將會提示輸入密碼。

     ![指定資料庫設定](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *指定資料庫設定*
6. 按 [下一步]  以繼續。
7. 選取 **本機 Git 儲存機制**使用，並按一下 原始檔控制**下一步**。

    > [!NOTE]
    > 您可能會提示您的部署認證 （使用者名稱和密碼）。

    ![建立 Git 存放庫](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *建立 Git 存放庫*
8. 等候，直到建立新的 web 應用程式。

    > [!NOTE]
    > 根據預設，Azure 會提供網域*azurewebsites.net*同時也提供您設定使用 Azure 管理入口網站的自訂網域的可能性。 不過，您只可以管理自訂網域，如果您使用特定的 Azure App Service 模式。
    > 
    > Azure App Service 是免費、 共用、 基本、 標準和 Premium edition 提供。 在免費與共用模式中，所有的 web 應用程式會在多租用戶環境中執行，並有 CPU、 記憶體和網路使用量的配額。 免費的應用程式的最大數目與您計劃而有所不同。 在標準模式中，您可以選擇哪些應用程式在對應的專用虛擬機器上執行的標準 azure 計算資源。 您可以找到中的 web 應用程式模式組態**擴展**web 應用程式的功能表。
    > 
    > ![Azure App Service 模式](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service 模式")
    > 
    > 如果您使用**Shared**或是**標準**模式中，您將能夠移至您的應用程式中管理 web 應用程式的自訂網域**設定**功能表，然後按一下**管理網域**底下*網域名稱*。
    > 
    > ![管理網域](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "管理網域")
    > 
    > ![管理自訂網域](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "管理自訂網域")
9. Web 應用程式建立之後，請按一下下方的連結**URL**檢查新的 web 應用程式是否正在執行的資料行。

    ![瀏覽至新的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *瀏覽至新的 web 應用程式*

    ![執行 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *執行 web 應用程式*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>工作 2-建立生產環境 SQL 資料庫

在這個工作中，您將使用**Entity Framework Code First Migrations**若要建立的資料庫目標**Azure SQL Database**您在上一個工作中建立的執行個體。

1. 在管理入口網站中，瀏覽至您在上一個工作中建立 web 應用程式並移至其**儀表板**。
2. 在 **儀表板**頁面上，按一下**檢視連接字串**連結底下**快速概覽**一節。

    ![檢視連接字串](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "檢視連接字串")

    *檢視連接字串*
3. 複製**連接字串**值，並關閉對話方塊。

    ![在 Azure 管理入口網站的連接字串](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "在 Azure 管理入口網站的連接字串")

    *在 Azure 管理入口網站的連接字串*
4. 按一下  **SQL Database**若要查看 Azure 中的 SQL 資料庫清單

    ![SQL Database 功能表](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database 功能表")

    *SQL Database 功能表*
5. 找出您在上一個工作中建立的資料庫，並按一下 在伺服器上。

    ![SQL Database 伺服器](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database 伺服器")

    *SQL Database 伺服器*
6. 在 **快速入門**頁面上的伺服器上，按一下**設定**。

    ![設定 功能表](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "設定功能表")

    *設定功能表*
7. 在 **允許的 IP 位址**區段中，按一下**加入至允許的 IP 位址**連結以啟用您的 IP 連線到 SQL Database 伺服器。

    ![允許的 IP 位址](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "允許的 IP 位址")

    *允許的 IP 位址*
8. 按一下 **儲存**底部的頁面，即可完成的步驟。
9. 切換回到 Visual Studio。
10. 在  **Package Manager Console**，執行下列命令取代 *[您連接字串]* 預留位置取代為您從 Azure 複製的連接字串

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![更新目標 Windows Azure SQL Database 的資料庫](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "更新目標 Windows Azure SQL Database 的資料庫")

    *更新目標 Azure SQL Database 的資料庫*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>工作 3-部署到預備環境使用 Git 的玩家測驗

在這個工作中，您將 web 應用程式中啟用預備的發行。 然後，您將使用 Git 發行 Geek 測驗應用程式直接從您的本機電腦到您的 web 應用程式的預備環境。

1. 返回入口網站並按一下底下的 web 應用程式名稱**名稱**資料行來顯示 [管理] 頁面。

    ![開啟 web 應用程式管理頁面](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *開啟 web 應用程式管理頁面*
2. 瀏覽至**擴展**頁面。 底下**一般**區段中，選取**標準**的設定，然後按一下**儲存**命令列中。

    > [!NOTE]
    > 在目前的區域和訂用帳戶中的執行所有的 web 應用程式**標準**模式中，保持**全選**中所選取的核取方塊**選擇站台**組態。 否則，請清除**全選**核取方塊。

    ![升級為標準模式的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "升級為標準模式的 web 應用程式")

    *升級為標準模式的 Web 應用程式*
3. 按一下 **是**來確認變更。

    ![確認為標準模式的變更](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "繼續進行變更的 web 應用程式模式")

    *確認為標準模式的變更*
4. 移至**儀表板**頁面，然後按一下**啟用預備發行**之下**快速概覽**一節。

    ![啟用預備的發行](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "啟用預備發行")

    *啟用預備的發行*
5. 按一下 **是**才能啟用預備的發行。

    ![確認預備發行](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "按一下 [是] 啟用預備的發行")

    *確認預備的發行*
6. 在 web 應用程式清單中，展開左側的 您的 web 應用程式名稱，以顯示預備網站位置的標記。 它具有的名稱，後面加上的 web 應用程式 ***（預備）***。 按一下以移至 [管理] 頁面的預備網站。

    ![瀏覽至預備 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "瀏覽至預備 web 應用程式")

    *瀏覽至預備應用程式*
7. 請注意，他管理頁面看起來像任何其他 web 應用程式的管理頁面。 瀏覽至**部署**頁面，然後複製**Git URL**值。 您將會稍後在本練習中使用它。

    ![複製的 Git URL 值](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *複製的 Git URL 值*
8. 開啟新**Git Bash**主控台並執行下列命令。 更新 *[路徑-您的應用程式]* 預留位置取代為路徑**GeekQuiz**方案，位於**Source\Ex1 DeployingWebSiteToStaging\Begin**資料夾這個實驗室中。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git 初始化 」 和 「 第一次認可](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Git 初始化 」 和 「 第一次認可*
9. 執行下列命令，以將您的 web 應用程式推送至遠端**Git**存放庫。 預留位置取代為您從管理入口網站取得的 URL。 將提示您為您的部署密碼。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![推送至 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *推送至 Azure*

    > [!NOTE]
    > 當您將內容部署至的 FTP 主機或 GIT 儲存機制的 web 應用程式時，您必須使用進行驗證**部署認證**您從 web 應用程式的建立**快速入門**或**儀表板**管理頁面。 如果您不知道您的部署認證可以輕鬆地重設它們使用管理入口網站。 開啟 web 應用程式**儀表板**頁面，然後按一下**重設您的部署認證**連結。 提供新密碼，然後按**確定**。 是部署認證都適用於使用與您訂用帳戶相關聯的所有 web 應用程式。
10. 若要確認 web 應用程式已成功推送至 Azure，請回到管理入口網站，並按一下**網站**。
11. 找出您的 web 應用程式，然後展開以顯示預備網站位置的項目。 按一下它**名稱**移至 [管理] 頁面。
12. 按一下 **部署**若要查看**部署歷程記錄**。 確認沒有**現用部署**與您*&quot;初始認可&quot;*。

    ![作用中的部署](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *作用中的部署*
13. 最後，按一下**瀏覽**移至 web 應用程式的命令列中。

    ![瀏覽 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *瀏覽 web 應用程式*
14. 如果已成功部署應用程式，您會看到 Geek 測驗的 [登入] 頁面。

    > [!NOTE]
    > 所部署的應用程式的 URL 包含後面接著的 web 應用程式名稱 *-預備*。

    ![在預備環境中執行的應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *在預備環境中執行的應用程式*
15. 如果您想要探索應用程式，請按一下**註冊**註冊新的使用者。 輸入使用者名稱、 電子郵件地址和密碼，以完成帳戶詳細資料。 接下來，應用程式會顯示測驗的第一個問題。 回答一些問題，以確定它正常運作。

    ![已備妥可用的應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *已備妥可用的應用程式*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>工作 4-升級至生產環境 Web 應用程式

既然您已驗證的 web 應用程式在預備環境中正常運作，您就將其升階到生產環境。 在這個工作中，您將會交換預備網站位置與生產站台位置。

1. 返回管理入口網站，並選取 預備網站位置。 按一下 **交換**命令列中。

    ![切換至生產環境](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *切換至生產環境*
2. 按一下 **是**在確認對話方塊中，若要繼續進行交換作業。 Azure 會立即在交換生產網站的預備網站內容的內容。

    > [!NOTE]
    > 某些設定從預備的版本將自動複製到實際執行版本 （例如連接字串覆寫處理常式對應、 等），但其他設定不會變更 （例如 DNS 端點、 SSL 繫結等）。

    ![確認交換作業](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *確認交換作業*
3. 交換完成後，選取 生產位置，然後按一下**瀏覽**若要開啟 將生產站台的命令列中。 請注意網址列中的 URL。

    > [!NOTE]
    > 您可能需要重新整理瀏覽器來清除快取。 在 Internet Explorer 中，您可以藉由按下**CTRL + R**。

    ![在生產環境中執行的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. 在  **GitBash**主控台中，更新本機 Git 存放庫為目標的生產位置的遠端 URL。 若要這樣做，請執行下列命令預留位置取代為您的部署使用者名稱和 web 應用程式的名稱。

    > [!NOTE]
    > 在下列練習中，您會將變更推送到生產網站，而不是只為了簡單起見實驗室的預備環境。 在真實世界案例中，建議將升階到生產環境之前，請先驗證預備環境中的變更。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>練習 3： 在生產環境中執行部署復原

其中您沒有預備位置來執行熱插拔之間預備和生產環境，例如，如果您正在使用的情況下**空閒**或是**共用**模式。 在這些情況下，您應該測試您的應用程式，在測試環境中 – 在本機或遠端站台中 – 之前部署至生產環境。 不過，就可以在測試階段偵測不到的問題可能會發生生產網站中。 在此情況下，務必要有機制，以輕鬆地切換至先前且更穩定版本的應用程式儘速。

在  **Azure App Service**，從原始檔控制的連續部署讓此可能感謝**重新部署**管理入口網站中可用動作。 Azure 會追蹤與推送至儲存機制的認可相關聯的部署，並提供選項來重新部署您的應用程式使用任何先前的部署中，在任何時間。

在這個練習中，您將執行中的程式碼變更**Geek 測驗**刻意插入的應用程式*bug*。 您將部署到生產環境應用程式，以查看錯誤，並接著您會利用重新部署功能，請回到先前的狀態。

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>工作 1-Geek 測驗應用程式更新

在這個工作中，重構程式碼的一小段**TriviaController**類別來擷取從資料庫擷取選取的測驗選項，為新方法的邏輯的一部分。

1. 切換至 Visual Studio 執行個體**GeekQuiz**從上一個練習的解決方案。
2. 在 [**方案總管] 中**，開啟**TriviaController.cs**檔案**控制器**資料夾。
3. 找出**StoreAsync**方法，然後選取程式碼以在下圖中反白顯示。

    ![選取的程式碼](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *選取的程式碼*
4. 以滑鼠右鍵按一下選取的程式碼中，展開**重構**功能表，然後選取**擷取方法...**.

    ![擷取程式碼做為新的方法](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *選取 擷取方法*
5. 在 [**擷取方法**] 對話方塊中，名稱的新方法*MatchesOption*然後按一下 **[確定]**。

    ![指定方法名稱](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *指定擷取方法的名稱*
6. 選取的程式碼接著會擷取至**MatchesOption**方法。 產生的程式碼是以下列程式碼片段所示。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. 按下**CTRL + S**以儲存變更。

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>工作 2-重新部署 Geek 測驗應用程式

您現在會推送至儲存機制，將會觸發新的部署到生產環境所做先前工作中的變更。 然後，您將在其中進行問題使用**F12 開發工具**提供 Internet Explorer 中，再回復到先前的部署從 Azure 管理入口網站。

1. 開啟新**Git Bash**主控台部署至 Azure App Service 更新的應用程式。
2. 執行下列命令，以將變更推送至 Azure。 更新 *[路徑-您的應用程式]* 預留位置取代為路徑**GeekQuiz**解決方案。 將提示您為您的部署密碼。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![將重構程式碼推送至 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *將重構程式碼推送至 Azure*
3. 開啟 Internet Explorer 並瀏覽至您的 web 應用程式 (例如`http://<your-web-site>.azurewebsites.net`)。 使用先前建立的認證登入。
4. 按下**F12**若要啟動的開發工具，選取**網路**索引標籤，然後按一下**播放**開始錄製 按鈕。

    ![啟動網路錄製](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "啟動網路記錄")

    *啟動網路記錄*
5. 選取任何選項的測驗。 您會看到沒有任何反應。
6. 在 [ **F12** ] 視窗中，對應至 POST HTTP 要求的項目會顯示 HTTP **500**結果。

    ![HTTP 500 錯誤](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 錯誤*
7. 選取 [**主控台**] 索引標籤。錯誤會記錄原因的詳細資料。

    ![記錄的錯誤](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *記錄的錯誤*
8. 找出錯誤的詳細資料部分。 很明顯地，此錯誤是重構前一個步驟中認可您的程式碼所造成。

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. 請勿關閉瀏覽器。
10. 在新的瀏覽器執行個體中，瀏覽至[Azure 管理入口網站](https://manage.windowsazure.com)並使用您的訂用帳戶相關聯的 Microsoft 帳戶登入。
11. 選取 **網站**按一下您在練習 2 中建立 web 應用程式。
12. 瀏覽至**部署**頁面。 請注意，執行的所有認可會都列出在部署歷程記錄中。

    ![現有部署的清單](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *現有部署的清單*
13. 選取 上一次認可，然後按一下**重新部署**命令列上。

    ![重新部署先前的認可](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *重新部署先前的認可*
14. 當系統提示您確認時，按一下**是**。

    ![確認重新部署](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. 當部署完成後時，切換回瀏覽器執行個體，與您的 web 應用程式，然後按**CTRL + F5**。
16. 按一下任何一個選項。 翻頁動畫的動畫現在需要的地方和結果 (*正確/不正確*) 將會顯示。
17. （選擇性）若要切換**Git Bash**主控台並執行下列命令來還原成先前的認可。

    > [!NOTE]
    > 這些命令會建立新的認可，復原 Git 存放庫中的不正確的認可所做的所有變更。 Azure 會再重新部署應用程式使用新的認可。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>使用 Azure 儲存體縮放練習 4:

**Blob**是最簡單的方式來儲存大量非結構化的文字或二進位資料如視訊、 音訊和影像。 移至儲存體的應用程式的靜態內容，可協助調整應用程式所提供的映像或直接到瀏覽器的文件。

在此練習中，您會將移至 Blob 容器的應用程式的靜態內容。 然後您將設定您的應用程式，以新增**ASP.NET URL 重寫規則**中**Web.config**重新導向至 Blob 容器的內容。

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>工作 1-建立 Azure 儲存體帳戶

在這項工作中，您將了解如何建立新的儲存體帳戶，使用管理入口網站。

1. 瀏覽至[Azure 管理入口網站](https://manage.windowsazure.com)並使用您的訂用帳戶相關聯的 Microsoft 帳戶登入。
2. 選取**新 |資料服務 |儲存體 |快速建立**開始建立新的儲存體帳戶。 輸入唯一的名稱，為該帳戶，然後選取**地區**從清單中。 按一下 **建立儲存體帳戶**以繼續。

    ![建立新的儲存體帳戶](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "建立新的儲存體帳戶")

    *建立新的儲存體帳戶*
3. 在 **儲存體**區段中，等到新的儲存體帳戶的狀態會變更為*線上*才能繼續進行下一個步驟。

    ![建立儲存體帳戶](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "建立儲存體帳戶")

    *建立儲存體帳戶*
4. 按一下儲存體帳戶名稱，然後按一下**儀表板**在頁面頂端的連結。 **儀表板**頁面可提供您的帳戶，並且可用於您的應用程式的服務端點的狀態相關資訊。

    ![顯示儲存體帳戶儀表板](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "顯示儲存體帳戶儀表板")

    *顯示儲存體帳戶儀表板*
5. 按一下 **管理存取金鑰**導覽列中的按鈕。

    ![管理存取金鑰 按鈕](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "管理存取金鑰 按鈕")

    *管理存取金鑰 按鈕*
6. 在**管理存取金鑰** 對話方塊中，複製**儲存體帳戶名稱**並**主要存取金鑰**當您將需要在下列練習中。 然後，關閉對話方塊。

    ![管理存取金鑰 對話方塊](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "管理存取金鑰 對話方塊")

    *管理存取金鑰 對話方塊*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>工作 2-將資產上傳至 Azure Blob 儲存體

在這個工作中，您將使用從 Visual Studio 的 [伺服器總管] 視窗來連接到您的儲存體帳戶。 然後，您會建立 blob 容器，並 Geek 測驗標誌檔案上傳至容器。

1. 切換至 Visual Studio 執行個體**GeekQuiz**從上一個練習的解決方案。
2. 從功能表列中，選取**檢視**，然後按一下**伺服器總管**。
3. 在 **伺服器總管**，以滑鼠右鍵按一下**Azure**節點，然後選取**連線到 Azure...**.使用您的訂用帳戶相關聯的 Microsoft 帳戶登入。

    ![連接到 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *連接到 Azure*
4. 依序展開**Azure**節點，以滑鼠右鍵按一下**儲存體**，然後選取**附加外部儲存體...**.
5. 在**加入新的儲存體帳戶**對話方塊方塊中，輸入**帳戶名稱**並**帳戶金鑰**您在上一個工作並按一下 取得**確定**.

    ![加入新的儲存體帳戶對話方塊](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *加入新的儲存體帳戶對話方塊*
6. 您的儲存體帳戶應該會出現下**儲存體**節點。 展開您的儲存體帳戶，以滑鼠右鍵按一下**Blob** ，然後選取**建立 Blob 容器...**.

    ![建立 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "建立 Blob 容器")

    *建立 Blob 容器*
7. 在 **建立 Blob 容器**對話方塊方塊中，輸入 blob 容器的名稱，然後按一下**確定**。

    ![建立 Blob 容器 對話方塊](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "建立 Blob 容器 對話方塊")

    *建立 Blob 容器 對話方塊*
8. 應加入新的 blob 容器**Blob**節點。 變更要將容器設為公用容器中的存取權限。 若要這樣做，請以滑鼠右鍵按一下**映像**容器，然後選取**屬性**。

    ![映像容器屬性](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "映像容器屬性")

    *映像容器屬性*
9. 在 **屬性**視窗中，將**公用讀取權限**來**容器**。

    ![變更公用讀取權限屬性](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "變更公用讀取權限屬性")

    *變更公用讀取權限屬性*
10. 當系統提示您是否確定要變更的公用存取屬性，請按一下**是**。

    ![Microsoft Visual Studio 警告](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio 警告")

    *Microsoft Visual Studio 警告*
11. 在 **伺服器總管**，以滑鼠右鍵按一下**映像**blob 容器，然後選取**檢視 Blob 容器**。

    ![檢視 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "檢視 Blob 容器")

    *檢視 Blob 容器*
12. 映像容器應該會開啟新視窗中，應該會顯示任何項目的圖例。 按一下 **上傳**圖示，以將檔案上傳至 blob 容器。

    ![無項目的映像容器](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "無項目的映像容器")

    *無項目的映像容器*
13. 在 **上傳 Blob**對話方塊方塊中，瀏覽至**資產**實驗室的資料夾。 選取 **標誌 big.png**檔案，然後按一下**開啟**。
14. 請等到檔案上傳。 上傳完成時，檔案應該列在 映像容器。 以滑鼠右鍵按一下檔案項目，然後選取**複製 URL**。

    ![複製 blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "複製 blob 檔案的 URL")

    *複製 blob URL*
15. 開啟 Internet Explorer，並貼上 URL。 下列映像應該會顯示在瀏覽器中。

    ![從 Windows Blob 儲存體的標誌 big.png 映像](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "標誌 big.png 映像從儲存體")

    *從 Azure Blob 儲存體的標誌 big.png 映像*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>工作 3-更新方案，以使用來自 Azure Blob 儲存體的靜態內容

在這個工作中，您會設定**GeekQuiz**解決方案，以使用映像上傳至 Azure Blob 儲存體 （而不是位於 web 應用程式中的映像） 新增中的 ASP.NET URL 重寫規則**web.config**檔案。

1. 在 Visual Studio 中開啟**Web.config**檔案內**GeekQuiz**專案，然後找出**&lt;system.webServer&gt;** 項目。
2. 加入下列程式碼，將 URL 重寫規則，更新您的儲存體帳戶名稱的預留位置。

    (程式碼片段- *WebSitesInProduction-Ex4-UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > URL 重寫為攔截傳入的 Web 要求，並將要求重新導向至不同的資源的程序。 URL 重寫規則會告訴重寫引擎要求時必須重新導向，以及其中應該將他們重新導向。 重寫規則由兩個字串所組成： 在要求 URL 中尋找的模式 （通常使用規則運算式），並找到要取代的模式，如果字串。 如需詳細資訊，請參閱 < [URL 以 ASP.NET 重寫](https://msdn.microsoft.com/library/ms972974.aspx)。
3. 按下**CTRL + S**以儲存變更。
4. 開啟新**Git Bash**主控台部署至 Azure App Service 更新的應用程式。
5. 執行下列命令，以將變更推送至 Azure。 更新 *[路徑-您的應用程式]* 預留位置取代為路徑**GeekQuiz**解決方案。 將提示您為您的部署密碼。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![將更新部署至 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *將更新部署至 Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>工作 4-驗證

在這項工作會使用**Internet Explorer**瀏覽**Geek 測驗**應用程式，並檢查 URL 重寫規則的映像運作，您會被重新導向至映像裝載在**Azure Blob儲存體**。

1. 開啟 Internet Explorer 並瀏覽至您的 web 應用程式 (例如`http://<your-web-site>.azurewebsites.net`)。 使用先前建立的認證登入。

    ![顯示玩家測驗 web 應用程式與映像](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "顯示玩家測驗 web 應用程式與映像")

    *顯示玩家測驗 web 應用程式與映像*
2. 按下**F12**若要啟動的開發工具，選取**網路**索引標籤，並開始錄製。

    ![啟動網路錄製](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "啟動網路記錄")

    *啟動網路記錄*
3. 按下**CTRL + F5**重新整理網頁。
4. 網頁完成載入之後，您應該會看到的 HTTP 要求 **/img/logo-big.png** URL 含有 HTTP **301**結果 （重新導向） 和另一個要求`http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`URL 含有 HTTP **200**結果。

    ![正在驗證的 URL 重新導向](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "開發人員工具中顯示重新導向")

    *正在驗證重新導向 URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>練習 5： 使用自動調整規模的 Web 應用程式

> [!NOTE]
> 此練習是選擇性的因為它需要支援的 Web 負載&amp;效能測試，僅適用於**Visual Studio 2013 Ultimate Edition**。 如需有關 Visual Studio 2013 的特定功能的詳細資訊，請比較版本[此處](https://www.microsoft.com/visualstudio/eng/products/compare)。


**Azure App Service Web Apps**中執行的 web 應用程式提供的自動調整功能**標準模式**。 自動調整可讓 Azure 自動調整您的 web 應用程式，根據負載的執行個體計數。 啟用自動調整時，Azure 會每 5 分鐘會檢查您的 web 應用程式的 CPU，並將執行個體，視需要在該時間點的。 如果 CPU 使用率偏低，Azure 將移除執行個體一次每隔 2 小時以確保您的 web 應用程式的效能不會降低。

在這個練習中，您會瀏覽設定所需的步驟**自動調整規模**功能**Geek 測驗**web 應用程式。 您將會執行產生足夠的 CPU 負載，應用程式觸發程序的執行個體升級到 Visual Studio 負載測試來驗證這項功能。

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>工作 1-設定 CPU 度量為基礎的自動調整規模

在這項工作中，您將使用 Azure 管理入口網站來啟用自動調整功能，以便您在練習 2 中建立 web 應用程式。

1. 在  [Azure 管理入口網站](https://manage.windowsazure.com/)，選取**網站**按一下您在練習 2 中建立 web 應用程式。
2. 瀏覽至**擴展**頁面。 底下**容量**區段中，選取**CPU** for**依度量調整規模**組態。

    > [!NOTE]
    > 調整時的 CPU，Azure 會動態調整應用程式使用的 CPU 使用量變更時的執行個體數目。

    ![選取即可依 CPU 調整規模](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "選取 CPU 計量自動調整")

    *選取即可依 CPU 調整規模*
3. 變更**目標 CPU**組態**20**-**40**百分比。

    > [!NOTE]
    > 這個範圍代表您的 web 應用程式的平均 CPU 使用量。 Azure 將會新增或移除執行個體，將您的 web 應用程式保留在這個範圍。 中指定用於調整的執行個體最小和最大數目**執行個體計數**組態。 Azure 永遠不會高於或超過該限制。
    > 
    > 預設值**目標 CPU**值只會修改這個實驗室的目的。 藉由設定 CPU 範圍值較小，您會增加以觸發自動調整的機會中, 度的負載放在應用程式時。

    ![變更目標會介於 20 到 40%的 CPU](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "變更會介於 20 到 40%的目標 CPU")

    *變更會介於 20 到 40%的目標 CPU*
4. 按一下 **儲存**在命令列中儲存的變更。

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>工作 2-使用 Visual Studio 負載測試

既然**自動調整規模**已設定，您將建立**Web 效能和負載測試專案**在 Visual Studio 中產生某些 web 應用程式上的 CPU 負載。

1. 開啟**Visual Studio Ultimate 2013** ，然後選取**檔案 |新 |專案...** 啟動新的解決方案。

    ![建立新的專案](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "建立新的專案")

    *建立新的專案*
2. 在 [**新的專案**對話方塊中，選取**Web 效能和負載測試專案**下**Visual C# |測試**] 索引標籤。請確定 **.NET Framework 4.5**是選取，將專案命名為*WebAndLoadTestProject*，選擇**位置**然後按一下**確定**。

    ![建立新的 Web 和負載測試專案](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "建立新的 Web 和負載測試專案")

    *建立新的 Web 和負載測試專案*
3. 在  **WebTest1.webtest**上按一下滑鼠右鍵**WebTest1**節點，然後按一下**加入要求**。

    ![將要求加入至 WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1 中加入要求")

    *將要求加入至 WebTest1*
4. 在 [**屬性**] 視窗的 [新要求] 節點中，更新**Url**屬性以指向您的 web 應用程式的 URL (例如*[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![變更 Url 屬性](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "變更 Url 屬性")

    *變更 Url 屬性*
5. 在  **WebTest1.webtest**  視窗中，以滑鼠右鍵按一下**WebTest1** ，按一下 **加入迴圈...**.

    ![將迴圈加入至 WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "將迴圈加入至 WebTest1")

    *將迴圈加入至 WebTest1*
6. 在 **新增的條件式規則和項目迴圈**對話方塊中，選取**For 迴圈**規則，並修改下列屬性。

   1. **終止值：** 1000年
   2. **內容參數名稱：** 迭代器
   3. **遞增值：** 1

      ![選取 For 迴圈規則並更新內容](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "選取 For 迴圈規則並更新內容")

      *選取 For 迴圈規則並更新內容*
7. 底下**迴圈中的項目**區段中，選取您要在迴圈的第一個和最後一個項目先前建立的要求。 按一下 [確定]  繼續操作。

    ![選取此迴圈的第一個和最後一個項目](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "選取迴圈的第一個和最後一個項目")

    *選取此迴圈的第一個和最後一個項目*
8. 在 [**方案總管] 中**，以滑鼠右鍵按一下**WebAndLoadTestProject**專案中，展開**新增**功能表，然後選取**負載測試...**.

    ![WebAndLoadTestProject 專案中加入負載測試](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "WebAndLoadTestProject 專案中加入負載測試")

    *WebAndLoadTestProject 專案中加入負載測試*
9. 在 **新增負載測試精靈** 對話方塊中，按一下**下一步**。

    ![新增負載測試精靈](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "新增負載測試精靈")

    *新增負載測試精靈*
10. 在 [**案例**頁面上，選取**不使用考慮時間**然後按一下**下一步]**。

    ![選取不想使用考慮時間](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "選取不想使用考慮時間")

    *選擇不使用考慮時間*
11. 在 **負載模式**頁面上，請確定**常數負載**選項。 變更**使用者計數**設為**250**使用者，然後按一下 [**下一步]**。

    ![變更使用者計數設為 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "變更的使用者計數設為 250")

    *變更為 250 的使用者計數*
12. 在 [**測試混合模型**頁面上，選取**依據循序測試順序**然後按一下**下一步]**。

    ![選取的測試混合模型](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "選取的測試混合模型")

    *選取的測試混合模型*
13. 在 **測試混合模型**頁面上，按一下**加入...** 將測試混合。

    ![將測試加入至測試混合](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "將測試加入至測試混合")

    *將測試加入至測試混合*
14. 在 [**新增測試**] 對話方塊中，按兩下**WebTest1**若要加入至測試**選取的測試**清單。 按一下 [確定]  繼續操作。

    ![加入 WebTest1 測試](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "加入 WebTest1 測試")

    *加入 WebTest1 測試*
15. 回到**測試混合**頁面上，按一下**下一步**。

    ![完成測試混合 頁面](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "完成測試混合 頁面")

    *完成測試混合 頁面*
16. 在 [**網路混合**頁面上，按一下**下一步]**。

    ![按一下 [網路混合] 頁面中的下一個](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "按一下網路混合 頁面中的下一個")

    *網路混合 頁面中按一下 下一步*
17. 在 [**瀏覽器混合**頁面上，選取**Internet Explorer 10.0**作為瀏覽器類型，然後按一下**下一步]**。

    ![選取 瀏覽器類型](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "選取瀏覽器類型")

    *選取 瀏覽器類型*
18. 在 **計數器集合**頁面上，按一下**下一步**。

    ![在 計數器集合 頁面中按一下 下一步](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "按一下 下一步 在 計數器集合 頁面")

    *在 [計數器集合] 頁面中按一下 [下一步]*
19. 在 **回合設定**頁面上，將**負載測試持續期間**來**5 分鐘**，按一下 **完成**。

    ![設定負載測試持續期間為 5 分鐘](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "設定負載測試持續期間為 5 分鐘")

    *設定負載測試持續期間為 5 分鐘*
20. 在 [**方案總管] 中**，按兩下**Local.settings**来探索的測試設定檔案。 根據預設，Visual Studio 會使用您的本機電腦來執行測試。

    > [!NOTE]
    > 或者，您可以設定測試專案，以在雲端中使用執行負載測試**Visual Studio Online (VSO)**。 VSO 提供雲端式負載測試會模擬更真實的負載的服務，避免本機環境的條件約束，例如 CPU 容量、 可用記憶體和網路頻寬。 如需使用 VSO 執行負載測試的詳細資訊，請參閱[這篇文章](https://www.visualstudio.com/get-started/load-test-your-app-vs)。

    ![測試設定](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>工作 3-自動調整規模的驗證

現在，您會執行您在上一個工作中建立負載測試，看您的 web 應用程式在負載下的運作方式。

1. 在 **方案總管**，按兩下**LoadTest1.loadtest**開啟負載測試。

    ![開啟 LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "開啟 LoadTest1.loadtest")

    *開啟 LoadTest1.loadtest*
2. 在 [ **LoadTest1.loadtest** ] 視窗中，按一下 [執行負載測試工具箱] 中的第一個按鈕。

    ![執行負載測試](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "執行負載測試")

    *執行負載測試*
3. 等候完成的負載測試。

    > [!NOTE]
    > 負載測試模擬多位使用者同時將要求傳送至 web 應用程式。 當測試執行時，您可以監視可用的計數器，來偵測任何錯誤、 警告或其他與負載測試回合相關的資訊。

    ![負載測試執行](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "等待，直到負載測試完成")

    *執行負載測試*
4. 測試完成後，返回管理入口網站，並瀏覽至**擴展**web 應用程式的頁面。 底下**容量** 區段中，您應該會看到在圖形中的新執行個體已自動部署。

    ![自動部署的新執行個體](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *自動部署的新執行個體*

    > [!NOTE]
    > 可能需要幾分鐘的時間才會出現在圖形中的變更 (請按**CTRL + F5**定期重新整理頁面)。 如果看不到任何變更，您可以嘗試下列各項：
    > 
    > - 增加的負載測試持續時間 (例如，新增到**10 分鐘**)
    > - 降低的最大和最小值**目標 CPU** web 應用程式的自動調整規模設定中的範圍
    > - 使用在雲端中執行負載測試**Visual Studio Online**。 更多資訊[這裡](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

在這個實際操作實驗室中，您已了解如何設定及部署您的生產 web 應用程式在 Azure 中的應用程式。 您開始透過偵測並更新您使用的資料庫**Entity Framework Code First Migrations**，然後繼續執行部署的站台使用的新版本**Git**以及執行回復您的站台的最新穩定版本。 此外，您已了解如何調整您的應用程式使用儲存體，將您的靜態內容移至 Blob 容器。
