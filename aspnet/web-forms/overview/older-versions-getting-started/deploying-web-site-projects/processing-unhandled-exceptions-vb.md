---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: 處理未處理的例外狀況 (VB) |Microsoft Docs
author: rick-anderson
description: 在生產環境中的 web 應用程式就會發生執行階段錯誤時，重要通知開發人員，並記錄錯誤，因此，它可能會在當下診斷出的 la...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c0f6883f2a19d060aff1573238cc1df1838e824
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365980"
---
<a name="processing-unhandled-exceptions-vb"></a>處理未處理的例外狀況 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) \(英文\) ([如何下載](/aspnet/core/tutorials/index#how-to-download-a-sample))

> 在生產環境中的 web 應用程式的執行階段錯誤發生時很重要通知開發人員，並記錄錯誤，如此可能會在稍後的時間點中診斷的時間。 本教學課程提供如何 ASP.NET 處理執行階段錯誤，以及查看有自訂程式碼執行時的未處理的例外狀況反昇至 ASP.NET 執行階段的一種方法的概觀。


## <a name="introduction"></a>簡介

在 ASP.NET 應用程式中處理的例外狀況時，它會顯示 ASP.NET 執行階段，這會引發到`Error`事件，並顯示適當的錯誤頁面。 有三種不同的錯誤頁面： 執行階段錯誤黃色螢幕的死亡 (YSOD);例外狀況詳細資料 YSOD;和自訂錯誤頁面。 在 [前述教學課程](displaying-a-custom-error-page-vb.md)我們設定要用於遠端使用者和例外狀況詳細資料 YSOD，瀏覽本機的使用者自訂錯誤網頁的應用程式。

使用符合網站的外觀與風格的人類易自訂錯誤頁面是慣用的預設執行階段錯誤 YSOD，但顯示自訂錯誤頁面是完整的錯誤處理方案的冰山一角。 在生產環境中的應用程式中發生錯誤時，很重要，開發人員會收到通知的錯誤使他們可以發現例外狀況的原因和解決。 此外，以便可以檢查錯誤，並在稍後時間在當下診斷出應記錄錯誤的詳細資料。

本教學課程會示範如何存取的未處理的例外狀況詳細資料，以便它們可以記錄以及通知開發人員。 下列這其中的兩個教學課程中，瀏覽錯誤記錄程式庫，之後的設定，會自動通知開發人員的執行階段錯誤並記錄其詳細資料。

> [!NOTE]
> 在本教學課程中檢查的資訊是最有用，如果您需要以某種唯一或自訂的方式處理未處理例外狀況。 在其中您只需要記錄例外狀況，並告知開發人員的情況下，使用錯誤記錄程式庫是最好的選擇。 下面兩個教學課程提供兩個這類程式庫的概觀。


## <a name="executing-code-when-theerrorevent-is-raised"></a>執行程式碼時`Error`就會引發事件

事件會提供物件發出訊號，已發生有趣的東西，以及執行程式碼在回應中的另一個物件的機制。 身為 ASP.NET 開發人員您習慣免除考慮事件。 如果您想要執行一些程式碼，當訪客按一下特定按鈕時，您會建立事件處理常式，這個按鈕`Click`事件並將程式碼放在那里。 有鑑於 ASP.NET 執行階段會引發其[`Error`事件](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)每次處理的例外狀況發生時，它會遵循記錄的錯誤詳細資料的程式碼就會在事件處理常式。 但您該如何建立事件處理常式`Error`事件？

`Error`事件是中的許多事件的其中一個[`HttpApplication`類別](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)所要求的存留期間引發之 HTTP 管線中的特定階段。 例如，`HttpApplication`類別的[`BeginRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx)開頭的每個要求，就會引發其[`AuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)安全性模組的已識別的要求者時所引發。 這些`HttpApplication`事件可以讓網頁開發人員在要求的存留期執行的各個點上的自訂邏輯的方法。

事件處理常式`HttpApplication`事件可以放在名為的特殊檔案`Global.asax`。 若要建立此檔案在您的網站，將新項目新增至您的網站名稱中使用全域應用程式類別範本的根目錄`Global.asax`。

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**圖 1**： 新增`Global.asax`您 Web 應用程式  
 ([按一下以檢視完整大小的影像](processing-unhandled-exceptions-vb/_static/image3.png))

內容和結構`Global.asax`稍有根據您所使用的 Web 應用程式專案 (WAP) 或網站專案 (WSP) Visual Studio 所建立的檔案而有所不同。 與 WAP，`Global.asax`實作為兩個不同的檔案-`Global.asax`和`Global.asax.vb`。 `Global.asax`檔案包含執行任何動作，但`@Application`指示詞參考`.vb`檔案; 中所定義的感興趣的處理常式活動`Global.asax.vb`檔案。 針對 WSPs，建立只有一個檔案， `Global.asax`，而定義的事件處理常式`<script runat="server">`區塊。

`Global.asax` WAP 中由 Visual Studio 的通用應用程式類別範本的檔案包含名為的事件處理常式`Application_BeginRequest`， `Application_AuthenticateRequest`，以及`Application_Error`，這是事件處理常式，如`HttpApplication`事件`BeginRequest`， `AuthenticateRequest`，和`Error`分別。 另外還有名為事件處理常式`Application_Start`， `Session_Start`， `Application_End`，和`Session_End`，這是 web 應用程式啟動時，當新的工作階段啟動時，應用程式結束時，所引發的事件處理常式和工作階段結束時，分別。 `Global.asax` Visual Studio 所建立的 WSP 檔案只包含`Application_Error`， `Application_Start`， `Session_Start`， `Application_End`，和`Session_End`事件處理常式。

> [!NOTE]
> 部署 ASP.NET 應用程式時，您必須複製`Global.asax`到生產環境的檔案。 `Global.asax.vb`檔案，WAP 中建立時，不需要將複製到生產環境中，因為此程式碼會編譯專案的組件。


Visual Studio 的通用應用程式類別範本所建立的事件處理常式並不詳盡。 您可以為任何新增事件處理常式`HttpApplication`所命名的事件處理常式的事件`Application_EventName`。 例如，您可以在其中新增下列程式碼`Global.asax`檔案以建立事件處理常式[`AuthorizeRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

同樣地，您可以移除任何事件處理常式的全域應用程式類別範本所建立，則不需要。 本教學課程中，我們只需要的事件處理常式`Error`事件; 隨時移除其他事件處理常式從`Global.asax`檔案。

> [!NOTE]
> *HTTP 模組*提供另一種方式來定義事件處理常式`HttpApplication`事件。 HTTP 模組會建立類別檔案，可直接置於 web 應用程式專案，或者分離到不同的類別庫。 HTTP 模組因為它們可以分割成的類別庫，提供更有彈性且可重複使用的模型來建立`HttpApplication`事件處理常式。 而`Global.asax`檔案是特定 web 應用程式所在的位置，HTTP 模組可以編譯成組件，此時若要將 HTTP 模組新增至網站很簡單，只要卸除組件中`Bin`資料夾並註冊中的模組`Web.config`。 本教學課程不會尋找在建立及使用 HTTP 模組，但在下列兩個教學課程中使用的兩個錯誤記錄程式庫會實作為 HTTP 模組。 如 HTTP 模組的優點的詳細背景，請參閱[使用 HTTP 模組和處理常式，以建立可外掛的 ASP.NET 元件](https://msdn.microsoft.com/library/aa479332.aspx)。


## <a name="retrieving-information-about-the-unhandled-exception"></a>擷取未處理的例外狀況的相關資訊

現在我們已使用 Global.asax 檔案`Application_Error`事件處理常式。 這個事件處理常式執行時我們必須通知錯誤的開發人員，並記錄其詳細資料。 若要完成這些工作，我們必須先決定所引發的例外狀況的詳細資料。 使用伺服器物件的[`GetLastError`方法](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx)擷取造成未處理例外狀況的詳細資料`Error`引發的事件。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

`GetLastError`方法會傳回型別的物件`Exception`，這是.NET Framework 中的所有例外狀況的基底類型。 不過，上述程式碼中我要將物件轉型例外狀況所傳回`GetLastError`成`HttpException`物件。 如果`Error`因為擲回例外狀況的 ASP.NET 資源處理期間所擲回的例外狀況會包裝內則會引發事件`HttpException`。 若要取得實際的例外狀況，導致錯誤事件使用`InnerException`屬性。 如果`Error`引發事件以 HTTP 為基礎的例外狀況，例如不存在 頁面上，要求因為`HttpException`擲回，但它並沒有內部例外狀況。

下列程式碼會使用`GetLastErrormessage`來擷取有關所觸發的例外狀況`Error`事件，儲存`HttpException`名為的變數`lastErrorWrapper`。 接著會儲存類型、 訊息和原始的例外狀況堆疊追蹤中三個字串變數，查看是否`lastErrorWrapper`是實際的例外狀況觸發`Error`事件 （如果是以 HTTP 為基礎的例外狀況） 或其是否只是處理要求時所擲回例外狀況的包裝函式。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

此時，您會有您需要撰寫程式碼，會將例外狀況的詳細資訊記錄至資料庫資料表的所有資訊。 您可以建立具有資料行的資料庫資料表，每個感興趣的類型、 訊息、 堆疊追蹤和等等-以及其他一項有用的資訊，例如要求頁面的 URL 和目前登入的使用者名稱的錯誤詳細資料。 在 `Application_Error`事件處理常式，您接著連接到資料庫並將記錄插入資料表。 同樣地，您可以新增要警示的錯誤，透過電子郵件的開發人員的程式碼。

在下面兩個教學課程中檢查錯誤記錄程式庫會提供內建的這類功能，因此不需要此錯誤記錄和通知自行建置。 不過，展示`Error`引發事件，`Application_Error`事件處理常式可以用來記錄錯誤詳細資料和通知開發人員，讓我們新增 會發生錯誤時通知開發人員的程式碼。

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>發生未處理的例外狀況時通知開發人員

在生產環境中處理的例外狀況時務必通知開發小組，讓他們可以評估錯誤並判斷必須採取的動作。 比方說，如果沒有在連接到資料庫，則您必須為 double 錯誤檢查您的連接字串，或許使用您的 web 主機服務公司開啟支援票證。 如果發生例外狀況，因為發生程式設計錯誤，可能需要額外的程式碼或驗證邏輯加入至預防這類錯誤的。

.NET Framework 中的類別[`System.Net.Mail`命名空間](https://msdn.microsoft.com/library/system.net.mail.aspx)輕鬆地傳送電子郵件。 [ `MailMessage`類別](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx)則表示電子郵件訊息，並具有屬性，例如`To`， `From`， `Subject`， `Body`，以及`Attachments`。 `SmtpClass`用來傳送`MailMessage`物件使用指定的 SMTP 伺服器，可以程式設計方式或以宣告方式在指定的 SMTP 伺服器設定[`<system.net>`項目](https://msdn.microsoft.com/library/6484zdc1.aspx)在`Web.config file`。 如需有關傳送電子郵件訊息中的 ASP.NET 應用程式查看我的文章中，[在 ASP.NET 中的 傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)，而[System.Net.Mail 常見問題集](http://systemnetmail.com/)。

> [!NOTE]
> `<system.net>`項目包含所使用的 SMTP 伺服器設定`SmtpClient`類別傳送電子郵件時。 您的 web 主機服務公司可能有可用來從您的應用程式傳送電子郵件的 SMTP 伺服器。 您應該使用您的 web 應用程式中的 SMTP 伺服器設定的詳細資訊，請參閱您的 web 主機的 [支援] 區段。


將下列程式碼加入`Application_Error`傳送為開發人員的電子郵件，發生錯誤時的事件處理常式：

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

上述程式碼相當長的時間時，大量建立顯示的 HTML 中傳送給開發人員的電子郵件。 藉由參考的程式碼會開始`HttpException`所傳回`GetLastError`方法 (`lastErrorWrapper`)。 實際要求所引發的例外狀況透過擷取`lastErrorWrapper.InnerException`並指派給變數`lastError`。 類型、 訊息和堆疊追蹤資訊會從`lastError`並儲存在三個字串變數。

下一步`MailMessage`名為物件`mm`建立。 電子郵件內文是 HTML 格式，並顯示所要求之頁面 URL]、 [目前登入的使用者，以及例外狀況 （類型、 訊息和堆疊追蹤） 的相關資訊的名稱。 其中一個棒`HttpException`類別是您可以產生用來建立例外狀況詳細資料黃色螢幕的死亡 (YSOD) 藉由呼叫的 HTML [GetHtmlErrorMessage 方法](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx)。 這個方法是此處用來擷取例外狀況詳細資料 YSOD 標記，並將它加入做為附件的電子郵件。 另外請注意： 如果例外狀況，觸發`Error`事件是以 HTTP 為基礎例外狀況 （例如不存在的頁面要求），然後`GetHtmlErrorMessage`方法會傳回`null`。

最後一個步驟是傳送`MailMessage`。 這是藉由建立新`SmtpClient`方法，並呼叫其`Send`方法。

> [!NOTE]
> Web 應用程式中使用此程式碼之前，您會想變更中的值`ToAddress`並`FromAddress`常數support@example.com任何電子郵件地址錯誤通知電子郵件會寄給和源自。 您也需要指定 SMTP 伺服器設定，在`<system.net>`一節中`Web.config`。 請參閱您的 web 主機提供者，以判斷要使用的 SMTP 伺服器設定。


與此程式碼都就緒每當發生錯誤開發人員會傳送電子郵件訊息，摘要說明此錯誤，並包含 YSOD。 在先前的教學課程中示範的執行階段錯誤所瀏覽 Genre.aspx 並傳入無效`ID`值，是透過查詢字串，例如`Genre.aspx?ID=foo`。 瀏覽的頁面`Global.asax`檔案會產生相同的使用者體驗，如在先前的教學課程-在開發環境中您將會繼續以查看例外狀況詳細資料黃色死亡畫面，而在您將在生產環境中請參閱自訂錯誤頁面。 除了這個現有的行為，開發人員會收到一封電子郵件。

**圖 2**顯示瀏覽時收到的電子郵件`Genre.aspx?ID=foo`。 電子郵件本文摘要說明例外狀況資訊，雖然`YSOD.htm`附件會顯示例外狀況詳細資料 YSOD] 所示的內容 (請參閱 < **[圖 3**)。

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**圖 2**： 開發人員處理的例外狀況時傳送電子郵件通知  
 ([按一下以檢視完整大小的影像](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**圖 3**： 電子郵件通知中包含例外狀況詳細資料做為附件 YSOD  
 ([按一下以檢視完整大小的影像](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>什麼使用自訂錯誤網頁嗎？

本教學課程示範如何使用`Global.asax`而`Application_Error`事件處理常式來處理的例外狀況發生時執行程式碼。 具體來說，我們使用這個事件處理常式來通知的錯誤，開發人員我們可以進一步擴充它也在資料庫中記錄錯誤的詳細資料。 是否存在`Application_Error`事件處理常式不會影響終端使用者體驗。 它們仍然會看到已設定的錯誤頁面上，錯誤詳細資料 YSOD、 執行階段錯誤 YSOD 中，或自訂錯誤頁面。

很自然地想知道是否`Global.asax`檔案和`Application_Error`事件時，需要使用自訂錯誤頁面。 發生錯誤時向使用者顯示自訂錯誤頁面，因此為什麼不能我們放來通知開發人員，並登入程式碼後置類別的自訂錯誤頁面的 錯誤詳細資料的程式碼？ 您當然可以將程式碼加入自訂錯誤頁面的程式碼後置類別時您沒有存取權所觸發的例外狀況的詳細資料`Error`事件時使用的技巧，我們在先前的教學課程中加以探索。 呼叫`GetLastError`從自訂錯誤頁面的方法會傳回`Nothing`。

此行為的原因是因為透過重新導向達到自訂錯誤頁面。 當未處理的例外狀況會到達 ASP.NET 執行階段的 ASP.NET 引擎會引發其`Error`事件 (它就會執行`Application_Error`事件處理常式)，然後*重新導向*使用者自訂錯誤頁面，藉由發出`Response.Redirect(customErrorPageUrl)`. `Response.Redirect`方法會傳送 HTTP 302 狀態碼，用戶端指示瀏覽器要求新的 URL，也就是自訂錯誤網頁的回應。 接著瀏覽器會自動要求這個新頁面。 您所見，從網頁個別要求自訂錯誤頁面，因為瀏覽器的網址列變更為自訂錯誤網頁的 URL 而產生錯誤 (請參閱**圖 4**)。

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**圖 4**： 發生錯誤時瀏覽器重新導向至自訂錯誤頁面 URL  
 ([按一下以檢視完整大小的影像](processing-unhandled-exceptions-vb/_static/image12.png))

最後的結果是當伺服器會回應 HTTP 302 重新導向時，發生未處理的例外狀況的要求會結束。 自訂錯誤網頁的後續要求是全新的要求;此時 ASP.NET 引擎會已捨棄的錯誤資訊，此外，有沒有辦法在前一個要求中未處理的例外狀況關聯的自訂錯誤頁面的新要求。 這就是為什麼`GetLastError`傳回`null`呼叫從自訂錯誤頁面時。

不過，就能夠在執行期間造成錯誤的相同要求的自訂錯誤頁面。 [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx)方法將執行轉移至指定的 URL，並加以處理相同要求中。 您無法移動程式碼中`Application_Error`事件處理常式來取代它的自訂錯誤網頁的程式碼後置類別`Global.asax`為下列程式碼：

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

現在當未處理的例外狀況發生時`Application_Error`事件處理常式會將控制權傳輸至適當的自訂錯誤網頁的 HTTP 狀態碼為基礎。 自訂錯誤頁面控制項已移轉，因為已透過未處理的例外狀況資訊的存取權`Server.GetLastError`和可以通知錯誤的開發人員，並記錄其詳細資料。 `Server.Transfer`呼叫停止 ASP.NET 引擎，從使用者重新導向至自訂錯誤頁面。 相反地，傳回自訂錯誤網頁的內容做為產生的錯誤頁面的回應。

## <a name="summary"></a>總結

在 ASP.NET web 應用程式中處理的例外狀況時，ASP.NET 執行階段會引發`Error`事件，並顯示設定的錯誤頁面。 我們會通知的錯誤，開發人員記錄其詳細資料，或透過其他方式，藉由建立錯誤事件的事件處理常式處理它。 有兩種方式可建立的事件處理常式`HttpApplication`等事件`Error`： 在`Global.asax`檔案或從 HTTP 模組。 本教學課程示範如何建立`Error`中的事件處理常式`Global.asax`通知錯誤的開發人員透過電子郵件訊息的檔案。

建立`Error`事件處理常式是很有用，如果您需要以某種唯一或自訂的方式處理未處理例外狀況。 不過，建立您自己`Error`記錄例外狀況，或通知開發人員的事件處理常式不是最有效率地使用您的時間，已經有免費又容易使用的錯誤記錄程式庫，可以在幾分鐘內完成設定。 下面兩個教學課程會檢查兩個這類程式庫。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET HTTP 模組和 HTTP 處理常式概觀](https://support.microsoft.com/kb/307985)
- [依正常程序回應未處理的例外狀況-處理未處理的例外狀況](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` 類別和 ASP.NET 應用程式物件](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP 處理常式和 ASP.NET 中的 HTTP 模組](http://www.15seconds.com/Issue/020417.htm)
- [在 ASP.NET 中傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [了解`Global.asax`檔案](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [使用 HTTP 模組和處理常式來建立可外掛的 ASP.NET 元件](https://msdn.microsoft.com/library/aa479332.aspx)
- [使用 ASP.NET`Global.asax`檔案](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [使用`HttpApplication`執行個體](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [上一頁](displaying-a-custom-error-page-vb.md)
> [下一頁](logging-error-details-with-asp-net-health-monitoring-vb.md)
