---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: "處理未處理的例外狀況 (VB) |Microsoft 文件"
author: rick-anderson
description: "在生產環境中的 web 應用程式就會發生執行階段錯誤時，務必通知開發人員，並記錄錯誤，因此可能會在中診斷 a la..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: f2c7b1324e75584a80530620eea94d4ecd7a7044
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="processing-unhandled-exceptions-vb"></a>處理未處理的例外狀況 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_12_VB.zip)或[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial12_ErrorHandling_vb.pdf)

> 在生產環境中的 web 應用程式發生執行階段錯誤時務必通知開發人員，並記錄錯誤，因此可能會在稍後中診斷的時間。 本教學課程提供如何 ASP.NET 處理執行階段錯誤，並查看有自訂程式碼執行時處理的例外狀況 （泡泡） 至 ASP.NET 執行階段的其中一種方式的概觀。


## <a name="introduction"></a>簡介

ASP.NET 應用程式中處理的例外狀況時，它會顯示最 ASP.NET 執行階段，這會引發多`Error`事件，並顯示適當的錯誤頁面。 有三種不同的錯誤網頁： 執行階段錯誤黃色螢幕的死亡 (YSOD);例外狀況詳細資料 YSOD;和自訂錯誤網頁。 在[前述教學課程](displaying-a-custom-error-page-vb.md)我們設定要用於遠端使用者及使用者瀏覽本機例外狀況詳細資料 YSOD 的自訂錯誤網頁應用程式。

使用符合網站的外觀與風格的人類看得方便自訂錯誤頁面，預設執行階段錯誤 YSOD，但顯示的自訂錯誤頁面是只有一個完整的錯誤處理方案的一部分。 在生產環境中應用程式中發生錯誤時，很重要的開發人員會通知錯誤的使他們可以 unearth 例外狀況的原因和解決。 此外，錯誤詳細資料應記錄，讓錯誤能夠檢查，並診斷在稍後的時間。

本教學課程會示範如何存取的未處理的例外狀況詳細資料，以便可以進行記錄和開發人員收到通知。 下列這其中的兩個教學課程中，瀏覽錯誤記錄程式庫之後的位元的設定，, 將自動通知開發人員的執行階段錯誤，並記錄其詳細資料。

> [!NOTE]
> 檢查此教學課程中的資訊是最有用，如果您需要唯一或自訂的方式處理未處理例外狀況。 在您只需要記錄的例外狀況，並通知開發人員的情況下，使用錯誤記錄程式庫是的方式。 下面兩個教學課程提供兩個這類程式庫的概觀。


## <a name="executing-code-when-theerrorevent-is-raised"></a>執行程式碼時`Error`就會引發事件

事件會提供物件的訊號，已發生一些有趣的東西，以及要在回應中執行程式碼的另一個物件的機制。 ASP.NET 開發人員會習慣思考事件。 如果您想要的訪客按一下特定的按鈕時執行一些程式碼，您會建立事件處理常式，按鈕`Click`事件並將程式碼放。 假設 ASP.NET 執行階段引發其[`Error`事件](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)每次處理的例外狀況發生時，它會依照，記錄錯誤的詳細資料的程式碼將會進入事件處理常式中。 如何建立事件處理常式，但`Error`事件嗎？

`Error`事件是在許多事件的其中一個[`HttpApplication`類別](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)，會在 HTTP 管線中的特定階段上引發要求的存留期間。 例如，`HttpApplication`類別的[`BeginRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx); 的每個要求開頭引發其[`AuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)要求者所識別的安全性模組時引發。 這些`HttpApplication`事件讓網頁開發人員在要求的存留期執行的各個點上的自訂邏輯方式。

事件處理常式`HttpApplication`事件可以放在名為的特殊檔案`Global.asax`。 若要建立此檔案在您的網站中，將新的項目加入至使用通用應用程式類別樣板的名稱您網站的根目錄`Global.asax`。

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**圖 1**： 新增`Global.asax`您 Web 應用程式  
 ([按一下以檢視完整大小的影像](processing-unhandled-exceptions-vb/_static/image3.png))

