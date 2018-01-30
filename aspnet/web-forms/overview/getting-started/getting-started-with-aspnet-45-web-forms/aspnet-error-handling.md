---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: "ASP.NET 錯誤處理 |Microsoft 文件"
author: Erikre
description: "此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 3f732ae6f1b7845bcae88912b4a4fe26574c10de
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-error-handling"></a>ASP.NET 錯誤處理
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。


在本教學課程中，您將修改 Wingtip Toys 範例應用程式，包括錯誤處理與錯誤記錄。 錯誤處理可讓應用程式正常地處理錯誤，並據以顯示錯誤訊息。 錯誤記錄可讓您找出並修正所發生的錯誤。 本教學課程先前的教學課程 」 的 URL 路由 」 為基礎，並是 Wingtip Toys 教學課程系列的一部分。

## <a name="what-youll-learn"></a>您將學習：

- 如何加入全域錯誤處理應用程式的組態。
- 如何新增錯誤處理應用程式、 頁面和程式碼層級。
- 如何記錄錯誤，以便稍後進行檢閱。
- 如何顯示不危及安全性的錯誤訊息。
- 如何實作錯誤記錄模組和處理常式 (ELMAH) 錯誤記錄。

## <a name="overview"></a>總覽

ASP.NET 應用程式必須能夠處理以一致的方式在執行期間發生的錯誤。 ASP.NET 會使用 common language runtime (CLR)，它提供統一的方式通知應用程式的錯誤方法。 發生錯誤時，會擲回例外狀況。 任何錯誤，條件或未預期的行為，應用程式所遇到之例外狀況。

在 .NET Framework 中，例外狀況是從 `System.Exception` 類別繼承的物件。 從發生問題的程式碼區域擲回例外狀況。 在呼叫堆疊上傳遞的例外狀況，其中應用程式提供處理例外狀況的程式碼的位置。 如果應用程式不會處理例外狀況，則瀏覽器會強制顯示錯誤詳細資料。

最佳做法，在中處理錯誤的程式碼層級`Try` / `Catch` / `Finally`區塊程式碼中的。 嘗試將這些區塊，讓使用者可以修正其發生的內容中的問題。 如果錯誤處理區塊太遠從哪裡發生錯誤，其困難度會為使用者提供所需的資訊來修正問題。

### <a name="exception-class"></a>例外狀況類別

例外狀況類別是從它繼承例外狀況的基底類別。 大部分的例外狀況物件是例外狀況類別，衍生類別的執行個體，例如`SystemException`類別`IndexOutOfRangeException`類別，或`ArgumentNullException`類別。 此例外狀況類別的屬性，例如`StackTrace`屬性，`InnerException`屬性，而`Message` 屬性，提供所發生之錯誤的特定資訊。

### <a name="exception-inheritance-hierarchy"></a>例外狀況的繼承階層

執行階段具有一組基本的例外狀況衍生自`SystemException`遇到例外狀況時，會擲回執行階段的類別。 大部分的類別繼承自例外狀況類別，例如`IndexOutOfRangeException`類別和`ArgumentNullException`類別中，不會實作其他成員。 因此，最重要例外狀況的資訊可以找到階層中的例外狀況、 例外狀況名稱，以及例外狀況中所包含的資訊。

### <a name="exception-handling-hierarchy"></a>例外狀況處理的階層

ASP.NET Web Form 應用程式中，可以根據特定處理階層來處理例外狀況。 下列層級，可以處理的例外狀況：

- 應用程式層級
- 頁面層級
- 程式碼層級

當應用程式處理例外狀況時，都繼承自 Exception 類別的例外狀況的其他資訊可以通常擷取並顯示給使用者。 除了應用程式、 頁面和程式碼層級，您也可以處理例外狀況 HTTP 模組層級，與使用 IIS 的自訂處理常式。

### <a name="application-level-error-handling"></a>應用程式層級的錯誤處理

您可以處理應用程式層級的預設錯誤，修改您的應用程式組態或是加入`Application_Error`中的處理常式*Global.asax*應用程式檔案。

