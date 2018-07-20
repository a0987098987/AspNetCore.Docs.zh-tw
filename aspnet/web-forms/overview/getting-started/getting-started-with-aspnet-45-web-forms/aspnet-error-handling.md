---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.NET 錯誤處理 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 8d6c19be7c77079b870261d1c4cf0ea62e0e2fd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807586"
---
<a name="aspnet-error-handling"></a>ASP.NET 錯誤處理
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。 Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。


在本教學課程中，您將修改 Wingtip Toys 範例應用程式，以包含錯誤處理和錯誤記錄。 錯誤處理可讓應用程式依正常程序處理錯誤，並據以顯示錯誤訊息。 錯誤記錄可讓您找出並修正所發生的錯誤。 此教學課程先前的教學課程 」 URL 路由 」，並且是 Wingtip Toys 教學課程系列的一部分。

## <a name="what-youll-learn"></a>您將學到什麼：

- 如何新增全域錯誤處理應用程式的設定。
- 如何新增錯誤處理應用程式、 頁面和程式碼層級。
- 如何記錄錯誤，以便稍後進行檢閱。
- 如何顯示錯誤訊息，不會危及安全性。
- 如何實作錯誤記錄模組和處理常式 (ELMAH) 的錯誤記錄。

## <a name="overview"></a>總覽

ASP.NET 應用程式必須能夠處理以一致的方式執行期間發生的錯誤。 ASP.NET 會使用 common language runtime (CLR)，可提供這種統一的方式通知應用程式的錯誤。 發生錯誤時，會擲回例外狀況。 例外狀況是錯誤、 條件或應用程式遇到非預期的行為。

在 .NET Framework 中，例外狀況是從 `System.Exception` 類別繼承的物件。 從發生問題的程式碼區域擲回例外狀況。 在呼叫堆疊中向上傳遞的例外狀況，其中應用程式會提供處理例外狀況的程式碼的位置。 如果應用程式未處理例外狀況，會強制瀏覽器顯示錯誤詳細資料。

最佳做法是處理中的程式碼層級中的錯誤`Try` / `Catch` / `Finally`內您的程式碼區塊。 嘗試將這些區塊，讓使用者可以修正其發生的內容中的問題。 如果錯誤處理區塊太遠而從發生錯誤，它變得更難使用者提供他們需要修正此問題的資訊。

### <a name="exception-class"></a>例外狀況類別

例外狀況類別是例外狀況從中繼承的基底類別。 大部分的例外狀況物件是例外狀況類別的一些衍生類別的執行個體，例如`SystemException`類別，`IndexOutOfRangeException`類別，或`ArgumentNullException`類別。 此例外狀況類別的屬性，例如`StackTrace`屬性，`InnerException`屬性，而`Message`屬性，提供有關發生之錯誤的特定資訊。

### <a name="exception-inheritance-hierarchy"></a>例外狀況的繼承階層架構

執行階段具有例外狀況衍生自基底集合`SystemException`例外狀況發生時，會擲回執行階段的類別。 大部分的類別，繼承自例外狀況類別，例如`IndexOutOfRangeException`類別和`ArgumentNullException`類別中，不會實作其他成員。 因此，可以找到例外狀況最重要的資訊，階層中的例外狀況、 例外狀況名稱和例外狀況中所包含的資訊。

### <a name="exception-handling-hierarchy"></a>例外狀況處理階層架構

ASP.NET Web Forms 應用程式，可以根據特定的處理階層來處理例外狀況。 在下列層級，可以處理例外狀況：

- 應用程式層級
- 頁面層級
- 程式碼層級

當應用程式處理的例外狀況時，繼承自例外狀況類別的例外狀況的其他資訊可以通常可以擷取和顯示給使用者。 除了應用程式、 頁面和程式碼層級，您也可以處理例外狀況的 HTTP 模組層級和使用 IIS 的自訂處理常式。

### <a name="application-level-error-handling"></a>應用程式層級的錯誤處理

您可以透過修改您的應用程式組態或是加入，處理應用程式層級的預設錯誤`Application_Error`中的處理常式*Global.asax*應用程式檔案。

