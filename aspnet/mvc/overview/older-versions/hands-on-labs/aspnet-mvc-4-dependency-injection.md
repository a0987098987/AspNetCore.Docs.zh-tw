---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4 相依性插入 |Microsoft 文件
author: rick-anderson
description: 注意： 這個實際操作實驗室假設您擁有的 ASP.NET MVC 與 ASP.NET MVC 4 的篩選條件的基本知識。 如果您沒有使用 ASP.NET MVC 4 的篩選器之前，我們建議...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: e6c24d03039f0e6005948a73348589627c9df2df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877654"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4 相依性插入

由[Web 營小組](https://twitter.com/webcamps)

[下載 Web 營訓練套件](https://aka.ms/webcamps-training-kit)

這個實際操作實驗室假設您擁有的基本知識**ASP.NET MVC**和**ASP.NET MVC 4 篩選**。 如果您不使用**ASP.NET MVC 4 篩選**之前，我們建議您在一段**ASP.NET MVC 自訂動作篩選條件**實際操作實驗室。

> [!NOTE]
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可從在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[ASP.NET MVC 4 相依性插入](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection)。

在**物件導向程式設計**開發架構時，物件一起運作共同作業模型中有 contributors 和取用者。 當然，此通訊模式會產生物件和元件，變得難以管理複雜度增加之間的相依性。

![相依性的類別和型號複雜性](aspnet-mvc-4-dependency-injection/_static/image1.png "類別相依性，並建立模型的複雜度")

*類別相依性和模型的複雜度*

您可能聽過**原廠模式**和介面和實作使用服務、 用戶端物件通常負責服務位置之間的分離。

相依性插入模式為的逆轉控制的特定實作。 **逆轉控制 (IoC)** 表示物件不會建立其他物件，它們所依賴執行其工作。 相反地，他們會收到需要從外部來源 （例如 xml 組態檔） 的物件。

**相依性插入 (DI)** 表示這是由物件介入通常會將建構函式參數傳遞的 framework 元件並設定屬性。

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>相依性插入 (DI) 設計模式

在高層級，相依性插入的目標是，用戶端類別 (例如*高爾夫球*) 必須符合介面的項目 (例如*IClub*)。 它不在意具象型別為何 (例如*WoodClub，IronClub，WedgeClub*或*PutterClub*)，它想要其他人來處理這種情況 (例如更佳*caddy*)。 ASP.NET MVC 中的相依性解析程式可讓您註冊您的相依性邏輯其他地方 (例如容器或*梅花包*)。

![相依性插入圖表](aspnet-mvc-4-dependency-injection/_static/image2.png "相依性插入圖")

*相依性插入-包括業餘高爾夫比喻*

使用相依性插入模式和逆轉控制的優點如下：

- 減少結合類別
- 會增加重複使用程式碼
- 改善程式碼維護性
- 改善應用程式測試

> [!NOTE]
> 相依性插入有時與抽象 Factory 設計模式，但是沒有這兩種方法之間有些微的差異。 DI 已解決相依性呼叫處理站和已註冊的服務使用背後的架構。


既然您了解相依性插入模式時，您將學習在這個實驗室整個套用在 ASP.NET MVC 4 的方式。 您會開始使用中的相依性插入**控制器**来包含的資料庫存取服務。 接下來，您會將套用至相依性插入**檢視**来使用的服務，並顯示資訊。 最後，您就可以擴充 DI 與 ASP.NET MVC 4 的篩選條件，將方案中的自訂動作篩選條件。

在這個實際操作實驗室中，您將學會如何：

- 將 ASP.NET MVC 4 與 Unity 整合相依性插入使用 NuGet 封裝
- 使用 ASP.NET MVC 控制器內的相依性插入
- 使用 ASP.NET MVC 檢視內的相依性插入
- 使用 ASP.NET MVC 動作篩選條件內的相依性插入

> [!NOTE]
> 這個實驗室使用 Unity.Mvc3 NuGet 封裝相依性解析，但可以調整任何相依性插入架構，才能使用 ASP.NET MVC 4。


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

如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室是由下列練習所組成：

1. [練習 1： 插入控制器](#Exercise1)
2. [練習 2： 將，以便檢視](#Exercise2)
3. [練習 3： 將篩選器](#Exercise3)

> [!NOTE]
> 每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。 如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。


完成本實驗室估計時間： **30 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>練習 1： 插入控制器

在此練習中，您將學習如何使用 ASP.NET MVC 控制器中的相依性插入，方法是整合 Unity 使用 NuGet 封裝。 因此，您將服務納入，邏輯與資料存取分開您 MvcMusicStore 控制站。 服務會建立新的相依於控制器建構函式，會使用相依性插入的協助解決**Unity**。

這個方法會顯示如何產生較少結合的應用程式，也就是更有彈性且容易維護和測試。 您也將學習如何將 ASP.NET MVC 與 Unity 的整合。

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>關於 StoreManager 服務

現在開始方案中提供 MVC Music Store 包含服務，可管理存放區控制站資料名為**StoreService**。 您將在下面找到儲存區服務實作。 請注意，所有的方法會傳回模型的實體。

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController**從 begin 方案現在會消耗**StoreService**。 已從移除所有資料參考**StoreController**，現在可以修改目前的資料存取提供者，而不需要變更的任何方法，會消耗和**StoreService**。

您會發現在下方**StoreController**實作具有的相依性**StoreService**類別建構函式內。

> [!NOTE]
> 在這個練習中導入的相依性與**的逆轉控制**(IoC)。
> 
> **StoreController**類別建構函式收到**IStoreService**型別參數，請務必執行從類別內的服務呼叫。 不過， **StoreController**未實作預設建構函式 （不含參數） 的任何控制站必須具有使用 ASP.NET MVC。
> 
> 若要解析相依性，控制器具有建立抽象的處理站 （會傳回指定之任何的類型物件類別）。


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> 類別會嘗試建立 StoreController，而不傳送服務物件，因為沒有宣告沒有無參數建構函式時，您會收到錯誤。


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>工作 1-執行應用程式

在這個工作中，您將執行 Begin 應用程式，包括入存放區控制站與應用程式邏輯分隔資料存取服務。

當執行應用程式，您會收到例外狀況，控制器服務不會傳遞做為參數預設為：

1. 開啟**開始**方案位於**Source\Ex01 插入 Controller\Begin**。

   1. 您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 按**Ctrl + F5**執行應用程式，但不偵錯。 您會收到錯誤訊息&quot;**沒有為這個物件定義的無參數建構函式**&quot;:

    ![執行 ASP.NET MVC 開始應用程式時發生錯誤](aspnet-mvc-4-dependency-injection/_static/image3.png "執行 ASP.NET MVC 開始應用程式時發生錯誤")

    *執行 ASP.NET MVC 開始應用程式時發生錯誤*
3. 關閉瀏覽器。

下列步驟中，您可以使用 [音樂] 存放區方案，將此控制站需要的相依性。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>工作 2-包括 Unity MvcMusicStore 解決方案

在這項工作，您會將包含**Unity.Mvc3**至方案的 NuGet 封裝。

> [!NOTE]
> ASP.NET MVC 3，設計 Unity.Mvc3 封裝，但與 ASP.NET MVC 4 完全相容。
> 
> Unity 執行個體是選擇性支援的輕量型、 可延伸的相依性插入容器，然後輸入攔截。 它是任何類型的.NET 應用程式中使用的一般用途容器。 它提供包括相依性插入機制中找到的所有一般功能： 建立物件，藉由指定相依性，在執行階段和彈性，延後至容器的元件組態需求的抽象概念。


1. 安裝**Unity.Mvc3**中 NuGet 套件**MvcMusicStore**專案。 若要這樣做，請開啟**Package Manager Console**從**檢視** | **其他視窗**。
2. 執行下列命令。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![正在安裝 Unity.Mvc3 NuGet 封裝](aspnet-mvc-4-dependency-injection/_static/image4.png "安裝 Unity.Mvc3 NuGet 封裝")

    *正在安裝 Unity.Mvc3 NuGet 封裝*
3. 一次**Unity.Mvc3**安裝封裝、 瀏覽的檔案和資料夾，它會自動新增以簡化 Unity 組態。

    ![安裝 Unity.Mvc3 套件](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 封裝安裝")

    *Unity.Mvc3 封裝安裝*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>工作 3-Global.asax.cs 應用程式中註冊的 Unity\_開始

在這項工作，您將更新**應用程式\_啟動**方法位於**Global.asax.cs**呼叫 Unity 啟動載入器初始設定式，並接著，更新 啟動載入器檔案註冊服務和控制站將用於相依性插入。

1. 現在，您將連結啟動載入器即初始化 Unity 容器的檔案和相依性解析程式。 若要這樣做，請開啟**Global.asax.cs**並加入下列反白顯示的程式碼內**應用程式\_啟動**方法。

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex01-初始化 Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. 開啟**Bootstrapper.cs**檔案。
3. 包含下列命名空間： **MvcMusicStore.Services**和**MusicStore.Controllers**。

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex01-啟動載入器加入命名空間*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. 取代**BuildUnityContainer**方法的內容存放區控制站和存放服務註冊為下列程式碼。

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex01-暫存器儲存控制站及服務*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>工作 4-執行應用程式

在這個工作中，您將執行應用程式，它可以立即載入之後包括 Unity 的確認。

1. 按**F5**執行應用程式，應用程式應該立即載入而不會顯示任何錯誤訊息。

    ![執行應用程式相依性插入](aspnet-mvc-4-dependency-injection/_static/image6.png "執行具有相依性插入應用程式")

    *執行具有相依性插入應用程式*
2. 瀏覽至 **/儲存**。 這將會叫用**StoreController**，立即建立使用**Unity**。

    ![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")

    *MVC Music Store*
3. 關閉瀏覽器。

在下列練習中，您將學習如何擴充至 ASP.NET MVC 檢視和動作篩選條件內使用它的相依性插入範圍。

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>練習 2： 將，以便檢視

在此練習中，您將學習如何使用 Unity 整合檢視中的新功能的 ASP.NET MVC 4 具有相依性插入。 若要這樣做，您會呼叫存放區瀏覽 檢視中，就會顯示一則訊息以及圖內的自訂服務。

然後，您將會與 Unity 整合專案，並建立自訂的相依性解析程式，將相依性。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>工作 1-建立使用服務的檢視

在這項工作，您將建立會執行服務呼叫，以產生新的相依性的檢視。 服務包含簡單的傳訊服務包含在此解決方案中。

1. 開啟**開始**方案位於**Source\Ex02 插入 View\Begin**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
      > 
      > 如需詳細資訊，請參閱這篇文章： [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。
2. 包含**MessageService.cs**和**IMessageService.cs**類別位於**來源 \Assets**資料夾中的 **/Services**。 若要這樣做，請以滑鼠右鍵按一下**服務**資料夾，然後選取**加入現有項目**。 瀏覽至檔案的位置，並將它們包含。

    ![新增訊息服務和服務介面](aspnet-mvc-4-dependency-injection/_static/image8.png "加入訊息服務與服務介面")

    *新增訊息服務和服務介面*

    > [!NOTE]
    > **IMessageService**介面會定義兩個屬性實作**MessageService**類別。 這些屬性-**訊息**和**ImageUrl**-儲存要顯示的訊息和影像的 URL。
3. 建立資料夾 **/頁面**在專案的根資料夾，並將現有的類別**MyBasePage.cs**從**Source\Assets**。 您將會繼承自基底頁面具有下列結構。

    ![Pages 資料夾](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages 資料夾")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. 開啟**Browse.cshtml**檢視從 **/檢視表/存放區**資料夾，並使其繼承自**MyBasePage.cs**。

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. 在**瀏覽**檢視中，將呼叫加入**MessageService**来顯示的映像和服務所擷取的訊息。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>工作 2-包括自訂相依性解析程式和自訂檢視頁面啟動項

在先前工作中，您可以插入內執行服務呼叫內文中檢視新的相依性。 現在，您將會解決該相依性，藉由實作 ASP.NET MVC 相依性插入介面**IViewPageActivator**和**IDependencyResolver**。 您會將包含方案的實作**IDependencyResolver** ，將會處理服務擷取使用 Unity。 然後，您將會包含其他自訂實作**IViewPageActivator**都解決此問題的檢視建立的介面。

> [!NOTE]
> ASP.NET MVC 3，因為相依性插入的實作已簡化註冊服務的介面。 **IDependencyResolver**和**IViewPageActivator**屬於的 ASP.NET MVC 3 的功能相依性插入。
> 
> **-IDependencyResolver**介面取代先前 IMvcServiceLocator。 IDependencyResolver 的實作者必須傳回服務或服務集合的執行個體。
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator**介面會提供更細微的控制，透過檢視頁面如何透過相依性插入具現。 類別可實作**IViewPageActivator**介面可以建立使用內容資訊的檢視執行個體。
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. 建立 /**工廠**專案的根資料夾中的資料夾。
2. 包含**CustomViewPageActivator.cs**至您的方案從 **/來源/資產/** 至**工廠**資料夾。 若要這樣做，請以滑鼠右鍵按一下 **/Factories**資料夾中，選取**新增 |現有項目**，然後選取  **CustomViewPageActivator.cs**。 這個類別會實作**IViewPageActivator**來保存 Unity 容器介面。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator**負責管理檢視的建立使用 Unity 的容器。
3. 包含**UnityDependencyResolver.cs**檔案從 **/來源/資產**至 **/Factories**資料夾。 若要這樣做，請以滑鼠右鍵按一下 **/Factories**資料夾中，選取**新增 |現有項目**，然後選取  **UnityDependencyResolver.cs**檔案。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver**類別是針對 Unity 自訂 dependencyresolver 來註冊。 Unity 容器內找不到服務，被 invocated 基底的解析程式。

這兩種實作會在下列工作中註冊，讓模型知道服務和檢視表的位置。

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>工作 3-Unity 容器內註冊相依性插入

在這項工作，您會將放方式來產生工作的相依性插入的所有先前內容。

到目前為止您方案中有下列項目：

- A**瀏覽**檢視繼承自**MyBaseClass**並取用**MessageService**。
- 中繼類別-**MyBaseClass**-具有相依性插入宣告服務介面。
- 服務- **MessageService** -及其介面**IMessageService**。
- 自訂相依性解析程式的 Unity- **UnityDependencyResolver** -服務擷取的處理。
- 檢視頁面啟動項- **CustomViewPageActivator** -建立頁面。

若要插入**瀏覽** 檢視中，您會立即註冊自訂相依性解析程式 Unity 容器中。

1. 開啟**Bootstrapper.cs**檔案。
2. 註冊執行個體**MessageService**到 Unity 容器初始化服務：

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-暫存器訊息服務*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. 將參考加入**MvcMusicStore.Factories**命名空間。

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-Factory 命名空間*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. 註冊**CustomViewPageActivator** Unity 容器檢視頁面啟動項為：

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-註冊 CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. ASP.NET MVC 4 預設相依性解析程式取代的執行個體**UnityDependencyResolver**。 若要這樣做，請取代**Initialise**方法內容以下列程式碼：

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-更新相依性解析程式*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC 提供預設相依性解析程式類別。 若要使用自訂的相依性解析程式做為我們已經為 unity 建立，這個解析程式已被取代。

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>工作 4-執行應用程式

在這個工作中，您將執行的應用程式，以確認存放區的瀏覽器使用該服務，並顯示的影像和擷取的訊息：

1. 按 **F5** 執行應用程式。
2. 按一下**Rock**內內容類型 功能表，請參閱如何**MessageService**所插入至檢視，並載入歡迎訊息和映像。 在此範例中，我們會輸入到&quot; **Rock**&quot;:

    ![MVC Music Store 檢視資料隱碼](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store 檢視資料隱碼")

    *MVC Music Store 檢視資料隱碼*
3. 關閉瀏覽器。

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>練習 3： 將動作篩選條件

在先前的實際操作實驗室**自訂動作篩選條件**您已使用篩選器的自訂及資料隱碼。 在此練習中，您將學習如何將篩選器與相依性插入，使用 Unity 容器。 若要這樣做，您將會加入 Music Store 方案自訂動作篩選條件會追蹤網站的活動。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>工作 1-在方案中包含追蹤篩選

在這項工作，您將會包含在 Music Store 中追蹤事件的自訂動作篩選條件。 做為自訂動作篩選條件概念已處理的上一個實驗室&quot;自訂動作篩選條件&quot;，您將只包含本實驗室中，從 Assets 資料夾篩選類別，然後再建立 Unity 篩選條件提供者：

1. 開啟**開始**方案位於**Source\Ex03-插入動作 Filter\Begin**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
      > 
      > 如需詳細資訊，請參閱這篇文章： [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。
2. 包含**TraceActionFilter.cs**檔案從 **/來源/資產**至 **/篩選**資料夾。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > 此自訂動作篩選條件執行 ASP.NET 追蹤。 您可以檢查&quot;ASP.NET MVC 4 本機和動態的動作篩選條件&quot;實驗室更多的參考。
3. 加入空類別**FilterProvider.cs**資料夾中的專案  **/篩選。**
4. 新增**System.Web.Mvc**和**Microsoft.Practices.Unity**中的命名空間**FilterProvider.cs**。

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-篩選條件提供者加入命名空間*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. 使類別繼承自**IFilterProvider**介面。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. 新增**IUnityContainer**屬性**FilterProvider**類別，然後再建立 指定容器的類別建構函式。

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-篩選條件提供者的建構函式*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > 篩選條件提供者類別的建構函式不建立**新**物件內。 容器會傳遞做為參數，並將相依性已解決的 Unity。
7. 在**FilterProvider**類別，實作方法**GetFilters**從**IFilterProvider**介面。

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-篩選條件提供者 GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>工作 2-註冊及啟用篩選器

在這項工作，您將啟用站台的追蹤。 若要這樣做，請將登錄中的篩選條件**Bootstrapper.cs BuildUnityContainer**方法來啟動追蹤：

1. 開啟**Web.config**位於專案的根目錄和啟用追蹤追蹤在 System.Web 群組。

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. 開啟**Bootstrapper.cs**在專案的根目錄。
3. 將參考加入**MvcMusicStore.Filters**命名空間。

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-啟動載入器加入命名空間*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. 選取**BuildUnityContainer**方法和註冊 Unity 容器中的篩選條件。 您必須註冊篩選條件提供者，以及動作篩選條件。

    (程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-註冊 FilterProvider 和 ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>工作 3-執行應用程式

在這項工作，您將執行應用程式，並測試自訂動作篩選條件追蹤的活動：

1. 按 **F5** 執行應用程式。
2. 按一下**Rock**內容類型 功能表中。 您可以瀏覽至多個內容類型如果您想要。

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Music 市集*
3. 瀏覽至 **/Trace.axd**以查看應用程式追蹤頁面，然後再按一下**檢視詳細資料**。

    ![應用程式追蹤記錄檔](aspnet-mvc-4-dependency-injection/_static/image12.png "應用程式追蹤記錄檔")

    *應用程式追蹤記錄檔*

    ![應用程式追蹤-要求詳細資料](aspnet-mvc-4-dependency-injection/_static/image13.png "追蹤應用程式-要求詳細資料")

    *應用程式追蹤-要求詳細資料*
4. 關閉瀏覽器。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室中，您已經學會如何使用 ASP.NET MVC 4 中的相依性插入，方法是整合 Unity 使用 NuGet 封裝。 若要達成此目標，您使用的相依性插入內控制站、 檢視和動作篩選條件。

涵蓋下列概念：

- ASP.NET MVC 4 相依性插入功能
- Unity 整合使用 Unity.Mvc3 NuGet 封裝
- 相依性插入控制器
- 在檢視中的相依性插入
- 相依性插入的動作篩選條件

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。

    ![VS Express for Web 方塊](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express for Web 方塊*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>附錄 b： 使用程式碼片段

程式碼片段，您會有您需要在您可以的所有程式碼。 實驗室文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](aspnet-mvc-4-dependency-injection/_static/image19.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")

*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*

***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始鍵入片段名稱 （不含空格或連字號）。
3. 觀察 IntelliSense 會顯示比對程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始輸入程式碼片段名稱](aspnet-mvc-4-dependency-injection/_static/image20.png "開始鍵入片段名稱")

*開始輸入程式碼片段名稱*

![按 Tab 鍵選取反白顯示的程式碼片段](aspnet-mvc-4-dependency-injection/_static/image21.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵選取反白顯示的程式碼片段*

![按 Tab 鍵一次程式碼片段就會擴展](aspnet-mvc-4-dependency-injection/_static/image22.png "按 Tab 鍵一次程式碼片段就會擴展")

*按 Tab 鍵一次程式碼片段就會擴展*

***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取**插入程式碼片段**後面**我的程式碼片段**。
2. 透過在按一下挑選清單中，從相關的程式碼片段。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-dependency-injection/_static/image23.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![透過在按一下挑選清單中，從相關的程式碼片段](aspnet-mvc-4-dependency-injection/_static/image24.png "上它即可挑選清單中，從相關的程式碼片段")

*透過在按一下挑選清單中，從相關的程式碼片段*