您可以藉由新增處理預設錯誤和 HTTP 錯誤`customErrors`區段*Web.config*檔案。 `customErrors`區段可讓您指定的使用者會被重新導向到發生錯誤時的預設頁面。 它也可讓您指定的特定狀態的程式碼錯誤的個別頁面。

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

不幸的是，當您將使用者重新導向至其他頁面使用的組態，您並沒有發生之錯誤的詳細資料。

不過，將程式碼加入應用程式中攔截的任何地方發生的錯誤`Application_Error`中的處理常式*Global.asax*檔案。

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>頁面層級錯誤的事件處理

頁面層級的處理常式傳回使用者的網頁發生錯誤，但不是會維護控制項的執行個體，將不會再有任何項目頁面上。 若要提供錯誤詳細資料給應用程式的使用者，必須特別的錯誤詳細資料寫入頁面。

記錄未處理的錯誤，或將使用者導向至可顯示有用資訊的頁面，您通常會使用頁面層級的錯誤處理常式。

這個程式碼範例會顯示 ASP.NET Web 網頁中的錯誤事件的處理常式。 此處理常式會攔截所有尚未內處理的例外狀況`try` / `catch`封鎖網頁中。

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

處理的錯誤之後，必須清除藉由呼叫`ClearError`伺服器物件的方法 (`HttpServerUtility`類別)，否則您會看到先前已發生錯誤。

### <a name="code-level-error-handling"></a>程式碼層級的錯誤處理

Try-catch 陳述式包含 try 區塊後面接著一個或多個 catch 子句，指定不同的例外狀況處理常式。 擲回例外狀況時，common language runtime (CLR) 會尋找處理此例外狀況的 catch 陳述式。 如果目前執行的方法不會包含在 catch 區塊，CLR 就會查看目前的方法，並在呼叫堆疊上呼叫的方法。 如果找不到任何的 catch 區塊，CLR 就向使用者顯示未處理的例外狀況訊息檔案，並停止程式執行中。

下列程式碼範例顯示使用的常見方式`try` / `catch` / `finally`來處理錯誤。

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

在上述程式碼中，try 區塊包含必須防範可能的例外狀況的程式碼。 區塊會執行直到擲回例外狀況或區塊已順利完成。 如果有任一個`FileNotFoundException`例外狀況或`IOException`發生例外狀況，執行會傳輸至其他頁面。 然後，程式碼中包含最後區塊執行時，是否或未發生錯誤。

## <a name="adding-error-logging-support"></a>將錯誤記錄支援

之前加入錯誤處理 Wingtip Toys 範例應用程式，您將新增錯誤記錄的支援，藉由新增`ExceptionUtility`類別*邏輯*資料夾。 如此一來，的每當應用程式處理錯誤，錯誤詳細資料加入至錯誤記錄檔。

1. 以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。   
 隨即顯示 [ 新增項目] 對話方塊。
2. 選取**Visual C#**  - &gt; **程式碼**左側的 [範本] 群組。 然後，選取**類別**中間清單並將其命名**ExceptionUtility.cs**。
3. 選擇 [新增]。 會顯示新的類別檔案。
4. 將現有的程式碼取代為下列程式碼：  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

當發生例外狀況時，例外狀況就寫入至例外狀況記錄檔藉由呼叫`LogException`方法。 這個方法會採用兩個參數、 例外狀況物件和字串，包含有關例外狀況來源的詳細資料。 例外狀況記錄會寫入*ErrorLog.txt*檔案*應用程式\_資料*資料夾。

### <a name="adding-an-error-page"></a>加入錯誤頁面

Wingtip Toys 範例應用程式，在一頁會用於顯示錯誤。 錯誤頁面被設計來顯示網站的使用者安全的錯誤訊息。 不過，如果使用者在提出 HTTP 要求提供服務時在本機電腦上的程式碼所在的開發人員，其他錯誤詳細資料會顯示錯誤頁面上。

1. 以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管 中**選取**新增** - &gt; **新項目**.   
 隨即顯示 [ 新增項目] 對話方塊。