您可以藉由新增處理預設錯誤和 HTTP 錯誤`customErrors`一節*Web.config*檔案。 `customErrors`區段可讓您指定的使用者會重新導向至發生錯誤時的預設頁面。 它也可讓您指定個別頁面特定狀態的程式碼錯誤。

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

不幸的是，當您將使用者重新導向至其他頁面使用的組態，您並沒有發生之錯誤的詳細資料。

不過，將程式碼加入應用程式中捕捉任何一處發生的錯誤`Application_Error`中的處理常式*Global.asax*檔案。

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>頁面層級的錯誤事件處理

頁面層級的處理常式會傳回至頁面的使用者發生錯誤，但因為不會維護控制項的執行個體，將不會再有任何項目頁面上。 若要向應用程式的使用者提供錯誤詳細資料，您必須特別寫入錯誤詳細資料頁面。

記錄未處理的錯誤，或將使用者帶到可顯示有用資訊的頁面，您通常會使用頁面層級的錯誤處理常式。

此程式碼範例顯示 ASP.NET 網頁中的錯誤事件的處理常式。 此處理常式會攔截所有尚未內處理的例外狀況`try` / `catch`頁面中的區塊。

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

處理錯誤之後，您必須清除它藉由呼叫`ClearError`方法之伺服器物件 (`HttpServerUtility`類別)，否則您會看到先前已發生錯誤。

### <a name="code-level-error-handling"></a>程式碼層級的錯誤處理

Try / catch 陳述式包含在 try 區塊後面接著一個或多個 catch 子句，指定不同的例外狀況的處理常式。 擲回例外狀況時，common language runtime (CLR) 會尋找處理此例外狀況的 catch 陳述式。 如果目前執行的方法不會包含 catch 區塊，CLR 會探討呼叫目前的方法，並依此類推項目，在呼叫堆疊的方法。 如果找不到任何的 catch 區塊，CLR 就向使用者顯示未處理的例外狀況訊息檔案，並停止程式執行中。

下列程式碼範例顯示使用的常見方式`try` / `catch` / `finally`來處理錯誤。

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

在上述程式碼中，try 區塊包含必須加以防範可能的例外狀況的程式碼。 區塊會執行直到例外狀況會擲回或區塊已順利完成。 如果有任一`FileNotFoundException`例外狀況或`IOException`發生例外狀況，執行會轉移至不同的頁面。 然後，在包含的程式碼會執行 finally 區塊，是否發生錯誤。

## <a name="adding-error-logging-support"></a>新增錯誤記錄的支援

然後再加入錯誤處理 「 Wingtip Toys 範例應用程式，您會新增錯誤記錄支援加上`ExceptionUtility`類別，即可*邏輯*資料夾。 透過這種方式，每次應用程式處理錯誤，錯誤詳細資料會加入錯誤記錄檔。

1. 以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。   
   隨即顯示 [ 新增項目] 對話方塊。
2. 選取  **Visual C#**  - &gt; **程式碼**左側的 範本 群組。 然後，選取**類別**從中間清單並將它命名**ExceptionUtility.cs**。
3. 選擇 [新增]。 會顯示新的類別檔案。
4. 將現有的程式碼取代為下列程式碼：  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

當發生例外狀況時，例外狀況可以寫入至例外狀況記錄檔呼叫`LogException`方法。 這個方法會採用兩個參數、 例外狀況物件和字串，包含有關例外狀況來源的詳細資料。 例外狀況記錄檔會寫入*ErrorLog.txt*中的檔案*應用程式\_資料*資料夾。

### <a name="adding-an-error-page"></a>加入錯誤頁面

在 Wingtip Toys 範例應用程式中，一頁將用於顯示的錯誤。 [錯誤] 頁面可向網站的使用者顯示安全的錯誤訊息。 不過，如果使用者是開發人員提出 HTTP 要求正在電腦上本機服務的程式碼位於何處，其他錯誤詳細資料會顯示在 [錯誤] 頁面。

1. 以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管**，然後選取**新增** - &gt; **新項目**.   
   隨即顯示 [ 新增項目] 對話方塊。
