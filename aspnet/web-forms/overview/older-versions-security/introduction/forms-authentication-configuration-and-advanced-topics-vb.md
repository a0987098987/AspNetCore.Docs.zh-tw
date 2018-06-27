---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: 表單驗證設定和進階的主題 (VB) |Microsoft 文件
author: rick-anderson
description: 本教學課程中我們將檢查各種表單驗證設定，並請參閱 < 如何修改它們透過表單項目。 這將會伴隨詳細...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: c6ef046100cf4773da57f6693a88e9bc6ec1790f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891717"
---
<a name="forms-authentication-configuration-and-advanced-topics-vb"></a>表單驗證設定和進階的主題 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip)或[下載 PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> 本教學課程中我們將檢查各種表單驗證設定，並請參閱 < 如何修改它們透過表單項目。 這將會伴隨詳細的探討自訂登入頁面使用自訂的 URL （例如 SignIn.aspx 而不是 Login.aspx) 與 cookie 的表單驗證票證的表單驗證票證逾時值。


## <a name="introduction"></a>簡介

在[上一個教學課程](an-overview-of-forms-authentication-vb.md)我們探討了 ASP.NET 應用程式中，從 Web.config 中的組態設定指定要建立頁面，即可顯示的記錄檔不同實作表單驗證所需的步驟已驗證和匿名使用者的內容。 還記得我們設定為使用表單驗證設定模式屬性的網站&lt;驗證&gt;Form 項目。 &lt;驗證&gt;元素可以選擇性地包含&lt;form&gt;子元素，可能會指定表單驗證設定的分類。

在本教學課程，我們會檢查各種表單驗證設定，請參閱 < 如何修改它們透過&lt;form&gt;項目。 這將會伴隨詳細的探討自訂登入頁面使用自訂的 URL （例如 SignIn.aspx 而不是 Login.aspx) 與 cookie 的表單驗證票證的表單驗證票證逾時值。 我們也會更仔細檢查表單驗證票證的結構，並請參閱 < ASP.NET 會確保票券的資料是從檢查和竄改安全預防措施。 最後，我們將探討如何將額外的使用者資料儲存在表單驗證票證以及如何建立自訂的主體物件透過此資料的模型。

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>步驟 1： 檢查&lt;form&gt;組態設定

在 ASP.NET 表單驗證系統提供可自訂的應用程式的應用程式為基礎的組態設定的數的字。 這包括設定，例如： 的存留期表單驗證票證。票證; 已套用何種保護在哪些條件 cookie 驗證票證才能使用。登入頁面; 路徑和其他資訊。 若要修改預設值，將[ &lt;form&gt;元素](https://msdn.microsoft.com/library/1d3t3c61.aspx)做為子系[&lt;驗證&gt;元素](https://msdn.microsoft.com/library/532aee0e.aspx)，指定這些屬性您想要自訂為 XML 屬性值，就像這樣：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

表 1 摘要說明您可以透過自訂的屬性&lt;form&gt;項目。 Web.config 是一個 XML 檔案，因為在左側的資料行中的屬性名稱會區分大小寫。


| <strong>屬性</strong> |                                                                                                                                                                                                                                     <strong>描述</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         cookie         |                                                                                                                這個屬性會指定在哪些情況下驗證票證會儲存在與 URL 中內嵌的 cookie。 允許值為： UseCookies;UseUri;自動偵測。和 UseDeviceProfile （預設值）。 步驟 2 會檢查此設定在更多詳細資料。                                                                                                                |
|         defaultUrl         |                                                                                                                                                         表示使用者在登入後從登入頁面時於 querystring 中指定的 RedirectUrl 值將被導向至的 URL。 預設值是 default.aspx。                                                                                                                                                         |
|           網域           | 當使用 cookie 為基礎的驗證票證時，此設定指定 cookie s 定義域值。 預設值為空字串，這會導致瀏覽器使用從中發行 （例如 www.yourdomain.com) 的網域。 在此情況下，cookie 將<strong>不</strong>子網域，例如 admin.yourdomain.com 進行要求時傳送。如果您想要傳遞至所有子網域，您需要自訂網域屬性將它設定為 yourdomain.com 的 cookie。 |
|  enableCrossAppRedirects   |                                                                                                                                                                   布林值，指出是否已驗證的使用者要記住，在同一部伺服器上的其他 web 應用程式重新導向至 Url 時。 預設為 false。                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      登入頁面 URL。 預設值為 login.aspx。                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   當使用 cookie 為基礎的驗證票證之 cookie 的名稱。 預設值為。ASPXAUTH。                                                                                                                                                                                                   |
|            路徑            |                                                                             當使用 cookie 為基礎的驗證票證時，此設定指定 cookie s path 屬性。 Path 屬性可讓開發人員限制到特定目錄階層 cookie 的範圍。 預設值是/通知要對網域執行任何要求傳送驗證票證 cookie 的瀏覽器。                                                                              |
|         保護         |                                                                                                                                            指出哪些技術可用來保護表單驗證票證。 允許值為： 全部 （預設）。加密;無。和驗證。 在步驟 3 中詳細討論這些設定。                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                布林值，指出是否需要 SSL 連線來傳送驗證 cookie。 預設值為 false。                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 布林值，指出是否每次重設驗證 cookie s 逾時使用者造訪網站的單一工作階段期間。 預設值為 true。 驗證票證逾時原則中指定的更詳細地討論票證的逾時值 > 一節。                                                                                                 |
|          逾時           |                                                                                                                               指定以分鐘為單位的驗證票證 cookie 過期前的時間。 預設值為 30。 驗證票證逾時原則中指定的更詳細地討論票證的逾時值 > 一節。                                                                                                                               |

**表 1**: 摘要&lt;form&gt;元素的屬性

