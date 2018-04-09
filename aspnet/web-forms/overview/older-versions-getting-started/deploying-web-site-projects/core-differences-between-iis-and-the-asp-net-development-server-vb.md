---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: 核心 IIS 和 ASP.NET 程式開發伺服器 (VB) 之間的差異 |Microsoft 文件
author: rick-anderson
description: 測試的 ASP.NET 應用程式在本機，機率是您使用 ASP.NET 開發 Web 伺服器。 不過，生產性網站是最有可能 pow...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 47b1959f9b92d161da0476b274c8154333ad80dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>核心 IIS 與 ASP.NET 程式開發伺服器 (VB) 之間的差異
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip)或[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> 測試的 ASP.NET 應用程式在本機，機率是您使用 ASP.NET 開發 Web 伺服器。 不過，生產性網站是最可能的電源的 IIS。 有一些差異如何這些 web 伺服器處理要求，而且這些差異可以重要的結果。 本教學課程會探討一些更密切關聯的差異。


## <a name="introduction"></a>簡介

每當使用者造訪 ASP.NET 應用程式他瀏覽器會將要求傳送至網站。 挑選該要求 web 伺服器軟體，而該控制項會協調使用 ASP.NET 執行階段產生，並傳回要求之資源的內容。 [**我**網際網路**我**若資訊**S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services)會提供以網際網路為基礎的通用功能，如的服務套件Windows 伺服器。 IIS 是在實際執行環境; 的 ASP.NET 應用程式的最常用的 web 伺服器最有可能是由您的 web 主機提供者來服務您的 ASP.NET 應用程式的 web 伺服器軟體。 IIS 也可用來當作 web 伺服器軟體，在開發環境中，雖然這項功能需要安裝 IIS 並正確加以設定。


ASP.NET 程式開發伺服器會替代 web 伺服器 選項，開發環境。它隨附於並整合到 Visual Studio。 除非已將 web 應用程式設定為使用 IIS，ASP.NET 程式開發伺服器會自動啟動且做為 web 伺服器使用您瀏覽網頁時 Visual Studio 中的第一次。 示範 web 應用程式，我們建立回[*決定需要的檔案部署*](determining-what-files-need-to-be-deployed-vb.md)教學課程中所未設定為使用 IIS 的兩個檔案系統為基礎的 web 應用程式。 因此，當瀏覽的其中一個是從 Visual Studio 中的這些網站會使用 ASP.NET 程式開發伺服器。

在理想的世界裡開發和生產環境就會相同。 不過，如前述教學課程所述不罕見的環境中有不同的組態設定。 在環境中使用不同的網頁伺服器軟體加入部署應用程式時必須納入考量的另一個變數。 本教學課程涵蓋 IIS 和 ASP.NET 程式開發伺服器之間的主要差異。 因為這些差異有的案例，其中執行都在開發環境中的程式碼擲回例外狀況，或在生產環境中執行時，表現。

## <a name="security-context-differences"></a>安全性內容的差異

每當 web 伺服器軟體會處理連入要求它產生關聯的要求與特定安全性內容。 此資訊安全內容資訊可由作業系統來判斷哪些動作所允許的要求。 例如，ASP.NET 網頁可能包含一些訊息記錄至磁碟上的檔案的程式碼。 為了讓這個 ASP.NET 頁面，以執行不會發生錯誤，必須有適當的檔案系統層級權限，也就是寫入該檔案的權限的安全性內容。

ASP.NET 程式開發伺服器會將內送要求與目前登入使用者的安全性內容產生關聯。 如果登入您的桌面以系統管理員，則 ASP.NET 程式開發伺服器所服務的 ASP.NET 頁面將有相同的存取權限，以系統管理員。 不過，ASP.NET 要求處理 iis 與相關聯的特定機器帳戶。 根據預設，網路服務的電腦帳戶都使用 iis 版本 6 和 7，不過您的 web 主機提供者可能針對每個客戶設定唯一的帳戶。 而且，您的 web 主機提供者可能提供有限的權限到此電腦帳戶。 最後結果就是您可能不會發生錯誤，在開發環境中，執行但產生授權相關例外狀況時裝載在實際執行環境中的網頁。

為了示範這種錯誤類型的我已經建立之活頁簿檢閱網站中的其他人建立儲存的最新的日期和時間的磁碟上的檔案的頁面檢視*教導您自己 ASP.NET 3.5 24 小時內*檢閱。 若要跟著做，請開啟`~/Tech/TYASP35.aspx`頁面上，並加入下列程式碼加入`Page_Load`事件處理常式：

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> [ `File.WriteAllText`方法](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx)建立新檔案，如果它不存在，然後寫入指定的內容。 如果檔案已經存在，則會覆寫它的現有內容。


接下來，請瀏覽*教導您自己 ASP.NET 3.5 24 小時內*使用 ASP.NET 程式開發伺服器開發環境中的活頁簿檢閱頁面。 假設您已登入您的電腦帳戶具有足夠的權限，即可建立和修改文字檔案中的 web 應用程式的根目錄書籍檢閱會顯示與之前相同，但每次頁面瀏覽的日期和時間以及使用者的 IP 位址儲存在`LastTYASP35Access.txt`檔案。 您的瀏覽器指向這個檔案。您應該會看到類似於圖 1 所示的訊息。


[![文字檔案包含的最後一個日期和時間書籍檢閱瀏覽&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**圖 1**： 文字檔案包含的最後一個日期和時間書籍檢閱瀏覽 ([按一下以檢視完整大小的影像](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


部署到生產環境 web 應用程式，然後瀏覽託管*教導您自己 ASP.NET 3.5 24 小時內*書籍 [檢閱] 頁面。 此時您應該看到活頁簿的 [檢閱] 頁面為 normal 或顯示在圖 2 中的錯誤訊息。 有些 web 主機提供者授與匿名 ASP.NET 電腦帳戶，大小寫的頁面將會運作無誤的寫入權限。 如果，不過，您的 web 主機提供者會禁止匿名帳戶的寫入權限則[`UnauthorizedAccessException`例外狀況](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx)時引發`TYASP35.aspx`網頁嘗試寫入目前的日期和時間`LastTYASP35Access.txt`檔案。


[![IIS 所使用的預設電腦帳戶沒有權限可以寫入檔案系統](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**圖 2**: 預設電腦帳戶供 IIS 不會不具有權限來寫入至檔案系統 ([按一下以檢視完整大小的影像](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


好消息是大部分 web 主機提供者具有某種權限 」 工具，可讓您在您的網站中指定檔案系統權限。 匿名 ASP.NET 帳戶寫入存取權授與目錄的根目錄，然後再次瀏覽活頁簿的 [檢閱] 頁面。 （如果需要請連絡您的 web 主機提供者尋求協助如何授與預設的 ASP.NET 帳戶的寫入權限。）此頁面應載入不會發生錯誤的時間和`LastTYASP35Access.txt`應該成功建立檔案。

Take 離開以下是，因為 ASP.NET 程式開發伺服器會與 IIS 在不同的安全性內容下運作，所以有可能的讀取或寫入至檔案系統中，ASP.NET 頁面，從讀取或寫入至 Windows 事件記錄檔中，或讀取或寫入至 Windows 登錄會開發上正常運作，但會產生實際執行上的例外狀況。 當建置 web 應用程式，將會部署至共用 web 裝載環境時，請勿讀取或寫入事件記錄檔或 Windows 登錄。 也請注意，讀取或寫入至檔案系統，因為您可能需要授與讀取和寫入適當的資料夾上的權限，在生產環境中任何 ASP.NET 網頁。

## <a name="differences-on-serving-static-content"></a>提供靜態內容的差異

IIS 與 ASP.NET 程式開發伺服器之間的另一個核心差異是靜態內容的要求的處理方式。 進入 ASP.NET 程式開發伺服器的 ASP.NET 網頁、 影像或 JavaScript 檔案的每個要求會由 ASP.NET 執行階段處理。 根據預設，IIS 只會叫用 ASP.NET 執行階段，當要求進入 ASP.NET 資源，例如 ASP.NET web 網頁、 Web 服務等等。 IIS 所擷取的靜態內容-影像、 CSS 檔案、 JavaScript 檔案、 PDF 檔案，ZIP 檔案，以及 like-要求不需要 ASP.NET 執行階段的涉入。 （它可指示 IIS 以使用 ASP.NET 執行階段，當服務靜態內容，請參閱 < 執行表單型驗證和 iis 7 的靜態檔案的 URL 驗證 > 章節，在本教學課程，如需詳細資訊）。

ASP.NET 執行階段會執行一些步驟來產生要求的內容，包括驗證 （識別要求者） 以及授權 （決定如果要求者有權檢視要求的內容）。 受歡迎的形式的驗證是*表單型驗證*，在使用者識別他們的認證，通常使用者名稱和密碼為輸入的文字方塊，在網頁上。 網站會儲存在時驗證其認證，*驗證票證*上使用者的瀏覽器中，每個後續要求傳送到網站以及什麼用來驗證使用者的 cookie。 此外，它可能會指定*URL 授權*規則，會指出哪些使用者可以或無法存取特定的資料夾。 許多的 ASP.NET 網站會使用表單型驗證和 URL 授權，以支援使用者帳戶，以及定義網站的只是已驗證的使用者或屬於特定角色的使用者可存取的部分。

> [!NOTE]
> 如 ASP 詳盡。網路的表單型驗證、 URL 授權和其他使用者帳戶相關的功能，請務必查看我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。


請考慮網站支援使用表單型驗證的使用者帳戶，有的資料夾，使用 URL 授權設定為只允許已驗證的使用者。 假設此資料夾包含 ASP.NET 網頁，PDF 檔案，其目的是僅已驗證的使用者可以檢視這些 PDF 檔案。

如果在訪客嘗試直接在其瀏覽器的網址列中輸入 URL 檢視其中一個 PDF 檔案會怎樣？ 若要了解，讓我們來建立新的資料夾中活頁簿檢閱站台、 加入一些 PDF 檔案，和將網站設定為使用 URL 授權来禁止匿名使用者瀏覽此資料夾。 如果您要下載示範應用程式中您會看到 建立資料夾，稱為`PrivateDocs`並新增 PDF 從我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)（如何適當 ！）。 `PrivateDocs`資料夾還包含`Web.config`檔案，指定要拒絕匿名使用者的 URL 授權規則：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

最後，設定 web 應用程式藉由更新 Web.config 檔案的根目錄中，使用表單型驗證取代：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

成為：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

使用 ASP.NET 程式開發伺服器，請瀏覽的網站，並輸入 PDF 檔案，您的瀏覽器網址列中的其中一個直接的 URL。 如果您已下載此教學課程中的 URL 應該類似相關聯的網站： `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

在網址列輸入此 URL，讓瀏覽器將要求傳送至檔案的 ASP.NET 程式開發伺服器。 ASP.NET 程式開發伺服器遞交 ASP.NET 執行階段進行處理的要求。 因為我們尚未登入，而且`Web.config`中`PrivateDocs`資料夾已設定為拒絕匿名存取，ASP.NET 執行階段自動我們將重新導向登入頁面， `Login.aspx` （請參閱圖 3）。 ASP.NET 時將使用者重新導向至登入頁面，包含`ReturnUrl`querystring 參數，指出頁面使用者嘗試檢視。 已成功登入使用者之後可回到此頁面。


[![未經授權的使用者會自動重新導向至登入頁面](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**圖 3**： 未經授權的使用者會自動重新導向至登入頁面 ([按一下以檢視完整大小的影像](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


現在讓我們來看看這在生產環境上的運作方式。 部署應用程式，並在 Pdf 的其中一個輸入直接`PrivateDocs`在生產環境中的資料夾。 這會提示您瀏覽器以傳送檔案的要求 IIS。 因為要求的靜態檔案時，IIS 會擷取並傳回檔案而不叫用 ASP.NET 執行階段。 如此一來，沒有任何 URL 授權檢查並未執行。真地私人 PDF 內容都知道檔案的直接 URL 的任何人可存取。


[![匿名使用者可以輸入檔案的直接 URL 下載私用 PDF 檔案](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**圖 4**： 匿名使用者可以下載私人 PDF 檔案由輸入直接 URL 檔案 ([按一下以檢視完整大小的影像](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>靜態檔案與 IIS 7 上執行表單型驗證和 URL 驗證

有幾個可用來防止未經授權的使用者的靜態內容的技術。 導入的 IIS 7*整合的管線*，其中 marries IIS 的 ASP.NET 執行階段的工作流程的工作流程。 簡單地說，您可以指示 IIS 來叫用 ASP.NET 執行階段驗證和授權模組 （包括像是 PDF 檔案的靜態內容） 的所有連入要求。 請連絡您的 web 主機提供者，了解如何設定您的網站使用整合式的管線。

後 IIS 已設定為使用整合式的管線新增到下列標記`Web.config`的根目錄中的檔案：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

這個標記會指示使用 ASP.NET 為基礎的驗證和授權模組的 IIS 7。 重新部署您的應用程式，然後重新瀏覽 PDF 檔案。 當 IIS 會處理要求這次它可讓 ASP.NET 執行階段驗證及授權邏輯來檢查要求。 因為只有驗證的使用者有權檢視的內容`PrivateDocs`匿名訪客 資料夾，會自動重新導向至登入頁面 （請參閱上一步圖 3）。

> [!NOTE]
> 如果您的 web 主機提供者仍在使用 IIS 6，您無法使用整合式的管線功能。 解決方法是將私人文件放在禁止的 HTTP 存取的資料夾 (例如`App_Data`)，然後建立頁面，即可提供這些文件。 此頁面可能呼叫`GetPDF.aspx`，並傳遞 PDF 透過查詢字串參數的名稱。 `GetPDF.aspx`頁面會先確認使用者有權檢視檔案，如果是的話，會使用[ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx)方法，以回到要求的用戶端傳送要求的 PDF 檔案的內容。 如果您不想啟用整合式的管線，則這項技術也可以運作適用於 IIS 7。


## <a name="summary"></a>總結

裝載在實際執行環境中的 web 應用程式使用 Microsoft IIS web 伺服器軟體。 在開發環境中，不過，應用程式可能裝載使用 IIS 或 ASP.NET 程式開發伺服器。 在理想情況下，相同的 web 伺服器軟體應透過兩個環境中因為使用不同的軟體會在混合中加入另一個變數。 不過，易於使用的 ASP.NET 程式開發伺服器使其成為吸引人的選擇中開發環境。 好消息是只有幾個基本差異 IIS 和 ASP.NET 程式開發伺服器，並注意這些差異如果您可以採取步驟以協助確保應用程式的運作方式，和相同的函數方式而定環境。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [與 IIS 7.0 ASP.NET 整合](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [與所有類型的內容，在 IIS 7 上使用 ASP.NET 論壇驗證](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx)（影片）
- [在 Visual Web Developer web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [上一頁](common-configuration-differences-between-development-and-production-vb.md)
> [下一頁](deploying-a-database-vb.md)