2. 選取**Visual C#**  - &gt; **Web**左側的 [範本] 群組。 從 [中間] 清單中選取**使用主版頁面的 Web Form**，並將其命名**ErrorPage.aspx**。
3. 按一下 [加入] 。
4. 選取*Site.Master*主版頁面中，為檔案，然後選擇 **確定**。
5. 以下列內容取代現有的標記：   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. 取代現有的程式碼的程式碼後置 (*ErrorPage.aspx.cs*) 使其顯示，如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

顯示錯誤頁面時，`Page_Load`事件處理常式會執行。 在`Page_Load`處理常式，先處理此錯誤的位置來決定。 然後，最後發生的錯誤由呼叫`GetLastError`伺服器物件的方法。 如果例外狀況不再存在，則會建立泛型例外狀況。 接著，如果已在本機發出的 HTTP 要求，會顯示所有的錯誤詳細資料。 在此情況下，只有本機電腦執行 web 應用程式會看到這些錯誤的詳細資料。 已經顯示錯誤資訊之後，錯誤會加入至記錄檔，並從伺服器會清除錯誤。

### <a name="displaying-unhandled-error-messages-for-the-application"></a>顯示應用程式的未處理的錯誤訊息

藉由新增`customErrors`區段*Web.config*檔案中，您可以快速處理簡單整個應用程式發生的錯誤。 您也可以指定如何處理根據其狀態的程式碼值，例如，404-找不到檔案的錯誤。

#### <a name="update-the-configuration"></a>更新設定

更新組態，藉由新增`customErrors`區段*Web.config*檔案。

1. 在**方案總管 中**、 尋找和開啟*Web.config* Wingtip Toys 範例應用程式根目錄的檔案。
2. 新增`customErrors`區段*Web.config*檔案內`<system.web>`節點，如下所示：   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. 儲存*Web.config*檔案。

`customErrors`區段會指定模式中，設定為 「 On 」。 它也會指定`defaultRedirect`，它會告訴應用程式發生錯誤時，瀏覽至哪一頁。 此外，您已加入指定如何處理 404 錯誤，找不到頁面時的特定錯誤項目。 稍後在本教學課程中，您將加入其他錯誤處理，將擷取的應用程式層級的錯誤詳細資料。

#### <a name="running-the-application"></a>執行應用程式

您可以執行應用程式現在以查看更新的路由。

1. 按**F5**執行 Wingtip Toys 範例應用程式。  
 瀏覽器隨即開啟並顯示*Default.aspx*頁面。
2. 在瀏覽器中輸入下列 URL (請務必使用**您**連接埠號碼):  
    `https://localhost:44300/NoPage.aspx`
3. 檢閱*ErrorPage.aspx*瀏覽器中顯示。 

    ![ASP.NET 錯誤處理-找不到網頁時發生錯誤](aspnet-error-handling/_static/image1.png)

當您要求*NoPage.aspx*頁面上，不存在，錯誤頁面就會顯示簡單的錯誤訊息以及詳細的錯誤資訊有提供其他詳細資料。 不過，如果使用者從遠端位置要求不存在的頁面上，[錯誤] 頁面會只有錯誤訊息以紅色顯示。

### <a name="including-an-exception-for-testing-purposes"></a>基於測試目的包括例外狀況

若要確認您的應用程式的運作方式錯誤時，就會發生，在 ASP.NET 中刻意建立錯誤條件。 在 Wingtip Toys 範例應用程式，您會擲回測試例外狀況時若要查看發生的事載入預設頁面。

1. 開啟 程式碼後置的*Default.aspx* Visual Studio 中的頁面。   
 *Default.aspx.cs*將會顯示程式碼後置頁面。
2. 在`Page_Load`處理常式，加入程式碼，這樣的處理常式隨即出現，如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

您可建立各種不同的例外狀況。 在上述程式碼中，您將建立`InvalidOperationException`時*Default.aspx*載入頁面。

#### <a name="running-the-application"></a>執行應用程式

