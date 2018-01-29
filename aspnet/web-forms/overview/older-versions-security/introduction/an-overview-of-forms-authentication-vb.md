---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: "表單驗證 (VB) 的概觀 |Microsoft 文件"
author: rick-anderson
description: "在此教學課程中我們將會開啟從只討論實作;特別是，我們將探討實作表單驗證。 Web 應用程式 w 中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 90bcff91d0642e6af66f43fd807b253cc516d277
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-forms-authentication-vb"></a>表單驗證 (VB) 的概觀
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip)或[下載 PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> 在此教學課程中我們將會開啟從只討論實作;特別是，我們將探討實作表單驗證。 我們一開始建構在此教學課程中的 web 應用程式仍可建置在後續的教學課程中，當我們從簡單的表單驗證移動到成員資格和角色。
> 
> 請參閱本主題中的 這段影片，如需詳細資訊： [ASP.NET 中使用基本的表單驗證](# "using-basic-forms-authentication-in-aspnet")。


## <a name="introduction"></a>簡介

在[前述教學課程](security-basics-and-asp-net-support-vb.md)我們討論各種驗證、 授權和使用者帳戶所提供的選項 ASP.NET。 在此教學課程中我們將會開啟從只討論實作;特別是，我們將探討實作表單驗證。 我們一開始建構在此教學課程中的 web 應用程式仍可建置在後續的教學課程中，當我們從簡單的表單驗證移動到成員資格和角色。

本教學課程開頭為表單驗證工作流程，我們在接觸到上一個教學課程中的主題將深入探討。 接下來，我們將建立 ASP.NET 網站，以示範表單驗證的概念。 接下來，我們會將網站設定為使用表單驗證，建立簡單 登入頁面上，請參閱如何判斷，在程式碼中是否驗證使用者，若是如此，使用者名稱它們來登入。

了解驗證工作流程，讓它在 web 應用程式，以及建立登入及登出頁面是所有重要的步驟，建立 ASP.NET 應用程式，以支援使用者帳戶，並且驗證使用者，透過網頁表單。 基於這點-因為這些教學課程是根據另一個-我建議您能夠在本教學課程中完整之前繼續移至下一個即使您已取得的體驗設定表單驗證，在過去的專案中。

## <a name="understanding-the-forms-authentication-workflow"></a>了解表單驗證工作流程

當 ASP.NET 執行階段處理的要求是 ASP.NET 的資源，例如 ASP.NET 頁面或 ASP.NET Web 服務，要求會在其生命週期引發的事件數目。 沒有在要求的要求已通過驗證和授權，在未處理的例外狀況，以及其他等等的情況下所引發的事件時引發的非常開始和結束時引發的事件。 若要查看事件的完整清單，請參閱[HttpApplication 物件事件](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx)。

*HTTP 模組*是受管理的程式碼執行以回應要求的生命週期中的特定事件的類別。 ASP.NET 隨附之 HTTP 模組執行基本工作，在幕後的數目。 兩個內建的 HTTP 模組，特別相關討論的是：

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -會驗證使用者，藉由檢查表單驗證票證，通常包含在使用者的 cookie 集合。 如果沒有表單驗證票證已存在，就像匿名使用者。
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -決定目前使用者是否獲授權存取要求的 URL。 此模組會判定應用程式的組態檔中指定的授權規則來判斷授權單位。 ASP.NET 也包含[FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) ，查閱要求的檔案 Acl 決定授權單位。

FormsAuthenticationModule 會嘗試驗證使用者之前執行 UrlAuthorizationModule （和 FileAuthorizationModule）。 如果提出要求的使用者未獲授權存取要求的資源，授權模組會終止要求，並且傳回[HTTP 401 未經授權](http://www.checkupdown.com/status/E401.html)狀態。 在 Windows 驗證的情況下，會傳回至瀏覽器的 HTTP 401 狀態。 此狀態碼讓瀏覽器會提示使用者輸入其認證能夠透過強制回應對話方塊。 使用表單驗證，不過，HTTP 401 未經授權的狀態會永遠不會傳送至瀏覽器由於 FormsAuthenticationModule 偵測到此狀態，並修改它改為將使用者重新導向至登入頁面 (透過[HTTP 302 重新導向](http://www.checkupdown.com/status/E302.html)狀態)。

登入頁面的責任是判斷使用者的認證是否有效，如果是，建立表單驗證票證，並將使用者重新導向回到頁面他們嘗試瀏覽。 驗證票證隨附於後續要求的網站程式 FormsAuthenticationModule 用來識別使用者的頁面。


[![表單驗證工作流程](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**圖 01**： 表單驗證工作流程 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>透過網頁瀏覽記住驗證票證

登入之後，表單驗證票證必須傳送至 web 伺服器上每個要求，讓使用者在瀏覽站台會維持已登入。 這通常會透過將驗證 ticket 放入使用者的 cookie 集合。 [Cookie](http://en.wikipedia.org/wiki/HTTP_cookie)是位於使用者的電腦上，而且會以每個要求建立 cookie 的網站上的 HTTP 標頭傳輸的小型文字檔案。 因此，一旦表單驗證票證已建立並儲存在瀏覽器的 cookie 中，每個後續的瀏覽至該網站會傳送驗證票證，以及要求，進而識別使用者。

> [!NOTE]
> 每個教學課程中所示範 web 應用程式是可供下載。 此下載的應用程式是以目標為.NET Framework 3.5 版的 Visual Web Developer 2008 建立的。 應用程式適用於.NET 3.5 為目標，因為其 Web.config 檔案會包含其他、 3.5 特有的組態項目。 長劇本短時，如果您有尚未安裝.NET 3.5，然後下載 web 應用程式的電腦上未先移除 3.5 特定標記從 Web.config，將無法運作。


Cookie 的其中一個層面是其到期時間，也就是瀏覽器會捨棄 cookie 的時間與日期。 表單驗證 cookie 過期時，使用者可以不再通過驗證，因此會變成匿名。 當使用者造訪從公用終止時，有可能是他們想要在關閉瀏覽器時到期其驗證票證。 當造訪從家裡時，不過，該相同的使用者可能會想驗證票證，使它們不需要記住瀏覽器重新啟動時重新登入每次造訪的網站。 這項決定通常會由記住我 核取方塊上的登入頁面表單中的使用者。 在步驟 3 中，我們將檢查如何實作登入頁面中的 [記住我] 核取方塊。 下列教學課程說明中的驗證票證逾時設定詳細資料。

> [!NOTE]
> 有可能用來登入網站的使用者代理程式可能不支援 cookie。 在此情況下，ASP.NET 可以使用 cookie 的表單驗證票證。 在此模式中，會編碼成 URL 的驗證票證。 我們將探討使用 cookie 驗證票證時，及它們如何建立及管理在下一個教學課程。


### <a name="the-scope-of-forms-authentication"></a>表單驗證領域

FormsAuthenticationModule managed 程式碼是 ASP.NET 執行階段的一部分。 在 Microsoft 的第 7 版之前[網際網路資訊服務 (IIS)](https://www.iis.net/) web 伺服器時發生 IIS 的 HTTP 管線和管線 ASP.NET 執行階段之間的相異屏障。 簡單地說，在 IIS 6 及更早版本，FormsAuthenticationModule 時才執行要求從 IIS 委派給 ASP.NET 執行階段。 根據預設，IIS 會處理之類的靜態內容本身的 HTML 網頁和 CSS 以及映像檔-和交給要求至 ASP.NET 執行階段，要求副檔名為.aspx、.asmx 或.ashx 頁面時。

IIS 7 不過，可讓整合式的 IIS 和 ASP.NET 的管線。 使用一些組態設定中，您可以設定要叫用的 FormsAuthenticationModule IIS 7*所有*要求。 此外，IIS 7，您可以定義任何類型檔案的 URL 授權規則。 如需詳細資訊，請參閱[變更之間 IIS6 和 IIS7 安全性](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above)， [Web 平台安全性](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)，和[了解 IIS7 URL 授權](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)。

長劇本短時，在 IIS 7 之前的版本，您可以只使用表單驗證來保護 ASP.NET 執行階段所處理的資源。 同樣地，URL 授權規則只會套用至由 ASP.NET 執行階段處理的資源。 但是，IIS 7 也可以將 FormsAuthenticationModule 和 UrlAuthorizationModule 整合到 IIS 的 HTTP 管線，因而擴充到所有的要求這項功能。

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>步驟 1： 建立用於此教學課程系列的 ASP.NET 網站

若要前往的最大的可能對象，我們將在這一系列整個建置 ASP.NET 網站將會建立與 Microsoft 的免費版本的 Visual Studio 2008， [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)。 我們會實作 SqlMembershipProvider 使用者存放區中的[Microsoft SQL Server 2005 Express 的 Edition](https://msdn.microsoft.com/sql/Aa336346.aspx)資料庫。 如果您使用 Visual Studio 2005 或不同版本的 Visual Studio 2008 或 SQL Server，也請別擔心-步驟會幾乎完全相同，指出任何重要的差異。

我們可以設定表單驗證之前，我們先需要 ASP.NET 網站。 開始建立新檔案系統以 ASP.NET 網站。 若要這麼做，請啟動 Visual Web Developer 然後移至 檔案 功能表，並選擇新的網站，顯示新的網站 對話方塊。 選擇 ASP.NET 網站範本、 到檔案系統設定 位置 下拉式清單、 選擇要將網站的資料夾和將語言設為.vb 之 這將使用 ASP.NET Default.aspx 頁面上，應用程式建立新的網站\_Data 資料夾，然後 Web.config 檔案。

> [!NOTE]
> Visual Studio 支援的專案管理的兩種模式： 網站專案和 Web 應用程式專案。 網站專案沒有專案檔中，而 Web 應用程式專案模仿專案架構在 Visual Studio.NET 2002年/2003年-它們包括專案檔和專案的來源的程式碼編譯為放置 /bin 資料夾中的單一組件。 Visual Studio 2005 一開始只支援的網站專案，雖然 Web 應用程式專案模型已經重新引入含 Service Pack 1。Visual Studio 2008 提供這兩個專案的模型。 Visual Web Developer 2005 和 2008年版本，不過，僅支援的網站專案。 我將使用的網站專案模型。 如果您正在使用非 Express edition，而且想要使用[Web 應用程式專案模型](https://msdn.microsoft.com/library/aa730880(vs.80).aspx)相反地，您可以任意去以執行這項操作，但請注意螢幕和相對於必須採取的步驟上看到的內容之間可能會有些不一致螢幕擷取畫面所示，在這些教學課程所提供的指示。


[![建立新檔案系統為基礎的網站](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**圖 02**： 建立 New File System-Based 網站 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>加入主版頁面

接下來，將新的主版頁面加入至名為 Site.master 的根目錄中的站台。 [主版頁面](https://msdn.microsoft.com/library/wtxbf3hh.aspx)讓網頁開發人員定義的全站台的範本，可以套用到 ASP.NET 頁面。 主版頁面的主要優點是站台的整體外觀可以定義在單一位置，藉此讓您輕鬆更新或調整網站的版面配置。


[![加入主版頁面名稱為網站 Site.master](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**圖 03**： 新增至網站的主版頁面名稱為 Site.master ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image9.png))


主版頁面中定義的全站台的頁面配置。 您可以使用 [設計] 檢視，並將在需要時，任何配置或 Web 控制項，或您可以手動在來源檢視中手動新增標記。 我結構化我主版頁面的版面配置，來模擬所使用的配置我*[在 ASP.NET 2.0 中使用資料](../../data-access/index.md)*教學課程系列 （請參閱圖 4）。 使用主版頁面[階層式樣式表](http://www.w3schools.com/css/default.asp)用來定位和樣式 （其中包含在本教學課程相關下載） 的 Style.css 檔案中定義的 CSS 設定。 雖然您無法分辨從標記如下所示，定義的 CSS 規則，瀏覽&lt;div&gt;的內容絕對置放出現在左邊，使其具有固定的寬度為 200 像素。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

主版頁面定義靜態頁面配置和使用主版頁面的 ASP.NET 頁面可編輯的區域。 這些內容可編輯區域以 ContentPlaceHolder 控制項內容中，可以看到&lt;div&gt;。 我們的主版頁面都有單一 ContentPlaceHolder (MainContent)，但是主版頁面可能會有多個 ContentPlaceHolders。

上面輸入的標記，切換至 [設計] 檢視會顯示主版頁面的版面配置。 任何使用此主版頁面的 ASP.NET 網頁將會有這個統一的版面配置和指定的標記 MainContent 區域的能力。


[![主版頁面，檢視透過 [設計] 檢視時](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**圖 04**: 主版頁面中，當檢視透過 設計檢視 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>建立內容頁面

現在我們有 Default.aspx 頁面中我們的網站，但不會使用我們剛才建立的主版頁面。 雖然您可以管理網頁使用主版頁面中，宣告式標記，如果頁面未包含任何內容尚未很容易，只要刪除頁面，並將其重新加入專案中，指定要使用的主版頁面。 因此，開始從專案刪除 Default.aspx。

接下來，在 [方案總管] 中的專案名稱上按一下滑鼠右鍵，然後選擇要加入新的 Web Form，名為 Default.aspx。 此時，檢查選取的主版頁面的核取方塊，並從清單中選擇 Site.master 主版頁面。


[![加入新的 Default.aspx 頁面選擇 選取主版頁面](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**圖 05**： 加入新 Default.aspx 頁面選擇 選取主版頁面 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image15.png))


[![使用 Site.master 主版頁面](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**圖 06**： 使用 Site.master 主版頁面 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> 如果您使用 Web 應用程式專案模型加入新項目 對話方塊不包含選取的主版頁面的核取方塊。 相反地，您需要加入 Web 內容的表單類型的項目。 選擇 Web 內容格式選項，並按一下 [新增] 之後，Visual Studio 會顯示相同的 Select Master 圖 6 所示的對話方塊。


只包含新 Default.aspx 頁面上的宣告式標記@Page指示詞指定的路徑主要頁面的主版頁面的 MainContent ContentPlaceHolder 的檔案和內容控制項。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

現在請將 Default.aspx 保留空白。 我們將稍後在本教學課程，將內容加入它。

> [!NOTE]
> 我們的主版頁面包含功能表或一些其他的巡覽介面 > 一節。 我們將在未來的教學課程中建立這類介面。


## <a name="step-2-enabling-forms-authentication"></a>步驟 2： 啟用表單驗證

建立 ASP.NET 網站後下, 一步是要啟用表單驗證。 透過指定應用程式的驗證設定[&lt;驗證&gt;元素](https://msdn.microsoft.com/library/532aee0e.aspx)Web.config 中。&lt;驗證&gt;元素包含單一屬性，名為指定的應用程式所使用的驗證模型的模式。 這個屬性可以具有下列四個值之一：

- **Windows** -述前述教學課程中，當應用程式使用 Windows 驗證是驗證訪客，web 伺服器的責任，並通常會透過基本、 摘要式或整合式 Windows驗證。
- **Form**-透過 web 網頁上的表單驗證使用者。
- **Passport**-使用 Microsoft Passport Network 驗證使用者。
- **無**-沒有驗證模型; 所有造訪者的匿名。

根據預設，ASP.NET 應用程式會使用 Windows 驗證。 若要變更表單驗證的驗證類型，然後，我們需要修改&lt;驗證&gt;表單項目的模式屬性。

如果您的專案尚未包含 Web.config 檔，請加入一個現在藉由以滑鼠右鍵按一下專案名稱，在 方案總管 中選擇 加入新項目，然後加入 Web 組態檔。


[![如果您的專案尚未包含 Web.config，立即加入它](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**圖 07**： 如果您的專案不會不尚未包含 Web.config，現在加入 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image21.png))


接下來，找出&lt;驗證&gt;項目和更新為使用表單驗證。 此變更後，您的 Web.config 檔案標記看起來應該如下所示：

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Web.config 是一個 XML 檔案，因為大小寫非常重要。 請確定表單，以設定模式屬性，以大寫 f。如果您使用不同的大小寫，例如表單，您就會收到發生組態錯誤造訪的網站，透過瀏覽器。


&lt;驗證&gt;元素可以選擇性地包含&lt;form&gt;包含表單驗證特定設定的子系項目。 現在，我們只要使用預設表單驗證設定。 我們將探討&lt;form&gt;詳述於下一個教學課程中的子項目。

## <a name="step-3-building-the-login-page"></a>步驟 3： 建立登入頁面

為了支援表單驗證我們的網站必須登入頁面。 了解將會自動重新導向 表單驗證工作流程區段中，FormsAuthenticationModule 中所述的登入頁面使用者當他們嘗試存取的頁面，它們不是檢視權限。 也有到匿名使用者的登入頁面會顯示連結的 ASP.NET Web 控制項。 這牽涉到一個問題，登入頁面的 URL 是什麼？

根據預設，預期會命名為 Login.aspx 登入頁面表單驗證系統，並放在 web 應用程式的根目錄中。 如果您想要使用不同的登入頁面 URL，您可以藉由指定 Web.config 中。我們會了解如何在後續的教學課程中這麼做。

登入頁面具有三個責任：

1. 提供可讓輸入其認證的造訪者的介面。
2. 判斷提交的認證是否有效。
3. 登入使用者建立的表單驗證票證。

### <a name="creating-the-login-pages-user-interface"></a>建立登入頁面使用者介面

讓我們開始使用第一項工作。 將新的 ASP.NET 網頁新增至名為 Login.aspx 的站台的根目錄，並關聯 Site.master 主版頁面。


[![加入新的 ASP.NET 頁面名稱為 Login.aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**圖 08**： 加入新 ASP.NET 網頁名為 Login.aspx ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image24.png))


典型的登入頁面介面包含兩個文字方塊-一個用於使用者的名稱，另一個適用於他們的密碼-和送出表單的按鈕。 網站通常包含記住我 核取方塊，如果選取此選項，則在瀏覽器重新啟動時保存產生的驗證票證。

將兩個文字方塊加入到 Login.aspx 並分別設定為 使用者名稱和密碼，其識別碼屬性。 也將在密碼的 TextMode 屬性設為密碼。 接下來，加入核取方塊控制項，將它的 ID 屬性設定為 RememberMe，其文字屬性設定為 記住我 接下來，加入名為的 LoginButton 其文字屬性設定為登入 按鈕。 最後，將標籤 Web 控制項，並設定它的 ID 屬性 InvalidCredentialsMessage，它的文字屬性，為您的使用者名稱或密碼無效。 請再試一次。，它 ForeColor 屬性為紅色，以及其可見屬性設定為 False。

此時您的畫面看起來應該類似螢幕擷取畫面圖 9 和網頁的宣告式語法應該類似如下：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![登入頁面包含兩個文字方塊中，核取方塊、 按鈕和標籤](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**圖 09**: 登入頁面包含兩個文字方塊中，核取方塊、 按鈕和標籤 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image27.png))


最後，建立事件處理常式 LoginButton 的按一下事件。 從設計工具中，只要按兩下按鈕控制項來建立此事件處理常式。

### <a name="determining-if-the-supplied-credentials-are-valid"></a>判斷提供的認證是否有效

我們現在需要實作工作 2 中按鈕的 Click 事件處理常式-判斷提供的認證是否有效。 若要這樣做需要有使用者存放區，其中保存所有使用者的認證，讓我們可以判斷是否提供的認證符合任何已知的認證。

在 ASP.NET 2.0 之前開發人員所負責實作自己使用者存放區，並撰寫程式碼以驗證對存放區提供的認證。 大部分的開發人員會實作使用者存放區在資料庫中，建立名為資料表的使用者其資料行，例如使用者名稱、 密碼、 電子郵件、 LastLoginDate，等等。 然後，此資料表中，會有一筆記錄，每個使用者帳戶。 正在驗證的使用者提供的認證會包含查詢比對的使用者名稱的資料庫，然後確保資料庫中的密碼，對應到所提供的密碼。

使用 ASP.NET 2.0 中，開發人員應該使用其中一個成員資格提供者來管理使用者存放區。 在此教學課程中，我們將使用 SqlMembershipProvider，用於使用者存放區中的 SQL Server 資料庫。 當使用 SqlMembershipProvider 我們需要實作的特定資料庫結構描述包括資料表、 檢視和預存程序提供者所預期。 我們將檢查如何實作這個結構描述中的*[在 SQL Server 中建立成員資格結構描述](../membership/creating-the-membership-schema-in-sql-server-vb.md)*教學課程。 備妥的成員資格提供者，以驗證使用者的認證的很簡單，呼叫[成員資格類別](https://msdn.microsoft.com/library/system.web.security.membership.aspx)的[ValidateUser (*username*，*密碼*)方法](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)，它會傳回布林值，指出是否應的有效性*username*和*密碼*組合。 因為我們還沒有實作 SqlMembershipProvider 使用者存放區，我們無法在這一次使用成員資格類別的 ValidateUser 方法。

而不是花一些時間來建立自己自訂的使用者資料庫資料表 （這會是已過時之後我們實作 SqlMembershipProvider,），讓我們改為硬式編碼內登入的有效認證頁面本身。 在 LoginButton Click 事件處理常式，加入下列程式碼：

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

如您所見，有三個有效的使用者帳戶-Scott、 Jisun，以及 Sam-，這三個有相同的密碼 （密碼）。 此程式碼迴圈尋找有效的使用者名稱和密碼比對使用者和密碼的陣列。 如果使用者名稱和密碼是有效的我們需要登入使用者，並再重新導向至適當的頁面。 如果是無效的認證，我們會顯示 InvalidCredentialsMessage 標籤。

當使用者輸入有效的認證時，所述，它們會再重新導向至適當的頁面。 何謂透過適當的頁面？ 前文提過，當使用者存取他們未獲授權檢視的頁面，FormsAuthenticationModule 自動重新導向至登入頁面。 在此情況下，它會包含透過 ReturnUrl 參數 querystring 中要求的 URL。 也就是說，如果使用者嘗試瀏覽 ProtectedPage.aspx，而且他們未獲授權執行這項操作，FormsAuthenticationModule 會將他們重新導向至：

Login.aspx?ReturnUrl=ProtectedPage.aspx

在成功登入，使用者應該被重新導向回到 ProtectedPage.aspx。 或者，使用者可能會在他們自己 volition 上瀏覽登入頁面。 在此情況下，在使用者登入之後，它們應該傳送至根資料夾的 Default.aspx 頁面。

### <a name="logging-in-the-user"></a>登入使用者

假設提供的認證是有效的我們需要建立表單驗證票證，藉此登入至站台的使用者。 [FormsAuthentication 類別](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx)中[System.Web.Security 命名空間](https://msdn.microsoft.com/library/system.web.security.aspx)提供各種的方法登入和登出使用者透過表單驗證系統。 雖然 FormsAuthentication 類別有數種方法，便會是我們在此時感興趣的三個：

- [GetAuthCookie (*username*， *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) -建立提供名稱的表單驗證票證*username*。 接下來，此方法會建立，並傳回 HttpCookie 物件，包含驗證 ticket 的內容。 如果*persistCookie*為 True 時，會建立永續性 cookie。
- [SetAuthCookie (*username*， *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -呼叫 GetAuthCookie (*username*， *persistCookie*)若要產生表單驗證 cookie 的方法。 此方法接著會將 cookie 傳回 GetAuthCookie （假設 cookie 為基礎的表單驗證正在使用; 否則這個方法會呼叫內部類別，處理 cookie 票證邏輯） 的 Cookie 集合。
- [RedirectFromLoginPage (*username*， *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -這個方法會呼叫 SetAuthCookie (*username*， *persistCookie*)，並再將使用者重新導向至適當的頁面。

當您需要先寫出的 cookie 的 Cookie 集合中修改驗證票證，GetAuthCookie 很方便。 如果您想要建立表單驗證票證，並將它加入至 Cookie 集合中，但不是想要將使用者重新導向至適當的頁面，SetAuthCookie 很有用。 可能是您想要將它們保存在登入頁面，或將它們傳送給一些其他的頁面。

由於我們想要登入使用者，並將它們重新導向至適當的頁面，讓我們使用 RedirectFromLoginPage。 更新 LoginButton 的按一下事件處理常式，加上註解的兩個 TODO 行取代成下列程式碼行：

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

建立表單驗證票證時我們使用表單驗證票證的使用者名稱文字方塊的 Text 屬性*username*參數和 RememberMe 核取方塊的核取的狀態*persistCookie*參數。

若要測試的登入頁面，請在瀏覽器中造訪。 先輸入認證無效，例如有一天的使用者名稱及密碼的錯誤。 按一下 [登入] 按鈕後會發生回傳，InvalidCredentialsMessage 標籤將會顯示。


[![InvalidCredentialsMessage 標籤會顯示當輸入不正確的認證](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**圖 10**: InvalidCredentialsMessage 標籤會顯示當輸入不正確的認證 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image30.png))


接下來，輸入有效的認證，然後按一下 [登入] 按鈕。 此時回傳發生表單驗證票證時建立，並將您自動重新導向回 Default.aspx。 此時您已登入到網站時，雖然沒有視覺提示，以表示您目前登入。 在步驟 4 中我們會了解如何以程式設計方式判斷使用者是否已登入，或不以及如何識別使用者瀏覽頁面。

步驟 5 中，會檢查記錄將使用者登出網站的技術。

### <a name="securing-the-login-page"></a>保護的登入頁面

（包含其密碼） 的認證時使用者輸入其認證，並提交登入頁面表單，會傳送至 web 伺服器在網際網路上*純文字*。 這表示使用者名稱和密碼，可以看到任何駭客探查網路流量。 若要避免這個問題，務必要使用加密的網路流量[安全通訊端層 (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)。 這可確保加密認證 （以及整個網頁的 HTML 標記），直到收到 web 伺服器離開瀏覽器的時間。

除非您的網站包含機密資訊，您只需要使用 SSL 登入頁面和其他頁面上，否則會傳送使用者的密碼以純文字方式透過網路。 您不需要擔心保護表單驗證票證，因為根據預設，就會同時進行加密及數位簽章 （以避免遭到竄改）。 下列教學課程中顯示表單驗證票證安全性的更完整討論。

> [!NOTE]
> 許多財務和醫療網站設定為使用 SSL 上*所有*網頁可以存取已驗證的使用者。 如果您要建置這類網站可以設定表單驗證系統，因此表單驗證票證，才會透過安全的連線傳輸。 我們會在下一個教學課程中，查看各種表單驗證組態選項*[表單驗證設定和進階主題](../membership/creating-the-membership-schema-in-sql-server-vb.md)*。


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>步驟 4： 偵測已驗證的訪客，並判斷其身分識別

此時我們啟用表單驗證並建立基本的登入頁面上，但我們尚未檢查我們如何判斷使用者是否已驗證或匿名。 在某些情況下，我們可能想要顯示不同的資料或根據已驗證或匿名使用者已瀏覽頁面的資訊。 此外，我們有時候需要知道的已驗證的使用者識別。

讓我們來強化現有的 Default.aspx 頁面，來說明這些技術。 在 Default.aspx 中加入兩個面板控制項，一個具名的 AuthenticatedMessagePanel 和另一個具名的 AnonymousMessagePanel。 加入名為 WelcomeBackMessage 第一個面板中的標籤控制項。 第二個面板中加入超連結控制項，其文字屬性設定為登入和它的 NavigateUrl 屬性以 ~ / Login.aspx。 此時 Default.aspx 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

您可能已經猜到目前為止，這裡的想法是已驗證的訪客並只為匿名的訪客 AnonymousMessagePanel 顯示只 AuthenticatedMessagePanel。 若要完成這項作業，我們需要設定這些面板根據使用者登入或不可見的屬性。

[Request.IsAuthenticated 屬性](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx)傳回布林值，指出是否已驗證要求。 下列程式碼輸入至網頁\_載入事件處理常式程式碼：

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

此位置的程式碼，請透過瀏覽器瀏覽 Default.aspx。 假設您沒有登入，您會看到登入頁面的連結 （請參閱圖 11）。 按一下此連結，登入網站。 如我們所見在步驟 3 中，輸入您的認證之後您將會傳回 default.aspx，但這次頁面會顯示歡迎回來 ！ 訊息 （請參閱圖 12）。


[![當瀏覽以匿名方式，記錄檔中連結會顯示](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**圖 11**： 會顯示當瀏覽以匿名方式、 記錄檔中連結 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image33.png))


[![已驗證的使用者會顯示歡迎回來 ！Message](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**圖 12**： 已驗證的使用者會顯示歡迎回來 ！ 訊息 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image36.png))


我們可以判斷目前登入使用者的身分識別，經由[HttpContext 物件](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)的[使用者屬性](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)。 HttpContext 物件代表目前的要求的相關資訊，和其他項目回應、 要求，以及工作階段中，這類一般 ASP.NET 物件首頁。 使用者屬性表示目前 HTTP 要求和實作的安全性內容[IPrincipal 介面](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx)。

使用者屬性是由設定 FormsAuthenticationModule。 具體而言，當 FormsAuthenticationModule 傳入要求中找到表單驗證票證，它會建立新的 GenericPrincipal 物件並將它指派給使用者屬性。

Principal 物件 （例如 GenericPrincipal) 提供的使用者身分識別和它們所隸屬的角色的相關資訊。 IPrincipal 介面會定義兩個成員：

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -傳回布林值，指出主體是否屬於指定角色的方法。
- [識別](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx)-傳回物件，用於實作的屬性[IIdentity 介面](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx)。 IIdentity 介面會定義三個屬性： [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx)， [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)，和[名稱](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx)。

我們可以判斷目前的訪客使用下列程式碼的名稱：

Dim currentUsersName 做為字串 = User.Identity.Name

當使用表單驗證， [FormsIdentity 物件](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx)建立 GenericPrincipal 識別屬性。 FormsIdentity 類別一律會傳回字串形式的 AuthenticationType 屬性和 True 的 IsAuthenticated 屬性。 Name 屬性會傳回建立的表單驗證票證時指定的使用者名稱。 這三個屬性，除了 FormsIdentity 包括透過基礎的驗證票證的存取其[票證屬性](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx)。 票證屬性傳回型別的物件[FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)，其中包含屬性，例如到期、 IsPersistent、 IssueDate、 名稱和等等。

才會離開以下是很重要的一點*username* FormsAuthentication.GetAuthCookie 中指定的參數 (*username*， *persistCookie*)，FormsAuthentication.SetAuthCookie (*username*， *persistCookie*)，和 FormsAuthentication.RedirectFromLoginPage (*username*， *persistCookie*) 方法是 User.Identity.Name 所傳回的相同值。 此外，這些方法所建立的驗證票證將 User.Identity FormsIdentity 物件轉型，然後存取票證屬性會提供：

Dim ident 為 FormsIdentity = CType （User.Identity、 FormsIdentity）

Dim authTicket As FormsAuthenticationTicket = ident.Ticket

讓我們提供更個人化的訊息在 Default.aspx 中。 更新頁面\_載入事件處理常式，讓 WelcomeBackMessage 標籤的 文字屬性指派字串歡迎回來， *username*！

WelcomeBackMessage.Text = "Welcome back, " &amp; User.Identity.Name &amp; "!"

圖 13 顯示這項修改的效果 （當使用者 Scott 身分登入）。


[![歡迎訊息包含目前記錄中使用者的名稱](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**圖 13**： 歡迎訊息包含目前登入使用者的名稱 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>使用 LoginView 和 LoginName 控制項

已驗證和匿名使用者顯示不同的內容是常見的需求。所以會顯示目前登入使用者的名稱。 因此，ASP.NET 會包含提供相同的功能，顯示在 「 圖 13，但不需要撰寫一行程式碼的兩個 Web 控制項。

[LoginView 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)是範本型 Web 控制項，以簡化顯示不同的資料驗證和匿名使用者。 LoginView 包含兩個預先定義的範本：

- AnonymousTemplate-任何標記加入至這個範本只會顯示為匿名的訪客。
- LoggedInTemplate-此範本的標記顯示只是為了通過驗證的使用者。

讓我們加入我們的網站主頁面上，Site.master LoginView 控制項。 而非新增 LoginView 控制項，不過，讓我們加入兩個新的 ContentPlaceHolder 控制項，再放入該新 ContentPlaceHolder 內 LoginView 控制項。 這項決策的基本原理會儘速明顯。

> [!NOTE]
> 除了 AnonymousTemplate，LoggedInTemplate LoginView 控制項可以包含特定角色的範本。 特定角色的範本會顯示標記，只對屬於指定角色的使用者。 在未來的教學課程中，我們將檢查 LoginView 控制項的以角色為基礎的功能。


開始加入主版頁面內瀏覽至名為 LoginContent ContentPlaceHolder &lt;div&gt;項目。 您只可以拖曳 ContentPlaceHolder 控制項從工具箱拖曳至來源檢視中，將產生的標記上面 TODO： 功能表將在此處的文字。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

接下來，加入內 LoginContent ContentPlaceHolder LoginView 控制項。 放入主版頁面的 ContentPlaceHolder 控制項的內容會被視為*預設內容*ContentPlaceHolder 的。 也就是 ASP.NET 網頁使用此主版頁面可以指定每個 ContentPlaceHolder 其本身的內容，或使用主版頁面的預設內容。

LoginView 與其他登入相關的控制項位於 工具箱 的登入 索引標籤。


[![LoginView 控制項，在 [工具箱]](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**圖 14**： 在 [工具箱] 的 LoginView 控制項 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image42.png))


接下來，加入兩個&lt;b /&gt;立即 LoginView 控制項之後，但仍在 ContentPlaceHolder 內的項目。 此時，瀏覽&lt;div&gt;項目的標記應該看起來如下所示：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

從設計工具或宣告式標記可定義 LoginView 的範本。 從 Visual Studio 設計工具中，展開 LoginView 的智慧標籤，列出在下拉式清單中設定的範本。 輸入文字 Hello，陌生人到 AnonymousTemplate;接下來，將超連結控制項，並將其文字和 NavigateUrl 屬性設定為登入和 ~ / Login.aspx，分別。

設定後 AnonymousTemplate，切換到 LoggedInTemplate 並輸入文字、 「 歡迎回來，"。 然後將 LoginName 控制項從 [工具箱] 拖曳到 LoggedInTemplate，放置 「 歡迎回來，"文字的後面。 [LoginName 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx)，正如其名，顯示目前登入使用者的名稱。 就內部而言，LoginName 控制項只會輸出 User.Identity.Name 屬性

LoginView 範本這些新增項目之後，請在標記看起來應該如下所示：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

此加入至 Site.master 主版頁面中，我們的網站中每一頁會顯示不同的訊息，根據是否已驗證使用者。 圖 15 顯示 Default.aspx 頁面上，當使用者 Jisun 透過瀏覽器瀏覽。 歡迎回來，Jisun 訊息重複兩次： 一次是在主版頁面的導覽 （透過剛才加入的 LoginView 控制項） 左邊 區段，一次在 Default.aspx 中的內容 （透過 Panel 控制項和程式設計邏輯） 的區域。


[![LoginView 控制項顯示歡迎回來 Jisun。](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**圖 15**: LoginView 控制項顯示歡迎回來，Jisun。 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image45.png))


因為我們 LoginView 加入主版頁面中，它可以出現在我們的網站上的每一頁。 不過，可能有網頁，我們不想顯示此訊息。 一個這類頁面是登入頁面上，因為登入頁面的連結看起來有位置超出。 因為我們 LoginView 控制項置於 ContentPlaceHolder 主版頁面中，我們可以在我們的內容頁面中覆寫此預設標記。 開啟 Login.aspx 並移至設計工具。 因為我們未明確定義的內容控制項在主版頁面 LoginContent ContentPlaceHolder 的 Login.aspx，登入頁面會顯示主版頁面的預設標記這個 ContentPlaceHolder。 您可以看到此設計工具-透過 LoginContent ContentPlaceHolder 顯示預設的標記 （LoginView 控制項）。


[![登入頁面會顯示預設內容主版頁面 LoginContent ContentPlaceHolder](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**圖 16**： 登入頁面顯示的 預設內容主版頁面 LoginContent ContentPlaceHolder ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image48.png))


LoginContent ContentPlaceHolder 覆寫預設標記，只要以滑鼠右鍵按一下設計工具中的區域，並從內容功能表中選擇 [建立自訂內容] 選項。 (當使用 Visual Studio 2008 ContentPlaceHolder 包含智慧標籤，選取時，通常會提供相同的選項。)這會將新的內容控制項網頁的標記，因此讓我們來定義自訂此頁面的內容。 您可以新增自訂訊息，請登入，例如，但是我們只將此留白。

> [!NOTE]
> 在 Visual Studio 2005 中建立自訂內容建立空內容 ASP.NET 網頁中的控制項。 在 Visual Studio 2008 中，不過，建立自訂內容的主版頁面的預設內容複製至新建立的內容控制項。 如果您使用 Visual Studio 2008，然後建立新的內容控制項之後請務必清除 覆寫從主版頁面的內容。


圖 17 顯示的 Login.aspx 頁面進行這項變更後，從瀏覽器瀏覽。 請注意，沒有任何 Hello 陌生人或歡迎回來， *username*訊息位於左邊功能窗格&lt;div&gt;因為沒有造訪 Default.aspx 時。


[![登入頁面會隱藏預設 LoginContent ContentPlaceHolder 標記](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**圖 17**： 登入頁面會隱藏預設 LoginContent ContentPlaceHolder 的標記 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>步驟 5： 登出

我們探討了建置到網站、 登入使用者的登入頁面在步驟 3 中，但我們尚未以了解如何將使用者登出。記錄中的使用者的方法，除了 FormsAuthentication 類別也提供[SignOut 方法](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)。 SignOut 方法只會終結表單驗證票證，藉此記錄將使用者登出網站。

供應項目 [登出] 連結共通功能的 ASP.NET 包含控制項，專門用來將使用者登出。[LoginStatus 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx)顯示登入 LinkButton 或登出 LinkButton，根據使用者的驗證狀態。 登入 LinkButton 呈現匿名使用者，而顯示已驗證的使用者登出 LinkButton。 登入及登出 LinkButtons 文字可以設定透過 LoginStatus LoginText 和 LogoutText 屬性。

按一下 登入 LinkButton 導致的回傳，從中發出重新導向至登入頁面。 按一下登出 LinkButton 導致 LoginStatus 控制項叫用 FormsAuthentication.SignOff 方法，然後將使用者導向至網頁。 [記錄] 頁面上關閉使用者重新導向至取決於 LogoutAction 屬性，可以指派給三個下列值之一：

- 重新整理-預設值。將使用者重新導向自己所造訪的頁面。 如果它們只已造訪的頁面不允許匿名使用者，然後 FormsAuthenticationModule 會自動將使用者重新導向的登入頁面。

您可能會好奇對於為什麼重新導向此執行。 如果使用者想要保留在相同頁面上，為什麼需要明確的重新導向？ 原因是因為使用者按一下登出 LinkButton 時，仍有其 cookie 集合中表單驗證票證。 因此，回傳的要求是已驗證的要求。 LoginStatus 控制項呼叫 SignOut 方法，但 FormsAuthenticationModule 已經驗證使用者之後，會發生的事。 因此，明確的重新導向會導致重新要求的網頁瀏覽器。 瀏覽器重新要求網頁時，表單驗證票證已移除，因此傳入的要求是匿名。

- 重新導向的使用者重新導向至 LoginStatus LogoutPageUrl 屬性所指定的 URL。
- RedirectToLoginPage-使用者重新導向至登入頁面。

讓我們將 LoginStatus 控制項加入主版頁面，並將它設定為使用重新導向選項來顯示訊息，確認，它們已登出頁面傳送給使用者。從名為 Logout.aspx 的根目錄中建立頁面開始。 別忘了關聯 Site.master 主版頁面的此頁面。 接下來，輸入網頁的標記已經登出的使用者說明中的訊息。

接下來，返回 Site.master 主版頁面，並以 LoginContent ContentPlaceHolder 加入下方 LoginView LoginStatus 控制項。 設 ~/Logout.aspx LoginStatus 控制項的 LogoutAction 屬性為重新導向，以及其 LogoutPageUrl 屬性。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

由於 LoginStatus LoginView 控制項之外，它會顯示針對匿名和已驗證使用者，沒關係，因為登入或登出 LinkButton LoginStatus 會正確顯示。 LoginStatus 控制項加入之後，記錄在中的超連結 AnonymousTemplate 多餘，因此將它移除。

圖 18 顯示 Default.aspx，當 Jisun 造訪。 請注意，左側資料行，將會顯示訊息，歡迎回來，以及連結的 Jisun 登出。按一下 登出 LinkButton 導致回傳 Jisun 登出系統，，然後將其重新導向至 Logout.aspx。 如圖 19 所示，Jisun 到達的 Logout.aspx 她已簽署，因此屬於匿名的時間。 因此，左側資料行顯示的文字歡迎陌生人和的登入頁面的連結。


[![Default.aspx 顯示歡迎回來，以及登出 LinkButton Jisun](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**圖 18**: Default.aspx 顯示  褖畫惎回、 Jisun 沿著與登出 LinkButton ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx 顯示 歡迎使用，以及登入 LinkButton 陌生人](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**圖 19**: Logout.aspx 顯示 歡迎使用，以及登入 LinkButton 陌生人 ([按一下以檢視完整大小的影像](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> 建議您將自訂 Logout.aspx 頁面，以隱藏主版頁面的 LoginContent ContentPlaceHolder （如同我們的 Login.aspx 在步驟 4）。 原因是因為 LoginStatus 控制項所呈現的登入 LinkButton (Hello 下, 一個陌生人) 將使用者傳送至目前 URL 傳遞 ReturnUrl querystring 參數中的登入頁面。 簡單地說，如果已登出的使用者按一下此 LoginStatus 登入 LinkButton，，然後登入它們會回到 Logout.aspx，無法輕鬆地將使用者重新導向。


## <a name="summary"></a>總結

本教學課程中我們將開始使用表單驗證工作流程的檢查，然後開啟在 ASP.NET 應用程式中實作表單驗證。 表單驗證由 FormsAuthenticationModule，具有兩個責任： 識別使用者根據其表單驗證票證，以及將未經授權的使用者重新導向至登入頁面。

.NET Framework FormsAuthentication 類別包含建立、 檢查和移除表單驗證票證的方法。 Request.IsAuthenticated 屬性和使用者物件提供額外的程式設計支援，決定是否已驗證要求及使用者的身分識別的相關資訊。 另外還有 LoginView、 LoginStatus 和 LoginName Web 控制項，可讓開發人員快速、 無程式碼的方式來執行許多常見的登入相關工作。 在未來教學課程中，我們會檢查這些和其他登入相關的 Web 控制項更詳細地。

本教學課程提供表單驗證的粗略概觀。 我們未檢查的各種的設定選項，查看如何在無 cookie 的表單驗證票證工作，或瀏覽 ASP.NET 如何保護表單驗證票證的內容。 我們將討論下列主題，以及在多個[下一個教學課程](forms-authentication-configuration-and-advanced-topics-vb.md)。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [IIS6 與 IIS7 安全性之間的變更](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [登入 ASP.NET 控制項](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [專業 ASP.NET 2.0 安全性、 成員資格和角色管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [&lt;驗證&gt;項目](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Form&gt;元素&lt;驗證&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教學課程所包含的主題訓練影片

- [在 ASP.NET 中使用基本的表單驗證](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 前導檢閱者在此教學課程包含 Alicja Maziarz、 John Suru 和本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)。

>[!div class="step-by-step"]
[上一頁](security-basics-and-asp-net-support-vb.md)
[下一頁](forms-authentication-configuration-and-advanced-topics-vb.md)
