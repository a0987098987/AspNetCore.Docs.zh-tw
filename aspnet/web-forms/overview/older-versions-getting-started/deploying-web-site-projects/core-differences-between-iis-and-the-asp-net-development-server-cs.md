---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: 核心 IIS 和 ASP.NET Development Server (C#) 之間的差異 |Microsoft Docs
author: rick-anderson
description: 測試在本機的 ASP.NET 應用程式，可能是您使用 ASP.NET 開發 Web 伺服器。 不過，生產性網站是最有可能 pow...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 50f3115fd124cf058e70cab71b8c0168d242ff21
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363412"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>IIS 與 ASP.NET 程式開發伺服器中 (C#) 的核心差異
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip)或[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> 測試在本機的 ASP.NET 應用程式，可能是您使用 ASP.NET 開發 Web 伺服器。 不過，生產性網站是最有可能支援的 IIS。 有一些差異這些 web 伺服器如何處理要求，而且這些差異可以有重要的結果。 本教學課程將探討一些更密切關聯的差異。


## <a name="introduction"></a>簡介

每當使用者造訪 ASP.NET 應用程式其瀏覽器會傳送要求至網站。 該要求被由協調 ASP.NET 執行階段產生，並傳回所要求資源的內容與 web 伺服器軟體。 [**我**網際網路**我**nformation **S**理 (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services)是一套服務提供以網際網路為基礎的常見功能，如Windows 伺服器。 IIS 是在生產環境; 中的 ASP.NET 應用程式的最常用的 web 伺服器它很可能是由您的 web 主機提供者來服務您的 ASP.NET 應用程式的 web 伺服器軟體。 IIS 也可用來當做 web 伺服器軟體，在開發環境中，雖然這需要安裝 IIS 並正確加以設定。


ASP.NET 程式開發伺服器是針對開發環境中，替代 web 伺服器 選項它隨附並整合至 Visual Studio。 除非 web 應用程式已設定為使用 IIS，ASP.NET Development Server 會自動啟動及做為 web 伺服器瀏覽網頁從 Visual Studio 中的第一次。 我們在上一步建立的示範 web 應用程式[*判斷哪些檔案需要部署*](determining-what-files-need-to-be-deployed-cs.md)教學課程中所未設定為使用 IIS 的兩個檔案系統為基礎的 web 應用程式。 因此，瀏覽從 Visual Studio 內的這些網站時，會使用 ASP.NET 程式開發伺服器。

在完美的世界中開發和生產環境就會相同。 不過，如先前的教學課程所述不常見的環境中有不同的組態設定。 在環境中使用不同的 web 伺服器軟體加上部署應用程式時必須列入考量的另一個變數。 本教學課程涵蓋 IIS 和 ASP.NET 程式開發伺服器的主要差異。 因為有這些差異有能夠完美地執行在開發環境中的程式碼會擲回例外狀況，或在生產環境中執行時，表現的情況。

## <a name="security-context-differences"></a>安全性內容的差異

只要 web 伺服器軟體會處理連入要求會將特定的安全性內容和關聯該要求。 此資訊安全內容資訊由作業系統用以判斷哪些動作所允許的要求。 例如，ASP.NET 網頁可能包含一些訊息記錄至磁碟上的檔案的程式碼。 為了讓這個 ASP.NET 網頁的執行不會發生錯誤，必須有適當的檔案系統層級權限，也就是寫入該檔案的權限的安全性內容。

ASP.NET Development Server 會將連入要求關聯的目前登入使用者的安全性內容。 如果您到您的桌面以系統管理員身分登入，由 ASP.NET 程式開發伺服器的 ASP.NET 網頁會有相同的存取權限，以系統管理員身分。 不過，由 IIS 的 ASP.NET 要求會在特定的電腦帳戶相關聯。 根據預設，網路服務的電腦帳戶使用 IIS 6 和 7 版，雖然您的 web 主機提供者可能針對每個客戶設定唯一的帳戶。 除此之外，您的 web 主機提供者也許會給有限的權限到此電腦帳戶。 最後結果就是，您可能會有執行不會在開發環境中，發生錯誤，但在生產環境中產生的授權相關例外狀況時裝載的網頁。

為了示範這種錯誤類型的操作我的書籍評論網站中的磁碟上建立檔案，其中儲存的最新的日期和時間某人建立了一個頁面檢視*教導您自己 ASP.NET 3.5 24 小時內*檢閱。 若要跟著做，請開啟`~/Tech/TYASP35.aspx`頁面上，並新增下列程式碼`Page_Load`事件處理常式：

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> [ `File.WriteAllText`方法](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx)建立新的檔案，如果它不存在，然後將指定的內容寫入至它。 如果檔案已經存在，則會覆寫其現有內容。


接下來，請瀏覽*教導您自己 ASP.NET 3.5 24 小時內*在開發環境中使用 ASP.NET 程式開發伺服器的活頁簿檢閱頁面。 假設您已登入您的電腦帳戶具有足夠的權限，即可建立和修改的文字檔中的 web 應用程式的根目錄書籍評論會顯示與之前相同，但每次頁面瀏覽的日期和時間以及使用者的 IP 位址儲存在`LastTYASP35Access.txt`檔案。 將瀏覽器指向這個檔案中;您應該會看到類似 圖 1 中所示的訊息。


[![文字檔案包含的最後一個日期和時間瀏覽書籍評論](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**圖 1**： 文字檔案包含的最後一個日期和時間的書籍評論瀏覽 ([按一下以檢視完整大小的影像](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png))


部署至生產環境 web 應用程式，，然後瀏覽託管*教導您自己 ASP.NET 3.5 24 小時內*活頁簿 [檢閱] 頁面。 此時您應該看到活頁簿的 檢閱 頁面為標準模式 或 圖 2 所示的錯誤訊息。 某些 web 主機提供者授與匿名 ASP.NET 電腦帳戶，可以在其中案例頁面將會運作不會發生錯誤的 「 寫入 」 權限。 如果，不過，您的 web 主機提供者會禁止匿名帳戶的寫入權限就[`UnauthorizedAccessException`例外狀況](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx)時引發`TYASP35.aspx`網頁嘗試寫入目前的日期和時間`LastTYASP35Access.txt`檔案。


[![IIS 所使用的預設電腦帳戶並沒有寫入檔案系統權限](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**圖 2**: 預設電腦帳戶供 IIS 不會不具有權限來寫入至檔案系統 ([按一下以檢視完整大小的影像](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))


好消息是工具的，大部分的 web 主機提供者有某種形式的權限，可讓您指定檔案系統權限，在您的網站。 匿名 ASP.NET 帳戶寫入存取權授與的根目錄，然後重新檢視活頁簿的 [檢閱] 頁面。 （如有需要請連絡您的 web 主機提供者如需如何將預設 ASP.NET 帳戶的寫入權限授與協助。）此頁面應該載入不會發生錯誤的時間和`LastTYASP35Access.txt`應該已成功建立檔案。

這裡的重點是，因為 ASP.NET 程式開發伺服器的運作於 IIS 在不同的安全性內容下，就可以讀取或寫入至檔案系統中，在 ASP.NET 網頁，讀取或寫入至 Windows 事件記錄檔中，或讀取或寫入至 Windows 登錄會開發上如預期運作，但會產生在生產環境時的例外狀況。 當建置 web 應用程式，將會部署至共用 web 裝載環境時，請勿讀取或寫入事件記錄檔或 Windows 登錄。 也請注意，讀取或寫入至檔案系統，因為您可能需要授與讀取和寫入適當的資料夾上的權限，在生產環境中的任何 ASP.NET 頁面。

## <a name="differences-on-serving-static-content"></a>提供靜態內容上的差異

IIS 和 ASP.NET Development Server 之間的另一個核心差異是它們如何處理靜態內容的要求。 進入 ASP.NET Development Server，無論是針對 ASP.NET 網頁、 影像或 JavaScript 檔案，每個要求是由 ASP.NET 執行階段處理。 根據預設，IIS 只會叫用 ASP.NET 執行階段，當要求傳入對於 ASP.NET 資源，例如 ASP.NET 網頁、 Web 服務等等。 映像、 CSS 檔案、 JavaScript 檔案、 PDF 檔案、 ZIP 檔案，等位-的靜態內容的要求會在 ASP.NET 執行階段沒有擷取 iis。 （它可能會指示 IIS 可以使用 ASP.NET 執行階段提供靜態內容時，在本教學課程，如需詳細資訊，請參閱 < 執行表單型驗證和上與 IIS 7 的靜態檔案的 URL 驗證 」 一節）。

ASP.NET 執行階段會執行一些步驟來產生要求的內容，包括 （用來識別要求者） 的驗證和授權 （決定如果要求者有權檢視要求的內容）。 是受歡迎的形式的驗證*表單型驗證*，在使用者識別在文字方塊中輸入其認證-通常是使用者名稱和密碼-在網頁上。 在驗證其認證，網站會儲存*驗證票證*可用來驗證使用者的每個後續要求傳送到網站，使用者的瀏覽器的 cookie。 此外，它可能會指定*URL 授權*規則，以指定哪些使用者可以或無法存取特定的資料夾。 許多 ASP.NET 網站使用表單型驗證和授權 URL，以支援使用者帳戶，並定義網站的僅供已驗證的使用者或屬於特定角色的使用者存取的部分。

> [!NOTE]
> 針對 ASP 徹底的檢查。NET 的表單型驗證、 URL 授權和其他使用者帳戶的相關功能，請務必看看我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。


請考慮支援使用表單型驗證的使用者帳戶，且具有的資料夾，使用 URL 授權設定為只允許已驗證的使用者的網站。 假設此資料夾包含 ASP.NET 網頁，而 PDF 檔案和其意圖是只有經過驗證的使用者可以檢視這些 PDF 檔案。

如果在訪客嘗試直接在其瀏覽器的網址列中輸入 URL，其中一個 PDF 檔案檢視發生什麼事？ 若要了解，讓我們的書籍評論網站中建立新的資料夾、 新增一些的 PDF 檔案，，和將網站設定為使用 URL 授權來禁止匿名使用者瀏覽此資料夾。 如果您下載示範應用程式就會看到，我會建立名為的資料夾`PrivateDocs`並新增從 PDF 我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)（如何調整 ！）。 `PrivateDocs`資料夾也包含`Web.config`指定 URL 授權規則，拒絕匿名使用者的檔案：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

最後，我已設定 web 應用程式使用表單型驗證藉由更新`Web.config`根目錄中，檔案中的取代：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

成為：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

使用 ASP.NET 程式開發伺服器，請瀏覽的網站，並輸入 PDF 檔案，您的瀏覽器網址列中的其中一個直接的 URL。 如果您下載本教學課程中的 URL 應該看起來像相關聯的網站： `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

在網址列輸入此 URL 會導致將要求傳送至 ASP.NET Development Server，檔案瀏覽器。 ASP.NET 程式開發伺服器會送出 ASP.NET 執行階段進行處理的要求。 因為我們尚未登入，而且`Web.config`中`PrivateDocs`資料夾已設定為拒絕匿名存取，ASP.NET 執行階段將自動重新導向我們登入 頁面中， `Login.aspx` （請參閱 圖 3）。 當將使用者重新導向至登入頁面中，ASP.NET 包含`ReturnUrl`查詢字串參數，指出頁面的使用者嘗試檢視。 已成功登入使用者之後可以傳回至此頁面。


[![未經授權的使用者會自動重新導向至登入頁面](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**圖 3**： 未經授權的使用者會自動重新導向至登入頁面 ([按一下以檢視完整大小的影像](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))


現在讓我們來看看這在生產環境上的運作方式。 部署您的應用程式，並在 Pdf 的其中一個輸入的直接 URL`PrivateDocs`在生產環境中的資料夾。 這會提示您的瀏覽器傳送要求 IIS 的檔案。 因為靜態檔案要求時，IIS 會擷取，並將檔案傳回而不叫用 ASP.NET 執行階段。 如此一來，已執行浮點數。 任何 URL 授權檢查應該私用 PDF 的內容都知道檔案的直接 URL 的任何人可以存取的。


[![匿名使用者可以輸入檔案的直接 URL 下載私用的 PDF 檔案](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**圖 4**： 匿名使用者可以下載私用 PDF 檔案的輸入直接 URL 到檔案 ([按一下以檢視完整大小的影像](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>靜態檔案與 IIS 7 上執行表單型驗證和 URL 驗證

有幾個可用來防範未經授權的使用者的靜態內容的技術。 IIS 7 引進*整合式的管線*，它能結合 IIS 與 ASP.NET 執行階段的工作流程的工作流程。 簡單的說，您可以指示要叫用 ASP.NET 執行階段的驗證和授權模組 （包括 PDF 檔案之類的靜態內容） 的所有連入要求的 IIS。 請連絡您的 web 主機提供者，以了解如何將網站設定成使用整合式的管線。

整合式的管線 IIS 已設定為使用之後新增下列標記來`Web.config`的根目錄中的檔案：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

此標記會指示以使用以 ASP.NET 為基礎的驗證和授權模組的 IIS 7。 重新部署您的應用程式，然後重新瀏覽的 PDF 檔案。 此時，IIS 會處理要求時它會讓 ASP.NET 執行階段的驗證和授權邏輯來檢查要求。 因為只有已驗證的使用者有權檢視的內容`PrivateDocs`匿名訪客] 資料夾，會自動重新導向至登入頁面 （請參閱上一步 [圖 3）。

> [!NOTE]
> 如果您的 web 主機提供者仍在使用 IIS 6，您無法使用整合式的管線功能。 一個解決方法是在禁止的 HTTP 存取的資料夾中放入您私人的文件 (例如`App_Data`)，然後建立要做這些文件的頁面。 此頁面可能會呼叫`GetPDF.aspx`，並傳遞的 PDF，透過查詢字串參數的名稱。 `GetPDF.aspx`頁面會先確認使用者有權檢視的檔案，如果是的話，會使用[ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx)方法將傳回給提出要求的用戶端傳送要求的 PDF 檔案的內容。 如果您不想啟用整合式的管線，則這項技術也會適用於 IIS 7。


## <a name="summary"></a>總結

在生產環境中的 web 應用程式裝載於 Microsoft 的 IIS web 伺服器軟體。 在開發環境中，不過，應用程式可能裝載使用 IIS 或 ASP.NET 程式開發伺服器。 在理想情況下，相同的 web 伺服器軟體應該能在這兩種環境中因為使用不同的軟體在混合中加入另一個變數。 不過，容易使用的 ASP.NET 程式開發伺服器就很好的選擇開發環境中。 好消息是，只有幾個基本差異 IIS 和 ASP.NET Development Server，而且如果您知道這些差異的您可以採取步驟以協助確保應用程式的運作方式，且函式相同的方式無論環境。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [與 IIS 7.0 ASP.NET 整合](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [使用所有類型的內容，在 IIS 7 上使用 ASP.NET 論壇驗證](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx)（影片）
- [Visual Web Developer 中的 web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [上一頁](common-configuration-differences-between-development-and-production-cs.md)
> [下一頁](deploying-a-database-cs.md)