內容和結構`Global.asax`稍微根據您是否使用 Web 應用程式專案 (WAP) 或網站專案 (WSP) Visual Studio 所建立的檔案而有所不同。 與 WAP，`Global.asax`實作為兩個不同的檔案-`Global.asax`和`Global.asax.vb`。 `Global.asax`檔案包含任何項目，但`@Application`指示詞參考`.vb`檔案; 事件中所定義的感興趣的處理常式`Global.asax.vb`檔案。 WSPs，建立單一檔案， `Global.asax`，且事件處理常式已定義在`<script runat="server">`區塊。

`Global.asax` WAP 中由 Visual Studio 通用應用程式類別範本檔案包含名為的事件處理常式`Application_BeginRequest`， `Application_AuthenticateRequest`，和`Application_Error`，這是事件處理常式，如`HttpApplication`事件`BeginRequest`， `AuthenticateRequest`，和`Error`分別。 另外還有名為事件處理常式`Application_Start`， `Session_Start`， `Application_End`，和`Session_End`，這是 web 應用程式啟動時，當新的工作階段啟動時，應用程式結束時，所引發的事件處理常式，並在工作階段結束時，分別。 `Global.asax`由 Visual Studio 建立 WSP 檔案只包含`Application_Error`， `Application_Start`， `Session_Start`， `Application_End`，和`Session_End`事件處理常式。

> [!NOTE]
> 部署 ASP.NET 應用程式時，您需要將複製`Global.asax`檔案到生產環境。 `Global.asax.vb` WAP 中建立的檔案不需要複製到生產環境，因為這段程式碼會編譯專案的組件。


