---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: 表單驗證 (VB) 的概觀 |Microsoft Docs
author: rick-anderson
description: 在本教學課程中我們將會開啟從只討論實作;特別是，我們將探討實作表單驗證。 Web 應用程式 w 中...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: df8b0595cbc02a99a39c5b39be9ddb2e92bc34aa
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385887"
---
<a name="an-overview-of-forms-authentication-vb"></a>表單驗證 (VB) 的概觀
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip)或[下載 PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> 在本教學課程中我們將會開啟從只討論實作;特別是，我們將探討實作表單驗證。 我們一開始建構在此教學課程中的 web 應用程式會繼續在後續的教學課程，建置在當我們從簡單的表單驗證移至成員資格和角色。
> 
> 請參閱本主題中的 此影片以取得詳細資訊：[在 ASP.NET 中使用基本的表單驗證](# "using-basic-forms-authentication-in-aspnet")。


## <a name="introduction"></a>簡介

在 [前述教學課程](security-basics-and-asp-net-support-vb.md)我們討論了各種驗證、 授權及使用者帳戶所提供的選項 ASP.NET。 在本教學課程中我們將會開啟從只討論實作;特別是，我們將探討實作表單驗證。 我們一開始建構在此教學課程中的 web 應用程式會繼續在後續的教學課程，建置在當我們從簡單的表單驗證移至成員資格和角色。

本教學課程開頭的表單驗證工作流程，我們在接觸到先前的教學課程中的主題將深入探討。 接下來，我們將建立 ASP.NET 網站，以示範表單驗證的概念。 接下來，我們會將網站設定為使用表單驗證、 建立簡單的登入頁面，並了解如何判斷，程式碼中，是否驗證使用者時，如果是的話，使用者名稱它們來登入。

了解驗證工作流程，讓它在 web 應用程式，並建立登入] 和 [登出頁面是所有重要的步驟，建立支援使用者及帳戶驗證使用者，透過網頁的 ASP.NET 應用程式的表單。 因此-因為這些教學課程建置彼此-我建議您之前移到下一個即使您已取得體驗設定表單驗證，在過去的專案中，完成本教學課程中完整處理。

## <a name="understanding-the-forms-authentication-workflow"></a>了解表單驗證工作流程

當 ASP.NET 執行階段處理的要求是 ASP.NET 的資源，例如 ASP.NET 網頁或 ASP.NET Web 服務，要求就會引發的事件數目在其生命週期。 沒有在要求的要求已通過驗證且獲授權，在未處理的例外狀況等等的情況下所引發的事件時引發的非常開頭並結束時引發的事件。 若要查看事件的完整清單，請參閱[HttpApplication 物件事件](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx)。

*HTTP 模組*會執行其程式碼以回應要求的生命週期中的特定事件的 managed 的類別。 ASP.NET 隨附於多個執行基本工作，在幕後的 HTTP 模組。 兩個內建的 「 HTTP 」 模組，尤其是與我們的討論是：

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -藉由檢查表單驗證票證，通常會包含使用者的 cookie 集合中驗證使用者。 如果沒有表單驗證票證存在時，使用者是匿名的。
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -決定目前使用者是否獲授權存取要求的 URL。 此模組會諮詢應用程式的組態檔中指定的授權規則，以決定授權單位。 ASP.NET 也包含[FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) consulting 要求的檔案 Acl 決定授權單位。

FormsAuthenticationModule 嘗試驗證前執行 UrlAuthorizationModule （和 FileAuthorizationModule） 的使用者。 提出要求的使用者未獲授權存取要求的資源，如果授權模組會終止要求，並傳回[HTTP 401 未經授權](http://www.checkupdown.com/status/E401.html)狀態。 在 Windows 驗證的情況下，會傳回至瀏覽器的 HTTP 401 狀態。 此狀態碼會導致瀏覽器提示使用者輸入其認證，透過強制回應對話方塊。 使用表單驗證，不過，HTTP 401 未經授權的狀態是永遠不會傳送至瀏覽器因為 FormsAuthenticationModule 偵測到此狀態，並修改它改為將使用者重新導向至登入頁面 (透過[HTTP 302 重新導向](http://www.checkupdown.com/status/E302.html)狀態)。

登入頁面的責任是判斷使用者的認證是否有效以及，若是如此，若要建立表單驗證票證，並將使用者重新導向回到頁面他們嘗試瀏覽。 驗證票證會包含在後續的要求，該網站，FormsAuthenticationModule 用來識別使用者的頁面。


[![表單驗證工作流程](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**圖 01**: 表單驗證工作流程 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>在網頁瀏覽記住驗證票證

登入之後，表單驗證票證必須傳送至 web 伺服器上每個要求，讓使用者保持登入，瀏覽網站。 這通常被透過將驗證 ticket 放在使用者的 cookie 集合中。 [Cookie](http://en.wikipedia.org/wiki/HTTP_cookie)位於使用者的電腦上，傳輸的每個要求來建立 cookie 之網站的 HTTP 標頭中的小型文字檔案。 因此，一旦建立並儲存在瀏覽器的 cookie 中表單驗證票證，每個後續的瀏覽至該網站會傳送驗證票證，以及要求，藉此識別使用者。

> [!NOTE]
> 每個教學課程中使用的示範 web 應用程式是可供下載。 這個可下載的應用程式是以目標為.NET Framework 3.5 版的 Visual Web Developer 2008 建立的。 應用程式適用於.NET 3.5 為目標，因為其 Web.config 檔案會包含其他的 3.5 特有的組態項目。 長話短說，如果您有尚未安裝.NET 3.5，然後下載的 web 應用程式在電腦上未先移除 3.5 專屬標記從 Web.config，將無法運作。


Cookie 的其中一個層面是其到期時間，也就是瀏覽器會捨棄 cookie 的時間與日期。 表單驗證 cookie 過期時，使用者可以不再通過驗證，因此會變成匿名。 當使用者造訪從公用終端機中時，有可能是他們想要其到期時關閉其瀏覽器的驗證票證。 瀏覽時從家裡，不過，該相同使用者可能想要驗證票證，以記住跨瀏覽器會重新啟動，使它們並沒有重新登入每次造訪的網站。 這項決定通常會由 [記住我] 核取方塊在登入頁面上的表單中的使用者。 在步驟 3 中，我們將檢查如何實作登入頁面中的 [記住我] 核取方塊。 下列教學課程說明詳細的驗證票證逾時設定。

> [!NOTE]
> 可以用來登入網站的使用者代理程式可能不支援 cookie。 在此情況下，ASP.NET 可以使用 cookieless 表單驗證票證。 在此模式中，驗證票證會編碼為 URL。 我們將探討使用 cookieless 驗證票證時，以及它們如何建立及管理在下一個教學課程中。


### <a name="the-scope-of-forms-authentication"></a>表單驗證領域

FormsAuthenticationModule managed 程式碼時，ASP.NET 執行階段的一部分。 在 microsoft 的第 7 版之前[Internet Information Services (IIS)](https://www.iis.net/) web 伺服器沒有 IIS 的 HTTP 管線及 ASP.NET 執行階段的管線之間的相異屏障。 簡單地說，在 IIS 6 和更早版本，FormsAuthenticationModule 才會執行當要求從 IIS ASP.NET 執行階段委派。 根據預設，IIS 會處理之類的靜態內容本身-HTML 頁面和 CSS 和映像的檔案-交給要求 ASP.NET 執行階段，要求的.aspx、.asmx 或.ashx 延伸模組的頁面時。

IIS 7 中，不過，用來整合的 IIS 和 ASP.NET 管線。 有一些組態設定中，您可以設定 IIS 7，以叫用的 FormsAuthenticationModule*所有*要求。 此外，IIS 7，您可以定義任何類型的檔案的 URL 授權規則。 如需詳細資訊，請參閱 < [IIS6 之間的變更和 IIS7 安全性](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above)，[您的 Web 平台安全性](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)，並[了解 IIS7 URL 授權](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)。

長話短說，在 IIS 7 之前的版本中，您可以只使用表單驗證來保護由 ASP.NET 執行階段的資源。 同樣地，URL 授權規則只會套用到由 ASP.NET 執行階段的資源。 但與 IIS 7 很可能將 FormsAuthenticationModule 和 UrlAuthorizationModule 整合到 IIS 的 HTTP 管線，藉此擴充這項功能的所有要求。

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>步驟 1： 建立本教學課程系列的 ASP.NET 網站

若要連線到廣泛的可能使用者，我們將在此系列建置 ASP.NET 網站將會建立與 Microsoft 的免費版本的 Visual Studio 2008 中， [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)。 我們將實作 SqlMembershipProvider 使用者存放區中的[Microsoft SQL Server 2005 Express 的 Edition](https://msdn.microsoft.com/sql/Aa336346.aspx)資料庫。 如果您使用 Visual Studio 2005 或不同版本的 Visual Studio 2008 或 SQL Server，別擔心-步驟將會幾乎完全相同，任何重要的差異也會指出。

我們可以設定表單驗證之前，我們首先需要 ASP.NET 網站。 開始建立新檔案系統架構 ASP.NET 網站。 若要這麼做，請啟動 Visual Web Developer 然後移至 [檔案] 功能表，並選擇新的網站上，顯示 [新的網站] 對話方塊。 選擇 ASP.NET 網站範本、 到檔案系統設定 位置 下拉式清單、 選擇的資料夾，以存放 web 站台和將語言設 VB 這會建立新的網站與 Default.aspx ASP.NET 網頁，應用程式\_資料資料夾和 Web.config 檔案。

> [!NOTE]
> Visual Studio 支援兩種專案管理模式： 網站專案和 Web 應用程式專案。 網站專案會缺少專案檔中，而 Web 應用程式專案模擬專案架構在 Visual Studio.NET 2002年/2003年-它們包含在專案檔和專案的原始程式碼編譯成單一組件，都會放在 /bin 資料夾。 Visual Studio 2005 一開始只支援的網站專案，雖然 Web 應用程式專案模型已重新引入含 Service Pack 1;Visual Studio 2008 提供了這兩個專案模型。 Visual Web Developer 2005 和 2008年版本，不過，僅支援網站專案。 我將使用的網站專案模型。 如果您正在使用非 Express edition，而且想要使用[Web 應用程式專案模型](https://msdn.microsoft.com/library/aa730880(vs.80).aspx)相反地，請隨意這樣做，但您看到您的畫面和必須與所採取的步驟之間可能會有些不一致螢幕擷取畫面所示，在這些教學課程中提供的指示。


[![建立新檔案系統為基礎的網站](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**圖 02**： 建立 New File System-Based 網站 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>加入主版頁面

接下來，加入名為 Site.master 的根目錄中的站台的新主版頁面。 [主版頁面](https://msdn.microsoft.com/library/wtxbf3hh.aspx)讓網頁開發人員定義可套用至 ASP.NET 網頁的全站台的範本。 主版頁面的主要優點是，站台的整體外觀可以定義在單一位置，藉此讓您輕鬆地更新，或調整網站的版面配置。


[![加入主版頁面名稱為 Site.master 網站](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**圖 03**： 將主版頁面名稱為 Site.master 新增至網站 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image9.png))


主版頁面中定義的整個網站的頁面配置。 您可以使用 [設計] 檢視，並新增任何版面配置] 或 [Web 控制項，您需要或您可以手動在原始碼檢視中手動新增標記。 我結構化我主版頁面的版面配置，來模擬所使用的配置我*[使用 ASP.NET 2.0 中的資料](../../data-access/index.md)* 教學課程系列 （請參閱 圖 4）。 主版頁面會使用[階層式樣式表](http://www.w3schools.com/css/default.asp)來定位和樣式 （這包含在本教學課程中的相關聯的下載） 的 Style.css 檔案中定義的 CSS 設定。 雖然您無法分辨從下方所顯示的標記，定義的 CSS 規則，瀏覽&lt;div&gt;的內容絕對位置，使其出現在左邊，並已在固定的寬度為 200 像素。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

主版頁面定義靜態頁面配置和使用主版頁面的 ASP.NET 頁面都可以編輯的區域。 這些內容的可編輯區域會以您所見的內容中的 ContentPlaceHolder 控制項&lt;div&gt;。 我們的主版頁面具有單一 ContentPlaceHolder (MainContent)，但主版頁面可能會有多個 ContentPlaceHolders。

使用上面輸入的標記，切換至 [設計] 檢視會顯示主版頁面的版面配置。 使用此主版頁面的任何 ASP.NET 頁面會有這個統一的版面配置，能夠指定 MainContent 區域標記。


[![主版頁面，檢視透過 [設計] 檢視時](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**圖 04**: 主版頁面中，當檢視透過 設計檢視 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>建立內容頁面

此時我們在我們的網站中，會在有 Default.aspx 頁面時，但它不會使用我們剛才建立的主版頁面。 雖然您可以管理網頁使用主版頁面中，宣告式標記，如果頁面未包含任何內容還很容易只要刪除頁面，並將其重新加入專案中，指定要使用的主版頁面。 因此，開始從專案刪除 Default.aspx。

接下來，在 [方案總管] 中的專案名稱上按一下滑鼠右鍵，然後選擇加入新的 Web 表單名為 Default.aspx。 此時，檢查選取的主版頁面的核取方塊，並從清單中選擇 Site.master 主版頁面。


[![加入新的 Default.aspx 頁面，選擇 選取主版頁面](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**圖 05**： 加入新 Default.aspx 頁面選擇 選取主版頁面 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image15.png))


[![使用 Site.master 主版頁面](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**圖 06**： 使用 Site.master 主版頁面 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> 如果您使用 Web 應用程式專案模型加入新項目 對話方塊不包含選取的主版頁面的核取方塊。 相反地，您需要將 Web 內容表單類型的項目。 選擇 Web 內容表單選項，然後按一下 [新增] 之後，Visual Studio 會顯示相同的 Select Master [圖 6] 所示的對話方塊。


新的 Default.aspx 頁面宣告式標記只包含@Page指示詞指定的路徑至主機的主版頁面的 MainContent ContentPlaceHolder 頁面檔案和內容的控制項。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

現在，請將 Default.aspx 保留空白。 我們會將內容加入本教學課程稍後傳回給它。

> [!NOTE]
> 我們的主版頁面包含功能表或一些其他的巡覽介面的區段。 我們將在未來的教學課程中建立這類介面。


## <a name="step-2-enabling-forms-authentication"></a>步驟 2： 啟用表單驗證

建立 ASP.NET 網站，我們的下一個工作是啟用表單驗證。 透過所指定的應用程式的驗證設定[ &lt;authentication&gt;項目](https://msdn.microsoft.com/library/532aee0e.aspx)Web.config 中。&lt;驗證&gt;元素包含單一屬性，名為指定的應用程式所使用的驗證模型的模式。 這個屬性可以具有下列四個值之一：

- **Windows** -中所述先前的教學課程中，當應用程式使用 Windows 驗證是 web 伺服器的責任驗證訪客，並通常這是透過基本、 摘要或整合式 Windows驗證。
- **Form**-使用者會透過 web 網頁上的表單驗證。
- **Passport**-使用 Microsoft Passport Network 驗證使用者。
- **無**-不使用任何驗證模型，是匿名的所有訪客。

根據預設，ASP.NET 應用程式會使用 Windows 驗證。 若要變更表單驗證的驗證類型，然後，我們必須修改&lt;驗證&gt;form 項目的模式屬性。

如果您的專案尚未包含 Web.config 檔案，新增一個現在藉由以滑鼠右鍵按一下 方案總管 中的專案名稱，選擇 加入新項目，然後加入 Web 組態檔。


[![如果您的專案尚未包含 Web.config，立即加入它](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**圖 07**： 如果您的專案不會不尚未包含 Web.config，現在新增 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image21.png))


接下來，尋找&lt;驗證&gt;項目和更新為使用表單驗證。 此變更後，您的 Web.config 檔案標記看起來應該如下所示：

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> 由於 Web.config XML 檔案，大小寫很重要。 請確定您將 mode 屬性設表單以大寫 f。如果您使用不同的大小寫，例如表單，您會在瀏覽的網站，透過瀏覽器時，收到組態錯誤。


&lt;驗證&gt;項目可選擇性地包含&lt;form&gt;子元素，其中包含表單驗證特定的設定。 現在，我們只要使用預設表單驗證設定。 我們將探討&lt;form&gt;在下一個教學課程中詳細的子項目。

## <a name="step-3-building-the-login-page"></a>步驟 3： 建立登入頁面

若要支援表單驗證我們的網站會需要登入頁面。 表單驗證工作流程 區段中，FormsAuthenticationModule 會自動重新導向的了解中所述的登入頁面使用者當他們嘗試存取的頁面，它們不是檢視權限。 另外還有給匿名使用者的登入頁面會顯示連結的 ASP.NET Web 控制項。 這牽涉到一個問題，登入頁面的 URL 是什麼？

根據預設，表單驗證系統必須要有命名為 Login.aspx 登入頁面，並放在 web 應用程式的根目錄中。 如果您想要使用不同的登入頁面 URL，則可以藉由指定 Web.config 中。我們將了解如何在後續的教學課程中這麼做。

登入頁面有三項責任：

1. 提供可輸入其認證的訪客介面。
2. 判斷提交的認證是否有效。
3. 將使用者登入所建立的表單驗證票證。

### <a name="creating-the-login-pages-user-interface"></a>建立登入頁面使用者介面

讓我們開始進行第一項工作。 將新的 ASP.NET 網頁新增至名為 Login.aspx 的站台的根目錄，並將它與 Site.master 主版頁面產生關聯。


[![加入新的 ASP.NET 頁面命名為 Login.aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**圖 08**： 加入新 ASP.NET 頁面上名為 Login.aspx ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image24.png))


典型的登入網頁介面是由兩個文字方塊-一個用於使用者的名稱、 密碼-和送出表單按鈕的其中一個所組成。 網站通常包含記住我 核取方塊，若選取此選項，則在跨瀏覽器會重新啟動保存產生的驗證票證。

將兩個文字方塊加入到 Login.aspx 和使用者名稱和密碼，分別設定其識別碼屬性。 也設定 TextMode 屬性設密碼的密碼。 接下來，新增核取方塊控制項，將其 ID 屬性設定為 RememberMe，其文字屬性設定為 記住我 接下來，新增名為的 LoginButton 其 Text 屬性設為登入 按鈕。 最後，將 Label Web 控制項，並設定其 ID 屬性 InvalidCredentialsMessage，它的文字屬性，為您的使用者名稱或密碼無效。 請再試一次。，前景色彩屬性設為紅色，與它的 Visible 屬性設為 False。

此時您的畫面看起來應該類似螢幕擷取畫面圖 9 中，頁面的宣告式語法，應如下所示：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![登入頁面包含兩個文字方塊、 核取方塊、 按鈕和標籤](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**圖 09**: 登入頁面包含兩個文字方塊、 核取方塊、 按鈕和標籤 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image27.png))


最後，建立事件處理常式，LoginButton 的 click 事件。 從設計工具中，只要按兩下按鈕控制項以建立這個事件處理常式。

### <a name="determining-if-the-supplied-credentials-are-valid"></a>判斷提供的認證是否有效

我們現在需要實作工作 2 中按鈕的 Click 事件處理常式-判斷提供的認證是否有效。 若要這樣做會需要保存的所有使用者的認證，好讓我們可以判斷是否提供的認證符合已知的任何認證的使用者存放區。

ASP.NET 2.0 中，開發人員之前，負責實作兩個自己的使用者存放區及撰寫程式碼來驗證對存放區提供的認證。 大部分的開發人員會實作使用者存放區在資料庫中，建立名為使用者與資料行，例如使用者名稱、 密碼、 電子郵件、 LastLoginDate，等資料表。 然後，此資料表中，會有一筆記錄，每個使用者帳戶。 查詢資料庫中的比對的使用者名稱，以及確保資料庫中的密碼會對應至所提供的密碼，則會包含驗證的使用者提供的認證。

利用 ASP.NET 2.0，開發人員應該使用其中一個成員資格提供者來管理使用者存放區。 在本教學課程系列中，我們將使用 SqlMembershipProvider，用於使用者存放區中的 SQL Server 資料庫。 當使用 SqlMembershipProvider 我們需要實作特定的資料庫結構描述，其中包含資料表、 檢視和預存程序提供者所預期。 我們將檢驗如何實作在這個結構描述*[在 SQL Server 中建立成員資格結構描述](../membership/creating-the-membership-schema-in-sql-server-vb.md)* 教學課程。 使用就地成員資格提供者，若要驗證使用者的認證很簡單，只要呼叫[Membership 類別](https://msdn.microsoft.com/library/system.web.security.membership.aspx)的[ValidateUser (*使用者名稱*，*密碼*)方法](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)，它會傳回布林值，指出是否的有效性*使用者名稱*並*密碼*組合。 因為我們還沒有實作 SqlMembershipProvider 的使用者存放區，我們無法使用 Membership 類別 ValidateUser 方法，這一次。

而不是花時間來建立自己自訂的使用者資料庫資料表 （這會是已過時，我們實作 SqlMembershipProvider 之後），讓我們改為硬式編碼有效的認證，在 登入頁面本身。 在 LoginButton 的 Click 事件處理常式，加入下列程式碼：

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

如您所見，有三個有效的使用者帳戶-Scott、 Jisun 和 Sam-和所有的三個都具有相同的密碼 （密碼）。 此程式碼迴圈尋找有效的使用者名稱和密碼比對使用者和密碼的陣列。 如果使用者名稱和密碼是有效的我們需要登入使用者，然後將它們重新導向至適當的頁面。 如果認證不正確的我們會顯示 InvalidCredentialsMessage 標籤。

當使用者輸入有效的認證時，我說過，他們會接著重新導向至適當的頁面。 不過，什麼是適當的頁面？ 回想一下，當使用者造訪他們未獲授權檢視的頁面，FormsAuthenticationModule 自動將它們重新導向至登入頁面。 在此情況下，其中包含透過 ReturnUrl 參數 querystring 中要求的 URL。 也就是說，如果使用者嘗試瀏覽 ProtectedPage.aspx，而且他們未獲授權執行這項操作，FormsAuthenticationModule 會將他們重新導向至：

Login.aspx?ReturnUrl=ProtectedPage.aspx

在成功登入時，使用者應該重新導向回到 ProtectedPage.aspx。 或者，使用者可以在其本身並非自願要瀏覽登入頁面。 在此情況下，在使用者登入後應該傳送至根資料夾的 Default.aspx 頁面。

### <a name="logging-in-the-user"></a>登入使用者

假設所提供的認證是有效的我們需要建立表單驗證票證，藉此登入至站台的使用者。 [FormsAuthentication 類別](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx)中[System.Web.Security 命名空間](https://msdn.microsoft.com/library/system.web.security.aspx)提供各種的方法，登入和登出使用者透過表單驗證系統。 雖然 FormsAuthentication 類別中有數種方法，在此時我們感興趣的三個將是：

- [GetAuthCookie (*使用者名稱*， *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) -建立提供的名稱的表單驗證票證*username*。 接下來，此方法會建立，並傳回 HttpCookie 物件，包含驗證 ticket 的內容。 如果*persistCookie*為 True 時，在建立永續性 cookie。
- [SetAuthCookie (*使用者名稱*， *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -呼叫 GetAuthCookie (*username*， *persistCookie*)若要產生表單驗證 cookie 的方法。 此方法接著會將 GetAuthCookie 至 （假設正在使用，否則為 cookie 為基礎的表單驗證，這個方法會呼叫處理無 cookie 的票證邏輯的內部類別） 的 Cookie 集合傳回的 cookie。
- [RedirectFromLoginPage (*使用者名稱*， *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -這個方法會呼叫 SetAuthCookie (*username*， *persistCookie*)，並再將使用者重新導向至適當的頁面。

當您需要修改驗證票證，寫出的 cookie 加入 Cookie 集合之前，GetAuthCookie 很方便。 如果您想要建立表單驗證票證，並將它新增至 Cookie 集合中，但不是想要將使用者重新導向到適當的網頁，SetAuthCookie 很有用。 或許您想要讓它們保持登入頁面上，或將它們傳送至一些其他的頁面。

因為我們想要將使用者登入，並將它們重新導向至適當的頁面，讓我們使用 RedirectFromLoginPage。 更新 LoginButton 的按一下事件處理常式，加上註解的兩個 TODO 行取代為下列程式碼行：

FormsAuthentication.RedirectFromLoginPage （UserName.Text、 RememberMe.Checked）

建立表單驗證票證時我們會使用使用者名稱文字方塊的 Text 屬性的表單驗證票證*使用者名稱*參數，而 RememberMe 核取方塊的核取的狀態*persistCookie*參數。

若要測試登入頁面，請在瀏覽器中造訪。 開始輸入無效的認證，例如有一天的使用者名稱和密碼的錯誤。 按一下 [登入] 按鈕後會發生回傳和 InvalidCredentialsMessage 標籤將會顯示。


[![InvalidCredentialsMessage 標籤會顯示當輸入不正確的認證](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**圖 10**: InvalidCredentialsMessage 標籤會顯示當輸入不正確的認證 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image30.png))


接下來，輸入有效的認證，然後按一下 [登入] 按鈕。 在回傳發生表單驗證票證這次會建立並將您自動重新導向回 Default.aspx。 現在您已登入網站，雖然有沒有視覺提示，可指出您目前登入。 在步驟 4 中我們將了解如何以程式設計方式判斷使用者是否已登入，或不以及如何識別使用者瀏覽的頁面。

步驟 5 會檢查記錄將使用者登出網站的技術。

### <a name="securing-the-login-page"></a>保障安全的登入頁面

當使用者輸入認證，並送出登入頁面表單時，（包括密碼） 的認證會透過網際網路到 web 伺服器中傳輸*純文字*。 這表示任何探查的網路流量的駭客可以看到使用者名稱和密碼。 若要避免這個問題，務必使用加密的網路流量[安全通訊端層 (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)。 這可確保加密的認證 （以及整個網頁的 HTML 標記），從目前的使用者會離開瀏覽器，但尚未接收由 web 伺服器。

除非您的網站包含機密資訊，您只必須使用 SSL 登入頁面和其他頁面上，否則會傳送使用者的密碼以純文字格式在網路上。 您不需要擔心如何保護表單驗證票證，因為根據預設，它會加密和數位簽章 （若要避免資料遭到竄改）。 下列教學課程中，會顯示表單驗證票證安全性的深入討論。

> [!NOTE]
> 許多金融和醫療網站已設定為使用上的 SSL*所有*頁面可以存取已驗證的使用者。 如果您要建置這類網站，讓表單驗證票證只會傳輸透過安全連線，您可以設定表單驗證系統。 我們將在下一個教學課程中，探討各種的表單驗證組態選項*[表單驗證組態和進階主題](../membership/creating-the-membership-schema-in-sql-server-vb.md)*。


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>步驟 4： 偵測已驗證的訪客，並判斷其身分識別

現在我們已啟用表單驗證，並建立基本的登入頁面上，但我們尚未檢查我們如何判斷使用者是否已驗證或匿名。 在某些情況下我們可能想要顯示不同的資料或根據已驗證或匿名使用者會瀏覽頁面的資訊。 此外，我們通常需要知道的已驗證的使用者識別。

讓我們來增強現有的 Default.aspx 頁面，來說明這些技術。 在 Default.aspx 中新增兩個面板控制項、 一個具名的 AuthenticatedMessagePanel 和另一個具名的 AnonymousMessagePanel。 新增名為 WelcomeBackMessage 在第一個面板中的標籤控制項。 在第二個面板中加入超連結控制項，其文字屬性設定為登入，其 NavigateUrl 屬性設定為 ~ / Login.aspx。 此時 Default.aspx 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

您可能已經猜到目前為止，意思就是向已驗證的訪客，只是匿名的訪客 AnonymousMessagePanel 顯示只 AuthenticatedMessagePanel。 若要這麼做，我們需要設定這些面板取決於是否使用者登入或不可見的屬性。

[Request.IsAuthenticated 屬性](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx)傳回布林值，指出是否已驗證要求。 在頁面中輸入下列程式碼\_載入事件處理常式程式碼：

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

使用此程式碼就緒之後，請透過瀏覽器瀏覽 Default.aspx。 假設您沒有登入，您會看到登入頁面的連結 （請參閱 圖 11）。 按一下此連結，登入網站。 如我們所見在步驟 3 中，輸入您的認證之後會傳回 default.aspx，但這次頁面會顯示歡迎回來 ！ 訊息 （請參閱 圖 12）。


[![當瀏覽以匿名方式，登入 連結顯示](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**圖 11**： 當瀏覽以匿名方式、 記錄檔中連結會顯示 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image33.png))


[![已驗證的使用者會顯示歡迎回來 ！訊息](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**圖 12**： 已驗證的使用者會顯示歡迎回來 ！ 訊息 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image36.png))


我們可以判斷目前登入使用者的身分識別，透過[HttpContext 物件](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)的[使用者屬性](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)。 HttpContext 物件代表目前的要求的相關資訊，以及其他項目是做為回應、 要求和工作階段中，這類通用 ASP.NET 物件的首頁。 [使用者] 屬性表示目前 HTTP 要求和實作的安全性內容[IPrincipal 介面](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx)。

使用者屬性是由設定 FormsAuthenticationModule。 具體來說，當 FormsAuthenticationModule 找到傳入要求中的表單驗證票證，它會建立新的 GenericPrincipal 物件並將它指派至的使用者屬性。

主體的物件 （例如 GenericPrincipal) 提供使用者的身分識別和其所隸屬的角色資訊。 IPrincipal 介面會定義兩個成員：

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -傳回布林值，指出主體是否屬於指定角色的方法。
- [身分識別](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx)-此屬性，傳回該物件會實作[IIdentity 介面](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx)。 IIdentity 介面會定義三個屬性： [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx)， [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)，並[名稱](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx)。

我們可以判斷目前使用下列程式碼的造訪者的名稱：

變暗 currentUsersName As String = User.Identity.Name

使用表單驗證，當[FormsIdentity 物件](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx)建立 GenericPrincipal 的身分識別屬性。 FormsIdentity 類別一律會傳回字串形式的 AuthenticationType 屬性和 True 其 IsAuthenticated 屬性。 Name 屬性會傳回建立的表單驗證票證時指定的使用者名稱。 這三個屬性，除了 FormsIdentity 包含存取基礎的驗證票證，透過其[票證屬性](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx)。 票證屬性傳回的物件型別的[FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)，其中包含屬性，例如到期、 IsPersistent、 IssueDate、 名稱和等等。

可以獲得以下是重要的一點*使用者名稱*FormsAuthentication.GetAuthCookie 中指定的參數 (*username*， *persistCookie*)，FormsAuthentication.SetAuthCookie (*使用者名稱*， *persistCookie*)，和 FormsAuthentication.RedirectFromLoginPage (*username*， *persistCookie*) 方法會傳回 User.Identity.Name 的相同值。 此外，這些方法所建立的驗證票證使用，請將 User.Identity FormsIdentity 物件轉型，並接著存取票證屬性：

變暗 ident 為 FormsIdentity = CType （User.Identity、 FormsIdentity）

變暗 authTicket 為 FormsAuthenticationTicket = ident。票證

讓我們提供更個人化的訊息在 Default.aspx 中。 更新頁面\_以便 WelcomeBackMessage Label 的 Text 屬性指派字串歡迎回來，載入事件處理常式*使用者名稱*！

WelcomeBackMessage.Text = 「 歡迎回來，「 &amp; User.Identity.Name &amp; "！"

[圖 13] 顯示這項修改的效果 （當使用者 Scott 身分登入）。


[![歡迎訊息包含目前記錄中使用者的名稱](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**圖 13**： 歡迎訊息會包含目前登入使用者的名稱 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>使用 LoginView 和 LoginName 控制項

顯示不同的內容給已驗證和匿名使用者是常見的需求;因此會顯示目前登入的使用者名稱。 基於這個理由，ASP.NET 會包含兩個圖 13 中，但不需要撰寫一行程式碼所示的相同功能的 Web 控制項。

[LoginView 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)範本為基礎的 Web 控制項，可讓您輕鬆地顯示不同的資料給已驗證和匿名使用者。 LoginView 包含兩個預先定義的範本：

- AnonymousTemplate-任何新增至這個樣板的標記只會顯示匿名訪客。
- LoggedInTemplate-這個樣板的標記才會看到已驗證的使用者。

讓我們加入我們的網站主版頁面，Site.master LoginView 控制項。 而不用新增 LoginView 控制項，不過，讓我們來新增這兩個 ContentPlaceHolder 控制項並將該新 ContentPlaceHolder 內 LoginView 控制項。 這項決策的基本原理即將揭曉短時間內。

> [!NOTE]
> 除了 AnonymousTemplate 和 LoggedInTemplate，LoginView 控制項可以包含特定角色的範本。 特定角色的範本會顯示標記，只讓屬於指定角色的使用者。 我們將在未來的教學課程中，檢視 LoginView 控制項的以角色為基礎的功能。


首先新增名為 LoginContent 轉換成主版頁面內巡覽 ContentPlaceHolder &lt;div&gt;項目。 您只需拖曳 ContentPlaceHolder 控制項從 [工具箱] 拖曳至來源檢視中，將放置產生的標記正上方 TODO： 功能表將在此處呈現的文字。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

接下來，新增 LoginContent ContentPlaceHolder 內 LoginView 控制項。 放入的主版頁面 ContentPlaceHolder 控制項的內容會被視為*預設內容*ContentPlaceHolder 的。 也就是使用此主版頁面的 ASP.NET 頁面可以指定每個 ContentPlaceHolder 他們自己的內容，或使用主版頁面的預設內容。

LoginView 與其他登入相關的控制項位於 工具箱 中的登入 索引標籤中。


[![LoginView 控制項，在 [工具箱]](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**圖 14**： 在 [工具箱] 的 LoginView 控制項 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image42.png))


接下來，新增兩個&lt;b /&gt;立即 LoginView 控制項之後，但仍在 ContentPlaceHolder 內的項目。 此時，瀏覽&lt;div&gt;項目的標記應該看起來如下所示：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

LoginView 的範本可以定義從設計工具或宣告式標記。 從 Visual Studio 的設計工具中，展開 LoginView 的智慧標籤，其中會列出下拉式清單中設定的範本。 輸入文字 Hello，陌生人到 AnonymousTemplate;接下來，新增超連結控制項，並將其文字和 NavigateUrl 屬性設定為 登入和 ~ / Login.aspx，分別。

設定之後 AnonymousTemplate，切換至 LoggedInTemplate 並輸入文字、 「 歡迎回來，"。 然後將 LoginName 控制項從 [工具箱] 拖曳至 LoggedInTemplate，並將它放在 「 歡迎回來，"文字的後面。 [LoginName 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx)，正如其名，會顯示目前登入的使用者名稱。 就內部而言，LoginName 控制項只會輸出 User.Identity.Name 屬性

完成之後這些新增項目至 LoginView 的範本，請標記看起來應該如下所示：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

添加這 Site.master 主版頁面，在我們的網站中的每個頁面會顯示不同的訊息取決於是否已驗證使用者。 圖 15 顯示當使用者 Jisun 透過瀏覽器瀏覽的 Default.aspx 頁面。 歡迎回來，也就是 Jisun 訊息會重複兩次： 一次是在 （透過我們剛才加入的 LoginView 控制項） 在左側的主版頁面的 [導覽] 區段，一次在 Default.aspx 的內容 （透過面板控制項和程式設計邏輯） 的區域。


[![LoginView 控制項顯示歡迎回來 Jisun。](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**圖 15**: LoginView 控制項顯示歡迎回來，Jisun。 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image45.png))


因為我們新增 LoginView 主版頁面時，它可以出現在我們網站上的每個頁面。 不過，可能有網頁我們不想再顯示此訊息。 一個這類頁面是登入頁面上，，因為登入頁面的連結看起來有位置不對。 因為我們會將 LoginView 控制項置於 ContentPlaceHolder 主版頁面中，我們可以在我們的內容頁來覆寫此預設標記。 開啟 Login.aspx 並移至設計工具。 因為我們未明確定義的內容控制項中的主版頁面中 LoginContent ContentPlaceHolder Login.aspx，登入頁面會針對此 ContentPlaceHolder 顯示主版頁面的預設標記。 您可以看到這透過設計工具-LoginContent ContentPlaceHolder 顯示的預設標記 （LoginView 控制項）。


[![登入頁面顯示的預設內容的主版頁面的 LoginContent ContentPlaceHolder](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**圖 16**： 登入頁面的主版頁面的 LoginContent ContentPlaceHolder 會顯示預設內容 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image48.png))


覆寫預設標記 LoginContent ContentPlaceHolder 的只要以滑鼠右鍵按一下設計工具中的區域，並從內容功能表中選擇 [建立自訂內容] 選項。 (當使用 Visual Studio 2008 ContentPlaceHolder 包含智慧標籤，選取時，提供相同的選項。)這會將新增新的內容控制項到網頁的標記，以便讓我們來定義此頁面的自訂內容。 您可以新增自訂訊息，請登入，例如，但是我們只將此保留空白。

> [!NOTE]
> 在 Visual Studio 2005 中，建立自訂內容建立空內容 ASP.NET 網頁中的控制項。 在 Visual Studio 2008 中，不過，建立自訂的內容將複製的主版頁面的預設內容到新建立的內容控制項。 如果您使用 Visual Studio 2008，然後建立新的內容控制項之後請務必清除 從主版頁面複製的內容。


[圖 17] 顯示當進行這項變更後，從瀏覽器瀏覽的 Login.aspx 頁面。 請注意，沒有陌生人或歡迎您再次啟用，沒有 Hello*使用者名稱*左側導覽中的訊息&lt;div&gt;因為沒有瀏覽 Default.aspx 時。


[![登入頁面會隱藏預設 LoginContent ContentPlaceHolder 標記](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**圖 17**： 登入頁面會隱藏預設 LoginContent ContentPlaceHolder 的標記 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>步驟 5： 登出

在步驟 3 中，我們討論過建置登入頁面使用者登入至站台，但我們仍須以了解如何將使用者登出。記錄中的使用者的方法，除了 FormsAuthentication 類別也提供[SignOut 方法](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)。 登出方法只會終結表單驗證票證，藉此記錄將使用者登出網站。

供應項目 [登出] 連結這類的常見的功能，ASP.NET 包含專為將使用者登出的控制項。[LoginStatus 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx)顯示登入 LinkButton 或登出 LinkButton，根據使用者的驗證狀態。 登入 LinkButton 轉譯為匿名使用者，而登出 LinkButton 會顯示已驗證的使用者。 您可以透過 LoginStatus 設定登入和登出 Linkbutton 的文字 LoginText 和 LogoutText 屬性。

按一下 登入 LinkButton 會導致的回傳，從中重新導向會發出登入頁面。 按一下登出 LinkButton 會導致叫用 FormsAuthentication.SignOff 方法以 LoginStatus 控制項，然後將使用者重新導向至頁面。 [記錄] 頁面上關閉使用者重新導向至取決於 LogoutAction 屬性可以指派給三個下列值之一：

- 重新整理-預設值;將使用者重新導向到自己所造訪的頁面。 如果它們只已造訪的頁面不允許匿名使用者，然後 FormsAuthenticationModule 會自動將使用者重新導向至登入頁面。

您可能知道為什麼重新導向此處沒有執行。 如果使用者想要保留在相同頁面上，為什麼需要明確的重新導向？ 原因是因為使用者按一下登出 LinkButton 時，仍然可以在他們的 cookie 集合中的表單驗證票證。 因此，回傳的要求是已驗證的要求。 LoginStatus 控制項呼叫登出方法，但這種情況發生之後 FormsAuthenticationModule 已驗證使用者。 因此，明確的重新導向會導致瀏覽器，以重新要求頁面。 瀏覽器重新要求頁面時，已移除表單驗證票證，並因此連入要求為匿名。

- 重新導向-使用者會重新導向至 LoginStatus LogoutPageUrl 屬性所指定的 URL。
- RedirectToLoginPage-使用者會重新導向至登入頁面。

讓我們將 LoginStatus 控制項新增至主版頁面，並將它設定為使用重新導向選項來將使用者傳送到頁面，其中會顯示訊息，確認，它們已登出。首先會建立名為 Logout.aspx 的根目錄中的頁面。 別忘了將此頁面與 Site.master 主版頁面產生關聯。 接下來，在網頁的標記，說明已登出的使用者輸入的訊息。

接下來，回到 Site.master 主版頁面，並加入 LoginStatus 控制項下方 LoginView LoginContent ContentPlaceHolder。 設 ~/Logout.aspx LoginStatus 控制項的重新導向的 LogoutAction 屬性並將其 LogoutPageUrl 屬性。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

LoginStatus 超出 LoginView 控制項，因為它會顯示匿名和已驗證的使用者，沒關係，因為 LoginStatus 會正確顯示登入或登出 LinkButton。 LoginStatus 控制項加入之後，記錄檔中的超連結 AnonymousTemplate 多餘，因此將它移除。

圖 18.顯示 Default.aspx，當 Jisun 造訪。 請注意，左側資料行，將會顯示訊息，歡迎回來，並有連結的 Jisun 登出。按一下 LinkButton 登出造成回傳 Jisun 登出系統，然後將其重新導向至 Logout.aspx。 如圖 19 所示，Jisun 達到的 Logout.aspx 她已簽署，因此屬於匿名的時間。 因此，左側資料行顯示歡迎畫面、 新奇且登入頁面連結的文字。


[![Default.aspx 顯示歡迎回來，以及登出 LinkButton Jisun](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**圖 18**: Default.aspx顯示歡迎回來，Jisun以及註銷LinkButton ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx 顯示歡迎畫面，以及登入 LinkButton 陌生人](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**圖 19**: Logout.aspx 顯示歡迎畫面，以及登入 LinkButton 陌生人 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> 建議您自訂 Logout.aspx 頁面，即可隱藏主版頁面的 LoginContent ContentPlaceHolder （就像我們在步驟 4 中的 Login.aspx）。 原因是因為 LoginStatus 控制項所呈現的登入 LinkButton (Hello 下, 一位陌生人) 會將 ReturnUrl 查詢字串參數中傳遞目前 URL 的登入頁面的使用者。 簡單地說，如果已登出的使用者按一下此 LoginStatus 登入 LinkButton，並再登入，他們將回到 Logout.aspx，可以輕鬆地會混淆使用者重新導向。


## <a name="summary"></a>總結

在本教學課程中我們使用表單驗證工作流程的檢查，然後開啟 ASP.NET 應用程式中實作表單驗證。 表單驗證由具有兩個責任 FormsAuthenticationModule： 找出使用者根據其表單驗證票證，並將未經授權的使用者重新導向至登入頁面。

.NET Framework 的 FormsAuthentication 類別包含建立、 檢查和移除表單驗證票證的方法。 Request.IsAuthenticated 屬性和使用者物件提供額外的程式設計支援判斷要求是否已驗證和使用者的身分識別的相關資訊。 另外還有 LoginView，LoginStatus、 LoginName Web 控制項，讓開發人員以快速、 無程式碼的方式來執行許多常見的登入相關工作。 我們將檢視這些及其他登入相關 Web 控制項，更詳細地在未來的教學課程。

本教學課程提供表單驗證粗略的概觀。 我們未檢查的各種的組態選項，看看如何 cookieless 表單驗證票證工作，或瀏覽 ASP.NET 如何保護表單驗證票證的內容。 我們將討論這些主題中的等[下一個教學課程](forms-authentication-configuration-and-advanced-topics-vb.md)。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [IIS6 和 IIS7 安全性之間的變更](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [登入 ASP.NET 控制項](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 安全性、 成員資格和角色管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [&lt;驗證&gt;項目](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Form&gt;項目&lt;驗證&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教學課程中所包含的主題的影片訓練

- [在 ASP.NET 中使用基本的表單驗證](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者包括 Alicja Maziarz、 John Suru 和 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)。

> [!div class="step-by-step"]
> [上一頁](security-basics-and-asp-net-support-vb.md)
> [下一頁](forms-authentication-configuration-and-advanced-topics-vb.md)
