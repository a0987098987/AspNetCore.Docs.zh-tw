---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Scaffold 和移轉 |Microsoft Docs
author: rick-anderson
description: 如果您已熟悉 ASP.NET MVC 4 控制器方法，或已完成&quot;Helper、 表單和驗證&quot;實際操作實驗室中，您應該注意...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814114"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework Scaffold 和移轉

藉由[Web Camp 小組](https://twitter.com/webcamps)

[下載 Web 研討會訓練套件](https://aka.ms/webcamps-training-kit)

如果您已熟悉 ASP.NET MVC 4 控制器方法，或已完成&quot;Helper、 表單和驗證&quot;實際操作實驗室中，您應該注意許多邏輯，以建立、 更新、 列出及移除它會重複任何資料實體在應用程式。 更何況，如果您的模型有幾個類別，操作，您將有可能需要花費相當長的時間撰寫每個實體作業，以及每個檢視的 POST 與 GET 動作方法。

在這個實驗室中，您將學習如何使用 ASP.NET MVC 4 scaffolding 自動產生應用程式的 CRUD （建立、 讀取、 更新和刪除） 的基準線。 從開始從簡單的模型類別，並不需要撰寫一行程式碼，您將建立一個控制站，會包含所有 CRUD 作業，以及所有必要的檢視。 在建置及執行簡單的解決方案之後，您必須產生的 MVC 邏輯和檢視資料操作的應用程式資料庫。

此外，您將學習使用 Entity Framework 移轉至執行在整個應用程式中的模型更新是多麼容易。 Entity Framework 移轉可讓您的模型已變更利用簡單的步驟之後，請修改您的資料庫。 所有這些考量，您將能夠建置及維護 web 應用程式更有效率地利用 ASP.NET MVC 4 的最新功能。

> [!NOTE]
> Web 研討會訓練套件中，在可從包含所有的範例程式碼和程式碼片段[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[ASP.NET MVC 4 Entity Framework Scaffold 和移轉](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations)。

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 使用 ASP.NET scaffolding 的控制器中的 CRUD 作業。
- 將使用 Entity Framework 移轉的資料庫模型的變更。

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目，即可完成此實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 A](#AppendixA)如需有關如何安裝它)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

下列練習中，組成此實際操作實驗室：

1. [使用 ASP.NET MVC 4 Scaffolding，Entity Framework 移轉與](#Exercise1)

> [!NOTE]
> 此練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。 如果您需要其他說明逐步練習，您可以使用此解決方案作為指南。


估計的時間才能完成這個實驗室： **30 分鐘**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>練習 1： 使用 ASP.NET MVC 4 Scaffolding，Entity Framework 移轉與

ASP.NET MVC scaffolding 提供快速的方式，以產生標準化的方式，建立所需的邏輯，可讓您的應用程式與資料庫層級互動的 CRUD 作業。

在此練習中，您將學習如何使用 ASP.NET MVC 4 的 scaffold 程式碼第一次建立 CRUD 方法。 然後，您將了解如何更新您的模型來使用 Entity Framework 移轉套用在資料庫中的變更。

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>工作 1-建立新的 ASP.NET MVC 4 專案使用 Scaffolding

1. 如果尚未開啟，啟動**Visual Studio 2012**。
2. 選取**檔案 |新的專案**。 在 [新專案] 對話方塊中，在**Visual C# |Web**區段中，選取**ASP.NET MVC 4 Web 應用程式**。 若要將專案命名**MVC4andEFMigrations**並將位置設定為**Source\Ex1 UsingMVC4ScaffoldingEFMigrations**本實驗室的資料夾。 設定**方案名稱**要**開始**，並確保**為方案建立目錄**已核取。 按一下 [確定 **Deploying Office Solutions**]。

    ![新 ASP.NET MVC 4 專案 對話方塊](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "新 ASP.NET MVC 4 專案 對話方塊")

    *新 ASP.NET MVC 4 專案 對話方塊*
3. 在 [**新的 ASP.NET MVC 4 專案**] 對話方塊中選取**網際網路應用程式**範本，並確定**Razor**是選取**檢視引擎**. 按一下 [確定] 建立專案。

    ![新的 ASP.NET MVC 4 網際網路應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "新的 ASP.NET MVC 4 網際網路應用程式")

    *新的 ASP.NET MVC 4 網際網路應用程式*
4. 在 [方案總管] 中，以滑鼠右鍵按一下**模型**，然後選取**新增 |類別**建立簡單的類別，個人 (POCO)。 命名**Person** ，按一下 **確定**。
5. 開啟 Person 類別，並插入下列屬性。

    (程式碼片段- *ASP.NET MVC 4 和 Entity Framework 移轉 Ex1 人員屬性*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. 按一下 **建置 |建置解決方案**儲存所做的變更，並建置專案。

    ![建置應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "建置應用程式")

    *建置應用程式*
7. 在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增 |控制器**。
8. 控制器*PersonController*並完成**Scaffolding 選項**具有下列值。

   1. 在 **樣板**下拉式清單中，選取**讀取/寫入動作和檢視、 使用 Entity Framework 的 MVC 控制器**選項。
   2. 在 **模型類別**下拉式清單中，選取**人員**類別。
   3. 在 **的資料內容類別**清單中，選取**&lt;新資料內容...&gt;**. 選擇任何名稱，然後按一下**確定**。
   4. 在 **檢視**下拉式清單中，請確定**Razor**已選取。

      ![新增人員控制站，使用 scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "新增 scaffolding 人員控制器")

      *新增 scaffolding 人員控制器*
9. 按一下 **新增**使用 scaffolding 建立人員的新控制器。 您現在已經產生控制器動作，以及檢視。

    ![使用 scaffolding 建立人員控制站之後](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "之後使用 scaffolding 建立人員控制站")

    *使用 scaffolding 建立人員控制站之後*
10. 開啟**PersonController**類別。 請注意有自動產生完整的 CRUD 動作方法。

   ![內部人員控制器](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "內部人員控制站")

   *內部人員控制站*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>工作 2-執行應用程式

此時，資料庫尚未建立。 在這個工作中，您將會執行第一次應用程式，並測試的 CRUD 作業。 將使用 Code First 立即建立資料庫。

1. 按 **F5** 執行應用程式。
2. 在瀏覽器中，加入 **/Person**至 URL，以開啟 [人員] 頁面。

    ![第一次執行的應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "第一次執行的應用程式")

    *第一次執行應用程式：*
3. 現在，您將探索人員頁面，以及測試的 CRUD 作業。

    1. 按一下 **新建**來新增新的人員。 輸入的名字和姓氏，然後按一下**建立**。

        ![加入新的個人](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "新增新的人員")

        *加入新的人員*
    2. 在連絡人的清單中，您可以刪除、 編輯或新增項目。

        ![人員清單](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "人員清單")

        *人員清單*
    3. 按一下 **詳細資料**開啟連絡人的詳細資料。

        ![人員詳細資料](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "人員詳細資料")

        *人員詳細資料*
4. 關閉瀏覽器，並返回 Visual Studio。 請注意您已建立完整的 CRUD 員 實體的整個應用程式-從檢視模型-而不需要撰寫一行程式碼 ！

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>工作 3-更新使用 Entity Framework 移轉資料庫

在這項工作中，您將會更新使用 Entity Framework 移轉的資料庫。 您會發現若要變更模型，並使用 Entity Framework 移轉功能，以反映您的資料庫中的變更是多麼容易。

1. 開啟 [Package Manager Console]。 選取**工具 |程式庫套件管理員 |套件管理員主控台**。
2. 在套件管理員主控台中，輸入下列命令：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![啟用移轉](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "啟用移轉")

    *啟用移轉*

    啟用移轉命令會建立**移轉**資料夾，其中包含指令碼來初始化資料庫。

    ![Migrations 資料夾](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations 資料夾")

    *Migrations 資料夾*
3. 開啟**Configuration.cs**移轉資料夾中的檔案。 尋找類別建構函式，並變更**AutomaticMigrationsEnabled**值 *，則為 true*。

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. 開啟 Person 類別並新增連絡人的中間名的屬性。 利用此新的屬性，您要變更模型。

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. 選取**建置 |建置解決方案**在建置應用程式 功能表上。

    ![建置應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "建置應用程式")

    *建置應用程式*
6. 在套件管理員主控台中，輸入下列命令：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    此命令會尋找資料物件中的變更，然後，它將據此修改資料庫所需的命令。

    ![新增中間名](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "新增中間名")

    *新增中間名*
7. （選擇性）您可以執行下列命令來產生 SQL 指令碼的差異更新。 這可讓您以手動方式更新資料庫 （在此情況下不需要），或套用其他資料庫中的變更：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![產生 SQL 指令碼](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "產生 SQL 指令碼")

    *產生 SQL 指令碼*

    ![SQL 指令碼更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL 指令碼更新")

    *SQL 指令碼更新*
8. 在 [套件管理員] 主控台中，輸入下列命令以更新資料庫：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![更新資料庫](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "更新資料庫")

    *更新資料庫*

    這會新增**MiddleName**中的資料行**人**表格以比對目前的定義**人員**類別。
9. 資料庫更新之後，以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增 |控制器**加入人員控制器一次 （使用相同的值完成）。 這會更新現有的方法和檢視表加入新的屬性。

    ![新增控制器更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "新增控制器更新")

    *更新控制器*
10. 按一下 [加入] 。 然後，選取 值**覆寫 PersonController.cs**而**覆寫相關聯的檢視**，按一下 **確定**。

   ![新增控制器覆寫](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *更新控制器*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4-執行應用程式

1. 按 **F5** 執行應用程式。
2. 開啟 **/Person**。 請注意，已保留資料，而中間名資料行加入。

    ![新增的中間名](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "新增的中間名")

    *新增的中間名*
3. 如果您按一下**編輯**，您將能夠加入目前使用者的中間名。

    ![中間名 edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "中間名版本")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

在這個實際操作實驗室中，您已了解簡單的步驟來建立與使用任何模型類別的 ASP.NET MVC 4 Scaffolding 的 CRUD 作業。 然後，您已了解如何利用 Entity Framework 移轉您的應用程式-從檢視的資料庫-執行端對端更新。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。
2. 按一下 **立即安裝**。 如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。

    ![接受授權條款](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *接受授權條款*
5. 等候完成的下載與安裝程序。

    ![安裝進度](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *安裝進度*
6. 安裝完成時，按一下**完成**。

    ![安裝已完成](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *安裝已完成*
7. 按一下 **結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。

    ![VS Express for Web 圖格](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express for Web 圖格*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>附錄 b： 使用程式碼片段

使用程式碼片段，您會有您需要在隨手可得的所有程式碼。 實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")

*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*

***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始輸入程式碼片段名稱 （不含空格或連字號）。
3. 觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始鍵入程式碼片段名稱](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "開始輸入程式碼片段名稱")

*開始鍵入程式碼片段名稱*

![按 Tab 鍵以選取反白顯示的程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵以選取反白顯示的程式碼片段*

![再次按 Tab 鍵和程式碼片段會依序展開](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "再次按 Tab 鍵和程式碼片段會展開")

*再次按 Tab 鍵和程式碼片段會展開*

***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取 **插入程式碼片段**後面**My Code Snippets**。
2. 選擇相關的程式片段，從清單中，對它按一下。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![對它按一下挑選清單中，相關程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "對它按一下挑選清單中，相關程式碼片段")

*對它按一下挑選清單中，相關程式碼片段*