2. 選取  **Visual C#**  - &gt; **Web**左側的 範本 群組。 從清單的中間，選取**使用主版頁面的 Web Form**，並將它命名**ErrorPage.aspx**。
3. 按一下 [加入] 。
4. 選取  *Site.Master*主版頁面中，為檔案，然後再選擇**確定**。
5. 以下列內容取代現有的標記：   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. 取代現有的程式碼的程式碼後置 (*ErrorPage.aspx.cs*)，使其出現，如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

顯示錯誤頁面時，請`Page_Load`事件處理常式會執行。 在 `Page_Load`處理常式中，決定先處理此錯誤的位置。 然後，最後發生的錯誤由呼叫`GetLastError`伺服器物件的方法。 如果例外狀況不再存在，則會建立一般的例外狀況。 然後，如果已在本機進行 HTTP 要求，就會顯示所有的錯誤詳細資料。 在此情況下，只有本機電腦執行的 web 應用程式會看到這些錯誤的詳細資料。 顯示錯誤訊息後，錯誤會加入至記錄檔和錯誤都會從伺服器。

### <a name="displaying-unhandled-error-messages-for-the-application"></a>顯示應用程式的未處理的錯誤訊息

藉由新增`customErrors`一節*Web.config*檔案中，您可以快速地處理簡單整個應用程式就會發生的錯誤。 您也可以指定如何處理錯誤狀態碼值，例如 404-找不到檔案。

#### <a name="update-the-configuration"></a>請更新組態

更新組態，藉由新增`customErrors`一節*Web.config*檔案。

1. 在 **方案總管**，尋找並開啟*Web.config* Wingtip Toys 範例應用程式根目錄的檔案。
2. 新增`customErrors`一節*Web.config*檔案內`<system.web>`節點，如下所示：   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. 儲存*Web.config*檔案。

`customErrors`區段指定的模式，它會設定為 [開啟]。 它也會指定`defaultRedirect`，這會告知應用程式發生錯誤時，瀏覽至頁面。 此外，您已新增指定如何處理 404 錯誤，找不到頁面時的特定錯誤項目。 稍後在本教學課程中，您將加入其他錯誤處理，將會擷取應用程式層級錯誤的詳細資料。

#### <a name="running-the-application"></a>執行應用程式

您可以執行應用程式現在若要查看更新的路由。

1. 按下**F5**執行 Wingtip Toys 範例應用程式。  
 瀏覽器隨即開啟並顯示*Default.aspx*頁面。
2. 在瀏覽器中輸入下列 URL (請務必使用**您**連接埠號碼):  
    `https://localhost:44300/NoPage.aspx`
3. 檢閱*ErrorPage.aspx*瀏覽器中顯示。 

    ![ASP.NET 錯誤處理： 找不到頁面時發生錯誤](aspnet-error-handling/_static/image1.png)

當您要求*NoPage.aspx*頁面上，不存在，[錯誤] 頁面會顯示簡單的錯誤訊息和詳細的錯誤資訊如果可用的其他詳細資料。 不過，如果使用者從遠端位置要求不存在的頁面，[錯誤] 頁面會只顯示錯誤訊息以紅色。

### <a name="including-an-exception-for-testing-purposes"></a>基於測試目的，包括例外狀況

若要確認您的應用程式的運作方式錯誤時，就會發生，在 ASP.NET 中刻意造成錯誤狀況。 在 Wingtip Toys 範例應用程式中，您將會擲回測試例外狀況在預設頁面載入時要看看結果如何。

1. 開啟的程式碼後置*Default.aspx* Visual Studio 中的頁面。   
   *Default.aspx.cs*將會顯示程式碼後置頁面。
2. 在 `Page_Load`處理常式，加入程式碼，以便處理常式看起來像這樣：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

可以建立各種不同例外狀況類型。 在上述程式碼中，您會建立`InvalidOperationException`時*Default.aspx*載入頁面。

#### <a name="running-the-application"></a>執行應用程式

您可以執行的應用程式，以查看應用程式如何處理例外狀況。