在 ASP.NET 2.0 和更新版本，預設值的表單驗證的值會是硬式編碼 FormsAuthenticationConfiguration 類別在.NET Framework 中。 必須套用的任何修改 Web.config 檔案中的應用程式的應用程式為基礎。 這不同於 ASP.NET 1.x 預設表單驗證值儲存在 machine.config 檔案中 （而因此無法修改透過編輯 machine.config）。 主題的 ASP.NET while 1.x 值得提到的表單驗證系統設定的數字有不同的預設值，在 ASP.NET 2.0 和 ASP.NET 中的比其他 1.x。 如果您要從 ASP.NET 1.x 環境移轉您的應用程式，務必留意這些差異。 請參閱[ &lt;form&gt;技術文件項目](https://msdn.microsoft.com/library/1d3t3c61.aspx)差異的清單。

> [!NOTE]
> 數個表單驗證設定，例如逾時、 網域和路徑中，指定產生的表單驗證票證 cookie 的詳細資料。 如需有關 cookie、 它們如何運作，以及其各種屬性的詳細資訊，請閱讀[本教學課程中的 Cookie](http://www.quirksmode.org/js/cookies.html)。


### <a name="specifying-the-tickets-timeout-value"></a>指定的票證逾時值

表單驗證票證是代表身分識別的權杖。 以 cookie 為基礎的驗證票證會保留在 cookie 中這個語彙基元，並將其傳送至上每個要求的網頁伺服器中。 本質上而言，宣告的權杖中，擁有，我*username*，我已登入，並會使用，以便在網頁瀏覽系統可記住使用者的身分識別。

表單驗證票證不僅包括使用者的身分識別，但也包含可協助確保完整性與安全性權杖的資訊。 在所有情況下，我們不想讓使用者能夠建立盜版語彙基元，或修改 underhanded 某種合適的語彙基元。

在票證中所包含的資訊一個這類位元是*到期*，這是票證已不再有效的時間與日期。 它會確保 FormsAuthenticationModule 會檢查驗證票證，每次票證到期不具有尚未成功。 如果有，忽略票證，用來識別為匿名使用者。 這個保護措施可協助防止重新執行攻擊。 沒有過期，如果駭客能夠以取得她手上使用者的有效驗證票證-可能是實體存取其電腦透過其 cookie 為根建立-它們無法將要求傳送至伺服器與此遭竊的驗證票證和取得項目。 當到期日期不會防止這種情況下時，它限制這類攻擊成功完成的期間的視窗。

> [!NOTE]
> 步驟 3 表單驗證系統用來保護驗證票證的詳細資料其他技巧。


在建立時的驗證票證，表單驗證系統會決定諮詢逾時設定其到期。 明表 1，逾時設定預設值為 30 分鐘，這表示表單驗證票證建立時的日期和時間在未來 30 分鐘內設定其到期。

過期狀態定義絕對未來表單驗證票證到期的時間。 但通常開發人員想要實作滑動過期，就會在每次使用者重新審視站台重設的其中一個。 此行為取決於 slidingExpiration 設定。 如果設為 true （預設值），每次 FormsAuthenticationModule 驗證使用者，則會更新票證到期。 如果設為 false，將在到期不會更新每個要求，因而導致票證過期完全逾時分鐘數超過第一次票證時建立。

> [!NOTE]
> 儲存在驗證票證到期是為絕對日期和時間值，例如 2008 年 8 月 2 日上午 11:34。 此外，日期和時間會與 web 伺服器的當地時間。 此設計決策可能有一些有趣的副作用周圍日光節約時間 (DST)，也就是在美國境內的時鐘移動前一小時 （假設在觀察到日光節約時間的地區設定中裝載的 web 伺服器） 時。 請考慮為 30 分鐘過期接近 DST 開始的時間與 ASP.NET 網站會發生什麼事 （此為上午 2:00）。 假設在訪客登入至站台在 2008 年 3 月 11 日上午 1:55。 這會產生表單驗證票證到期在 2008 年 3 月 11 日上午 2:25 （在未來 30 分鐘）。 不過，一旦上午 2:00 開始周圍，時鐘會跳到上午 3:00 因為 DST。 當使用者載入新的頁面 （在上午 3:01） 登入後六分鐘內時，FormsAuthenticationModule 備忘稿票證已過期，將使用者導向至登入頁面。 針對這個和其他驗證票證逾時 oddities，以及因應措施的更完整討論，收取一份 Stefan Schackow *Professional ASP.NET 2.0 安全性、 成員資格和角色管理*(ISBN:978-0-7645-9698-8)。


圖 1 說明工作流程時 slidingExpiration 設為 false，逾時設定為 30。 請注意，在登入時所產生的驗證票證包含到期日，並在後續的要求不會更新這個值。 如果 FormsAuthenticationModule 找到票證已過期，它會捨棄它，並要求視為匿名。


[![表單驗證票證到期時 slidingExpiration 圖形表示為 false](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**圖 01**： 表單驗證票證到期時 slidingExpiration 圖形表示法為 false ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))


圖 2 顯示工作流程時 slidingExpiration 設為 true，且逾時設定為 30。 （含未過期的票證） 收到已驗證的要求時屆到期會更新以在未來分鐘的逾時數。


[![表單驗證票證的圖形表示 slidingExpiration 為真時，](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**圖 02**： 表單驗證票證的圖形表示法 slidingExpiration 為 true 時 ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))


當使用 cookie 為基礎的驗證票證 （預設值），本討論內容會變成更容易混淆，因為 cookie 也可以指定自己 expiries。 Cookie 的到期 （或短缺） 會指示瀏覽器 cookie 應該予以終結時。 如果 cookie 缺少過期時，它就會終結瀏覽器關閉時。 如果過期，不過，cookie 到期的日期會儲存在使用者電腦上，而且已經過到期中指定的時間。 Cookie 終結時，瀏覽器，它不會再傳送至 web 伺服器。 因此，損毀是 cookie 的類似於使用者登出網站。

> [!NOTE]
> 當然，使用者可能會主動移除儲存在他們的電腦上任何 cookie。 在 Internet Explorer 7，您會前往 [工具]，[選項]，然後按一下 [瀏覽歷程記錄] 區段中的 [刪除] 按鈕。 從該處，按一下 [刪除 cookie] 按鈕。


表單驗證系統會建立要傳入的值而定的工作階段或到期 cookie *persistCookie*參數。 FormsAuthentication 類別 GetAuthCookie、 SetAuthCookie 和 RedirectFromLoginPage 方法會採用兩個輸入參數中的重新叫用： *username*和*persistCookie*。 我們在先前的教學課程所建立的登入頁面包含記住我 核取方塊，決定是否已建立永續性 cookie 中。 永續性 cookie 是到期為基礎;非持續性 cookies 是工作階段為基礎。

已經討論過的逾時和 slidingExpiration 概念套用在這兩個工作階段與過期的 cookie。 沒有執行中只能有一個次要的差異： 搭配使用時到期基礎 cookie slidingTimeout 設為 true，cookie 的到期，才會更新時超過一半的指定時間已經過去。

讓我們來更新我們的網站驗證票證逾時原則，使的票證逾時後一小時 （60 分鐘），使用滑動期限。 若要讓這項變更生效，更新 Web.config 檔案中，加入&lt;form&gt;元素&lt;驗證&gt;以下列標記的項目：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>使用 Login.aspx 以外的登入頁面 URL

由於 FormsAuthenticationModule 將自動重新導向未經授權的使用者登入頁面，它必須知道登入頁面 URL。 此 URL 中的 loginUrl 屬性所指定&lt;form&gt;項目，預設值為 login.aspx。 如果您要移植透過現有的網站，您可能已經有具有不同的 URL，其中一個已被標示為書籤並由搜尋引擎編製索引的登入頁面。 而不是重新命名現有的登入頁面為 login.aspx 並中斷連結，而且使用者的書籤，您可以改為修改 loginUrl 屬性，以指向您登入頁面。

比方說，如果您登入頁面命名為 SignIn.aspx，而且已位於使用者的目錄中，您無法指向 loginUrl 組態設定加入 ~/Users/SignIn.aspx 就像這樣：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

我們目前應用程式已經具有名為 Login.aspx 的登入頁面，因為沒有指定自訂值中的不需要&lt;form&gt;項目。

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>步驟 2： 使用 Cookie 的表單驗證票證

根據預設表單驗證系統會決定是否要儲存其驗證票證的 cookie 集合中，或將其內嵌在瀏覽網站的使用者代理程式為基礎的 URL。 所有主要的桌面瀏覽器類似 Internet Explorer、 Firefox、 Opera 與 Safari，支援 cookie，但並非所有的行動裝置。

使用表單驗證系統的 cookie 原則取決於在無 cookie 設定&lt;form&gt;項目，可以指派四個值之一：

- UseCookies-指定將一律使用 cookie 為基礎的驗證票證。
- UseUri-表示不會使用 cookie 為基礎的驗證票證。
- 不會自動偵測-如果裝置設定檔不支援 cookie，cookie 為基礎的驗證票證。如果裝置設定檔支援 cookie，探查機制用來判斷是否已啟用 cookie。
- UseDeviceProfile-預設值。只有當裝置設定檔支援 cookie，則會使用 cookie 為基礎的驗證票證。 會使用探查機制。

自動偵測及 UseDeviceProfile 設定依賴*裝置設定檔*中演練是否要使用 cookie 為基礎或 cookie 驗證票證。 ASP.NET 會維護資料庫的各種裝置和其功能，例如它們是否支援 cookie，它們支援時，JavaScript 和等等的版本。 每次裝置要求網頁從 web 伺服器，它會傳送沿*使用者代理程式*識別的裝置類型的 HTTP 標頭。 ASP.NET 自動符合其資料庫中指定的對應設定檔提供的使用者代理字串。

> [!NOTE]
> 裝置功能的這個資料庫會儲存在 XML 檔案的遵守的數字[瀏覽器定義檔案結構描述](https://msdn.microsoft.com/library/ms228122.aspx)。 預設的裝置設定檔位於 %windir%\microsoft.net\framework\v2.0.50727\config\browsers。 您也可以加入自訂的檔案至您的應用程式的應用程式\_瀏覽器資料夾。 如需詳細資訊，請參閱[How To： 偵測到 ASP.NET Web Pages 中的瀏覽器類型](https://msdn.microsoft.com/library/3yekbd5b.aspx)。


預設值是 UseDeviceProfile，因為其設定檔會報告不支援 cookie 的裝置所造訪的網站時將使用 cookie 的表單驗證票證。

### <a name="encoding-the-authentication-ticket-in-the-url"></a>在 URL 中的驗證票證的編碼方式

Cookie 是自然的媒體，以便在每個要求至特定網站，這是預設表單驗證設定會使用 cookie，如果正在瀏覽裝置支援它們的原因包括從瀏覽器的資訊。 如果不支援 cookie，則必須採用其他方式將驗證 ticket 從用戶端傳遞至伺服器。 編碼 URL 中的 cookie 資料是一般的因應措施無 cookie 的環境中使用。

若要查看如何內嵌這類資訊，在 URL 中的最佳方式是要強制站台使用 cookie 驗證票證。 這可藉由將 cookie 的組態設定來 UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

一旦您對進行這項變更，請瀏覽的網站透過瀏覽器。 當造訪匿名使用者的身分，Url 看起來會如同之前。 例如，造訪 Default.aspx 頁面時我瀏覽器的網址列會顯示下列 URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

不過，在登入，表單驗證票證會內嵌到 URL。 例如，瀏覽登入頁面，並以 Sam 登入之後, 我回到 Default.aspx 頁面上，但的 URL 這次：

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

表單驗證票證已經內嵌 URL 中。 字串 (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) 代表十六進位編碼的驗證票證資訊，而且是相同的資料通常儲存在 cookie 中。

Cookie 驗證票證，工作順序，系統必須編碼在頁面上的所有 Url，以包括驗證票券資料，否則當使用者按一下連結時驗證票證將會遺失。 幸好此內嵌的邏輯會自動執行。 為了示範這項功能，開啟 Default.aspx 頁面，並加入超連結控制項，將其文字和 NavigateUrl 屬性分別設定為 測試連結和 SomePage.aspx。 它並不重要，其實沒有頁面在名為 SomePage.aspx 受測專案中。

將變更儲存至 Default.aspx，然後透過瀏覽器瀏覽它。 以便在 URL 中內嵌表單驗證票證登入站台。 接下來，從 Default.aspx 中，按一下 [測試連結] 連結。 發生什麼情況？ 如果存在名為 SomePage.aspx 沒有頁面，然後 404 錯誤發生，但是這不是很重要這裡。 相反地，專注於您的瀏覽器中的網址列。 請注意，在 URL 中包含表單驗證票證 ！

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

連結 URL SomePage.aspx 已自動轉換成 URL 包含驗證 ticket-我們不需要撰寫的程式碼上 ！ 表單驗證票證會自動內嵌在不會啟動 http:// 的任何超連結的 URL 或 /。 如果在呼叫 Response.Redirect、 超連結控制項，或 HTML 錨定項目中顯示超連結，並不重要 (亦即， &lt;href ="..."&gt;...&lt;/a&gt;)。 只要 URL 不是像 http://www.someserver.com/SomePage.aspx 或 /SomePage.aspx，驗證票證會內嵌為我們的表單。

> [!NOTE]
> Cookie 的表單驗證票證遵守相同的逾時原則，以 cookie 為基礎的驗證票證。 不過，cookie 驗證票證會更容易重新執行攻擊，因為直接在 URL 中內嵌的驗證票證。 假設使用者瀏覽網站、 登入，然後將 URL 貼在電子郵件給同事。 如果達到到期之前同事按一下該連結，就會記錄的使用者身分傳送的電子郵件 ！


## <a name="step-3-securing-the-authentication-ticket"></a>步驟 3： 保護驗證票證

表單驗證票證經由網路傳輸在 cookie 或直接在 URL 中內嵌。 除了識別資訊的驗證票證也會包含使用者資料 （如我們所見在步驟 4）。 因此，務必確定已加密的票證資料和 （甚至更重要的是），從表單驗證系統可以保證票證已未遭竄改。

若要確保票證資料的隱私權，表單驗證系統可以加密票證資料。 加密的票證資料失敗會以純文字方式透過網路傳送潛在的敏感資訊。

若要保證票證的真實性，表單驗證系統必須*驗證*票證。 驗證是確保特定的資料未修改，而且透過完成*[訊息驗證碼 (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*。 簡而言之，MAC 是資訊的一小部分來識別需要驗證 （在此情況下，此票證） 的資料。 如果 MAC 所代表之資料遭到修改，然後 MAC 和資料將不匹配。 此外，會計算硬駭客同時修改的資料，並產生已修改的資料對應到他的 MAC。

建立 （或修改） 時票證時，表單驗證系統建立版本的 MAC，並將它附加至票券的資料。 後續的要求抵達時，表單驗證系統會比較 MAC 和票證驗證票券資料的真確性資料。 圖 3 以圖形方式說明這個工作流程。


[![票證的確有透過 MAC](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**圖 03**: 票證的確有透過 MAC ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))


哪些安全性量值會套用到驗證票證中的保護設定而定&lt;form&gt;項目。 保護設定可能會指派給下列三個值之一：

- 所有的票證同時加密和數位簽章 （預設值）。
- 加密-只加密已套用-產生沒有 MAC。
- 無-票證尚未加密或數位簽章。
- 驗證-MAC 會產生，但以純文字方式透過網路傳送的票券資料。

Microsoft 強烈建議使用的所有設定。

### <a name="setting-the-validation-and-decryption-keys"></a>設定驗證和解密金鑰

加密和雜湊演算法來加密和驗證的驗證票證使用表單驗證系統會透過可自訂[ &lt;machineKey&gt;元素](https://msdn.microsoft.com/library/w8h3skw9.aspx)Web.config 中。表 2 概述&lt;machineKey&gt;元素的屬性和可能的值。

| **屬性** | **描述** |
| --- | --- |
| 解密 | 表示用來加密的演算法。 這個屬性可以具有下列四個值之一:-自動-預設值。決定 decryptionKey 屬性的長度為基礎的演算法。 -使用 AES 使用[進階加密標準 (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard)演算法。 使用 DES-[資料加密標準 (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard)這個演算法會被視為弱式運算資源，而且不應使用。 使用-3DES-[三重 DES](http://en.wikipedia.org/wiki/Triple_DES)演算法，其運作方式是將 DES 演算法套用三次。 |
| decryptionKey | 使用的加密演算法的祕密金鑰。 此值必須是適當的長度 （根據解密中的值）、 自動產生或附加，任一個值的十六進位字串 IsolateApps。 加入 IsolateApps 指示要用於每個應用程式的唯一值的 ASP.NET。 預設為 AutoGenerate，IsolateApps。 |
| 驗證 | 表示用來驗證的演算法。 這個屬性可以具有下列四個值之一:-AES-使用進階加密標準 (AES) 演算法。 使用 MD5-[訊息摘要 5 (MD5)](http://en.wikipedia.org/wiki/MD5)演算法。 -使用 SHA1- [SHA1](http://en.wikipedia.org/wiki/Sha1)演算法 （預設值）。 -3DES-使用三重 DES 演算法。 |
| validationKey | 使用的驗證演算法的祕密金鑰。 此值必須是適當的長度 （根據驗證中的值）、 自動產生或附加，任一個值的十六進位字串 IsolateApps。 加入 IsolateApps 指示要用於每個應用程式的唯一值的 ASP.NET。 預設為 AutoGenerate，IsolateApps。 |

**表 2**: &lt;machineKey&gt;元素屬性

這些加密及驗證選項，以及專業人員的完整討論和優缺點的各種演算法，已超出本教學課程的範圍。 針對深入查看這些問題，包括指引何種加密和驗證演算法，若要使用，若要使用，哪些金鑰長度以及如何妥善地產生這些金鑰，請參閱*Professional ASP.NET 2.0 安全性、 成員資格和角色管理*.

根據預設，每個應用程式，會自動產生金鑰用於加密和驗證，而且這些金鑰會儲存在本機安全性授權 (LSA)。 簡單地說，預設設定保證唯一索引鍵的 web 伺服器的 web 伺服器和應用程式的應用程式為基礎。 因此，此預設行為將不適用於下列兩個案例：

- **Web 伺服陣列**-在[web 伺服陣列](http://en.wikipedia.org/wiki/Web_farm)基於延展性和備援能力的多部網頁伺服器上裝載案例中，單一的 web 應用程式。 每個傳入要求分派至伺服器陣列中，這表示使用者的工作階段的存留期間，透過不同的伺服器可能會用來處理他的各種要求的伺服器。 因此，每一部伺服器，以便建立表單驗證票證，必須使用相同的加密和驗證金鑰、 加密及驗證上一部伺服器可以解密和驗證伺服器陣列中的不同伺服器上。
- **跨應用程式票證共用**-單一 web 伺服器可以裝載多個 ASP.NET 應用程式。 如果您需要這些不同的應用程式共用單一的表單驗證票證，請務必加密和驗證金鑰匹配。

在 web 伺服陣列設定，或在同一部伺服器上的應用程式之間共用驗證票證工作時，您必須設定&lt;machineKey&gt;受影響的應用程式中的項目，讓其 Validationkey 和validationKey 值比對。

雖然上述狀況都不會套用至我們的範例應用程式，我們仍然可以指定明確的 Validationkey 和 validationKey 值，並定義要使用的演算法。 新增&lt;machineKey&gt;設為 Web.config 檔案：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

如需詳細資訊請參閱[How To： 在 ASP.NET 2.0 中的設定 MachineKey](https://msdn.microsoft.com/library/ms998288.aspx)。

> [!NOTE]
> 從拍攝的 Validationkey 和 validationKey 值[Steve Gibson](http://www.grc.com/stevegibson.htm)的[完美密碼網頁](https://www.grc.com/passwords.htm)，如此就會在每個頁面造訪產生 64 隨機十六進位字元。 若要減少到實際執行應用程式進行一直這些機碼的可能性，即建議取代隨機產生的項目從完美密碼頁面上方的索引鍵。


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>步驟 4： 將額外的使用者資料儲存在票證

許多 web 應用程式顯示的相關資訊，或目前登入使用者的基礎頁面的顯示。 例如，網頁可能會顯示使用者的名稱和她上次登入之每個頁面的上方角落中的日期。 表單驗證票證會儲存目前登入使用者的使用者名稱，但時所需的任何其他資訊的頁面必須移至使用者存放區-通常是資料庫-查閱資訊不會儲存在驗證票證。

利用最少的程式碼我們可以將額外的使用者資訊儲存表單驗證票證。 這類資料可以透過表示[FormsAuthenticationTicket 類別](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)的[UserData 屬性](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx)。 這是很有用的位置，用來放置通常所需的使用者資訊的資訊量很少。 UserData 屬性中包含的驗證票證 cookie，並像其他票證欄位、 加密，且驗證中指定的值會根據表單驗證系統的組態。 根據預設，UserData 為空字串。

若要將使用者資料儲存的驗證票證，我們要撰寫許多程式碼會擷取使用者特定資訊，並將它儲存在票證中的登入頁面中。 因為 UserData 是 String 類型的屬性，在其中儲存的資料必須正確序列化為字串。 例如，假設我們使用者存放區包含每個使用者的出生日期和其雇主，名稱，而我們想要儲存這些兩個屬性值中的驗證票證。 我們無法藉由串連使用者的日期的生日的字串以垂直線 (|)，後面接著雇主名稱，將這些值序列化成字串。 使用者的出生上 1974 年 8 月 15，適用於 Northwind Traders，我們會指派 UserData 屬性字串： 1974年-08-15 |Northwind Traders。

每當我們需要存取儲存在票證中的資料，我們可以這樣做抓取目前的要求 FormsAuthenticationTicket，然後還原序列化 [UserData] 屬性。 在生日與雇主名稱範例日期，我們會 UserData 字串分割成兩個子字串分隔符號 (|) 為基礎。


[![其他使用者資訊可以儲存在驗證票證](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**圖 04**： 其他使用者資訊可以儲存在驗證 Ticket ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))


### <a name="writing-information-to-userdata"></a>寫入資訊至 UserData

不幸的是，將使用者特定資訊新增至表單驗證票證不是直接，可能會以為。 UserData FormsAuthenticationTicket 類別屬性是唯讀的並只能透過 FormsAuthenticationTicket 類別建構函式指定。 當指定 UserData 屬性建構函式中，我們還需要提供票證的其他值： 使用者名稱、 發行日期、 到期日等等。 當我們建立的登入頁面在先前的教學課程中時，這是所有我們由處理 FormsAuthentication 類別。 當加入 FormsAuthenticationTicket UserData，我們必須撰寫程式碼來複寫許多 FormsAuthentication 類別已經提供的功能。

讓我們來探索處理的更新來記錄使用者的驗證票證的其他資訊的 Login.aspx 頁面 UserData 必要的程式碼。 假設我們使用者存放區包含適用於使用者的公司和其標題的相關資訊，我們想要擷取這項資訊驗證票證。 更新的 Login.aspx 頁面 LoginButton Click 事件處理常式，使程式碼看起來如下：

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

讓我們逐步執行此程式碼的同一行一次。 藉由定義四個字串陣列的方法會啟動： 使用者、 密碼、 companyName 和 titleAtCompany。 這些陣列保留使用者名稱、 密碼、 公司名稱和標題的使用者帳戶的系統中的這三個： Scott、 Jisun 和 Sam。 在實際應用中，會從使用者存放區中，沒有硬式編碼在頁面的原始程式碼中查詢這些值。

在上一個教學課程中，如果提供的認證不正確我們只被呼叫 FormsAuthentication.RedirectFromLoginPage UserName.Text (RememberMe.Checked），這會執行下列步驟：

1. 建立表單驗證票證
2. 寫入適當的存放區中的票證。 Cookie 為基礎的驗證票證，則使用瀏覽器的 cookie 集合。cookie 的驗證票證的票券資料序列化至 URL
3. 重新導向至適當的頁面的使用者

上述程式碼中複寫這些步驟。 首先，我們最終將儲存在 [UserData] 屬性的字串會形成結合的公司名稱和標題，用來分隔兩個值使用縱線字元 (|)。

Dim userDataString As String = String.Concat(companyName(i), "|", titleAtCompany(i))

接下來，FormsAuthentication.GetAuthCookie 叫用方法時，會建立驗證 ticket，加密和驗證根據組態設定，並將它放在 HttpCookie 物件。

Dim authCookie As HttpCookie = FormsAuthentication.GetAuthCookie(UserName.Text, RememberMe.Checked)

若要使用內嵌在 cookie FormAuthenticationTicket，我們要呼叫 FormAuthentication 類別[解密方法](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)，傳入的 cookie 值。

Dim ticket As FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value)

然後建立*新*FormsAuthenticationTicket 執行個體根據現有的 FormsAuthenticationTicket 值。 不過，此新票證包含使用者特定資訊 (userDataString)。

Dim newTicket As FormsAuthenticationTicket = New FormsAuthenticationTicket(ticket.Version, ticket.Name, ticket.IssueDate, ticket.Expiration, ticket.IsPersistent, userDataString)

我們再加密 （及驗證） 新 FormsAuthenticationTicket 執行個體，藉由呼叫[加密方法](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)，並放回到 authCookie 此加密 （及驗證） 的資料。

authCookie.Value = FormsAuthentication.Encrypt(newTicket)

最後，authCookie 加入至 Response.Cookies 集合並 GetRedirectUrl 方法會呼叫來決定適當的頁面，即可傳送給使用者。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

這段程式碼需要因為 UserData 屬性是唯讀的而且 FormsAuthentication 類別並未提供在其 GetAuthCookie、 SetAuthCookie 或 RedirectFromLoginPage 方法中指定 UserData 資訊的任何方法。

> [!NOTE]
> 我們只檢查程式碼會將使用者特定資訊儲存在 cookie 為基礎的驗證票證。 類別負責序列化表單驗證票證的 url 是內部的.NET framework。 簡短的長劇本，您無法儲存使用者資料在無 cookie 表單驗證票證。


### <a name="accessing-the-userdata-information"></a>存取 UserData 資訊

此時每位使用者的公司名稱和標題會儲存在表單驗證票證 UserData 屬性在登入時。 這項資訊可以從任何頁面上的驗證票證存取，而不需要為使用者存放區的路線。 若要說明如何從 UserData 屬性擷取這項資訊，讓我們將更新 Default.aspx，使其歡迎訊息包含不只的使用者名稱，但也可針對公司和其標題。

目前，Default.aspx 包含 AuthenticatedMessagePanel 面板與名為 WelcomeBackMessage 標籤控制項。 此面板只會顯示已驗證的使用者。 更新 Default.aspx 的頁面中的程式碼\_載入事件處理常式，讓它看起來如下：

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

如果 Request.IsAuthenticated 為 True，則 WelcomeBackMessage Text 屬性第一次設歡迎回來， *username*。 然後，User.Identity 屬性會轉換成 FormsIdentity 物件，以便我們可以存取基礎 FormsAuthenticationTicket。 一旦 FormsAuthenticationTicket，我們會還原序列化的公司名稱和標題的 UserData 屬性。 這被透過分割上縱線字元的字串。 然後，公司名稱和標題會顯示在 WelcomeBackMessage 標籤。

圖 5 顯示這個畫面的螢幕擷取畫面中的動作。 以Scott的身份登錄，並顯示包含Scott的公司和頭銜的歡迎信息。


[![會顯示目前登入使用者的公司和標題](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**圖 05**： 會顯示目前登入使用者的公司和標題 ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))


> [!NOTE]
> 驗證票證 UserData 屬性做為使用者存放區的快取。 像任何快取，它需要修改基礎資料時，會更新。 比方說，如果沒有使用者可以更新其設定檔的網頁，快取 UserData 屬性中的欄位必須重新整理以反映使用者所做的變更。


## <a name="step-5-using-a-custom-principal"></a>步驟 5： 使用自訂主體

在每個傳入要求上 FormsAuthenticationModule 會嘗試驗證使用者。 如果未過期的驗證票證，FormsAuthenticationModule 會 HttpContext.User 屬性指派給新的 GenericPrincipal 物件。 這個 GenericPrincipal 物件都有識別的型別 FormsIdentity，包含表單驗證票證的參考。 GenericPrincipal 類別包含的裸機實作 IPrincipal 的類別所需的最低功能-只包含識別屬性和 IsInRole 方法。

主體的物件具有兩個責任： 指出哪些使用者所屬的角色，並提供識別資訊。 這是透過 IPrincipal 介面 IsInRole (*roleName*) 方法，並識別屬性，分別。 GenericPrincipal 類別允許透過其建構函式; 指定之角色名稱的字串陣列其 IsInRole (*roleName*) 方法只會檢查是否要傳入中*roleName*存在於字串陣列。 當 FormsAuthenticationModule 建立 GenericPrincipal 時，傳遞空字串陣列中的 GenericPrincipal 建構函式。 因此，IsInRole 的任何呼叫一定會傳回 False。

GenericPrincipal 類別符合大部分的表單型的驗證情況下，不使用角色的需求。 針對沒有足夠的預設角色處理這些情況下，或當您需要將自訂的 IIdentity 物件與使用者產生關聯，您可以在驗證工作流程期間建立自訂的 IPrincipal 物件，並將它指派給 HttpContext.User 屬性。

> [!NOTE]
> 我們在未來將會看到教學課程中，當 ASP。已啟用網路的角色架構它會建立自訂的主體物件的型別[RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx)並覆寫的表單驗證建立 GenericPrincipal 物件。 它會以自訂的主體 IsInRole 方法，以與角色架構的應用程式開發介面的介面。


由於我們有不在意自己角色尚未，我們必須在此時建立自訂主體的唯一理由是自訂的識別物件的主體產生關聯。 步驟 4 中我們探討了額外的使用者資訊儲存在驗證票證 UserData 屬性中，特別是，使用者的公司名稱和其標題。 不過，UserData 資訊才會透過驗證票證和唯一，然後為序列化字串時，這表示我們想要檢視使用者資訊儲存在票證每當我們需要剖析 UserData 屬性。

我們可以建立可實作 IIdentity 和包括 CompanyName 和 Title 屬性的類別來改善開發人員的體驗。 這樣一來，開發人員可以存取目前登入使用者的公司名稱和標題直接透過的公司名稱和標題的屬性，而不需要知道如何剖析 UserData 屬性。

### <a name="creating-the-custom-identity-and-principal-classes"></a>建立自訂身分識別和主體類別

本教學課程，讓我們來建立自訂的主體和身分識別物件 」 應用程式中\_程式碼資料夾。 藉由新增應用程式啟動\_程式碼加入專案的資料夾-方案總管] 中的專案名稱上按一下滑鼠右鍵，選取加入 ASP.NET 資料夾選項，然後選擇 [應用程式\_程式碼。 應用程式\_程式碼資料夾是特殊的 ASP.NET 資料夾所在類別檔案的特定網站。

> [!NOTE]
> 應用程式\_管理透過網站專案模型專案時，應該只使用程式碼資料夾。 如果您使用[Web 應用程式專案模型](https://msdn.microsoft.com/asp.net/Aa336618.aspx)、 建立標準的資料夾，並將類別加入至該。 比方說，您無法加入名為類別的新資料夾，並那里放置您的程式碼。


接下來，將兩個新的類別檔案加入至應用程式\_程式碼資料夾，一個具名的 CustomIdentity.vb，另一個名為 CustomPrincipal.vb。


[![專案中加入的 CustomIdentity 和 CustomPrincipal 類別](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**圖 06**： 您的專案中加入的 CustomIdentity 和 CustomPrincipal 類別 ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))


CustomIdentity 類別會負責實作 IIdentity 介面，它會定義 AuthenticationType、 IsAuthenticated 和名稱屬性。 除了這些必要的屬性，我們有興趣公開基礎表單驗證票證，以及使用者的公司名稱和標題的屬性項目。 下列程式碼輸入 CustomIdentity 類別。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

請注意此類別包含 FormsAuthenticationTicket 成員變數 (\_票證)，此票證資訊必須透過建構函式來提供。 此票證資料用在傳回的識別名稱。其 [UserData] 屬性會剖析為傳回的 CompanyName 和 Title 屬性的值。

接下來，建立 CustomPrincipal 類別。 由於我們不注重角色在此時，CustomPrincipal 類別的建構函式接受只有 CustomIdentity 物件;其 IsInRole 方法一律會傳回 False。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>CustomPrincipal 物件指派給內送要求的安全性內容

我們現在有了延伸要包括 CompanyName 和 Title 屬性的預設識別規格的類別，以及使用自訂身分識別的自訂主體類別。 我們準備好逐步執行的 ASP.NET 管線，而且我們自訂的主體物件指派到連入要求的安全性內容。

接受連入要求的 ASP.NET 管線並進行處理透過幾個步驟。 在每個步驟中，會引發特定事件，讓開發人員挖掘 ASP.NET 管線，並修改在其生命週期中特定點上的要求。 FormsAuthenticationModule，比方說，等候要引發的 ASP.NET [AuthenticateRequest 事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)，此時它會檢查連入要求的驗證票證。 如果找到驗證票證，GenericPrincipal 物件建立，並指派給 HttpContext.User 屬性。

AuthenticateRequest 事件之後引發的 ASP.NET 管線[PostAuthenticateRequest 事件](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx)，它是我們可以在其中取代的執行個體 FormsAuthenticationModule 所建立的 GenericPrincipal 物件我們CustomPrincipal 物件。 圖 7 說明此工作流程。


[![GenericPrincipal 取代 CustomPrincipal PostAuthenticationRequest 事件中](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**圖 07**: CustomPrincipal PostAuthenticationRequest 事件中取代 GenericPrincipal ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))


若要執行程式碼中的 ASP.NET 管線事件，我們可以在 Global.asax 中建立適當的事件處理常式，或建立自己的 HTTP 模組。 此教學課程中我們來建立要在 Global.asax 中的事件處理常式。 啟動者 Global.asax 加入您的網站。 以滑鼠右鍵按一下方案總管 中的專案名稱，然後加入名為 Global.asax 全域應用程式類別類型的項目。


[![Global.asax 檔案加入您的網站](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**圖 08**: Global.asax 檔案加入您的網站 ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))


預設 Global.asax 範本包含的數字的 ASP.NET 管線事件，包括開始時，結束的事件處理常式和[錯誤事件](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)，和其他項目。 請放心地移除這些事件處理常式，因為我們不需要它們為此應用程式。 我們有興趣的事件是 PostAuthenticateRequest。 更新程式 Global.asax 檔案，讓其標記看起來類似下：

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

應用程式\_OnPostAuthenticateRequest 方法執行每個 ASP.NET 執行階段引發 PostAuthenticateRequest 事件，這在每個連入的頁面要求上一次發生的時間。 此事件處理常式會啟動，檢查使用者是否經過驗證，並且透過表單驗證的驗證。 如果是的話，新的 CustomIdentity 物件建立，並在其建構函式中傳遞目前要求的驗證票證。 接下來，會 CustomPrincipal 物件建立，並在其建構函式中傳遞的剛建立 CustomIdentity 物件。 最後，將目前要求的安全性內容會指派給新建立的 CustomPrincipal 物件。

請注意最後一個步驟-建立 CustomPrincipal 物件關聯與要求的安全性內容-會將主體指派給兩個屬性： HttpContext.User 和 Thread.CurrentPrincipal。 這些兩個的指派是必要的因為 ASP.NET 中處理的安全性內容的方式。 .NET Framework 與每個執行中的執行緒; 產生關聯的安全性內容這項資訊可透過 IPrincipal 物件[執行緒物件](https://msdn.microsoft.com/library/system.threading.thread.aspx)的[CurrentPrincipal 屬性](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx)。 什麼是令人困惑是 ASP.NET 有它自己的安全性內容資訊 (HttpContext.User)。

在某些情況下，判斷安全性內容中; 時，會檢查 Thread.CurrentPrincipal 屬性在其他情況下，會使用 HttpContext.User。 例如，.NET 中的安全性功能可讓開發人員以宣告方式狀態哪些使用者或角色可以具現化類別或特定的方法會叫用 (請參閱[企業和資料層使用新增授權規則PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx))。 基本上，這些宣告式方法會決定透過 Thread.CurrentPrincipal 屬性的安全性內容。

在其他情況下，會使用 HttpContext.User 屬性。 例如，在上一個教學課程中我們用這個屬性來顯示目前登入使用者的使用者名稱。 很明顯地，請務必 Thread.CurrentPrincipal 和 HttpContext.User 屬性中的安全性內容資訊匹配。

ASP.NET 執行階段自動同步處理為我們的這些屬性值。 不過，此同步處理，將 AuthenticateRequest 事件之後，就會發生但*之前*PostAuthenticateRequest 事件。 因此，我們需要確定手動指派 Thread.CurrentPrincipal 或 else Thread.CurrentPrincipal PostAuthenticateRequest 事件中加入自訂的主體時，且 HttpContext.User 會同步。請參閱[Context.User vs。Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/)的更詳細討論此問題。

### <a name="accessing-the-companyname-and-title-properties"></a>存取公司名稱和標題屬性

每當要求到達並分派至應用程式的 ASP.NET 引擎\_OnPostAuthenticateRequest 在 Global.asax 中的事件處理常式就會引發。 如果 FormsAuthenticationModule 成功驗證要求，事件處理常式會建立新的 CustomPrincipal 物件與基礎表單驗證票證 CustomIdentity 物件。 使用就地這類邏輯，存取目前登入使用者的公司名稱和標題資訊就非常簡單的。

返回頁面\_Load 事件處理常式在 Default.aspx 中，其中在步驟 4 中撰寫程式碼來擷取表單驗證票證和剖析 UserData 屬性以顯示使用者的公司名稱和標題。 現在使用中的 CustomPrincipal 和 CustomIdentity 物件沒有需要剖析超出票證 UserData 屬性值。 相反地，只要取得 CustomIdentity 物件的參考，並使用其公司名稱和標題的屬性：

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>總結

在本教學課程，我們會檢查如何自訂表單驗證系統的設定，透過 Web.config。我們探討了驗證票證到期的處理方式，以及如何使用加密和驗證的保護措施來防止檢查和修改的票證。 最後，我們將討論使用的驗證票證的 UserData 屬性儲存在票證本身，其他使用者資訊及如何使用自訂主體和身分識別的物件公開此資訊以更方便開發人員的方式。

本教學課程結束時，我們在 ASP.NET 表單驗證檢查。 下一個教學課程會啟動我們到成員資格架構的旅程。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [將分解的表單驗證](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [說明： 在 ASP.NET 2.0 中的表單驗證](https://msdn.microsoft.com/library/aa480476.aspx)
- [如何： 保護表單驗證，在 ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [專業 ASP.NET 2.0 安全性、 成員資格和角色管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [設定登入控制項的安全性](https://msdn.microsoft.com/library/ms178346.aspx)
- [&lt;驗證&gt;項目](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Form&gt;元素&lt;驗證&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt;項目](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [了解的表單驗證票證和 Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教學課程所包含的主題訓練影片

- [如何變更表單驗證屬性](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [如何在 ASP.NET 應用程式中的設定並使用 Cookie 無驗證](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [ASP 表單登入重新配置](../../../videos/authentication/asp-forms-login-relocation.md)
- [表單登入自訂金鑰組態](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [將自訂資料新增至驗證方法](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [使用自訂的主體物件](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Alicja Maziarz。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com)。

> [!div class="step-by-step"]
> [上一步](an-overview-of-forms-authentication-vb.md)
