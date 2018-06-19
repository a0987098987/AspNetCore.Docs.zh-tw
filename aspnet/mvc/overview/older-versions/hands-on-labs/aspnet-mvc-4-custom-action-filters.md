---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 自訂動作篩選條件 |Microsoft 文件
author: rick-anderson
description: ASP.NET MVC 提供動作篩選條件之前或之後呼叫動作方法執行篩選邏輯。 動作篩選條件是自訂屬性 tha...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877706"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 自訂動作篩選條件

由[Web 營小組](https://twitter.com/webcamps)

[下載 Web 營訓練套件](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 提供動作篩選條件之前或之後呼叫動作方法執行篩選邏輯。 動作篩選條件是自訂屬性，提供將動作前和動作後的行為加入控制器動作方法的宣告式方法。

在這個實際操作實驗室中，您將建立自訂動作篩選條件屬性到 MvcMusicStore 解決方案來攔截控制器的要求，並記錄在站台到資料庫資料表的活動。 您可以將記錄篩選的資料隱碼加入至任何控制器或動作。 最後，您會看到記錄檢視，顯示造訪者的清單。

這個實際操作實驗室假設您擁有的基本知識**ASP.NET MVC**。 如果您不使用**ASP.NET MVC**之前，我們建議您在一段**ASP.NET MVC 4 的基本概念**實際操作實驗室。

> [!NOTE]
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可從在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[ASP.NET MVC 4 自訂動作篩選條件](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters)。

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將學會如何：

- 建立自訂動作篩選條件屬性，來擴充篩選的功能
- 自訂的篩選條件屬性套用至特定層級的資料隱碼
- 全域註冊自訂動作篩選條件

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目才能完成本實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 c： 使用程式碼片段](#AppendixC)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室是由下列練習所組成：

1. [練習 1： 記錄動作](#Exercise1)
2. [練習 2： 管理多個動作篩選條件](#Exercise2)

完成本實驗室估計時間： **30 分鐘**。

> [!NOTE]
> 每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。 如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>練習 1： 記錄動作

在此練習中，您將學習如何使用 ASP.NET MVC 4 篩選條件提供者建立自訂動作記錄的篩選條件。 針對該目的您會將記錄篩選器套用至選取控制站會記錄所有活動的 MusicStore 站台。

篩選會擴充**ActionFilterAttributeClass**並覆寫**OnActionExecuting**攔截每個要求，然後執行 記錄動作方法。 HTTP 要求的內容資訊中，執行方法、 結果和參數將由 ASP.NET MVC 提供**ActionExecutingContext**類別 **。**

> [!NOTE]
> ASP.NET MVC 4 也有預設篩選器提供者，您可以使用而建立自訂篩選器。 ASP.NET MVC 4 提供下列類型的篩選器：
> 
> - **授權**篩選，讓安全性決策，決定是否要執行的動作方法，例如執行驗證或驗證要求的屬性。
> - **動作**包裝動作方法執行篩選條件。 此篩選器可以執行額外處理，例如提供動作方法額外的資料、 檢查傳回的值，或取消執行的動作方法
> - **結果**包裝 ActionResult 物件執行篩選條件。 此篩選器可以執行額外處理的結果，例如修改 HTTP 回應。
> - **例外狀況**篩選器，它會執行處理的動作方法，授權篩選條件的開始和結束與結果的執行中的某處擲回的例外狀況時。 例外狀況篩選條件可以用於工作，例如記錄或顯示錯誤頁面。
> 
> 如需篩選器提供者的詳細資訊，請造訪此 MSDN 連結: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx))。


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>有關 MVC 音樂市集應用程式記錄功能

此 Music Store 方案中有新的資料模型資料表的網站記錄**ActionLog**，使用下列欄位： 接收要求、 呼叫動作、 用戶端 IP 和時間戳記的控制器名稱。

![資料模型。ActionLog 資料表。](aspnet-mvc-4-custom-action-filters/_static/image1.png "資料模型。ActionLog 資料表。")

*資料模型-ActionLog 資料表*

解決方案提供動作記錄檔可以找到的 ASP.NET MVC 檢視**MvcMusicStores/檢視表/ActionLog**:

![動作記錄檔檢視](aspnet-mvc-4-custom-action-filters/_static/image2.png "動作記錄檔檢視")

*動作記錄檔檢視*

與此提供結構的所有工作，將都著重於插斷控制器的要求及使用自訂篩選執行記錄中。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>工作 1-建立自訂篩選器，以攔截控制器的要求

在這項工作中，您將建立自訂篩選條件屬性類別將包含記錄邏輯。 針對該目的，您將延伸 ASP.NET MVC **Actionfilterattribut**類別和實作的介面**IActionFilter**。

> [!NOTE]
> **Actionfilterattribut**是所有屬性篩選條件的基底類別。 它提供下列方法來執行特定邏輯之後，以及控制器動作執行之前：
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): 此動作之前呼叫方法。
> - **OnActionExecuted**(ActionExecutedContext filterContext): 呼叫動作方法之後之前的結果執行 （在之前檢視呈現）。
> - **OnResultExecuting**(ResultExecutingContext filterContext): 之前的結果執行 （在之前檢視呈現）。
> - **OnResultExecuted**(ResultExecutedContext filterContext): （在呈現檢視） 之後的結果執行之後。
> 
> 由衍生類別覆寫任何一種方法時，您可以執行您自己的篩選程式碼。


1. 開啟**開始**方案位於**\Source\Ex01-LoggingActions\Begin**資料夾。

   1. 您必須下載某些遺漏的 NuGet 封裝，然後再繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
      > 
      > 如需詳細資訊，請參閱這篇文章： [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。
2. 加入新 C# 類別到**篩選**資料夾並將其命名*CustomActionFilter.cs*。 這個資料夾會儲存所有自訂篩選器。
3. 開啟**CustomActionFilter.cs**並加入參考**System.Web.Mvc**和**MvcMusicStore.Models**命名空間：

    (程式碼片段- *ASP.NET MVC 4 自訂動作篩選條件-Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. 繼承**CustomActionFilter**類別從**Actionfilterattribut**然後進行**CustomActionFilter**類別實作**IActionFilter**介面。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. 請**CustomActionFilter**類別覆寫方法**OnActionExecuting**並加入所需的邏輯記錄的篩選條件的執行。 若要這樣做，請將加入下列反白顯示的程式碼內**CustomActionFilter**類別。

    (程式碼片段- *ASP.NET MVC 4 自訂動作篩選條件-Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting**的方法使用**Entity Framework**来加入新的 ActionLog 暫存器。 它會建立並填入新的實體執行個體的內容資訊**filterContext**。
    > 
    > 您可以深入了解**ControllerContext**類別在[msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx)。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>工作 2-插入的程式碼攔截到存放區控制器類別

在這項工作中，您會加入自訂篩選器插入所有控制器類別和控制器的動作會登入。 基於此練習中，目的存放區控制器類別會有記錄檔。

此方法**OnActionExecuting**從**ActionLogFilterAttribute**呼叫插入的項目時要執行自訂篩選器。

它也可攔截特定控制器方法。

1. 開啟**StoreController**在**MvcMusicStore\Controllers**並加入參考**篩選**命名空間：

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. 插入自訂篩選器**CustomActionFilter**到**StoreController**類別藉由新增 **[CustomActionFilter]** 類別宣告之前的屬性。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > 當篩選條件會插入控制器類別時，也插入它的動作。 如果您想要將篩選套用到只針對一組動作，您必須將 **[CustomActionFilter]** 到其中的每一個：
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>工作 3-執行應用程式

在這項工作，您將測試正在記錄篩選器。 您將會啟動應用程式，並瀏覽市集，則您將會檢查記錄的活動。

1. 按 **F5** 執行應用程式。
2. 瀏覽至 **/ActionLog**以查看記錄檔檢視的初始狀態：

    ![記錄之前呼叫 」 活動的追蹤器狀態](aspnet-mvc-4-custom-action-filters/_static/image3.png "記錄之前呼叫 」 活動的追蹤器狀態")

    *記錄之前呼叫 」 活動的追蹤器狀態*

   > [!NOTE]
   > 根據預設，它會永遠顯示一個項目擷取現有的內容類型功能表時所產生。
   > 
   > 為了簡單起見我們正在清理**ActionLog**資料表每次應用程式執行時，讓它只會顯示每個特定工作的驗證的記錄檔。
   > 
   > 您可能需要移除下列程式碼從**工作階段\_啟動**方法 (在**Global.asax**類別)，以便儲存在存放區中執行的所有動作的歷程記錄控制站。
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. 按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。
4. 瀏覽至 **/ActionLog** ，而且如果記錄檔是空的按**F5**重新整理頁面。 追蹤您造訪的檢查：

    ![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image4.png "動作記錄檔與記錄活動")

    *動作記錄檔與記錄活動*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>練習 2： 管理多個動作篩選條件

在這個練習中您會將第二個的自訂動作篩選條件加入至 StoreController 類別，並定義特定的順序執行兩個篩選條件。 然後，您將會更新全域註冊篩選條件的程式碼。

沒有定義的篩選條件執行順序時，請考慮不同的選項。 例如，Order 屬性和篩選的範圍：

您可以定義**範圍**針對每個篩選條件，比方說，您無法限制範圍內執行的所有動作篩選條件**控制器都範圍**，和執行中的所有授權篩選條件**全域都範圍**. 範圍有已定義的執行順序。

此外，每個動作篩選條件具有順序屬性用來判斷篩選器的範圍中的執行順序。

如需自訂動作篩選條件執行順序的詳細資訊，請瀏覽 MSDN 上的本文: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx))。

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>工作 1： 建立新的自訂動作篩選條件

在這個工作中，您將建立新的自訂動作篩選條件，將插入 StoreController 類別中，了解如何管理篩選器的執行順序。

1. 開啟**開始**方案位於**\Source\Ex02-ManagingMultipleActionFilters\Begin**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

    1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
    2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
    3. 最後，按一下 建置方案**建置** | **建置方案**。

        > [!NOTE]
        > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
        > 
        > 如需詳細資訊，請參閱這篇文章： [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。
2. 加入新 C# 類別到**篩選**資料夾並將其命名*MyNewCustomActionFilter.cs*
3. 開啟**MyNewCustomActionFilter.cs**並加入參考**System.Web.Mvc**和**MvcMusicStore.Models**命名空間：

    (程式碼片段- *ASP.NET MVC 4 自訂動作篩選條件-Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. 預設類別宣告替換成下列程式碼。

    (程式碼片段- *ASP.NET MVC 4 自訂動作篩選條件-Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > 此自訂動作篩選條件是您在上一個練習中建立一個幾乎相同的。 主要差異在於它有*&quot;記錄由&quot;* 更新這個新類別的名稱來找出根據篩選器屬性登錄記錄。

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>工作 2： 將新的程式碼攔截器插入 StoreController 類別

在這項工作，您會將新的自訂篩選器加入至 StoreController 類別，並執行方案來驗證這兩種篩選器一起運作。

1. 開啟**StoreController**類別位於**MvcMusicStore\Controllers** ，以及將新的自訂篩選器**MyNewCustomActionFilter**到**StoreController**之類類別會顯示下列程式碼。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. 現在，執行應用程式，以便查看這些兩個自訂動作篩選條件的正常運作。 若要這樣做，請按**F5**並等待應用程式啟動。
3. 瀏覽至 **/ActionLog**以查看記錄檔檢視的初始狀態。

    ![記錄之前呼叫 」 活動的追蹤器狀態](aspnet-mvc-4-custom-action-filters/_static/image5.png "記錄之前呼叫 」 活動的追蹤器狀態")

    *記錄之前呼叫 」 活動的追蹤器狀態*
4. 按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。
5. 請檢查此時間;追蹤您造訪兩次： 一旦您在每個自訂動作篩選條件加入**StorageController**類別。

    ![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image6.png "動作記錄檔與記錄活動")

    *動作記錄檔與記錄活動*
6. 關閉瀏覽器。

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>工作 3： 管理篩選器順序

在這項工作，您將學習如何管理使用順序屬性設定的篩選條件執行順序。

1. 開啟**StoreController**類別位於**MvcMusicStore\Controllers**並指定**順序**喜歡這兩個篩選中的屬性如下所示。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. 現在，請確認篩選其順序屬性的值而定的執行方式。 您會發現具有的最小的順序值的篩選條件 (**CustomActionFilter**) 執行的第一個。 按**F5**並等待應用程式啟動。
3. 瀏覽至 **/ActionLog**以查看記錄檔檢視的初始狀態。

    ![記錄之前呼叫 」 活動的追蹤器狀態](aspnet-mvc-4-custom-action-filters/_static/image7.png "記錄之前呼叫 」 活動的追蹤器狀態")

    *記錄之前呼叫 」 活動的追蹤器狀態*
4. 按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。
5. 核取，此時，您瀏覽追蹤依篩選條件的順序值： **CustomActionFilter**記錄檔的第一次。

    ![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image8.png "動作記錄檔與記錄活動")

    *動作記錄檔與記錄活動*
6. 現在，您將更新的篩選條件的順序值，並確認變更的記錄順序的方式。 在**StoreController**類別中，更新的篩選條件的順序值，例如如下所示。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. 按一次執行應用程式**F5**。
8. 按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。
9. 請檢查這個時間，記錄檔所建立的**MyNewCustomActionFilter**篩選器會先出現。

    ![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image9.png "動作記錄檔與記錄活動")

    *動作記錄檔與記錄活動*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>工作 4： 註冊全域篩選

在這項工作，您將更新方案以註冊新的篩選器 (**MyNewCustomActionFilter**) 做為全域的篩選條件。 如此一來，就會觸發的所有動作執行應用程式中並不只是在 StoreController 項目與前一項工作。

1. 在**StoreController**類別中，移除 **[MyNewCustomActionFilter]** 屬性和 [順序] 屬性從 **[CustomActionFilter]**。 它看起來應該如下所示：

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. 開啟**Global.asax**檔案，並尋找**應用程式\_啟動**方法。 請注意，每次應用程式啟動時，它藉由呼叫註冊全域篩選**RegisterGlobalFilters**方法內**FilterConfig**類別。

    ![在 Global.asax 中註冊全域篩選](aspnet-mvc-4-custom-action-filters/_static/image10.png "在 Global.asax 中註冊全域篩選")

    *在 Global.asax 中註冊全域篩選*
3. 開啟**FilterConfig.cs**檔案內**應用程式\_啟動**資料夾。
4. 將參考加入使用 system.web.mvc 的參考。使用 MvcMusicStore.Filters;命名空間。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. 更新**RegisterGlobalFilters**方法加入您的自訂篩選條件。 若要這樣做，請將反白顯示的程式碼：

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. 執行應用程式按**F5**。
7. 按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。
8. 核取現在 **[MyNewCustomActionFilter]** 媵檔晻 HomeController 和 ActionLogController 太。

    ![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image11.png "動作記錄檔與記錄活動")

    *動作記錄檔記錄的全域活動*

> [!NOTE]
> 此外，您可以部署此應用程式以 Windows Azure Web Sites 下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室中，您已經學會如何擴充動作篩選條件執行自訂動作。 您也已經學會如何插入頁面控制器的任何篩選條件。 使用下列概念：

- 如何建立自訂動作篩選條件與 ASP.NET MVC Actionfilterattribut 類別
- 如何將篩選器插入至 ASP.NET MVC 控制器
- 如何管理篩選器順序使用 Order 屬性
- 如何註冊全域篩選

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。

    ![VS Express for Web 方塊](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *VS Express for Web 方塊*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy

此附錄將說明如何從 Windows Azure 管理入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>工作 1-建立新的網站，從 Windows Azure 入口網站

1. 移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。

    > [!NOTE]
    > 與 Windows Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。 您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。

    ![登入 Windows Azure 入口網站](aspnet-mvc-4-custom-action-filters/_static/image17.png "登入 Windows Azure 入口網站")

    *登入 Windows Azure 管理入口網站*
2. 按一下**新增**命令列上。

    ![建立新的網站](aspnet-mvc-4-custom-action-filters/_static/image18.png "建立新的網站")

    *建立新的網站*
3. 按一下**計算** | **網站**。 然後選取**快速建立**選項。 新的網站上提供可用的 URL，然後按一下**建立網站**。

    > [!NOTE]
    > Windows Azure 網站是您可以控制和管理在雲端中執行的 web 應用程式的主機。 快速建立 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。 它不包含設定資料庫的步驟。

    ![建立新的網站使用 快速建立](aspnet-mvc-4-custom-action-filters/_static/image19.png "建立新的網站使用 快速建立")

    *建立新的網站使用 快速建立*
4. 等候新**網站**建立。
5. 建立網站之後按一下底下的連結**URL**資料行。 請檢查新的網站運作。

    ![瀏覽至新的網站](aspnet-mvc-4-custom-action-filters/_static/image20.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行網站](aspnet-mvc-4-custom-action-filters/_static/image21.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。

    ![開啟網站管理頁面](aspnet-mvc-4-custom-action-filters/_static/image22.png "開啟網站管理頁面")

    *開啟網站管理頁面*
7. 在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有 web 應用程式發行至 Windows Azure 網站的每個已啟用的發行方法所需的資訊。 發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Windows Azure 網站。

    ![下載網站發行設定檔](aspnet-mvc-4-custom-action-filters/_static/image23.png "下載網站發行設定檔")

    *下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何使用這個檔案從 Visual Studio 將 web 應用程式至 Windows Azure 網站發行。

    ![儲存發行設定檔](aspnet-mvc-4-custom-action-filters/_static/image24.png "儲存發行設定檔")

    *儲存發行設定檔*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Windows Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器儀表板**。 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。 並未建立資料庫，因為它會在後續階段中建立。

    ![SQL Database 伺服器儀表板](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取 [IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊，然後按一下![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) ] 按鈕。

    ![加入用戶端 IP 位址](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *加入用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *確認變更*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 方案。 在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。

    ![發行應用程式](aspnet-mvc-4-custom-action-filters/_static/image29.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作中。

    ![匯入發行設定檔](aspnet-mvc-4-custom-action-filters/_static/image30.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下**驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連線](aspnet-mvc-4-custom-action-filters/_static/image31.png "驗證連線")

    *驗證連接*
4. 在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署設定](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連接，如下所示：

   - 在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:* 前置詞。
   - 在**使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在**密碼**輸入伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱。

     ![設定目的地連接字串](aspnet-mvc-4-custom-action-filters/_static/image33.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫依序按一下**是**。

    ![建立資料庫](aspnet-mvc-4-custom-action-filters/_static/image34.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "連接字串指向 SQL 資料庫")

    *連接字串指向 SQL 資料庫*
8. 在**預覽**頁面上，按一下**發行**。

    ![Web 應用程式發行](aspnet-mvc-4-custom-action-filters/_static/image36.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>附錄 c： 使用程式碼片段

程式碼片段，您會有您需要在您可以的所有程式碼。 實驗室文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](aspnet-mvc-4-custom-action-filters/_static/image37.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")

*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*

***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始鍵入片段名稱 （不含空格或連字號）。
3. 觀察 IntelliSense 會顯示比對程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始輸入程式碼片段名稱](aspnet-mvc-4-custom-action-filters/_static/image38.png "開始鍵入片段名稱")

*開始輸入程式碼片段名稱*

![按 Tab 鍵選取反白顯示的程式碼片段](aspnet-mvc-4-custom-action-filters/_static/image39.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵選取反白顯示的程式碼片段*

![按 Tab 鍵一次程式碼片段就會擴展](aspnet-mvc-4-custom-action-filters/_static/image40.png "按 Tab 鍵一次程式碼片段就會擴展")

*按 Tab 鍵一次程式碼片段就會擴展*

***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取**插入程式碼片段**後面**我的程式碼片段**。
2. 透過在按一下挑選清單中，從相關的程式碼片段。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-custom-action-filters/_static/image41.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![透過在按一下挑選清單中，從相關的程式碼片段](aspnet-mvc-4-custom-action-filters/_static/image42.png "上它即可挑選清單中，從相關的程式碼片段")

*透過在按一下挑選清單中，從相關的程式碼片段*