1. 按下**CTRL + F5**執行 Wingtip Toys 範例應用程式。  
 應用程式會擲回 InvalidOperationException。 

    > [!NOTE] 
    > 
    > 您必須按下**CTRL + F5**顯示網頁，而不需要分成多個程式碼以在 Visual Studio 中檢視錯誤的來源。
2. 檢閱*ErrorPage.aspx*瀏覽器中顯示。 

    ![ASP.NET 錯誤處理-錯誤頁面](aspnet-error-handling/_static/image2.png)

如您所見錯誤詳細資料，例外狀況已受困於連結`customError`一節*Web.config*檔案。

### <a name="adding-application-level-error-handling"></a>加入應用程式層級的錯誤處理

而不是例外狀況使用的設陷`customErrors`一節*Web.config*檔案中，在您取得例外狀況的極少資訊時，您可以捕捉此錯誤，應用程式層級，並擷取錯誤詳細資料。

1. 在 **方案總管**，尋找並開啟*Global.asax.cs*檔案。
2. 新增**應用程式\_錯誤**處理常式，以便它看起來像這樣：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

在 應用程式時，發生錯誤時`Application_Error`呼叫處理常式。 在這個處理常式中，上次的例外狀況是擷取和檢閱。 如果未處理的例外狀況和例外狀況包含內部例外狀況詳細資料 (也就是`InnerException`不是 null)，應用程式將執行轉移至 [錯誤] 頁面會顯示例外狀況詳細資料的位置。

#### <a name="running-the-application"></a>執行應用程式

您可以執行的應用程式，以查看所處理的例外狀況，應用程式層級提供的其他錯誤詳細資料。

1. 按下**CTRL + F5**執行 Wingtip Toys 範例應用程式。  
 應用程式會擲回`InvalidOperationException`。
