---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: ASP.NET Web Pages 2 中的最上層功能 |Microsoft 文件
author: microsoft
description: 本主題概略說明 ASP.NET Web Pages 2，隨附於 WebMatr 的輕量型 web 程式設計架構中的最上層的新功能...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: f0d32edd3ab54c55aa06c803cd91e01cbbb8f08a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>在 ASP.NET Web Pages 2 上的功能
====================
by [Microsoft](https://github.com/microsoft)

> 本文章提供在 ASP.NET Web Pages 2 RC 中，隨附於的輕量型 web 程式設計架構最上層的新功能的概觀[Microsoft WebMatrix 2 Rc<](https://www.microsoft.com/web/)。
> 
> **包含的內容：** 
> 
> - [安裝 WebMatrix](#install)
> - [新功能和增強功能](#New_and_Enhanced_Features)
> 
>     - [RC 版本變更](#Changes_for_the_RC_Version)
>     - [測試版的變更](#Changes_for_the_Beta_Version)
>     - [使用新的和更新的網站範本](#templates)
>     - [驗證使用者輸入](#validation)
>     - [啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入](#oauthsetup)
>     - [新增對應使用對應 Helper](#maphelper)
>     - [執行網頁應用程式並排顯示](#sidebyside)
>     - [行動裝置的轉譯頁面](#mobile)
> - [其他資源](#resources)
> 
> > [!NOTE]
> > 本主題假設您使用 WebMatrix 來處理您的 ASP.NET Web Pages 2 程式碼。 不過，如 Web Pages 1 中，您也可以建立使用 Visual Studio Web Pages 2 網站，可讓您強化 IntelliSense 功能，以及偵錯。 若要使用 Visual Studio 中的網頁，您必須先安裝 Visual Studio 2010 SP1、 Visual Web Developer Express 2010 SP1 或 Visual Studio 11 Beta。 接著，安裝 ASP.NET MVC 4 Beta，其中包含用於 Visual Studio 中建立 ASP.NET MVC 4 和 Web Pages 2 應用程式範本和工具。
> 
> 
> *上次更新： 18 年 6 月 2012年*


<a id="install"></a>
## <a name="installing-webmatrix"></a>安裝 WebMatrix

若要安裝 Web 網頁，您可以使用 Microsoft Web Platform Installer 中，這是免費的應用程式，可讓您輕鬆安裝及設定 web 相關的技術。 您將安裝 WebMatrix 2 beta 版，包括 Web Pages 2 Beta。

1. 瀏覽至 Web Platform Installer 的最新版本的安裝頁面：

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > 如果您已經安裝 WebMatrix 1，這項安裝會更新它到 WebMatrix 2 Beta。 您可以執行相同的電腦上使用 1 或 2 的版本所建立的網站。 如需詳細資訊，請參閱的章節[執行網頁應用程式並存](#sidebyside)。
2. 選擇**立即安裝**。 

    如果您使用 Internet Explorer，請移至下一個步驟。 如果您使用不同的瀏覽器，如 Mozilla Firefox 或 Google Chrome，提示您儲存*Webmatrix.exe*檔案儲存到電腦。 儲存檔案，然後按一下以啟動安裝程式。
3. 執行安裝程式，並選擇**安裝** 按鈕。 這會安裝 WebMatrix 及網頁。

## <a id="New_and_Enhanced_Features"></a>  新功能和增強功能

### <a id="Changes_for_the_RC_Version"></a>  RC 版本 (年 6 月 2012) 的變更

2012 年 6 月的 RC 版本版有已於 2012 年 3 月發行的 Beta 版本重新整理的一些變更。 這些變更包括：

- A`Validation.AddFormError`方法已加入至`Validation`協助程式。 這非常有用，如果您以手動方式執行的驗證 （例如，您驗證傳遞查詢字串中的值） 和您想要新增錯誤訊息，可以顯示`Html.ValidationSummary`方法。 如需詳細資訊，請參閱下節[驗證資料，不會直接從使用者](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users)中[ASP.NET Web Pages (Razor) 網站中驗證使用者輸入](https://go.microsoft.com/fwlink/?LinkId=253002)。
- 統合及縮製的功能已經移除了核心 ASP.NET Web Pages 2 組件。 因此，`Assets`不提供本文件稍後所列的協助程式。 相反地，您必須安裝[ASP.NET 最佳化](http://nuget.org/packages/Microsoft.Web.Optimization/0.1)NuGet 封裝。 如需詳細資訊，請參閱[組合和 ASP.NET Web Pages (Razor) 網站中的 用資產](https://go.microsoft.com/fwlink/?LinkId=255373)。
- 已新增支援 ASP.NET Web Pages 2 的其他組件。 只有明顯的影響，這項變更是您可能會看到多個組件中的站台*bin*資料夾在建立站台，或將網站部署至主機服務提供者。

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Beta 版中 (2 月版 2012) 的變更

在 2012 年 2 月發行的 Beta 版有只有少數變更從 2011 年 12 月發行的 Beta 版本。 這些變更包括：

- Razor 現在支援條件式屬性。 HTML 項目，如果您設定屬性的值，會在伺服器程式碼中解析`false`或`null`，ASP.NET 不會完全呈現的屬性。 例如，假設您有下列核取方塊標記：

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    如果值`checked1`解析成`false`或`null`、`checked`屬性不會轉譯。 這是一項重大變更。
- `Validation.GetHtml`方法重新命名為`Validation.For`。 這是一項重大變更。`Validation.GetHtml`在 Beta 版中無法運作。
- 您現在可以包含`~`運算子標記中的，而不使用參考網站根目錄`Href`函式。 (亦即，Razor 剖析器現在可以找出並解決`~`運算子，而不需要的明確方法呼叫`Href`。)`Href`方法仍能運作，因此這不是一項重大變更。

    例如，如果您先前已標記如下：

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    您現在可以使用標記如下：

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts`已取代為資產 （資源） 管理的協助程式`Assets`helper，有稍微不同的方法，如下所示：

  - 如`Scripts.Add`，使用 `Assets.AddScript`
  - 如`Scripts.GetScriptTags`，使用 `Assets.GetScripts`

    這是一項重大變更。`Scripts`類別不是 Beta 版中。 這項變更已更新此文件的程式碼範例使用資產管理。

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>使用新的和更新的網站範本

**入門網站**範本已更新以便在 Web Pages 2 上執行，預設值。 它也包含下列新功能：

- 行動設備友善網頁呈現。 透過使用 CSS 樣式和`@media`選取器**入門網站**提供改善的呈現頁面的較小的螢幕，包括行動裝置螢幕上。
- 改良的成員資格和驗證選項。 您可以讓使用者登入您從其他社交網路網站，例如 Twitter、 Facebook 和 Windows Live 使用其帳戶的網站。 如需詳細資訊，請參閱[啟用登入的 Facebook 和其他站台使用 OAuth 和 OpenID](#oauthsetup) > 一節。
- HTML5 項目。

新**個人網站**範本可讓您建立包含個人部落格、 相片頁面和 Twitter 網頁的網站。 您可以自訂為基礎的站台**個人網站**範本，方法如下：

- 藉由編輯配置檔案中變更網站的外觀 (*\_SiteLayout.cshtml*) 和樣式檔案 (*Site.css*)。
- 安裝 NuGet 封裝會將功能加入至您的網站。 如需如何安裝封裝，包括 ASP.NET Web Helpers Library，請參閱本教學課程[安裝 helper](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)。

若要存取**個人網站**範本中，選擇**範本**上 WebMatrix**快速入門**螢幕。

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

在**範本**對話方塊方塊中，選擇**個人網站**範本。

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

登陸頁面**個人網站**範本可讓您設定您的部落格連結 Twitter 網頁和相片頁面。

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>驗證使用者輸入

在 Web Pages 1，來驗證使用者輸入，在提交表單中，您使用`System.Web.WebPages.Html.ModelState`類別。 (說明這種情形有幾個程式碼範例在 Web Pages 1 教學課程中標題為[使用資料](../data/5-working-with-data.md)。)您仍然可以使用這種方法，在 Web Pages 2。 不過，Web Pages 2 也提供改良的工具，驗證使用者輸入：

- 新的驗證類別，包括`System.Web.WebPages.ValidationHelper`和`System.Web.WebPages.Validator`，它可讓您執行幾行程式碼的功能強大的驗證工作。
- （選擇性） 用戶端驗證，而不需要往返伺服器使用者，若要檢查立即回應提供驗證錯誤。 （基於安全性理由，驗證伺服器上執行則即使事先在用戶端已執行檢查。）

若要使用新的驗證功能，執行下列作業：

在頁面的程式碼，註冊項目以進行驗證所使用的方法`Validation`helper: `Validation.RequireField`， `Validation.RequireFields` （若要註冊為必要的多個項目），或`Validation.Add`。 `Add`方法可讓您指定其他類型的驗證檢查，例如資料型別檢查、 比較不同的欄位，字串長度檢查中的項目，以及模式 （使用規則運算式）。 以下是一些範例：

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

若要顯示特定欄位的錯誤，請呼叫`Html.ValidationMessage`中驗證每個項目的標記：

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

若要顯示的摘要 (`<ul>`清單) 的頁面中的所有錯誤`Html.ValidationSummary`標記中：

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

這些步驟便足以實作伺服器端驗證。 如果您想要新增用戶端驗證，請執行下列此外。

加入下列指令碼檔參考內部`<head>`web 頁面的區段。 前兩個指令碼參考會指向內容傳遞網路 (CDN) 伺服器上的遠端檔案。 第三個參考會指向本機指令碼檔案。 無法使用 CDN 時，實際執行應用程式應該實作後援。 測試此後援。

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

若要取得的本機副本最簡單的方式*jquery.validate.unobtrusive.min.js*程式庫是建立新的 Web Pages 站台中，根據其中一個網站範本 （例如入門網站）。 在範本所建立的站台包含*jquery.validate.unobtrusive.js*資料夾中檔案的指令碼，從中您可以將它複製到您的網站。

如果您的網站使用<em>\_SiteLayout</em>來控制頁面版面配置頁面上，您可以包含這些指令碼參考在該頁面，以便驗證是否能夠使用所有的內容頁面。 如果您想要執行驗證，只在特定的頁面上，您可以使用的資產管理員註冊在只有這些頁面上的指令碼。 若要這樣做，請呼叫`Assets.AddScript(path)`在頁面中您想要驗證，並參考每個指令碼檔案。 然後將呼叫加入`Assets.GetScripts`中 <em>\_SiteLayout</em>頁面呈現註冊所`<script>`標記。 如需詳細資訊，請參閱下節[與資產管理員註冊的指令碼](#resmanagement)。

在標記中的個別項目，呼叫`Validation.For`方法。 這個方法會發出屬性該 jQuery 可以攔截以便提供用戶端驗證。 例如: 

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

下列範例會驗證使用者輸入，在表單上的頁面。 若要執行測試此驗證程式碼，請執行下列動作：

1. 建立新的網站使用其中一個 WebMatrix 2 的網站範本，其中包含*指令碼*資料夾，例如**入門網站**範本。
2. 在新的站台建立新*.cshtml*頁面，然後以下列程式碼取代頁面內容。
3. 執行網頁瀏覽器中。 輸入有效和無效的值，以查看上驗證的效果。 例如，將必要的欄位保留空白，或輸入中的字母**信用額度**欄位。


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

當使用者提交有效的輸入，以下是頁面：

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

當使用者提交它具有必要的欄位保留空白，以下是頁面：

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

當使用者將它提交使用中的整數以外的項目時，以下是頁面**信用額度**欄位：

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

如需詳細資訊，請參閱下列部落格文章：

- [更新網頁 v2 驗證](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344)新增驗證使用的基本概念`Validation`helper （只有伺服器端）
- [更新網頁 v2、 第 2 部分中的驗證](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347)加入用戶端驗證。
- [更新網頁 v2、 第 3 中的驗證](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351)格式驗證錯誤。

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>註冊指令碼使用的資產管理員

資產管理員是新的功能，您可以使用伺服端程式碼來註冊及轉譯用戶端指令碼。 當您正在使用中 （例如版面配置頁、 內容頁面，協助程式 」 等） 會結合成單一頁面上，在執行階段的多個檔案的程式碼時，此功能很實用。 資產管理員協調之來源檔案，請確定指令碼檔案的參考都正確並有效率地在轉譯頁面上，不論哪一個程式碼檔案從呼叫或所呼叫的次數。 資產管理員也會呈現`<script>`標記在正確的位置，以便快速 （而不需要下載指令碼時轉譯） 可以載入的頁面和避免錯誤，可能會發生，如果在呈現之前呼叫的指令碼已完成。

例如，假設您建立自訂的 helper 可呼叫的 JavaScript 檔案，並於三個不同的地方呼叫此 helper 內容頁面上的程式碼中。 如果您不使用的資產管理員來註冊指令碼會呼叫中的協助程式，有三個不同`<script>`所有指向相同的指令碼檔案會都出現在轉譯頁面的標記。 此外，根據 where`<script>`標記會插入在呈現的頁面、 指令碼嘗試存取頁面完全載入之前的特定頁面項目可能會發生錯誤。 如果您使用的資產管理員註冊的指令碼時，會避免這些問題。

您可以執行此動作，與資產管理員註冊指令碼：

- 需要參考指令碼的程式碼，在呼叫`Assets.AddScript`方法。
- 在 *\_SiteLayout*頁面上，呼叫`Assets.GetScripts`方法以呈現`<script>`標記。 

    > [!NOTE]
    > 將呼叫`Assets.GetScripts`中的最後一個項目為`<body>`元素 *\_SiteLayout*頁面。 這有助於頁面更快載入，而且可以協助避免指令碼錯誤。

下列範例會顯示資產管理員如何運作。 程式碼包含下列項目：

- 名為的自訂 helper `MakeNote`。 此 helper 來呈現的方塊內的字串換行`div`項目周圍的框線及新增具有樣式&quot;附註：&quot;它。 Helper 也會呼叫的 JavaScript 檔案新增至便箋的執行階段行為。 而不是使用指令碼的參考`<script>`標記，協助專家登錄的指令碼，藉由呼叫`Assets.AddScript`。
- JavaScript 檔案。 這是檔案所呼叫的協助程式，而且會暫時增加附註項目時的字型大小`mouseover`事件。
- 內容頁面上，即在參考<em>\_SiteLayout</em>  頁面上，呈現在主體中，某些內容，然後呼叫`MakeNote`協助程式。
- A  *\_SiteLayout*頁面。 此頁面提供的通用標頭和頁面配置結構。 它也包含呼叫`Assets.GetScripts`，即資產管理員如何轉譯指令碼會呼叫在網頁中。

若要執行範例：

1. 建立空白 Web Pages 2 網站。 您可以使用 WebMatrix**空白網站**這個範本。
2. 建立名為的資料夾*指令碼*站台中。
3. 在*指令碼*資料夾中，建立名為*Test.js*，複製*Test.js*範例中，從內容到它，然後儲存檔案...
4. 建立名為的資料夾*應用程式\_程式碼*站台中。
5. 在*應用程式\_程式碼*資料夾中，建立名為*Helpers.cshtml*、 將範例程式碼複製到其中，並將它儲存在名為的資料夾中*應用程式\_程式碼*根資料夾中。
6. 在站台的根資料夾中，建立名為 *\_SiteLayout.cshtml，*範例複製到它，然後儲存檔案。
7. 在站台根目錄中，建立名為*ContentPage.cshtml*、 加入範例程式碼，並將它儲存。
8. 執行*ContentPage*瀏覽器中。 您傳遞給字串`MakeNote`helper 會呈現為 boxed 的附註。
9. 滑鼠指標通過附註。 指令碼會暫時增加便箋的字型大小。
10. 檢視所呈現頁面的來源。 由於您放置呼叫`Assets.GetScripts`，呈現`<script>`呼叫的標記*Test.js*是網頁主體中的最後一個項目。

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

下列螢幕擷取畫面顯示*ContentPage.cshtml*當您透過便箋滑鼠指標在瀏覽器中：

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入

Web Pages 2 提供增強的成員資格和驗證的選項。 主要增強功能是在新[OAuth](http://oauth.net/)和[OpenID](http://openid.net/)提供者。 使用這些提供者，您可以讓使用者登入您的網站使用其現有的認證從 Facebook、 Twitter、 Windows Live、 Google 和 Yahoo。 例如，使用 Facebook 帳戶進行登入，使用者就可以選擇一個 Facebook 圖示，重新導向至在其中輸入他們的使用者資訊的 Facebook 登入頁面。 他們可將其網站上的帳戶產生關聯 Facebook 登入。 網頁的成員資格功能相關的增強功能是，使用者可將多個登入 （包括從社交網路網站的登入） 與您的網站上的單一帳戶。

下圖顯示登入頁面**入門網站**範本，其中使用者可以選擇要啟用外部的帳戶登入的 Facebook、 Twitter、 或 Windows Live 圖示：

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

您可以啟用 OAuth 和 OpenID 使用幾行程式碼的成員資格。 您的方法和屬性用來處理 OAuth 和 OpenID 提供者位於`WebMatrix.Security.OAuthWebSecurity`類別。

不過，而不是使用程式碼來啟用登入從其他站台，若要開始使用新的提供者的建議的方式是使用新**入門網站**隨附 WebMatrix 2 beta 版的範本。 **入門網站**範本包含完整的成員資格基礎結構，您需要讓使用者登入您的網站使用本機認證或來自另一個站台完成與登入頁面、 成員資格資料庫和所有的程式碼.

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>如何啟用使用 OAuth 和 OpenID 提供者的登入

本節提供如何讓使用者從外部網站 （Facebook、 Twitter、 Windows Live、 Google 或 Yahoo） 登入的範例為基礎的站台**入門網站**範本。 建立入門網站之後, 您這樣做，（詳細資料）：

- 針對使用 OAuth 提供者 （Facebook、 Twitter 和 Windows Live） 的網站，建立外部站台上的應用程式。 這讓您將需要以叫用這些站台的登入功能的應用程式金鑰。 使用 OpenID 提供者 （Google、 Yahoo） 的網站，您沒有建立應用程式。 針對所有這些站台，您必須有帳戶才能登入，並建立開發人員應用程式。 

    > [!NOTE]
    > Windows Live 應用程式只接受即時工作網站 URL，因此您無法使用本機網站 URL，測試登入。
- 若要指定適當的驗證提供者，以及提交的登入您想要使用的站台，請編輯網站中的幾個檔案。

**若要啟用 Google 和 Yahoo 登入**:

1. 在您的網站編輯 *\_AppStart.cshtml*頁面上，並在呼叫之後，新增 Razor 程式碼區塊中的下列兩行程式碼`WebSecurity.InitializeDatabaseConnection`方法。 此程式碼可讓 Google 和 Yahoo OpenID 提供者。 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. 在*~/Account/Login.cshtml*頁面上，移除註解下列`<fieldset>`接近頁面的結束標記的區塊。 若要取消註解程式碼，移除`@*`字元在的`<fieldset>`區塊。 產生的程式碼區塊看起來像這樣：

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. 新增`<input>`Google 或 Yahoo 提供者的項目`<fieldset>`群組*~/Account/Login.cshtml*頁面。 已更新`<fieldset>`群組與`<input>`元素 Google 和 Yahoo 看起來類似下列的範例： 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. 在*~/Account/AssociateServiceAccount.cshtml*頁面上，新增`<input>`Google 或 Yahoo 至的項目`<fieldset>`群組接近檔案結尾。 您可以複製相同`<input>`剛加入的項目`<fieldset>`一節中*~/Account/Login.cshtml*頁面。 

    *~/Account/AssociateServiceAccount.cshtml*入門網站範本中的頁面可以使用，如果您想要建立的使用者可以與您的網站上的單一帳戶聯想從其他站台的多個登入頁面。

現在您可以測試 Google 和 Yahoo 登入。

1. 執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。
2. 在*登入*頁面上，於**使用另一個服務登入**區段中，選擇  **Google**或**Yahoo**送出按鈕。 這個範例會使用 Google 登入。 

    網頁上將要求重新導向至 Google 登入頁面。

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. 現有的 Google 帳戶輸入認證。
4. 如果 Google 會詢問您是否要允許使用來自帳戶的資訊，請按一下 Localhost**允許**。

    程式碼來驗證使用者使用 Google 語彙基元，並會返回此頁面，在您的網站。 此頁面可讓使用者將其 Google 登入與現有的帳戶，在您的網站產生關聯，或他們可以註冊新的帳戶產生關聯的外部登入您的站台上。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. 選擇**關聯** 按鈕。 瀏覽器會返回應用程式的首頁。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**若要啟用 Facebook 登入**:

1. 移至[Facebook 開發人員網站](https://developers.facebook.com/apps)（登入如果您未登入）。
2. 選擇**建立新的應用程式**按鈕，然後再遵循提示來命名和建立新的應用程式。
3. 一節中**選取與 Facebook 整合您的應用程式將會如何**，選擇**網站**> 一節。
4. 填寫**網站 URL**欄位與您的網站 URL (例如， [ `http://www.example.com` ](http://www.example.com))。 **網域**欄位是選擇性的; 您可以使用此提供驗證整個網域 (例如*example.com*)。 

    > [!NOTE]
    > 如果您正在執行網站的 url 在本機電腦上要`http://localhost:12345`（其中數目是本機連接埠號碼），您可以將此值以**網站 URL**欄位以供測試您的網站。 不過，任何時候您本機站台的變更的通訊埠編號，您將需要更新**網站 URL**應用程式的欄位。
5. 選擇**儲存變更** 按鈕。
6. 選擇**應用程式**同樣地，索引標籤，然後再檢視應用程式的 [開始] 頁面。
7. 複製**應用程式識別碼**和**應用程式秘鑰**應用程式的值並貼到暫時的文字檔。 在網站上的程式碼中，您將會 Facebook 提供者來傳遞這些值。
8. 結束 Facebook 開發人員網站。

現在您變更兩個頁面中您的網站，讓使用者將能夠登入使用其 Facebook 帳戶的站台。

1. 在您的網站編輯 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Facebook OAuth 提供者。 取消註解的程式碼區塊看起來如下： 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. 複製**應用程式識別碼**從 Facebook 應用程式做為值的值`consumerKey`（引號） 內的參數。
3. 複製**應用程式秘鑰**從 Facebook 應用程式做為值`consumerSecret`參數值。
4. 儲存並關閉檔案。
5. 編輯*~/Account/Login.cshtml*頁面上，並移除從註解`<fieldset>`接近頁面結尾區塊。 若要取消註解程式碼，移除`@*`字元在的`<fieldset>`區塊。 使用註解的程式碼區塊中移除看起來如下所示： 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. 儲存並關閉檔案。

現在您可以測試 Facebook 登入。

1. 執行站台的*default.cshtml*頁面上，然後選擇 [**登入**] 按鈕。
2. 在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Facebook**圖示。 

    網頁上將要求重新導向至 Facebook 登入頁面。

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. 登入 Facebook 帳戶。 

    程式碼來驗證您使用 Facebook 語彙基元，然後傳回頁面您可以在其中將與您的網站登入您的 Facebook 登入。 使用者名稱或電子郵件地址填入**電子郵件**欄位在表單上。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. 選擇**關聯** 按鈕。 

    瀏覽器會返回 [首頁] 頁面，並在登入。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**若要啟用 Twitter 登入：** 

1. 瀏覽至[Twitter 開發人員網站](https://dev.twitter.com/)。
2. 選擇**建立應用程式**連結，然後再登入網站。
3. 在**建立應用程式**表單中，填寫**名稱**和**描述**欄位。
4. 在**網站**欄位中，輸入您網站的 URL (例如， [ `http://www.example.com` ](http://www.example.com))。 

    > [!NOTE]
    > 如果您要測試您的網站，在本機 (使用的 URL，例如`http://localhost:12345`)，Twitter 可能無法接受 URL。 不過，您可以使用本機回送 IP 位址 (例如`http://127.0.0.1:12345`)。 這樣可簡化測試您的應用程式在本機的程序。 不過，每次變更您的本機網站的通訊埠編號，您必須先更新**網站**應用程式的欄位。
5. 在**回呼 URL**欄位中，輸入您的網站，您要讓使用者在後返回記錄到 Twitter 中頁面的 URL。 例如，要傳送給使用者 （這會將其登入的狀態） 的入門網站的首頁，輸入您在輸入的相同 URL**網站**欄位。
6. 接受條款，然後選擇 [**建立應用程式 Twitter** ] 按鈕。
7. 在**我的應用程式**登陸頁面上，選擇您所建立的應用程式。
8. 在**詳細資料**索引標籤上，捲動到底部，然後選擇**建立我的存取權杖** 按鈕。
9. 上**詳細資料**索引標籤上，複製**取用者索引鍵**和**取用者秘密**應用程式的值並貼到暫時的文字檔。 在網站上的程式碼中，您將 Twitter 提供者來傳遞這些值。
10. 結束 Twitter 站台。

現在您變更兩個頁面中您的網站，讓使用者能夠登入使用 Twitter 帳戶的網站。

1. 在您的網站編輯 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Twitter OAuth 提供者。 取消註解的程式碼區塊看起來像這樣： 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. 複製**取用者索引鍵**從 Twitter 應用程式做為值的值`consumerKey`（引號） 內的參數。
3. 複製**取用者秘密**從 Twitter 應用程式做為值的值`consumerSecret`參數。
4. 儲存並關閉檔案。
5. 編輯*~/Account/Login.cshtml*頁面上，並移除從註解`<fieldset>`接近頁面結尾區塊。 若要取消註解程式碼，移除`@*`字元在的`<fieldset>`區塊。 使用註解的程式碼區塊中移除看起來如下所示： 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. 儲存並關閉檔案。

現在您可以測試 Twitter 登入。

1. 執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。
2. 在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Twitter**圖示。 

    網頁的要求重新導向至您所建立的應用程式的 Twitter 登入頁面。

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. 登入 Twitter 帳戶。
4. 程式碼來驗證使用者使用 Twitter 語彙基元，然後傳回您的頁面您可在關聯與您網站的帳戶登入。 您的名稱或電子郵件地址填入**電子郵件**欄位在表單上。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. 選擇**關聯** 按鈕。 

    瀏覽器會返回 [首頁] 頁面，並在登入。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>新增對應使用對應 Helper

Web Pages 2 包括附加的 ASP.NET Web Helpers Library，這個 Web Pages 站台中的增益集的封裝。 下列其中一種是所提供的對應元件`Microsoft.Web.Helpers.Maps`類別。 您可以使用`Maps`類別來產生地圖根據位址或一組經度和緯度座標。 `Maps`類別可讓您直接呼叫包括 Bing、 Google、 MapQuest 和 Yahoo 熱門地圖引擎。

若要使用新`Maps`類別在您的網站，您必須先安裝了第 2 版 Web Helpers Library。 若要這樣做，請移至安裝的目前發行的版本的指示[ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)並安裝第 2 版。

將對應新增至網頁的步驟都相同不論哪一對應引擎呼叫。 您只要加入您的對應頁面的 JavaScript 檔案參考，然後再新增呈現呼叫`<script>`標記放在您的頁面上。 然後在您的對應頁面上，呼叫您想要使用的對應引擎。

下列範例會示範如何建立會根據位址，將模式轉譯的頁面及呈現地圖經度和緯度座標為基礎的另一個頁面。 位址對應範例會使用 Google 地圖和座標的對應範例會使用 Bing 地圖服務。 請注意程式碼中的下列元素：

- 若要呼叫`Assets.AddScript`頂端的兩個對應頁面。 這個方法會將參考加入*jquery 1.6.2.min.js*檔案所包含的**入門網站**範本，而且需要透過`Maps`類別。
- 若要呼叫`Assets.GetScripts`配置檔案中的方法。 這個方法會呈現`<script>`標記的兩個對應頁面。
- 若要呼叫`@Maps.GetGoogleHtml`和`@Maps.GetBingHtml`中的對應頁面的方法。 若要對應的位址，您必須傳遞位址字串。 若要在地圖座標中，您必須傳遞經度和緯度座標。 Bing 地圖服務引擎，您還必須傳遞的索引鍵 (在註冊免費取得[Bing 地圖服務開發人員網站](https://www.microsoft.com/maps/developers/web.aspx))。 其他對應引擎方法的運作方式類似 (`@Maps.GetYahooHtml`， `@Maps.GetMapQuestHtml`)。

若要建立對應頁面：

1. 建立網站，根據**入門網站**範本。
2. 建立名為*MapAddress.cshtml*根目錄中的站台。 此頁面將會產生傳遞給它的位址為基礎的對應。
3. 將下列程式碼複製到檔案，覆寫現有的內容。 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. 建立名為 *\_MapLayout.cshtml*根目錄中的站台。 此頁面將兩個對應頁面的版面配置頁。
5. 將下列程式碼複製到檔案，覆寫現有的內容。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. 建立名為*MapCoordinates.cshtml*根目錄中的站台。 此頁面將會產生一組您傳遞給它的座標為基礎的對應。
7. 將下列程式碼複製到檔案，覆寫現有的內容。 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

若要測試您的對應頁面：

1. 執行網頁*MapAddress.cshtml*檔案。
2. 輸入街道地址、 狀態或市和郵遞區號，包括完整位址字串，然後選擇**Map It**  按鈕。 頁面會從 Google 地圖呈現對應： 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. 尋找特定位置的緯度和經度座標的集合。
4. 執行網頁*MapCoordinates.cshtml*。 輸入座標，然後選擇**Map It**  按鈕。 頁面會引導模式轉譯來自 Bing 地圖服務： 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>執行網頁應用程式並排顯示

Web Pages 2 還新增了可執行應用程式並存。 這可讓您繼續執行應用程式 Web Pages 1、 建立新的 Web Pages 2 應用程式，並在相同電腦上執行所有程式。

以下是要記住，當您使用 WebMatrix 安裝 Web Pages 2 beta 版的一些事項：

- 根據預設，現有的 Web Pages 應用程式會在電腦上執行做為第 2 版應用程式。 （第 2 版的組件安裝在 GAC 和會自動使用）。
- 如果您想要執行站台使用網頁版本 1 （而不是預設值，如同先前的點），您可以設定站台執行此作業。 如果還沒有網站*web.config*檔根目錄中的站台、 建立一個新並將下列 XML 複製到其中，覆寫現有的內容。 如果網站已包含*web.config* file、 add`<appSettings>`元素類似下列的其中一個來`<configuration>`> 一節。

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-如果您未指定的版本中*web.config*檔案，網站會部署為第 2 版的站台。 (第 2 版組件會複製到*bin*資料夾中已部署的站台。)
- 您使用 Web Matrix 2 Beta 包含在網站的網頁版本 2 組件的版本中的網站範本所建立的新應用程式*bin*資料夾。

一般情況下，您一律可以控制哪個版本的網頁使用與您的網站使用 NuGet 將適當的組件安裝到站台的*bin*資料夾。 若要尋找的封裝，請瀏覽[NuGet.org](http://NuGet.org)。

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>行動裝置的轉譯頁面

Web Pages 2 可讓您建立來呈現內容的自訂顯示行動裝置或其他裝置上。

`System.Web.WebPages`命名空間包含下列類別，可讓您使用的顯示模式： `DefaultDisplayMode`， `DisplayInfo`，和`DisplayModes`。 您可以直接使用這些類別，並撰寫呈現特定裝置的正確輸出的程式碼。

或者，您可以建立裝置的特定頁面所使用的檔案命名模式如下：<em>檔名。</em><em>行動</em><em>.cshtml</em>。 例如，您可以建立兩個版本的頁面上，一個名為<em>MyFile.cshtml</em> ，而另一個名為<em>MyFile.Mobile.cshtml</em>。 在執行的階段，當行動裝置的要求<em>MyFile.cshtml</em>，網頁呈現內容從<em>MyFile.Mobile.cshtml</em>。 否則， <em>MyFile.cshtml</em>轉譯。

下列範例會示範如何藉由新增行動裝置的內容頁面中啟用行動裝置的轉譯。 *Page1.cshtml*包含內容加上導覽提要欄位。 *Page1.Mobile.cshtml*包含相同的內容，但會省略 [資訊看板]。

若要建置並執行程式碼範例：

1. 在網頁上，建立名為*Page1.cshtml*並複製*Page1.cshtml*到其中的內容範例。
2. 建立名為*Page1.Mobile.cshtml*並複製*Page1.Mobile.cshtml*到其中的內容範例。 請注意行動版的頁面會省略更好的呈現較小螢幕上的瀏覽區段。
3. 執行桌面瀏覽器並瀏覽至*Page1.cshtml*。
4. 執行行動瀏覽器 （或行動裝置模擬器），並瀏覽至*Page1.cshtml*。 請注意，目前網頁呈現頁面的行動裝置的版本。 

    > [!NOTE]
    > 若要測試行動頁面，您可以使用行動裝置模擬器，桌面的電腦上執行。 此工具可讓您測試網頁，因為它們會在行動裝置上看起來 （也就是通常具有較小顯示區域）。 模擬器的其中一個範例是[使用者代理程式切換器附加元件](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/)Mozilla Firefox，這可讓您模擬 Firefox 桌面版本從各種行動瀏覽器。

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml*桌面瀏覽器中呈現：

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* Apple iPhone 模擬器檢視中 Firefox 瀏覽器中顯示。 即使該要求是*Page1.cshtml*，應用程式呈現*Page1.Mobile.cshtml*。

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>其他資源

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web Pages 1 資源

> [!NOTE]
> 大部分的網頁 1 程式設計和應用程式開發介面資源仍然會套用到 Web Pages 2。

- [ASP.NET Web Pages 程式設計簡介](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>WebMatrix 資源

- [WebMatrix 2 最新消息](http://webmatrix.com/next)
- [Microsoft WebMatrix Site](https://go.microsoft.com/fwlink/?LinkID=195076)
- [從 Microsoft WebMatrix Web 程式開發](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)（包括完整的範例網頁應用程式）