您可以執行的應用程式，以查看應用程式如何處理例外狀況。

1. 按**CTRL + F5**執行 Wingtip Toys 範例應用程式。  
 應用程式會擲回 InvalidOperationException。 

    > [!NOTE] 
    > 
    > 您必須按**CTRL + F5**顯示頁面，而不會中斷程式碼，在 Visual Studio 中檢視錯誤的來源。
2. 檢閱*ErrorPage.aspx*瀏覽器中顯示。 

    ![ASP.NET 錯誤處理-錯誤頁面](aspnet-error-handling/_static/image2.png)

您可以看到錯誤詳細資料中，例外狀況已受困於連結`customError`一節中*Web.config*檔案。

### <a name="adding-application-level-error-handling"></a>加入應用程式層級的錯誤處理

而不是設陷例外狀況使用`customErrors`一節中*Web.config*檔案中，在您取得少數例外狀況的相關資訊，您可以捕捉此錯誤，應用程式層級，並擷取錯誤詳細資料。

1. 在**方案總管 中**、 尋找和開啟*Global.asax.cs*檔案。
2. 新增**應用程式\_錯誤**處理常式，使其顯示，如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

應用程式時，發生錯誤時`Application_Error`會呼叫處理常式。 這個處理常式的最後一個例外狀況是擷取，然後檢閱。 如果未處理的例外狀況和例外狀況包含內部例外狀況詳細資料 (也就是`InnerException`不是 null)，應用程式將執行轉移至錯誤網頁顯示例外狀況詳細資料的位置。

#### <a name="running-the-application"></a>執行應用程式

您可以執行應用程式所處理的例外狀況，應用程式層級提供的其他錯誤詳細資料。

1. 按**CTRL + F5**執行 Wingtip Toys 範例應用程式。  
 應用程式會擲回`InvalidOperationException`。