2. 檢閱*ErrorPage.aspx*瀏覽器中顯示。 

    ![ASP.NET 錯誤處理： 應用程式層級錯誤](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>加入頁面層級的錯誤處理

您可以加入頁面層級的錯誤處理頁面使用 新增`ErrorPage`屬性設定為`@Page`指示詞的頁面上，或藉由新增`Page_Error`頁面的程式碼後置的事件處理常式。 在本節中，您將新增`Page_Error`就會傳送至執行的事件處理常式*ErrorPage.aspx*頁面。

1. 在 **方案總管**，尋找並開啟*Default.aspx.cs*檔案。
2. 新增`Page_Error`處理常式，讓程式碼後置會顯示為追蹤：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

在頁面上，發生錯誤時`Page_Error`會呼叫事件處理常式。 在這個處理常式中，上次的例外狀況是擷取和檢閱。 如果`InvalidOperationException`發生時，`Page_Error`事件處理常式將執行轉移至 [錯誤] 頁面會顯示例外狀況詳細資料的位置。

#### <a name="running-the-application"></a>執行應用程式

您可以執行應用程式現在若要查看更新的路由。

1. 按下**CTRL + F5**執行 Wingtip Toys 範例應用程式。  
 應用程式會擲回`InvalidOperationException`。
2. 檢閱*ErrorPage.aspx*瀏覽器中顯示。 

    ![ASP.NET 錯誤處理-頁面層級錯誤](aspnet-error-handling/_static/image4.png)
3. 關閉瀏覽器視窗。

### <a name="removing-the-exception-used-for-testing"></a>移除用於測試的例外狀況

若要讓 Wingtip Toys 範例應用程式，而不擲回的例外狀況，您稍早在本教學課程中新增函式，移除 例外狀況。

1. 開啟的程式碼後置*Default.aspx*頁面。
2. 在 `Page_Load`處理常式中，移除的程式碼，則會擲回例外狀況，以便處理常式看起來像這樣：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>加入程式碼層級的錯誤記錄

如稍早在本教學課程中所述，您可以加入 try/catch 陳述式，嘗試執行的程式碼區段，並處理所發生的第一個錯誤。 在此範例中，您將只寫入錯誤詳細資料的錯誤記錄檔，以便您可以稍後檢閱錯誤。

1. 在 **方案總管**，請在*邏輯*資料夾中，尋找並開啟*PayPalFunctions.cs*檔案。
2. 更新`HttpCall`方法讓，程式碼會顯示為如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

上述程式碼會呼叫`LogException`方法中包含`ExceptionUtility`類別。 您加入*ExceptionUtility.cs*類別檔案，以*邏輯*稍早在本教學課程中的資料夾。 `LogException` 方法會接受兩個參數。 第一個參數是例外狀況物件。 第二個參數是用來識別錯誤來源的字串。

### <a name="inspecting-the-error-logging-information"></a>檢查錯誤記錄資訊

如先前所述，您可以使用錯誤記錄檔，以判斷應該先修正應用程式中的錯誤。 當然，不會記錄錯誤截取和寫入錯誤記錄檔。

1. 在 **方案總管**，尋找並開啟*ErrorLog.txt*檔案中*應用程式\_資料*資料夾。   
 您可能需要選取 」**顯示所有檔案**」 選項或"**重新整理**」 選項，從頂端**方案總管 中**若要查看*ErrorLog.txt*檔案。
2. 檢閱顯示在 Visual Studio 中的錯誤記錄檔： 

    ![ASP.NET 錯誤處理-ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>安全的錯誤訊息

很**務必注意**，當您的應用程式會顯示錯誤訊息，它不應將惡意使用者可能會發現有幫助攻擊您的應用程式的資訊。 例如，如果您的應用程式未能成功嘗試寫入資料庫，它不應該顯示錯誤訊息，其中包含它所使用的使用者名稱。 基於這個理由，紅色的一般錯誤訊息會顯示給使用者。 所有其他錯誤詳細資料只會顯示在本機電腦上的程式開發人員。

## <a name="using-elmah"></a>使用 ELMAH

ELMAH （錯誤記錄模組和處理常式） 是您插入您的 ASP.NET 應用程式，以 NuGet 套件形式的錯誤記錄功能。 ELMAH 提供下列功能：

- 記錄未處理例外狀況。
- 若要檢視錄製的未處理例外狀況的整個記錄檔的網頁。
- 若要檢視完整的詳細資料，每個網頁記錄例外狀況。
- 它發生時每個錯誤的電子郵件通知。
- 從記錄檔，RSS 摘要的最後 15 個錯誤。

您可以使用 ELMAH，您必須先安裝它。 這是容易使用*NuGet*套件安裝程式。 如稍早在本教學課程系列中所述，NuGet 就會是 Visual Studio 擴充功能，可讓您更輕鬆地安裝及更新的開放原始碼程式庫和 Visual Studio 中的工具。

1. 在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員** - &gt; **管理解決方案的 NuGet 套件**。 

    ![ASP.NET 錯誤處理-管理方案的 NuGet 套件](aspnet-error-handling/_static/image6.png)
2. **管理 NuGet 套件**對話方塊隨即出現在 Visual Studio 中。
3. 中**管理 NuGet 套件**對話方塊方塊中，展開**線上**左邊，然後選取**nuget.org**。然後，尋找並安裝**ELMAH**從線上可用的套件清單中的封裝。 

    ![ASP.NET 錯誤處理-ELMA NuGet 套件](aspnet-error-handling/_static/image7.png)
4. 您必須有網際網路連線以下載封裝。
5. 在 **選取專案**對話方塊方塊中，請確定**WingtipToys**選取項目已選取，然後按一下**確定**。 

    ![ASP.NET 錯誤處理： 選取 [專案] 對話方塊](aspnet-error-handling/_static/image8.png)
6. 按一下 [**關閉**中**管理 NuGet 套件**視] 對話方塊。
7. 如果 Visual Studio 會要求您重新載入任何開啟的檔案，請選取 「**全部皆是**"。
8. ELMAH 封裝本身中加入項目*Web.config*您的專案根目錄的檔案。 如果 Visual Studio 會詢問您是否要重新載入已修改*Web.config*檔案中，按一下**是**。

ELMAH 現在就來儲存任何未處理發生的錯誤。

### <a name="viewing-the-elmah-log"></a>檢視 ELMAH 記錄檔

檢視 ELMAH 記錄檔很容易，但首先您要建立將會記錄在 ELMAH 記錄未處理例外狀況。

1. 按下**CTRL + F5**執行 Wingtip Toys 範例應用程式。
2. 若要寫入的 ELMAH 記錄未處理的例外狀況，請瀏覽至下列 URL （使用您的連接埠號碼） 瀏覽器中：  
    `https://localhost:44300/NoPage.aspx` [錯誤] 頁面隨即出現。
3. 若要顯示的 ELMAH 記錄檔，請瀏覽至下列 URL （使用您的連接埠號碼） 瀏覽器中：  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET 錯誤處理-ELMAH 錯誤記錄檔](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>總結

在本教學課程中，您已了解處理的應用程式層級、 頁面層級和程式碼層級的錯誤。 您也學到如何記錄處理和未處理的錯誤，以便稍後進行檢閱。 您已新增 ELMAH 公用程式，以提供例外狀況記錄和您使用 NuGet 的應用程式的通知。 此外，您已了解關於安全的錯誤訊息的重要性。

## <a name="tutorial-series-conclusion"></a>教學課程系列的結論

*跟著感謝。我希望這套教學課程可協助您更加熟悉 ASP.NET Web Form。如果您需要提供在 ASP.NET 4.5 和 Visual Studio 2013 中的 Web Form 功能的詳細資訊，請參閱* [ *ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊*](../../../../visual-studio/overview/2013/release-notes.md) *.此外，請務必看看教學課程中所述* ***接下來的步驟 * * * 區段和 defintely 試用* [*免費 Azure 試用*](https://azure.microsoft.com/pricing/free-trial/)*.*

![感謝您-Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>後續步驟

深入了解部署到 Microsoft Azure web 應用程式，請參閱 <<c0> [ 部署至 Azure 網站的安全 ASP.NET Web Forms 應用程式成員資格、 OAuth 和 SQL Database](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)。

## <a name="free-trial"></a>免費試用版

[Microsoft Azure-免費試用版](https://azure.microsoft.com/pricing/free-trial/)  
 您的網站發佈至 Microsoft Azure 將會節省時間、 維護和費用。 它是快速的程序，以將您的 web 應用程式部署至 Azure。 當您需要維護及監視您的 web 應用程式時，Azure 會提供各種不同的工具和服務。 管理資料、 資料傳輸、 身份識別、 備份、 訊息、 媒體和在 Azure 中的效能。 而且，所有這些都非常符合成本效益的方法中提供。

## <a name="additional-resources"></a>其他資源

[使用 ASP.NET 狀況監控記錄錯誤的詳細資料](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>謝誌

我想要感謝下列人士提出重大貢獻的內容，本教學課程系列：

- [Alberto Poblacion、 MVP &amp; MCT，西班牙](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen，荷蘭](http://blog.alexthissen.nl/)(twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier，USA](http://andret503.wordpress.com/)
- Apurva Joshi 撰寫 Microsoft
- [Bojan Vrhovnik，斯洛維尼亞](http://twitter.com/bvrhovnik)
- [Bruno sonnino 在此，巴西](http://msmvps.com/blogs/bsonnino)(twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos dos Santos 巴西](http://www.carloscds.net/)
- [Dave Campbell，USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway，Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael 升記號，美國](http://www.930solutions.com/)(twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike 主教
- [Mitchel 賣方，美國](http://www.mitchelsellers.com/)(twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba Microsoft](http://linqto.me/Links/pcociuba)
- [聖保羅 Morgado 葡萄牙](http://paulomorgado.net/)
- [請參閱 Pranav Rastogi Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>社群投稿

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Visual Studio 2012 相關的 MSDN 上的程式碼範例：[導覽 Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Visual Studio 2012 相關的 MSDN 上的程式碼範例： [ASP.NET 4.5 Web Form 教學課程系列中 Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo-Microsoft 技術的對象參與者 (twitter: @driazevedo)  
  Visual Studio 2012 轉譯： [Iniciando com Visão Geral 的 ASP.NET Web Form 4.5-Parte 1-Introdução e](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [上一步](url-routing.md)