Visual Studio 通用應用程式類別範本所建立的事件處理常式並不詳盡。 您可以加入事件處理常式的任何`HttpApplication`藉由命名事件處理常式的事件`Application_EventName`。 例如，您可以加入下列程式碼加入`Global.asax`檔案以建立事件處理常式[`AuthorizeRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

同樣地，您可以移除任何事件處理常式的全域應用程式類別範本所建立不需要用到。 此教學課程中，我們只需要的事件處理常式`Error`事件; 逕行移除的其他事件處理常式`Global.asax`檔案。

> [!NOTE]
> *HTTP 模組*提供另一種方式來定義事件處理常式`HttpApplication`事件。 HTTP 模組會建立成個別的類別程式庫可以直接在 web 應用程式專案內放置或來區隔的類別檔案。 它們可以分割成的類別庫，因為 HTTP 模組提供更為彈性和可重複使用模型來建立`HttpApplication`事件處理常式。 而`Global.asax`檔案是特定 web 應用程式所在的位置，HTTP 模組可以編譯為組件，此時若要將 HTTP 模組加入至網站很簡單，卸除組件中`Bin`資料夾和登錄中的模組`Web.config`。 本教學課程不查看建立和使用 HTTP 模組，但在下列兩個教學課程中使用的兩個錯誤記錄程式庫會實作為 HTTP 模組。 如需更多背景之優點的 HTTP 模組，請參閱[使用 HTTP 模組和處理常式，以建立隨插即用的 ASP.NET 元件](https://msdn.microsoft.com/library/aa479332.aspx)。


## <a name="retrieving-information-about-the-unhandled-exception"></a>擷取未處理的例外狀況的相關資訊

現在我們有的 Global.asax 檔案`Application_Error`事件處理常式。 此事件處理常式都會執行我們需要通知錯誤的開發人員，並記錄其詳細資料。 若要完成這些工作，我們必須先決定所引發的例外狀況的詳細資料。 使用伺服器物件[`GetLastError`方法](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx)擷取詳細資料的未處理的例外狀況造成`Error`引發事件。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

`GetLastError`方法會傳回型別的物件`Exception`，這是.NET Framework 中的所有例外狀況的基底類型。 不過，上述程式碼我轉型所傳回的例外狀況物件`GetLastError`到`HttpException`物件。 如果`Error`因為例外狀況擲回了 ASP.NET 資源的處理期間所擲回的例外狀況會包裝內則會引發事件`HttpException`。 若要取得實際的例外狀況，導致錯誤事件使用`InnerException`屬性。 如果`Error`已引發事件，因為 HTTP 為基礎的例外狀況，例如不存在的網頁要求`HttpException`擲回，但它並沒有內部的例外狀況。

下列程式碼會使用`GetLastErrormessage`擷取觸發的例外狀況資訊`Error`事件，儲存`HttpException`在名為`lastErrorWrapper`。 它接著會儲存類型、 訊息與原始的例外狀況堆疊追蹤中三個字串變數，檢查是否`lastErrorWrapper`的實際的例外狀況會觸發`Error`事件 （如果是以 HTTP 為基礎的例外狀況），或者它是只是處理要求時擲回的例外狀況的包裝函式。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

此時，您會有您需要撰寫程式碼，將記錄到資料庫資料表中的例外狀況詳細資料的所有資訊。 您可以建立資料庫資料表與資料行，每個感興趣的類型、 訊息、 堆疊追蹤和等等的有用的其他資訊，例如要求網頁的 URL 和目前登入的使用者名稱及錯誤詳細資料。 在`Application_Error`事件處理常式接著連接到資料庫，並將記錄插入資料表。 同樣地，您可以新增要警示的錯誤，透過電子郵件的開發人員的程式碼。

下面兩個教學課程中檢查錯誤記錄程式庫會提供根據預設，這類功能，因此不需要建置此錯誤記錄和通知自己。 不過，為了說明，`Error`就會引發事件，`Application_Error`事件處理常式可以用來記錄錯誤詳細資料和通知開發人員，讓我們加入程式碼發生錯誤時通知開發人員。

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>發生未處理的例外狀況時通知開發人員

在生產環境中發生未處理的例外狀況時務必提醒開發小組，讓它們可以評估錯誤，並判斷必須採取的動作。 比方說，如果沒有中連接到資料庫，則您必須為 double 的錯誤檢查您的連接字串，而且可能是，使用虛擬主機公司提出支援票證。 如果例外狀況發生的程式設計錯誤，可能需要額外的程式碼或驗證邏輯加入至預防這類錯誤的。

.NET Framework 中的類別[`System.Net.Mail`命名空間](https://msdn.microsoft.com/library/system.net.mail.aspx)輕鬆傳送電子郵件。 [ `MailMessage`類別](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx)代表電子郵件訊息，並具有屬性，例如`To`， `From`， `Subject`， `Body`，和`Attachments`。 `SmtpClass`用來傳送`MailMessage`物件使用指定的 SMTP 伺服器，則可以透過程式設計方式或以宣告方式在指定的 SMTP 伺服器設定[`<system.net>`元素](https://msdn.microsoft.com/library/6484zdc1.aspx)中`Web.config file`。 如需有關傳送電子郵件中的 ASP.NET 應用程式的訊息簽出我的文件： [ASP.NET 中的 傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)，而[System.Net.Mail 常見問題集](http://systemnetmail.com/)。

> [!NOTE]
> `<system.net>`元素包含所使用的 SMTP 伺服器設定`SmtpClient`類別時傳送電子郵件。 虛擬主機公司可能會有您可以使用從您的應用程式傳送電子郵件的 SMTP 伺服器。 您應該使用 web 應用程式中的 SMTP 伺服器設定的詳細資訊，請參閱 web 主機的支援 > 一節。


將下列程式碼加入`Application_Error`傳送開發人員的電子郵件，發生錯誤時的事件處理常式：

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

相當冗長，上述程式碼時，大量建立顯示的 HTML 傳送給開發人員的電子郵件中。 程式碼參考開始`HttpException`傳回`GetLastError`方法 (`lastErrorWrapper`)。 實際要求所引發的例外狀況透過擷取`lastErrorWrapper.InnerException`和指派給變數`lastError`。 類型、 訊息和堆疊追蹤資訊會從`lastError`並儲存在三個字串變數。

下一步`MailMessage`名為物件`mm`建立。 電子郵件內文是 HTML 格式，並顯示要求網頁的 URL，目前登入的使用者，以及例外狀況 （類型、 訊息和堆疊追蹤） 的相關資訊的名稱。 其中一個最有趣的事情有關`HttpException`類別是您可以產生用來建立例外狀況詳細資料黃色螢幕的死亡 (YSOD) 藉由呼叫的 HTML [GetHtmlErrorMessage 方法](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx)。 這個方法是此處用來擷取例外狀況詳細資料 YSOD 標記，並將它加入做為附件的電子郵件。 一個單字，需要注意的事項： 如果例外狀況，觸發`Error`事件未以 HTTP 為基礎的例外狀況 （例如不存在的網頁要求） 則`GetHtmlErrorMessage`方法會傳回`null`。

最後一個步驟是傳送`MailMessage`。 這樣做，建立新`SmtpClient`方法，並呼叫其`Send`方法。

> [!NOTE]
> 在 web 應用程式中使用此程式碼之前您要變更中的值`ToAddress`和`FromAddress`常數support@example.com錯誤通知電子郵件應該傳送至和來自任何電子郵件地址。 您還需要指定 SMTP 伺服器設定中的`<system.net>`一節中`Web.config`。 請參閱您的 web 主機提供者，來判斷要使用的 SMTP 伺服器設定。


這個程式碼的位置錯誤每當開發人員會傳送電子郵件訊息，摘要說明此錯誤，並包含 YSOD。 在先前的教學課程示範的執行階段錯誤是前往 Genre.aspx，傳入無效的`ID`值透過查詢字串，例如`Genre.aspx?ID=foo`。 瀏覽的頁面`Global.asax`就地檔案會產生相同的使用者經驗，如在先前的教學課程-在開發環境中您必須繼續時，您就可在生產環境，請參閱例外狀況詳細資料黃色螢幕的死，請參閱自訂錯誤網頁。 除了這個現有的行為，開發人員會傳送電子郵件。

**圖 2**顯示造訪時收到的電子郵件`Genre.aspx?ID=foo`。 電子郵件本文摘要說明例外狀況資訊，而`YSOD.htm`附件會顯示例外狀況詳細資料 YSOD 中所顯示的內容 (請參閱**圖 3**)。

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**圖 2**： 開發人員處理的例外狀況時傳送電子郵件通知  
 ([按一下以檢視完整大小的影像](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**圖 3**： 電子郵件通知中包含例外狀況詳細資料做為附件 YSOD  
 ([按一下以檢視完整大小的影像](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>如何使用自訂錯誤網頁嗎？

本教學課程示範了如何使用`Global.asax`和`Application_Error`事件處理常式來處理的例外狀況發生時執行程式碼。 具體來說，我們要通知的錯誤，開發人員使用此事件處理常式我們可以擴充它也在資料庫中記錄錯誤的詳細資料。 與否`Application_Error`事件處理常式不會影響使用者的體驗。 他們仍然看到設定的錯誤頁面上，錯誤詳細資料 YSOD、 執行階段錯誤 YSOD 中，或自訂錯誤網頁。

很自然想知道是否`Global.asax`檔案和`Application_Error`事件時，需要使用自訂錯誤網頁。 發生錯誤時顯示自訂錯誤網頁的使用者因此為什麼無法我們放通知開發人員和錯誤詳細資料記錄到自訂錯誤網頁的程式碼後置類別的程式碼？ 您當然可以自訂錯誤網頁的程式碼後置類別加入程式碼時您沒有存取權所觸發的例外狀況的詳細資料`Error`事件時使用先前的教學課程中我們已探索的技巧。 呼叫`GetLastError`從自訂錯誤頁面的方法會傳回`Nothing`。

此行為的原因是因為自訂錯誤網頁達到透過重新導向。 當未處理的例外狀況會到達 ASP.NET 執行階段 ASP.NET 引擎引發其`Error`事件 (其中執行`Application_Error`事件處理常式)，然後*重新導向*使用者自訂錯誤頁面，藉由發出`Response.Redirect(customErrorPageUrl)`. `Response.Redirect`方法將回應傳送到用戶端使用 HTTP 302 狀態程式碼中，指示來要求新的 URL，也就是自訂錯誤網頁瀏覽器。 然後用瀏覽器自動要求新的頁面。 您所見，從頁面個別要求的自訂錯誤網頁，因為瀏覽器的網址列變更為自訂錯誤頁面 URL 而產生錯誤 (請參閱**圖 4**)。

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**圖 4**： 發生錯誤時瀏覽器重新導向至自訂錯誤頁面 URL  
 ([按一下以檢視完整大小的影像](processing-unhandled-exceptions-vb/_static/image12.png))

結果是未處理的例外狀況的發生位置要求結束時，伺服器會回應 HTTP 302 重新導向。 自訂錯誤網頁的後續要求是全新的要求。此時 ASP.NET 引擎會已捨棄錯誤訊息和此外，並沒有上一個要求中未處理的例外狀況關聯的自訂錯誤網頁的新要求的方式。 這就是為什麼`GetLastError`傳回`null`從自訂錯誤網頁呼叫時。

不過，很可能有相同造成錯誤的要求時執行的自訂錯誤頁面。 [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx)方法將執行轉移至指定的 URL，而且會處理相同要求中。 您無法移動程式碼中`Application_Error`事件處理常式，加入自訂錯誤網頁的程式碼後置類別，並取代在`Global.asax`為下列程式碼：

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

現在當未處理的例外狀況，就會發生`Application_Error`事件處理常式會將控制權傳輸至適當的自訂錯誤網頁的 HTTP 狀態碼為基礎。 自訂錯誤頁面已傳輸控制項，因為已透過未處理例外狀況資訊的存取權`Server.GetLastError`及可通知開發人員的錯誤和記錄其詳細資料。 `Server.Transfer`呼叫停止中將使用者重新導向至自訂錯誤網頁的 ASP.NET 引擎。 相反地，就會傳回自訂錯誤網頁的內容，做為頁面產生錯誤的回應。

## <a name="summary"></a>總結

ASP.NET 執行階段中的 ASP.NET web 應用程式發生未處理的例外狀況時引發`Error`事件會顯示設定的錯誤頁面。 我們會通知開發人員的錯誤，記錄其詳細資料，或以其他方式，藉由建立錯誤事件的事件處理常式處理它。 有兩種方式來建立事件處理常式`HttpApplication`事件喜歡`Error`： 在`Global.asax`檔案或 HTTP 模組。 本教學課程示範了如何建立`Error`中的事件處理常式`Global.asax`通知錯誤的開發人員透過電子郵件訊息的檔案。

建立`Error`事件處理常式是您需要唯一或自訂的方式處理未處理例外狀況時相當實用。 不過，建立您自己的`Error`事件處理常式記錄的例外狀況，或通知開發人員不是最有效率的使用中的時間因為已經有免費且使用簡單的錯誤記錄程式庫可能是安裝程式會在幾分鐘的時間。 下面兩個教學課程會檢查兩個這類程式庫。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET HTTP 模組和 HTTP 處理常式概觀](https://support.microsoft.com/kb/307985)
- [依正常程序回應未處理的例外狀況-處理未處理例外狀況](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication`類別與 ASP.NET 應用程式物件](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP 處理常式和 ASP.NET 中的 HTTP 模組](http://www.15seconds.com/Issue/020417.htm)
- [在 ASP.NET 中傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [了解`Global.asax`檔案](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [使用 HTTP 模組和處理常式來建立隨插即用的 ASP.NET 元件](https://msdn.microsoft.com/library/aa479332.aspx)
- [使用 ASP.NET`Global.asax`檔案](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [使用`HttpApplication`執行個體](https://msdn.microsoft.com/library/a0xez8f2.aspx)

>[!div class="step-by-step"]
[上一頁](displaying-a-custom-error-page-vb.md)
[下一頁](logging-error-details-with-asp-net-health-monitoring-vb.md)
