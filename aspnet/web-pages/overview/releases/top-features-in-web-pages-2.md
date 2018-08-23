---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: ASP.NET Web pages 2 的頂端功能 |Microsoft Docs
author: microsoft
description: 本主題概要說明在 ASP.NET Web Pages 2，隨附於 WebMatr 的輕量型 web 程式設計架構中最上層的新功能...
ms.author: riande
ms.date: 02/13/2012
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: e06db7eb33dc2891d86b65fa56c20b9e8cae1970
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831157"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>ASP.NET Web pages 2 最上層的功能
====================
by [Microsoft](https://github.com/microsoft)

> 這篇文章提供在 ASP.NET Web Pages 2 RC 中，所含的輕量型 web 程式設計架構最上層的新功能的概觀[Microsoft WebMatrix 2 Rc<](https://www.microsoft.com/web/)。
> 
> **包含的內容：** 
> 
> - [安裝 WebMatrix](#install)
> - [全新和增強功能](#New_and_Enhanced_Features)
> 
>     - [RC 版本的變更](#Changes_for_the_RC_Version)
>     - [針對 Beta 版的變更](#Changes_for_the_Beta_Version)
>     - [使用新的和更新的網站範本](#templates)
>     - [驗證使用者輸入](#validation)
>     - [啟用登入和來自 Facebook 與其他站台使用 OAuth 和 OpenID](#oauthsetup)
>     - [新增對應使用 Map 協助程式](#maphelper)
>     - [執行網頁應用程式並排顯示](#sidebyside)
>     - [行動裝置的轉譯頁面](#mobile)
> - [其他資源](#resources)
> 
> > [!NOTE]
> > 本主題假設您使用 WebMatrix 來處理您的 ASP.NET Web Pages 2 程式碼。 不過，因為 Web Pages 1，您也可以建立使用 Visual Studio 的 Web Pages 2 網站，可讓您增強 IntelliSense 功能和偵錯。 若要使用 Visual Studio 中的網頁，您必須先安裝 Visual Studio 2010 SP1、 Visual Web Developer Express 2010 SP1 或 Visual Studio 11 beta 版。 接著，安裝 ASP.NET MVC 4 Beta，其中包含用於在 Visual Studio 中建立 ASP.NET MVC 4 和 Web Pages 2 應用程式範本和工具。
> 
> 
> *上次更新： 18 年 6 月 2012年*


<a id="install"></a>
## <a name="installing-webmatrix"></a>安裝 WebMatrix

若要安裝的網頁，您可以使用 Microsoft Web Platform Installer 中，這是免費的應用程式，可讓您更輕鬆地安裝及設定 web 相關的技術。 您將安裝 WebMatrix 2 beta 版，其中包含 Web Pages 2 的 beta 版。

1. 瀏覽至 Web Platform Installer 的最新版本的安裝頁面：

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > 如果您已經安裝 WebMatrix 1，此安裝會更新它 WebMatrix 2 beta 版。 您可以執行使用同一部電腦上的版本 1 或 2 所建立的網站。 詳細資訊，請參閱上一節[執行的網頁應用程式並排顯示](#sidebyside)。
2. 選擇**立即安裝**。 

    如果您使用 Internet Explorer，請移至下一個步驟。 如果您使用不同的瀏覽器，例如 Mozilla Firefox 或 Google Chrome，系統會提示您儲存*Webmatrix.exe*檔案至您的電腦。 儲存檔案，然後按一下以啟動安裝程式。
3. 執行安裝程式，然後選擇**安裝** 按鈕。 這會安裝 WebMatrix 和網頁。

## <a id="New_and_Enhanced_Features"></a>  全新和增強功能

### <a id="Changes_for_the_RC_Version"></a>  變更的 RC 版本 (第 2012 年 6 月)

2012 年 6 月發行的 RC 版本有一些變更，從已於 2012 年 3 月發行的 Beta 版本重新整理。 這些變更包括：

- A`Validation.AddFormError`方法加入至`Validation`協助程式。 如果您手動執行驗證，這非常有用 （例如，您驗證傳遞查詢字串中的值） 和您想要新增錯誤訊息，可以藉由顯示`Html.ValidationSummary`方法。 如需詳細資訊，請參閱[驗證資料，並不會直接從使用者](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users)中[ASP.NET Web Pages (Razor) 網站中驗證使用者輸入](https://go.microsoft.com/fwlink/?LinkId=253002)。
- 統合和縮製的功能都已從 ASP.NET Web Pages 2 核心組件。 如此一來，`Assets`本文件稍後所列的協助程式未提供。 相反地，您必須安裝[ASP.NET 最佳化](http://nuget.org/packages/Microsoft.Web.Optimization/0.1)NuGet 套件。 如需詳細資訊，請參閱 <<c0> [ 統合及縮小資產的 ASP.NET Web Pages (Razor) 網站](https://go.microsoft.com/fwlink/?LinkId=255373)。
- 已新增支援 ASP.NET Web Pages 2 的其他組件。 只有明顯的影響，這項變更是您可能會看到多個組件中的站台*bin*資料夾之後，您建立網站，或將網站部署至主機服務提供者。

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>搶鮮版 (年 2 月 2012) 的變更

在 2012 年 2 月發行的 Beta 版有只有少數變更從 2011 年 12 月發行的 Beta 版。 這些變更包括：

- Razor 現在支援條件式屬性。 在 HTML 項目，如果您將屬性設定為值，則是在伺服器程式碼中解決`false`或`null`，ASP.NET 不會完全呈現的屬性。 例如，假設您有下列標記核取方塊：

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    如果的值`checked1`解析成`false`上，或者`null`，則`checked`屬性則不會呈現。 這是一項重大變更。
- `Validation.GetHtml`方法已重新命名為`Validation.For`。 這是一項重大變更;`Validation.GetHtml` Beta 版中無法運作。
- 您現在可以包含`~`標記中的運算子來參考網站根目錄，而不需使用`Href`函式。 (也就是 Razor 剖析器現在可以找到並解決`~`而不需要明確的方法呼叫運算子`Href`。)`Href`方法仍可運作，因此這不是一項重大變更。

    例如，如果您先前已有此類標記：

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    您現在可以使用像這樣的標記：

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts`資產 （資源） 管理的協助程式已取代為`Assets`協助程式，有稍微不同的方法，如下所示：

  - 針對`Scripts.Add`，使用 `Assets.AddScript`
  - 針對`Scripts.GetScriptTags`，使用 `Assets.GetScripts`

    這是一項重大變更;`Scripts`類別不是 Beta 版中。 這項變更已更新這份文件中使用資產管理的程式碼範例。

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>使用新的和更新的網站範本

**入門網站**範本已更新以便在 Web Pages 2 上執行，根據預設。 它也包含下列新功能：

- 行動設備友善網頁呈現。 透過使用 CSS 樣式與`@media`選取器中，**入門網站**較小的螢幕，包括行動裝置螢幕上提供的頁面的呈現改善。
- 改善的成員資格和驗證選項。 您可以讓使用者登入您的網站使用他們的帳戶，從其他社交網路網站，例如 Twitter、 Facebook 和 Windows Live。 如需詳細資訊，請參閱 <<c0> [ 啟用的登入和來自 Facebook 與其他站台使用 OAuth 和 OpenID](#oauthsetup)一節。
- HTML5 項目。

新**個人網站**範本可讓您建立包含個人部落格、 相片 頁面上，以及 Twitter 網頁的網站。 您可以自訂網站，根據**個人網站**範本，執行下列動作：

- 編輯版面配置檔，以變更網站的外觀 (*\_SiteLayout.cshtml*) 和樣式檔案 (*Site.css*)。
- 安裝 NuGet 套件，將功能新增至您的網站。 如需如何安裝封裝的資訊，包括 ASP.NET Web Helpers Library，請參閱本教學課程的相關[安裝協助程式](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)。

若要存取**個人網站**範本中，選擇**範本**在 WebMatrix**快速入門**螢幕。

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

在 **範本**對話方塊方塊中，選擇**個人網站**範本。

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

登陸頁面**個人網站**範本可讓您遵循連結來設定您的部落格、 Twitter 頁面和相片頁面。

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>驗證使用者輸入

在 Web Pages 1，來驗證使用者輸入，在提交表單，您會使用`System.Web.WebPages.Html.ModelState`類別。 (所示的程式碼範例的數個標題為 Web Pages 1 教學課程[使用資料](../data/5-working-with-data.md)。)您仍然可以使用這種方法，在 Web Pages 2。 不過，Web Pages 2 也提供改良的工具，來驗證使用者輸入：

- 新的驗證類別，包括`System.Web.WebPages.ValidationHelper`和`System.Web.WebPages.Validator`，可讓您執行功能強大的驗證工作，只要幾行程式碼。
- （選擇性） 用戶端驗證，即時回饋給使用者，而不需要往返伺服器檢查是否有驗證錯誤。 （基於安全性理由，被執行驗證的伺服器上即使事先在用戶端已執行檢查。）

若要使用新的驗證功能，執行下列作業：

在頁面的程式碼，註冊項目以進行驗證所使用的方法`Validation`協助程式： `Validation.RequireField`， `Validation.RequireFields` （若要註冊為需要多個項目），或`Validation.Add`。 `Add`方法可讓您指定其他類型的驗證檢查，例如資料型別檢查、 比較在不同的欄位，字串長度檢查的項目和模式 （使用規則運算式）。 以下是一些範例：

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

若要顯示欄位特有的錯誤，請呼叫`Html.ValidationMessage`正在驗證每個項目的標記中：

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

若要顯示的摘要 (`<ul>`清單) 的頁面上中的所有錯誤`Html.ValidationSummary`在標記中：

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

下列步驟便足以實作伺服器端驗證。 如果您想要新增用戶端驗證時，執行下列作業之外。

新增下列指令碼檔案參考，在`<head>`網頁上的一節。 前兩個指令碼參考會指向內容傳遞網路 (CDN) 伺服器上的遠端檔案。 第三個參考會指向本機指令碼檔案。 無法使用 CDN 時，生產環境應用程式應該實作後援。 測試此後援。

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

若要取得的本機副本最簡單的方式*jquery.validate.unobtrusive.min.js*程式庫是要建立新的網頁站台，根據其中一個網站範本 （例如入門網站）。 包含範本所建立的站台*jquery.validate.unobtrusive.js*檔案在其指令碼資料夾中，從中您可以將它複製到您的網站。

如果您的網站使用<em>\_SiteLayout</em>來控制頁面版面配置頁面中，您可以在該頁面中包含這些指令碼參考，以便驗證內容的所有頁面，您可以使用。 如果您想要執行驗證，只在特定的頁面上，您可以使用資產管理員註冊的指令碼，只需將這些頁面上。 若要這樣做，請呼叫`Assets.AddScript(path)`在頁面中您想要驗證，並參考每個指令碼檔案。 然後將呼叫加入`Assets.GetScripts`中 <em>\_SiteLayout</em>若要呈現已註冊的頁面`<script>`標記。 如需詳細資訊，請參閱[註冊的指令碼與資產管理員](#resmanagement)。

在標記中的個別項目，呼叫`Validation.For`方法。 這個方法會發出屬性，jQuery 可以連結以提供用戶端驗證。 例如: 

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

下列範例會驗證使用者輸入，在表單上的頁面。 若執行並測試此驗證程式碼，請執行下列動作：

1. 建立新的網站上，使用其中一個 WebMatrix 2 的網站範本，其中包含*指令碼*資料夾，例如**入門網站**範本。
2. 在新的站台，建立新 *.cshtml*頁面，然後以下列程式碼取代頁面內容。
3. 執行網頁瀏覽器中。 輸入有效和無效的值，以查看驗證上的效果。 例如，將必要的欄位保留空白，或輸入內的字母**信用額度**欄位。


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

當使用者提交有效的輸入，以下是頁面：

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

使用者所提交的是它具有必要的欄位保留空白時，以下是頁面：

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

在使用者提交使用中的整數以外的項目時，以下是頁面**信用額度**欄位：

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

如需詳細資訊，請參閱下列部落格文章：

- [更新 Web Pages v2 中的驗證](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344)新增驗證使用的基本概念`Validation`協助程式 （僅伺服器端）
- [更新 Web Pages v2，第 2 部分中的驗證](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347)加入用戶端驗證。
- [更新 Web Pages v2，第 3 部分中的驗證](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351)格式化驗證錯誤。

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>正在註冊指令碼使用資產管理員

資產管理員是新的功能，您可以使用伺服端程式碼來註冊及轉譯用戶端指令碼。 當您正在使用 （例如版面配置頁面中，內容頁面，協助程式等） 會結合成單一頁面上，在執行階段的多個檔案的程式碼，這個功能就很有幫助。 資產管理員協調，原始程式檔，以確定指令碼檔案的參考都正確並有效率地在轉譯頁面上，不論哪一個程式碼檔案從呼叫或呼叫的次數。 資產管理員也會呈現`<script>`標記在正確的位置，讓網頁可以載入快速 （而不需要下載指令碼在呈現時），並以避免如果呈現之前會呼叫指令碼可能會發生的錯誤已完成。

例如，假設您建立自訂的協助程式會呼叫 JavaScript 檔案，並在三個不同地方呼叫此協助程式在您的內容頁面程式碼。 如果您未使用的資產管理員註冊的指令碼會呼叫中的協助程式，有三個不同`<script>`都指向相同的指令碼檔案會出現在轉譯頁面的標記。 此外，根據 where`<script>`標記會插入在呈現的頁面、 指令碼嘗試存取特定頁面項目 頁面完全載入之前可能會發生錯誤。 如果您使用 「 資產管理員 」 來註冊指令碼時，您會避免這些問題。

您可以執行此動作，與資產管理員註冊的指令碼：

- 需要參考指令碼的程式碼，在呼叫`Assets.AddScript`方法。
- 在   *\_SiteLayout*頁面上，呼叫`Assets.GetScripts`方法，以呈現`<script>`標記。 

    > [!NOTE]
    > 呼叫`Assets.GetScripts`中的最後一個項目為`<body>`項目 *\_SiteLayout*頁面。 這有助於網頁載入速度，而且可以協助避免指令碼錯誤。

下列範例會顯示資產管理員的運作方式。 程式碼包含下列項目：

- 名為自訂 helper `MakeNote`。 此協助程式會在方塊內的字串呈現藉由包裝`div`其周圍的項目，加上框線，並可加入已設定樣式&quot;附註：&quot;給它。 協助專家也會呼叫 JavaScript 檔案，將執行階段行為加上附註。 而不是使用指令碼的參考`<script>`標記協助程式登錄的指令碼，藉由呼叫`Assets.AddScript`。
- JavaScript 檔案。 這是檔案，稱為協助程式，並會暫時增加的附註項目時的字型大小`mouseover`事件。
- 內容頁面，參考<em>\_SiteLayout</em>頁面上，呈現在本文中，某些內容，然後呼叫`MakeNote`協助程式。
- A  *\_SiteLayout*頁面。 此頁面提供常見的標頭和頁面配置結構。 它也包含呼叫`Assets.GetScripts`，也就是 「 資產管理員如何轉譯指令碼會呼叫在網頁中。

若要執行範例：

1. 建立空的 Web Pages 2 網站。 您可以使用 WebMatrix**空白網站**這個範本。
2. 建立名為的資料夾*指令碼*站台中。
3. 在 *指令碼*資料夾中，建立名為*Test.js*，複製*Test.js*範例中，從內容到其中，並將檔案儲存...
4. 建立名為的資料夾*應用程式\_程式碼*站台中。
5. 在 *應用程式\_程式碼*資料夾中，建立名為*Helpers.cshtml*，將範例程式碼複製到其中，並將它儲存在名為的資料夾*應用程式\_的程式碼*根資料夾中。
6. 在站台的根資料夾中，建立名為 *\_SiteLayout.cshtml，* 將範例複製到它，然後儲存檔案。
7. 在站台根目錄中，建立名為*ContentPage.cshtml*、 加入範例程式碼，並將它儲存。
8. 執行*ContentPage*瀏覽器中。 您傳遞給字串`MakeNote`helper 會呈現為 boxed 的附註。
9. 滑鼠指標通過附註。 指令碼暫時增加的附註的字型大小。
10. 檢視所呈現頁面的來源。 因為您放置呼叫`Assets.GetScripts`，轉譯`<script>`呼叫的標記*Test.js*是頁面的主體中的最後一個項目。

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

下列螢幕擷取畫面示*ContentPage.cshtml*在瀏覽器當滑鼠指標停留在附註：

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>啟用登入和來自 Facebook 與其他站台使用 OAuth 和 OpenID

Web Pages 2 提供成員資格和驗證增強功能的的選項。 主要的增強功能，是新[OAuth](http://oauth.net/)並[OpenID](http://openid.net/)提供者。 使用這些提供者，您可以讓使用者登入您的網站使用其現有的認證，利用 Facebook、 Twitter、 Windows Live、 Google 和 Yahoo。 比方說，若要登入的 Facebook 帳戶，使用者就可以選擇 [Facebook] 圖示，將它們重新導向至 Facebook 登入頁面，讓他們輸入其使用者資訊。 它們可以再將其帳戶在您的網站上 Facebook 登入關聯。 網頁的成員資格功能相關的增強功能是使用者可以將產生關聯 （包括從社交網路網站的登入） 的多個登入與您的網站上的單一帳戶。

下圖顯示登入頁面**入門網站**範本，使用者可以在其中選擇 Facebook、 Twitter 或 Windows Live 的圖示，以啟用登入的外部帳戶：

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

您可以使用幾行程式碼中的 OAuth 和 OpenID 成員資格。 您的方法和屬性用來處理與 OAuth 和 OpenID 提供者位於`WebMatrix.Security.OAuthWebSecurity`類別。

不過，而不是使用程式碼，以啟用從其他站台的登入，若要開始使用新的提供者的建議的方式是以使用新**入門網站**隨附 WebMatrix 2 beta 版的範本。 **入門網站**範本包含完整的成員資格的基礎結構，您需要讓使用者登入您的網站使用本機認證或來自另一個站台完成登入頁面、 成員資格資料庫中，與所有的程式碼.

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>如何啟用使用 OAuth 和 OpenID 提供者的登入

本節提供如何讓使用者從外部網站 （Facebook、 Twitter、 Windows Live、 Google 或 Yahoo） 登入的範例為基礎的站台**入門網站**範本。 建立入門網站之後, 您這樣做，（詳細資料）：

- 使用 OAuth 提供者 （Facebook、 Twitter 和 Windows Live） 的網站，建立外部站台上的應用程式。 這可讓您將需叫用這些站台的登入功能的應用程式金鑰。 使用 OpenID 提供者 （Google、 Yahoo） 的網站，您不必建立應用程式。 針對所有的這些網站，您必須有帳戶才能登入，並建立開發人員應用程式。 

    > [!NOTE]
    > Windows Live 應用程式只接受即時工作網站 URL，因此您無法使用本機的網站 URL，測試登入。
- 以指定適當的驗證提供者，並提交至您想要使用的站台的登入，請編輯您的網站中的幾個檔案。

**若要啟用 Google 和 Yahoo 登入**:

1. 在您的網站，請編輯 *\_AppStart.cshtml*頁面，然後在 Razor 程式碼區塊中新增下列兩行程式碼，呼叫之後`WebSecurity.InitializeDatabaseConnection`方法。 此程式碼可讓在 Google 和 Yahoo OpenID 提供者。 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. 在  *~/Account/Login.cshtml*頁面上，移除下列註解`<fieldset>`標記頁面結尾附近的區塊。 若要取消註解的程式碼，移除`@*`字元在`<fieldset>`區塊。 產生的程式碼區塊看起來像這樣：

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. 新增`<input>`Google 或 Yahoo 提供者的項目`<fieldset>`群組中 *~/Account/Login.cshtml*頁面。 已更新`<fieldset>`群組以及`<input>`項目，如 Google 和 Yahoo 看起來如下列範例所示： 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. 在  *~/Account/AssociateServiceAccount.cshtml*頁面上，加入`<input>`Google 或 Yahoo 到的項目`<fieldset>`檔案結尾附近的群組。 您可以複製相同`<input>`剛加入的項目`<fieldset>`一節 *~/Account/Login.cshtml*頁面。 

    *~/Account/AssociateServiceAccount.cshtml*可以用入門網站範本中的頁面上，如果您想要建立的使用者可以聯想從其他站台的多個登入您的網站上的單一帳戶頁面。

現在您可以測試 Google 和 Yahoo 登入。

1. 執行*default.cshtml*網站頁面，然後選擇**登入** 按鈕。
2. 上*登入*頁面上，於**使用其他服務進行登入**區段中，選擇  **Google**或是**Yahoo**送出按鈕。 此範例會使用 Google 登入。 

    網頁上將要求重新導向至 Google 登入頁面。

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. 輸入現有的 Google 帳戶認證。
4. 如果 Google 會詢問您是否要允許使用來自帳戶的資訊，請按一下 Localhost**允許**。

    程式碼會驗證使用者，使用 Google 的權杖，然後回到此頁面，在您的網站。 這個頁面可讓您在網站上，現有的帳戶相關聯其 Google 登入的使用者，或他們可以註冊新的帳戶建立關聯外部登入與您站台上。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. 選擇**產生關聯** 按鈕。 在瀏覽器會返回您的應用程式首頁。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**若要啟用 Facebook 登入**:

1. 移至[Facebook 開發人員網站](https://developers.facebook.com/apps)（登入如果您還沒登入）。
2. 選擇**建立新的應用程式**按鈕，然後依照提示來命名，並建立新的應用程式。
3. 一節**選取您的應用程式將會如何整合 Facebook**，選擇**網站**一節。
4. 填寫**站台 URL**欄位與您網站的 URL (例如[ `http://www.example.com` ](http://www.example.com))。 **網域**欄位是選擇性的; 您可以使用此選項來提供整個網域的驗證 (例如*example.com*)。 

    > [!NOTE]
    > 如果您正在 URL 與您本機電腦上的站台也`http://localhost:12345`（其中數字是本機連接埠號碼），您可以新增此值，以**站台 URL**欄位來測試您的網站。 不過，任何時候您本機站台的變更的通訊埠編號，您必須更新**站台 URL**應用程式的欄位。
5. 選擇**儲存變更** 按鈕。
6. 選擇**應用程式**同樣地，索引標籤，然後檢視 應用程式的 開始 頁面。
7. 複製**應用程式識別碼**並**應用程式祕密**應用程式的值並貼到暫存的文字檔。 網站程式碼中，您將 Facebook 提供者來傳遞這些值。
8. 結束 Facebook 開發人員網站。

現在您對變更兩個頁面在您的網站，讓使用者能夠登入使用其 Facebook 帳戶的網站。

1. 在您的網站，請編輯 *\_AppStart.cshtml*頁面和程式碼取消註解的 Facebook OAuth 提供者。 取消註解的程式碼區塊看起來如下所示： 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. 複製**應用程式識別碼**Facebook 應用程式的值介於`consumerKey`參數 （以引號）。
3. 複製**應用程式祕密**從 Facebook 應用程式，做為值`consumerSecret`參數值。
4. 儲存並關閉檔案。
5. 編輯 *~/Account/Login.cshtml*頁面上，並移除的註解`<fieldset>`頁面結尾附近的區塊。 若要取消註解的程式碼，移除`@*`字元在`<fieldset>`區塊。 使用註解的程式碼區塊中移除看起來如下所示： 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. 儲存並關閉檔案。

現在您可以測試 Facebook 登入。

1. 執行站台*default.cshtml*頁面上，然後選擇**登入** 按鈕。
2. 在 *登入*頁面上，於**另一個服務用來登入**區段中，選擇**Facebook**圖示。 

    網頁上將要求重新導向至 Facebook 登入頁面。

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. 登入的 Facebook 帳戶。 

    程式碼來驗證您使用 Facebook 權杖，然後返回頁面，您可以讓您的 Facebook 登入關聯站台的登入。 您的使用者名稱或電子郵件地址填入**電子郵件**欄位在表單上。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. 選擇**產生關聯** 按鈕。 

    瀏覽器會返回首頁並登入。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**若要啟用 Twitter 登入：** 

1. 瀏覽至[Twitter 開發人員網站](https://dev.twitter.com/)。
2. 選擇**建立應用程式**連結，然後再登入網站。
3. 在上**建立應用程式**表單中填寫**名稱**並**描述**欄位。
4. 在 **網站**欄位中，輸入您網站的 URL (例如[ `http://www.example.com` ](http://www.example.com))。 

    > [!NOTE]
    > 如果您要測試您的網站，在本機 (使用之類的 URL `http://localhost:12345`)，Twitter 可能不會接受 URL。 不過，您可以使用本機回送 IP 位址 (例如`http://127.0.0.1:12345`)。 這可簡化測試您的應用程式在本機的程序。 不過，每次您的本機網站的連接埠號碼變更時，您將需要更新**網站**應用程式的欄位。
5. 在 **回呼 URL**欄位中，輸入您要讓使用者之後若要返回登入 Twitter 的網站中的頁面的 URL。 比方說，若要將使用者傳送至 （這會辨識其登入的狀態） 的入門網站的 [首頁] 頁面中，輸入相同 URL 即可中輸入**網站**欄位。
6. 接受條款，然後選擇**建立 Twitter 應用程式** 按鈕。
7. 在 **我的應用程式**登陸頁面上，選擇您所建立的應用程式。
8. 在 [**詳細資料**索引標籤上，捲動到底部，然後選擇**建立我的存取權杖**] 按鈕。
9. 上**詳細資料**索引標籤上，複製**取用者索引鍵**並**取用者祕密**應用程式的值並貼到暫存的文字檔。 您要傳遞至 Twitter 提供者的這些值，在您的網站程式碼。
10. 結束 Twitter 網站。

現在您對變更兩個頁面在您的網站，讓使用者能夠登入使用 Twitter 帳戶的網站。

1. 在您的網站，請編輯 *\_AppStart.cshtml*頁面及 Twitter OAuth 提供者，程式碼取消註解。 取消註解的程式碼區塊看起來像這樣： 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. 複製**取用者索引鍵**從 Twitter 應用程式，做為值的值`consumerKey`參數 （以引號）。
3. 複製**取用者祕密**從 Twitter 應用程式，做為值的值`consumerSecret`參數。
4. 儲存並關閉檔案。
5. 編輯 *~/Account/Login.cshtml*頁面上，並移除的註解`<fieldset>`頁面結尾附近的區塊。 若要取消註解的程式碼，移除`@*`字元在`<fieldset>`區塊。 使用註解的程式碼區塊中移除看起來如下所示： 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. 儲存並關閉檔案。

現在您可以測試 Twitter 登入。

1. 執行*default.cshtml*網站頁面，然後選擇**登入** 按鈕。
2. 在 *登入*頁面上，於**另一個服務用來登入**區段中，選擇**Twitter**圖示。 

    網頁的要求重新導向至您所建立的應用程式的 Twitter 登入頁面。

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. 登入 Twitter 帳戶。
4. 程式碼來驗證使用者使用 Twitter 語彙基元，然後傳回您頁面，您可以將與您網站的帳戶登入。 您的姓名或電子郵件地址填入**電子郵件**欄位在表單上。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. 選擇**產生關聯** 按鈕。 

    瀏覽器會返回首頁並登入。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>新增對應使用 Map 協助程式

Web Pages 2 包含新增至 ASP.NET Web Helpers Library，這是 Web Pages 網站的增益集的封裝。 其中是所提供的對應元件`Microsoft.Web.Helpers.Maps`類別。 您可以使用`Maps`類別，以產生對應根據的地址或一組經度和緯度座標。 `Maps`類別可讓您直接呼叫包括 Bing、 Google、 MapQuest 和 Yahoo 的熱門地圖引擎。

若要使用新`Maps`類別在您的網站，您必須先安裝第 2 版 Web Helpers Library。 若要這樣做，請移至安裝的目前發行的版本的指示[ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)並安裝第 2 版。

將對應加入至頁面的步驟都相同不論哪一個 map 引擎呼叫。 您只要加入您的對應頁面的 JavaScript 檔案參考，然後再新增呈現呼叫`<script>`標記在頁面上。 接著，在對應上，呼叫您想要使用的地圖引擎。

下列範例示範如何建立網頁呈現在地圖上的位址，且呈現在地圖上經度和緯度座標的另一個頁面。 位址對應範例會使用 Google 地圖和座標對應範例會使用 Bing 地圖服務。 請注意下列程式碼中的項目：

- 若要呼叫`Assets.AddScript`頂端的兩個對應頁面。 這個方法會將參考加入*jquery 1.6.2.min.js*隨附的檔案**入門網站**範本及所需`Maps`類別。
- 若要呼叫`Assets.GetScripts`配置檔案中的方法。 這個方法會呈現`<script>`標記上的兩個對應頁面。
- 在呼叫`@Maps.GetGoogleHtml`而`@Maps.GetBingHtml`[對應] 頁面中的方法。 若要對應的位址，您必須傳遞地址字串。 若要對應的座標，您必須傳遞經度和緯度座標。 Bing 地圖服務引擎，您也必須傳遞一個金鑰 (您在註冊取得免費[Bing 地圖服務開發人員網站](https://www.microsoft.com/maps/developers/web.aspx))。 類似的方式運作的其他對應引擎的方法 (`@Maps.GetYahooHtml`， `@Maps.GetMapQuestHtml`)。

若要建立對應頁面：

1. 建立網站，根據**入門網站**範本。
2. 建立名為*MapAddress.cshtml*根目錄中的站台。 此頁面將會產生對應，根據您傳遞給它的位址。
3. 將下列程式碼複製到檔案中，覆寫現有的內容。 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. 建立名為 *\_MapLayout.cshtml*根目錄中的站台。 此頁面將會是兩個對應頁面的版面配置頁。
5. 將下列程式碼複製到檔案中，覆寫現有的內容。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. 建立名為*MapCoordinates.cshtml*根目錄中的站台。 此頁面將會產生一組您傳遞給它的座標為基礎的對應。
7. 將下列程式碼複製到檔案中，覆寫現有的內容。 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

若要測試您的對應頁面：

1. 執行網頁*MapAddress.cshtml*檔案。
2. 輸入完整的地址字串包括街道地址、 狀態或省和郵遞區號，然後選擇**Map It**  按鈕。 頁面會從 Google 地圖呈現對應： 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. 找到的特定位置的緯度和經度座標組。
4. 執行網頁*MapCoordinates.cshtml*。 輸入座標，然後選擇**Map It**  按鈕。 頁面會將模式轉譯來自 Bing 地圖服務： 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>執行網頁應用程式並排顯示

Web Pages 2 新增以執行應用程式並排顯示的功能。 這可讓您繼續執行您的 Web Pages 1 應用程式，建置新的 Web Pages 2 應用程式，並執行所有人都在相同電腦上。

以下是要記住，當您使用 WebMatrix 安裝 Web Pages 2 的 beta 版的一些事項：

- 根據預設，現有的 Web Pages 應用程式會在電腦上執行做為第 2 版的應用程式。 （第 2 版的組件在 GAC 中安裝的而且會自動使用）。
- 如果您想要執行站台使用網頁第 1 版 （而不是預設值，如同先前的點），您可以設定站台，若要這麼做。 如果您的網站尚未*web.config*檔案根目錄中的站台，建立一個新，並將下列 XML 複製到其中，覆寫現有的內容。 如果網站已包含*web.config*檔案中，新增`<appSettings>`至如下所示的項目`<configuration>`一節。

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-如果您未指定的版本*web.config*檔案，網站會部署為第 2 版網站。 (第 2 版組件複製到*bin*資料夾中已部署的站台。)
- 您使用 Web Matrix 版本 2 的 beta 版納入站台的網頁的第 2 版組件中的網站範本所建立的新應用程式*bin*資料夾。

一般情況下，您一律可以控制的網頁，以搭配您的網站，使用 NuGet 將適當的組件安裝到站台的版本*bin*資料夾。 若要尋找套件，請造訪[NuGet.org](http://NuGet.org)。

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>行動裝置的轉譯頁面

Web Pages 2 可讓您在行動裝置或其他裝置上建立自訂的顯示，來呈現內容。

`System.Web.WebPages`命名空間包含下列類別可讓您使用的顯示模式： `DefaultDisplayMode`， `DisplayInfo`，和`DisplayModes`。 您可以直接使用這些類別，並撰寫呈現特定裝置的正確輸出的程式碼。

或者，您可以建立裝置的特定頁面所使用的檔案命名模式如下：<em>檔名。</em><em>行動</em><em>.cshtml</em>。 例如，您可以在其中建立兩個版本的頁面上，一個名為<em>MyFile.cshtml</em>一個名為<em>MyFile.Mobile.cshtml</em>。 在執行的階段，當行動裝置的要求<em>MyFile.cshtml</em>，網頁呈現的內容，從<em>MyFile.Mobile.cshtml</em>。 否則，請<em>MyFile.cshtml</em>轉譯。

下列範例示範如何藉由新增行動裝置的內容頁面中啟用行動裝置的轉譯。 *Page1.cshtml*包含內容，加上導覽提要欄位。 *Page1.Mobile.cshtml*包含相同的內容，但省略了資訊看板。

若要建置並執行程式碼範例：

1. 在網頁的網站中，建立名為*Page1.cshtml*並複製*Page1.cshtml*到其中的內容範例。
2. 建立名為*Page1.Mobile.cshtml*並複製*Page1.Mobile.cshtml*到其中的內容範例。 請注意頁面的行動裝置的版本會省略更好的呈現較小螢幕上的 [導覽] 區段。
3. 執行桌面瀏覽器並瀏覽至*Page1.cshtml*。
4. 執行行動瀏覽器 （或行動裝置模擬器），並瀏覽至*Page1.cshtml*。 請注意，此時網頁呈現頁面的行動裝置的版本。 

    > [!NOTE]
    > 若要測試行動網頁，您可以使用行動裝置模擬器，在桌面的電腦上執行。 此工具可讓您測試網頁，因為它們看起來在行動裝置上 （也就是通常具有較小顯示區域）。 模擬器的其中一個範例是[使用者代理程式切換器附加元件](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/)適用於 Mozilla Firefox，可讓您模擬各種不同的行動瀏覽器，從 Firefox 的桌面版本。

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml*在桌面瀏覽器中呈現：

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* Apple iPhone 模擬器中的檢視 Firefox 瀏覽器中顯示。 即使要求是要*Page1.cshtml*，應用程式轉譯*Page1.Mobile.cshtml*。

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>其他資源

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web Pages 1 資源

> [!NOTE]
> 大部分的 Web Pages 1 程式設計和 API 資源仍然會套用到 Web Pages 2。

- [ASP.NET Web Pages 程式設計簡介](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>WebMatrix 資源

- [WebMatrix 2 的新功能](http://webmatrix.com/next)
- [Microsoft WebMatrix 網站](https://go.microsoft.com/fwlink/?LinkID=195076)
- [從 Microsoft WebMatrix 網頁程式開發](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)（包括完整的範例網頁應用程式）