2. 檢閱*ErrorPage.aspx*瀏覽器中顯示。 

    ![ASP.NET 錯誤處理-應用程式層級錯誤](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>加入頁面層級的錯誤處理

您可以加入頁面層級的錯誤處理頁面使用 新增`ErrorPage`屬性`@Page`指示詞的頁面上，或藉由新增`Page_Error`程式碼後置頁面的事件處理常式。 在本節中，您將加入`Page_Error`會傳輸到執行的事件處理常式*ErrorPage.aspx*頁面。

1. 在**方案總管 中**、 尋找和開啟*Default.aspx.cs*檔案。
2. 新增`Page_Error`處理常式會使程式碼後置會顯示為遵循：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

在頁面上，發生錯誤時`Page_Error`會呼叫事件處理常式。 這個處理常式的最後一個例外狀況是擷取，然後檢閱。 如果`InvalidOperationException`發生，`Page_Error`事件處理常式將執行轉移至錯誤網頁顯示例外狀況詳細資料的位置。

#### <a name="running-the-application"></a>執行應用程式

您可以執行應用程式現在以查看更新的路由。

1. 按**CTRL + F5**執行 Wingtip Toys 範例應用程式。  
 應用程式會擲回`InvalidOperationException`。
2. 檢閱*ErrorPage.aspx*瀏覽器中顯示。 

    ![ASP.NET 錯誤處理的頁面層級錯誤](aspnet-error-handling/_static/image4.png)
3. 關閉瀏覽器視窗。

### <a name="removing-the-exception-used-for-testing"></a>移除用於測試的例外狀況

若要讓 Wingtip Toys 範例應用程式，而不擲回的例外狀況，您稍早在本教學課程中加入函式，移除例外狀況。

1. 開啟 程式碼後置的*Default.aspx*頁面。
2. 在`Page_Load`處理常式中，移除此程式碼擲回例外狀況，這樣的處理常式隨即出現，如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>加入程式碼層級的錯誤記錄

如稍早在本教學課程中所述，您可以加入嘗試執行程式碼區段，並處理，就會發生的第一個錯誤的 try/catch 陳述式。 在此範例中，您只會寫入錯誤詳細資料的錯誤記錄檔，以便稍後可以檢閱錯誤。

1. 在**方案總管 中**，請在*邏輯*資料夾、 尋找和開啟*PayPalFunctions.cs*檔案。
2. 更新`HttpCall`方法讓，程式碼會顯示為如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

上述程式碼會呼叫`LogException`方法中所包含`ExceptionUtility`類別。 您加入*ExceptionUtility.cs*類別檔案，以*邏輯*稍早在本教學課程中的資料夾。 `LogException`方法會採用兩個參數。 第一個參數是例外狀況物件。 第二個參數是用來識別錯誤來源的字串。

### <a name="inspecting-the-error-logging-information"></a>檢查錯誤記錄資訊

如先前所述，您可以使用錯誤記錄檔以判斷應該先修正您的應用程式中的錯誤。 當然，不會記錄錯誤，截取，寫入錯誤記錄檔。

1. 在**方案總管 中**、 尋找和開啟*ErrorLog.txt*檔案*應用程式\_資料*資料夾。   
 您可能需要選取"**顯示所有檔案**」 選項或"**重新整理**」 選項，從頂端**方案總管 中**查看*ErrorLog.txt*檔案。
2. 檢閱顯示在 Visual Studio 中的錯誤記錄檔： 

    ![ASP.NET 錯誤處理-ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>安全的錯誤訊息

它是**很重要的一點**，當您的應用程式會顯示錯誤訊息，它不應將惡意使用者可能會發現很有幫助攻擊您的應用程式的資訊。 例如，如果您的應用程式未順利嘗試寫入資料庫，它不應該顯示錯誤訊息，包括它正在使用的使用者名稱。 基於這個理由，以紅色的一般錯誤訊息會顯示給使用者。 所有其他錯誤詳細資料只會顯示在本機電腦上，開發人員。

## <a name="using-elmah"></a>使用 ELMAH

ELMAH （錯誤記錄模組和處理常式） 是錯誤記錄工具，您可外掛至您的 ASP.NET 應用程式，以 NuGet 套件。 ELMAH 提供下列功能：

- 記錄的未處理例外狀況。
- 若要檢視錄製之未處理的例外狀況的整個記錄檔的網頁。
- 若要檢視完整的詳細資料，每個網頁記錄例外狀況。
- 它發生時每個錯誤的電子郵件通知。
- 從記錄檔，RSS 摘要的最後 15 個錯誤。

您可以使用 ELMAH，您必須先安裝它。 這是容易使用*NuGet*套件安裝程式。 如稍早在本教學課程中所述，NuGet 是 Visual Studio 擴充功能，可讓您輕鬆安裝及更新的開放原始碼程式庫和 Visual Studio 中的工具。

1. 在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員** - &gt; **管理方案的 NuGet 套件**。 

    ![ASP.NET 錯誤處理-管理方案的 NuGet 封裝](aspnet-error-handling/_static/image6.png)
2. **管理 NuGet 封裝**Visual Studio 中會顯示對話方塊。
3. 在**管理 NuGet 封裝**對話方塊方塊中，展開 **線上**的左側，然後選取**nuget.org**。然後，尋找並安裝**ELMAH**封裝從線上可用的封裝清單。 

    ![ASP.NET 錯誤處理-ELMA NuGet 封裝](aspnet-error-handling/_static/image7.png)
4. 您必須要有網際網路連線來下載套件。
5. 在**選取專案**對話方塊方塊中，請確定**WingtipToys**選取項目已選取，然後按一下**確定**。 

    ![ASP.NET 錯誤處理-選取的專案 對話方塊](aspnet-error-handling/_static/image8.png)
6. 按一下**關閉**中**管理 NuGet 封裝**視 對話方塊。
7. 如果 Visual Studio 會要求您重新載入任何開啟的檔案，請選取 「**全部皆是**"。
8. ELMAH 封裝本身中加入項目*Web.config*專案根目錄的檔案。 如果 Visual Studio 會要求您如果您想要重新載入已修改*Web.config*檔案，請按一下**是**。

ELMAH 現在已準備好儲存發生的任何未處理的錯誤。

### <a name="viewing-the-elmah-log"></a>檢視 ELMAH 記錄檔

檢視 ELMAH 記錄檔很容易，但首先您要建立將 ELMAH 記錄檔中記錄未處理例外狀況。

1. 按**CTRL + F5**執行 Wingtip Toys 範例應用程式。
2. 若要寫入 ELMAH 記錄未處理的例外狀況，請瀏覽至下列 URL （使用連接埠號碼） 瀏覽器中：  
    `https://localhost:44300/NoPage.aspx`將會顯示錯誤頁面。
3. 若要顯示 ELMAH 記錄檔，請瀏覽至下列 URL （使用連接埠號碼） 瀏覽器中：  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET 錯誤處理-ELMAH 錯誤記錄檔](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>總結

在本教學課程中，您已經學會有關在應用程式層級、 頁面層級及程式碼層級的錯誤處理。 您也已經學會如何記錄處理和未處理的錯誤，以便稍後進行檢閱。 您已新增 ELMAH 公用程式，以提供例外狀況記錄和通知給您的應用程式使用 NuGet。 此外，您已經學會關於安全的錯誤訊息的重要性。

## <a name="tutorial-series-conclusion"></a>教學課程系列的結論

*感謝您沿著下列。我希望這組教學課程，幫助您更熟悉 ASP.NET Web Form。如果您需要適用於 ASP.NET 4.5 和 Visual Studio 2013 的 Web Form 功能的詳細資訊，請參閱* [ *ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊*](../../../../visual-studio/overview/2013/release-notes.md) *.此外，請務必查看本教學課程中所述* ***接下來的步驟 * * * 區段和 defintely 試用* [*免費試用 Azure*](https://azure.microsoft.com/pricing/free-trial/)*.*

![感謝您-Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>後續步驟

深入了解部署到 Microsoft Azure web 應用程式，請參閱[部署至 Azure 網站的安全 ASP.NET Web Form 應用程式成員資格、 OAuth、 與 SQL Database](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)。

## <a name="free-trial"></a>免費試用版

[Microsoft Azure-免費試用版](https://azure.microsoft.com/pricing/free-trial/)  
 您的網站發行到 Microsoft Azure 可節省時間、 維護和費用。 它是快速處理程序將您的 web 應用程式部署至 Azure。 當您需要維護和監視您的 web 應用程式時，Azure 會提供各種不同的工具和服務。 管理資料、 傳輸、 識別、 備份、 訊息、 媒體和在 Azure 中的效能。 並且，全部都提供非常符合成本效益的方法。

## <a name="additional-resources"></a>其他資源

[記錄錯誤的詳細資訊與 ASP.NET 健康監視](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>謝誌

我想要感謝下列重大貢獻內容本教學課程系列的人：

- [Alberto Poblacion、 MVP &amp; mct 規範、 西班牙](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen、 荷蘭](http://blog.alexthissen.nl/)(twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier，美國](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik、 斯洛維尼亞](http://twitter.com/bvrhovnik)
- [Bruno Sonnino，巴西](http://msmvps.com/blogs/bsonnino)(twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos dos Santos 巴西](http://www.carloscds.net/)
- [Dave Campbell，USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway、 Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael 升記號、 美國](http://www.930solutions.com/)(twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike 教宗
- [Mitchel 銷售、 美國](http://www.mitchelsellers.com/)(twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado，葡萄牙](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>社群投稿

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
 Visual Studio 2012 相關的 MSDN 上的程式碼範例：[瀏覽 Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
 Visual Studio 2012 相關的 MSDN 上的程式碼範例： [ASP.NET 4.5 Web Form 教學課程系列在 Visual Basic 中](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo-Microsoft 技術的對象參與者 (twitter: @driazevedo)  
 Visual Studio 2012 轉譯： [Iniciando com Visão Geral 的 ASP.NET Web Form 4.5-Parte 1-Introdução e](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

>[!div class="step-by-step"]
[上一步](url-routing.md)
