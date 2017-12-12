---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: "ASP.NET MVC 4 Entity Framework Scaffolding 和移轉 |Microsoft 文件"
author: rick-anderson
description: "如果您已熟悉 ASP.NET MVC 4 控制器方法，或已完成&quot;Helper、 表單和驗證&quot;實際操作實驗室中，您應該注意..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 15db1589eb90739458b430c35cea38e93e3dec5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework Scaffolding 和移轉
====================
由[Web 營小組](https://twitter.com/webcamps)

> 如果您已熟悉 ASP.NET MVC 4 控制器方法，或已完成&quot;Helper、 表單和驗證&quot;實際操作實驗室中，您應該注意許多邏輯，以建立、 更新、 列出及移除它會重複任何資料實體在應用程式。 更別說，如果您的模型有數個類別以管理，您將會有可能要花很長的時間，寫入每個實體作業，以及每個檢視的 POST 與 GET 動作方法。
> 
> 在這個實驗室中，您將學習如何使用 ASP.NET MVC 4 scaffolding 自動產生應用程式的 CRUD （建立、 讀取、 更新和刪除） 的基準線。 從開始從簡單的模型類別，並不需要撰寫一行程式碼，您將建立一個控制站，會包含所有 CRUD 作業，以及所有必要的檢視。 在建置及執行簡單的解決方案之後，您必須產生，以及 MVC 邏輯和資料管理檢視的應用程式資料庫。
> 
> 此外，您將學習使用 Entity Framework 移轉執行整個應用程式模型更新是多麼的輕鬆。 Entity Framework 移轉可讓您的模型已變更執行簡單的步驟之後，修改您的資料庫。 進行所有這些動作記住，您將能夠建立及維護的 web 應用程式更有效率地運用 ASP.NET MVC 4 的最新的功能。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將學會如何：

- 使用 ASP.NET scaffolding 控制站的 CRUD 作業。
- 變更使用 Entity Framework 移轉的資料庫模型。

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目才能完成本實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段**.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

下列練習構成這個實際操作實驗室：

1. [使用 ASP.NET MVC 4 Scaffolding 與 Entity Framework 移轉](#Exercise1)

> [!NOTE]
> 這個練習會伴隨著**結束**包含所產生的解決方案，您必須先完成本練習之後取得資料夾。 如果您需要其他協助逐步練習，您可以使用這個方案做為指南。


完成本實驗室估計時間： **30 分鐘**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>練習 1： 使用 ASP.NET MVC 4 Scaffolding 與 Entity Framework 移轉

ASP.NET MVC scaffolding 提供快速的方法來產生 CRUD 作業的標準化方式，建立必要的邏輯，可讓您的應用程式與資料庫層級互動。

在此練習中，您將學習如何使用 ASP.NET MVC 4 scaffolding 程式碼第一次建立 CRUD 方法。 然後，您將學習如何更新資料庫中的變更套用使用 Entity Framework 移轉您的模型。

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>工作 1-建立新的 ASP.NET MVC 4 專案使用 Scaffolding

1. 如果尚未開啟，啟動**Visual Studio 2012**。
2. 選取**檔案 |新的專案**。 在 [新增專案] 對話方塊中，在**Visual C# |Web**區段中，選取**ASP.NET MVC 4 Web 應用程式**。 若要將專案命名**MVC4andEFMigrations**並將位置設定為**Source\Ex1 UsingMVC4ScaffoldingEFMigrations**本實驗室的資料夾。 設定**方案名稱**至**開始**，並確定**為方案建立目錄**已核取。 按一下 [確定]。

    ![新 ASP.NET MVC 4 專案 對話方塊](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "新 ASP.NET MVC 4 專案 對話方塊")

    *新增 ASP.NET MVC 4 專案對話方塊*
3. 在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**範本，並確定**Razor**是所選**檢視引擎**. 按一下**確定**建立專案。

    ![新的 ASP.NET MVC 4 網際網路應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "新 ASP.NET MVC 4 網際網路應用程式")

    *新的 ASP.NET MVC 4 網際網路應用程式*
4. 在 [方案總管] 中，以滑鼠右鍵按一下**模型**選取**新增 |類別**建立簡單的類別個人 (POCO)。 命名**人員**按一下**確定**。
5. 開啟 Person 類別，並插入下列屬性。

    (程式碼片段- *ASP.NET MVC 4 和實體架構移轉 Ex1 人員屬性*)


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. 按一下**建置 |建置方案**以儲存變更，並建置專案。

    ![建置應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "建置應用程式")

    *建置應用程式*
7. 在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增 |控制器**。
8. 控制器*PersonController*並完成**Scaffolding 選項**具有下列值。

    1. 在**範本**下拉式清單中，選取**具有讀取/寫入動作和檢視表、 使用 Entity Framework 的 MVC 控制器**選項。
    2. 在**模型類別**下拉式清單中，選取**人員**類別。
    3. 在**資料內容類別**清單中，選取**&lt;新的資料內容...&gt;**. 選擇的任何名稱，然後按一下**確定**。
    4. 在**檢視**下拉式清單中，請確定**Razor**已選取。

    ![新增人員控制器的 scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "加入 scaffolding 人員控制器")

    *新增人員控制器的 scaffolding*
9. 按一下**新增**scaffolding 人員建立新的控制站。 現在您已經有產生控制器動作，以及檢視。

    ![使用 scaffolding 建立人員控制站之後](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "使用 scaffolding 建立人員控制站之後")

    *使用 scaffolding 建立人員控制站之後*
10. 開啟**PersonController**類別。 請注意，已自動產生完整的 CRUD 動作方法。

    ![內部的人員控制站](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "內人員控制站")

    *內部的人員控制站*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>工作 2-執行應用程式

此時，資料庫尚未建立。 在這項工作，您將會執行第一次應用程式，並測試 CRUD 作業。 使用 Code First 立即將會建立此資料庫。

1. 按 **F5** 執行應用程式。
2. 在瀏覽器中，加入**/Person**至 URL，以開啟 [人員] 頁面。

    ![第一次執行的應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "第一次執行的應用程式")

    *第一次執行應用程式：*
3. 現在，您會瀏覽的人員頁面，並測試 CRUD 作業。

    1. 按一下**新建**来加入新的人員。 輸入的名字和姓氏，然後按一下**建立**。

        ![加入新的個人](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "加入新的人員")

        *加入新的人員*
    2. 在清單中的人員，您可以刪除、 編輯或加入項目。

        ![人員清單](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "人員清單")

        *人員清單*
    3. 按一下**詳細資料**以開啟該人員的詳細資料。

        ![人員的詳細資料](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "人員的詳細資料")

        *人員的詳細資料*
4. 關閉瀏覽器，並返回 Visual Studio。 請注意您已建立整個 CRUD 員 實體的整個來源來檢視模型-應用程式而不需要撰寫一行程式碼 ！

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>工作 3-更新使用 Entity Framework 移轉資料庫

在這項工作中，您將更新使用 Entity Framework 移轉的資料庫。 您會發現若要變更模型，並使用 Entity Framework 的移轉功能，以反映您的資料庫中的變更是多麼的輕鬆。

1. 開啟 封裝管理員主控台。 選取**工具 |程式庫套件管理員 |Package Manager Console**。
2. 在 Package Manager Console 中，輸入下列命令：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![啟用移轉](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "啟用移轉")

    *啟用移轉*

    啟用移轉命令會建立**移轉**資料夾，其中包含指令碼來初始化資料庫。

    ![Migrations 資料夾](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations 資料夾")

    *Migrations 資料夾*
3. 開啟**configuration.cs 中**Migrations 資料夾中的檔案。 尋找類別建構函式並將變更**AutomaticMigrationsEnabled**值設定為*true*。


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. 開啟 Person 類別並加入個人的中間名的屬性。 利用此新屬性，您要變更模型。


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. 選取**建置 |建置方案**上建置應用程式的功能表。

    ![建置應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "建置應用程式")

    *建置應用程式*
6. 在 Package Manager Console 中，輸入下列命令：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    此命令會尋找資料物件中的變更，然後，它會在其中加入必要的命令，據此修改資料庫。

    ![加入 「 中間名](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "加入中間名")

    *加入 「 中間名*
7. （選擇性）您可以執行下列命令來產生 SQL 指令碼有差異的更新。 這可讓您手動更新資料庫 （在此情況下有不必要），或適用於其他資料庫中的變更：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![產生 SQL 指令碼](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "產生 SQL 指令碼")

    *產生 SQL 指令碼*

    ![SQL 指令碼更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL 指令碼更新")

    *SQL 指令碼更新*
8. 在 Package Manager Console 中，輸入下列命令以更新資料庫：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![更新資料庫](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "更新資料庫")

    *更新資料庫*

    這會新增**MiddleName**中的資料行**人員**表格以比對目前的定義**人員**類別。
9. 資料庫的更新，以滑鼠右鍵按一下 [控制器] 資料夾，並選取**新增 |控制器**加入人員控制器再次 （完成相同的值）。 這會更新現有的方法和檢視表加入新的屬性。

    ![加入控制器更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "加入控制器更新")

    *更新控制器*
10. 按一下 [加入] 。 然後，選取 值**覆寫 PersonController.cs**和**覆寫相關聯的檢視**按一下**確定**。

    ![加入控制器覆寫](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    *更新控制器*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4-執行應用程式

1. 按 **F5** 執行應用程式。
2. 開啟**/Person**。 請注意，已保留資料，而中間名資料行已加入。

    ![加入的中間名](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "加入的中間名")

    *加入的中間名*
3. 如果您按一下**編輯**，您將能夠加入至目前的人員的中間名。

    ![中間名 edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "中間名版本")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

在這個實際操作實驗室中，您已經學會簡單的步驟來建立 ASP.NET MVC 4 Scaffolding 使用任何模型類別 CRUD 作業。 然後，您已經學會如何使用 Entity Framework 移轉您的應用程式層來檢視資料庫-執行的端對端的更新。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . 下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; *Visual Studio Express 2012 for Web 與 Windows Azure SDK*&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。

    ![VS Express for Web 方塊](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express for Web 方塊*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>附錄 b： 使用程式碼片段

程式碼片段，您會有您需要在您可以的所有程式碼。 實驗室文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")

*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*

***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始鍵入片段名稱 （不含空格或連字號）。
3. 觀察 IntelliSense 會顯示比對程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始輸入程式碼片段名稱](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "開始鍵入片段名稱")

*開始輸入程式碼片段名稱*

![按 Tab 鍵選取反白顯示的程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵選取反白顯示的程式碼片段*

![按 Tab 鍵一次程式碼片段就會擴展](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "按 Tab 鍵一次程式碼片段就會擴展")

*按 Tab 鍵一次程式碼片段就會擴展*

***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取**插入程式碼片段**後面**我的程式碼片段**。
2. 透過在按一下挑選清單中，從相關的程式碼片段。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![透過在按一下挑選清單中，從相關的程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "上它即可挑選清單中，從相關的程式碼片段")

*透過在按一下挑選清單中，從相關的程式碼片段*
