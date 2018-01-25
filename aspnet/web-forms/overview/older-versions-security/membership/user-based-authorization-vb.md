---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-vb
title: "使用者為基礎的授權 (VB) |Microsoft 文件"
author: rick-anderson
description: "在此教學課程中我們將探討限制存取頁面，並限制頁面層級功能，透過不同的技術。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: bc937e9d-5c14-4fc4-aec7-440da924dd18
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 5579292930da97b142ff6db5d34d33be77aeea4b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="user-based-authorization-vb"></a>使用者為基礎的授權 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_VB.zip)或[下載 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_vb.pdf)

> 在此教學課程中我們將探討限制存取頁面，並限制頁面層級功能，透過不同的技術。


## <a name="introduction"></a>簡介

大部分的 web 應用程式提供使用者帳戶，這樣做要限制特定訪客存取網站內特定網頁組件中。 最線上留言板站台，例如，-匿名和已驗證的所有使用者都都可以檢視的留言板文章，但只有經過驗證的使用者可以瀏覽網頁，即可建立新的文章。 而且可能才能夠存取特定的使用者 （或一組特定的使用者） 的系統管理頁面。 此外，使用者以使用者為基礎的頁面層級功能可以與不同。 檢視時一份文章，已驗證的使用者會顯示評等每個使用 post 要求的介面，而這個介面不是匿名的訪客使用。

ASP.NET 輕鬆地定義使用者為基礎的授權規則。 中的標記只需要稍微`Web.config`，特定網頁或整個目錄可以鎖定，使其只能存取指定的使用者子集。 頁面層級功能可以開啟或關閉根據目前登入的使用者，透過程式設計和宣告式的方式。

在此教學課程中我們將探討限制存取頁面，並限制頁面層級功能，透過不同的技術。 讓我們開始吧 ！

## <a name="a-look-at-the-url-authorization-workflow"></a>在 URL 授權工作流程的外觀

中所述[*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-vb.md)教學課程中，當 ASP.NET 執行階段會處理要求的 ASP.NET 資源要求在其生命週期期間引發的事件數目。 *HTTP 模組*是受管理的程式碼執行以回應要求的生命週期中的特定事件的類別。 ASP.NET 隨附之 HTTP 模組執行基本工作，在幕後的數目。

一個這類 HTTP 模組是[ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)。 上一個教學課程的主要功能中所述`FormsAuthenticationModule`是要判斷目前的要求識別。 這是藉由檢查表單驗證票證，位於 cookie 或是 URL 中內嵌達成。 這個識別發生期間[`AuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)。

另一個重要的 HTTP 模組是[ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)，這會引發以回應[`AuthorizeRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)(也就是之後`AuthenticateRequest`事件)。 `UrlAuthorizationModule`會檢查組態中的標記`Web.config`判斷目前的身分識別是否有授權造訪指定的頁面。 此程序指*URL 授權*。

我們將檢驗 URL 授權規則，在步驟 1 中的語法但首先讓我們看看什麼`UrlAuthorizationModule`並根據要求是否已獲授權或未。 如果`UrlAuthorizationModule`決定要求已獲得授權，則不會執行任何動作，以及要求會繼續透過其生命週期。 不過，如果要求是*不*獲授權，然後在`UrlAuthorizationModule`中止生命週期，並指示`Response`要傳回物件[HTTP 401 未經授權](http://www.checkupdown.com/status/E401.html)狀態。 使用表單驗證此 HTTP 401 狀態會永遠不會傳回至用戶端因為如果`FormsAuthenticationModule`偵測狀態是 HTTP 401 來加以修改[HTTP 302 重新導向](http://www.checkupdown.com/status/E302.html)登入頁面。

圖 1 所示的 ASP.NET 管線中的工作流程`FormsAuthenticationModule`，而`UrlAuthorizationModule`未經授權的要求送達時。 特別是，圖 1 顯示的要求為匿名訪客`ProtectedPage.aspx`，這是拒絕匿名使用者存取的頁面。 訪客是匿名的因為`UrlAuthorizationModule`中止要求，並傳回 HTTP 401 未經授權的狀態。 `FormsAuthenticationModule`然後將 401 狀態轉換成 302 重新導向至登入頁面。 透過登入頁面會驗證使用者之後，他會重新導向`ProtectedPage.aspx`。 這次`FormsAuthenticationModule`識別使用者根據其驗證票證。 現在，造訪者進行驗證時，`UrlAuthorizationModule`允許存取網頁。


[![表單驗證和 URL 授權工作流程](user-based-authorization-vb/_static/image2.png)](user-based-authorization-vb/_static/image1.png)

**圖 1**: 表單驗證和 URL 授權工作流程 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image3.png))


圖 1 顯示匿名的訪客嘗試存取資源，不是可供匿名使用者使用時，就會發生的互動。 在這種情況下，她嘗試瀏覽於 querystring 中指定的網頁登入頁面會重新導向匿名訪客。 使用者已成功登入之後，她將會自動重新導向回到她一開始所嘗試檢視的資源。

由匿名使用者進行未經授權的要求時，此工作流程很簡單，而且可輕易來了解發生了什麼事的訪客和原因。 但請注意，`FormsAuthenticationModule`將會重新導向*任何*未經授權的使用者登入 頁面上，即使該要求由已驗證的使用者。 這可能會導致令人混淆的使用者經驗如果已驗證的使用者嘗試瀏覽的頁面，她的不足的授權單位。

假設我們的網站已設定其 URL 授權規則的 ASP.NET 網頁`OnlyTito.aspx`是可以存取此僅 Tito。 現在，假設 Sam 瀏覽網站、 登入，並嘗試瀏覽`OnlyTito.aspx`。 `UrlAuthorizationModule`會中止要求的生命週期，並傳回 HTTP 401 未經授權的狀態，其中`FormsAuthenticationModule`會偵測並再將 Sam 重新導向至登入頁面。 因為已將 Sam 記錄，不過，她可能會想知道為什麼她已傳送至登入頁面。 她可能原因，她的登入認證已遺失以某種方式，或使用者輸入認證無效。 Sam 重新進入自己的認證登入頁面從她 （一次） 登入和，是否會重新導向至`OnlyTito.aspx`。 `UrlAuthorizationModule` Sam 無法造訪此頁，她將會傳回登入頁面將會偵測。

圖 2 說明此令人混淆的工作流程。


[![預設工作流程可能會導致令人混淆的循環](user-based-authorization-vb/_static/image5.png)](user-based-authorization-vb/_static/image4.png)

**圖 2**: 預設工作流程可能會導致令人困惑的循環 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image6.png))


圖 2 中所述的工作流程可以快速 befuddle 甚至的大部分電腦有訪客。 我們將探討如何避免混淆在步驟 2 中的循環。

> [!NOTE]
> ASP.NET 會使用兩種機制來判斷目前使用者是否可以存取特定網頁： URL 授權和授權檔案。 檔案授權由實作[ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx)，查閱要求的檔案 Acl 授權單位所決定。 因為 Acl 是套用到 Windows 帳戶的權限，檔案授權是最常用來使用 Windows 驗證。 使用表單驗證時，所有的作業系統和檔案系統層級要求是由相同的 Windows 帳戶，無論使用者瀏覽的網站中執行。 因為這個教學課程系列著重於表單驗證，我們將不會討論檔案授權。


### <a name="the-scope-of-url-authorization"></a>URL 授權範圍

`UrlAuthorizationModule`是 managed 程式碼是 ASP.NET 執行階段的一部分。 在 Microsoft 的第 7 版之前[網際網路資訊服務 (IIS)](https://www.iis.net/) web 伺服器時發生 IIS 的 HTTP 管線和管線 ASP.NET 執行階段之間的相異屏障。 簡單地說，在 IIS 6 及更早版本，ASP。網路的`UrlAuthorizationModule`要求從 IIS 委派給 ASP.NET 執行階段時才執行。 根據預設，IIS 處理之類的靜態內容本身的 HTML 網頁和 CSS、 JavaScript 和影像檔案，並只送出要求的 ASP.NET 執行階段時的擴充功能的分頁`.aspx`， `.asmx`，或`.ashx`要求。

IIS 7 不過，可讓整合式的 IIS 和 ASP.NET 的管線。 使用一些組態設定中，您可以設定要叫用的 IIS 7`UrlAuthorizationModule`如*所有*要求，這表示，可以針對任何類型的檔案中定義 URL 授權規則。 此外，IIS 7 包含它自己的 URL 授權引擎。 如需有關 ASP.NET 整合和 IIS 7 原生的 URL 授權功能的詳細資訊，請參閱[了解 IIS7 URL 授權](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)。 ASP.NET 和 IIS 7 的整合更深入探討，收取一份 Shahram Khosravi 書籍*Professional IIS 7 和 ASP.NET 整合程式設計*(ISBN: 978 0470152539)。

簡單地說，在 IIS 7 之前的版本，URL 授權規則只適用於 ASP.NET 執行階段所處理的資源。 但是，IIS 7 您也可以使用 IIS 的原生的 URL 授權功能或整合 ASP。網路的`UrlAuthorizationModule`至 IIS 的 HTTP 管線，因而擴充到所有的要求這項功能。

> [!NOTE]
> 有一些細微但重要的差異，如何在 ASP。網路的`UrlAuthorizationModule`和 IIS 7 處理授權規則的 URL 授權功能。 本教學課程不會檢查 IIS 7 URL 授權功能或它如何剖析相較於授權規則的差異`UrlAuthorizationModule`。 如需有關這些主題的詳細資訊，請參閱 MSDN 上或在 IIS 7 文件[www.iis.net](https://www.iis.net/)。


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>步驟 1： 定義中的 URL 授權規則`Web.config`

`UrlAuthorizationModule`決定是否要授與或拒絕存取所要求的資源的應用程式的組態中定義的 URL 授權規則所根據的特定身分識別。 授權規則拼中[`<authorization>`元素](https://msdn.microsoft.com/library/8d82143t.aspx)形式`<allow>`和`<deny>`子項目。 每個`<allow>`和`<deny>`子元素可以指定：

- 特定使用者
- 以逗號分隔的使用者清單
- 所有匿名使用者，表示由問號 （？）
- 所有使用者，以星號表示 (\*)

下列標記會說明如何使用 URL 授權規則允許 Tito 與 Scott 的使用者，以及拒絕其他所有項目：

[!code-xml[Main](user-based-authorization-vb/samples/sample1.xml)]

`<allow>`項目會定義哪些使用者允許 Tito 和 Scott 為時`<deny>`項目指示的*所有*使用者被拒絕。

> [!NOTE]
> `<allow>`和`<deny>`項目也可以指定角色的授權規則。 我們會檢查在未來的教學課程中的角色為基礎的授權。


下列設定會授與使用者 （包括匿名的使用者） 的 Sam 以外的任何存取權：

[!code-xml[Main](user-based-authorization-vb/samples/sample2.xml)]

若要允許已驗證的使用者，請使用下列設定中，它會拒絕存取權給所有匿名使用者：

[!code-xml[Main](user-based-authorization-vb/samples/sample3.xml)]

授權規則內定義`<system.web>`中的項目`Web.config`和適用於所有 web 應用程式中的 ASP.NET 資源。 有時候，應用程式會有不同的授權規則，不同的區段。 例如，在電子商務網站上，所有造訪者可能瀏覽產品、 查看產品評論、 搜尋類別目錄，等等。 不過，只有經過驗證的使用者可能會碰到簽出或要管理的出貨歷程記錄的頁面。 此外，可能是網站的僅提供所選取的使用者，例如站台系統管理員可存取的部分。

ASP.NET 輕鬆地在網站定義不同的檔案和資料夾的其他授權規則。 指定根資料夾中的授權規則`Web.config`檔案套用至所有 ASP.NET 網站中的資源。 不過，這些預設授權設定會覆寫特定的資料夾加`Web.config`與`<authorization>`> 一節。

讓我們來更新我們的網站，以便只有驗證的使用者可以瀏覽 ASP.NET 網頁中的`Membership`資料夾。 若要完成此我們需要加入`Web.config`檔案`Membership`資料夾，然後設定其以拒絕匿名使用者的授權設定。 以滑鼠右鍵按一下`Membership`資料夾中的在 [方案總管] 中，從內容功能表中，選擇 [加入新項目] 功能表，然後加入新的 Web 組態檔名為`Web.config`。


[![將 Web.config 檔案新增至 [成員資格] 資料夾](user-based-authorization-vb/_static/image8.png)](user-based-authorization-vb/_static/image7.png)

**圖 3**： 新增`Web.config`檔案`Membership`資料夾 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image9.png))


此時您的專案應包含兩個`Web.config`檔案： 其中一個在目錄的根目錄，一個在`Membership`資料夾。


[![您的應用程式現在應該會包含兩個 Web.config 檔案](user-based-authorization-vb/_static/image11.png)](user-based-authorization-vb/_static/image10.png)

**圖 4**： 您的應用程式應該現在包含兩個`Web.config`檔案 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image12.png))


更新組態檔中的`Membership`資料夾，讓它禁止匿名使用者存取權。

[!code-xml[Main](user-based-authorization-vb/samples/sample4.xml)]

這就是這麼簡單 ！

若要測試這項變更，請瀏覽在瀏覽器首頁，請確定您登出。由於 ASP.NET 應用程式的預設行為是讓所有造訪者，而且因為我們並未進行任何授權修改的根目錄`Web.config`檔案中，我們就能瀏覽為匿名的訪客的根目錄中的檔案。

按一下左邊的資料行中找到建立使用者帳戶連結。 這會帶您到`~/Membership/CreatingUserAccounts.aspx`。 因為`Web.config`檔案`Membership`資料夾都會定義授權規則，以禁止匿名存取，`UrlAuthorizationModule`中止要求，並傳回 HTTP 401 未經授權的狀態。 `FormsAuthenticationModule`會傳送給我們登入頁面 302 重新導向狀態來加以修改。 請注意，畫面我們已嘗試存取 (`CreatingUserAccounts.aspx`) 傳遞至透過登入頁面`ReturnUrl`querystring 參數。


[![URL 授權規則禁止匿名存取，因為我們會重新導向至登入頁面](user-based-authorization-vb/_static/image14.png)](user-based-authorization-vb/_static/image13.png)

**圖 5**: URL 授權規則禁止匿名存取，因為我們會重新導向至登入頁面 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image15.png))


在成功登入，我們會重新導向至`CreatingUserAccounts.aspx`頁面。 這次`UrlAuthorizationModule`允許存取網頁，因為我們不會再為匿名。

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>套用至特定位置的 URL 授權規則

中定義的授權設定`<system.web>`區段`Web.config`適用於所有該目錄及其子目錄中的 ASP.NET 資源 (直到否則覆寫另一個`Web.config`檔案)。 不過，在某些情況下，我們可能想指定的目錄，才能具有特定授權組態除了一個或兩個特定的頁面中的所有 ASP.NET 資源。 這可藉由新增`<location>`中的項目`Web.config`指向不同的授權規則，該檔案，其中定義其唯一授權規則。

為了說明使用`<location>`項目，以覆寫特定資源的組態設定，讓我們自訂授權設定，以便只 Tito 可以瀏覽`CreatingUserAccounts.aspx`。 若要達成此目的，將`<location>`元素`Membership`資料夾的`Web.config`檔案，並更新其標記，讓它看起來如下：

[!code-xml[Main](user-based-authorization-vb/samples/sample5.xml)]

`<authorization>`中的項目`<system.web>`定義 ASP.NET 中的資源的預設 URL 授權規則`Membership`資料夾以及其子資料夾。 `<location>`項目可讓我們來覆寫這些規則的特定資源。 在上述標記`<location>`項目參考`CreatingUserAccounts.aspx`頁面上，並指定其授權規則，例如允許 Tito，但拒絕其他人。

若要測試這項授權變更，啟動瀏覽的網站，為匿名使用者。 如果您嘗試瀏覽中的任何頁面`Membership`資料夾，例如`UserBasedAuthorization.aspx`、`UrlAuthorizationModule`將會拒絕要求，您將會被重新導向至登入頁面。 您可以瀏覽中的任何頁面之後，Scott，以登入`Membership`資料夾*除了*的`CreatingUserAccounts.aspx`。 嘗試瀏覽`CreatingUserAccounts.aspx`登入，因為任何人都但 Tito 將會導致未經授權的存取嘗試，將您重新導向回到登入頁面。

> [!NOTE]
> `<location>`項目必須出現在組態之外`<system.web>`項目。 您需要使用個別`<location>`之每個資源的授權設定，您想要覆寫的項目。


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>看看如何`UrlAuthorizationModule`使用授權規則授與或拒絕存取

`UrlAuthorizationModule`判斷是否授權特定 URL 的特定識別，藉由分析 URL 授權規則一次，從第一個啟動，然後向下進行工作。 找到相符項目，因為使用者被授與或拒絕存取，取決於如果比對中找不到`<allow>`或`<deny>`項目。 **如果找到相符項目，授與使用者存取。** 因此，如果您想要限制存取，務必您使用`<deny>`URL 授權設定中的最後一個元素的項目。 **如果您省略 * * *`<deny>`* * * 元素中，所有使用者會被授都與存取。**

若要深入了解使用的程序`UrlAuthorizationModule`若要判斷授權單位，假設我們探討了稍早在此步驟中的 URL 授權規則。 第一個規則`<allow>`元素，讓您存取 Tito 與 Scott。 第二個規則都是`<deny>`每個人都拒絕存取的項目。 如果匿名使用者造訪，`UrlAuthorizationModule`是匿名的啟動要求，Scott 或 Tito 嗎？ 回應，很明顯地，為 [否]，讓它繼續進行第二個規則。 是匿名集中的每一個人？ 因為回應以下是 [是]，`<deny>`規則會置於作用中，而且造訪者會重新導向至登入頁面。 同樣地，如果正在造訪 Jisun`UrlAuthorizationModule`啟動要求，是 Jisun Scott 或 Tito 嗎？ 因為她是不是，`UrlAuthorizationModule`繼續進行第二個問題，是 Jisun 集中的每一個人？ 她是，因此，也就無法存取。 最後，如果 Tito 造訪時，第一個問題所造成`UrlAuthorizationModule`是肯定的回應，因此 Tito 被授與存取。

因為`UrlAuthorizationModule`授權的規則是由上而下停止在任何相符項目，而它是一定要有更特定的規則之前不特定的處理程序。 亦即，若要定義禁止 Jisun 和匿名使用者，但允許其他所有已驗證的使用者的授權規則，您會使用最適用的規則-一個影響 Jisun-啟動，然後繼續進行較不特定的規則-允許所有其他的已驗證的使用者，但拒絕所有匿名使用者。 下列的 URL 授權規則實作這項原則，方法是先拒絕 Jisun，，然後再拒絕任何匿名使用者。 Jisun 以外的任何已驗證的使用者會被授與存取因為這兩種情況`<deny>`陳述式會比對。

[!code-xml[Main](user-based-authorization-vb/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>步驟 2： 未經授權的、 已驗證使用者修正工作流程

如同我們稍早在本教學課程中查看的 URL 授權工作流程區段中所討論，隨時瓿未經授權的要求，`UrlAuthorizationModule`中止要求，並傳回 HTTP 401 未經授權的狀態。 修改此 401 狀態`FormsAuthenticationModule`到 302 重新導向至登入頁面將使用者的狀態。 此工作流程就會發生任何未經授權的要求，即使使用者經過驗證。

它們混淆，因為已有登入系統可能會傳回已驗證的使用者登入頁面。 利用最少的工作，協助我們改進此工作流程重新導向對說明他們已嘗試存取受限制的頁面的頁面進行未經授權之要求的已驗證的使用者。

藉由名為 web 應用程式的根資料夾中建立新的 ASP.NET 網頁啟動`UnauthorizedAccess.aspx`; 忘了將使用此頁面`Site.master`主版頁面。 在建立之後此頁面，移除參考的內容控制項`LoginContent`ContentPlaceHolder 以便主版頁面的預設內容將會顯示。 接下來，新增訊息，也就是說明這種情況，使用者嘗試存取受保護的資源。 新增這類訊息後`UnauthorizedAccess.aspx`網頁的宣告式標記看起來應該類似下列：

[!code-aspx[Main](user-based-authorization-vb/samples/sample7.aspx)]

我們現在需要變更工作流程，因此如果未經授權的要求由已驗證的使用者執行他們傳送至`UnauthorizedAccess.aspx`而不是登入頁面的頁面。 將未經授權的要求重新導向至登入頁面的邏輯埋藏內的私用方法`FormsAuthenticationModule`類別中，因此我們不能自訂這個行為。 我們可以做什麼，不過，會將自己的邏輯加入至重新導向至使用者的登入頁面`UnauthorizedAccess.aspx`有需要。

當`FormsAuthenticationModule`未經授權的訪客會將重新導向至登入頁面附加至名稱與查詢字串的要求、 未經授權 URL `ReturnUrl`。 例如，如果未經授權的使用者嘗試瀏覽`OnlyTito.aspx`、`FormsAuthenticationModule`會將他們重新導向至`Login.aspx?ReturnUrl=OnlyTito.aspx`。 因此，如果查詢字串，其中包含與已驗證的使用者登入頁面已達到`ReturnUrl`參數，則我們知道此驗證的使用者剛嘗試瀏覽她未獲授權檢視的頁面。 在這種情況下，我們想要重新導向她`UnauthorizedAccess.aspx`。

若要達成此目的，將下列程式碼加入至登入頁面`Page_Load`事件處理常式：

[!code-vb[Main](user-based-authorization-vb/samples/sample8.vb)]

上述程式碼會經過驗證、 未經授權的使用者重新導向`UnauthorizedAccess.aspx`頁面。 若要查看此邏輯在動作中的，請瀏覽網站，因為匿名的訪客，然後按一下左側資料行中的 [建立使用者帳戶] 連結。 這會帶您到`~/Membership/CreatingUserAccounts.aspx` 頁面，在步驟 1 中我們設定為僅允許 Tito 存取。 由於匿名使用者會禁止，`FormsAuthenticationModule`將我們重新導向回到登入頁面。

此時我們為匿名，因此`Request.IsAuthenticated`傳回`False`和我們沒有重新導向至`UnauthorizedAccess.aspx`。 相反地，登入頁面會顯示。 Tito，例如 Bruce 以外的使用者身分登入。 輸入適當的認證之後, 登入頁面上將重新導向至我們回`~/Membership/CreatingUserAccounts.aspx`。 不過，此頁面只有為 Tito，因為我們未經授權檢視它，會立即傳回至登入頁面。 此時，不過，`Request.IsAuthenticated`傳回`True`(和`ReturnUrl`querystring 參數存在)，因此我們會重新導向至`UnauthorizedAccess.aspx`頁面。


[![驗證過，未經授權的使用者重新導向至 UnauthorizedAccess.aspx](user-based-authorization-vb/_static/image17.png)](user-based-authorization-vb/_static/image16.png)

**圖 6**： 通過驗證，未經授權的使用者重新導向至`UnauthorizedAccess.aspx`([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image18.png))


這個自訂的工作流程會呈現更合理且簡單的使用者體驗最短運算循環圖 2 所示。

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>步驟 3： 限制目前已登入使用者為基礎的功能

URL 授權可讓您輕鬆地指定粗略的授權規則。 如我們所見在步驟 1 中，使用 URL 授權我們可以簡潔狀態允許哪些身分識別，以及哪些拒絕檢視資料夾中的特定頁面或所有頁面。 在某些情況下，不過，我們可能會想要允許所有使用者 頁面上，請瀏覽，但根據使用者造訪網站的網頁的功能限制。

請考慮電子商務網站可允許已驗證的訪客檢閱其產品的大小寫。 當匿名使用者造訪的產品 頁面上時，它們將會看到產品資訊，且不會提供保留檢閱的機會。 不過，瀏覽相同的頁面已驗證的使用者會看到檢閱介面。 如果已驗證的使用者具有尚未檢閱本產品，介面會使其能夠送出評論。否則，它會顯示它們他們之前送出的檢閱。 若要採取這種情況下一個步驟此外，[產品] 頁面可能會顯示其他資訊，提供可用於電子商務公司的這些使用者的擴充的功能。 例如，產品頁面可能會列出庫存清查並包含選項，可編輯產品的價格和描述當造訪員工。

可能是以宣告方式或透過程式設計方式 （或兩者的某種組合），可以實作這類細部授權規則。 下一節中，我們會看到如何實作透過 LoginView 控制項的細部授權。 接下來，我們將探討程式設計技巧。 我們來看看套用微調授權規則之前，不過，我們首先要建立的功能取決於使用者瀏覽它的頁面。

讓我們來建立頁面，其中列出在 GridView 內的特定目錄中的檔案。 GridView 會列出每個檔案的名稱、 大小和其他資訊，以及包含 LinkButtons 的兩個資料行： 一個標題為檢視和一個標題為的 Delete。 如果檢視 LinkButton 已按下，將會顯示所選檔案的內容。如果刪除的 LinkButton 已按下，將會刪除檔案。 讓我們一開始建立這個頁面使其檢視和刪除功能可供所有使用者。 在使用中我們會了解如何啟用或停用這些功能，根據使用者瀏覽頁面 LoginView 控制項和以程式設計的方式限制功能的各節。

> [!NOTE]
> 我們即將建置的 ASP.NET 網頁使用 GridView 控制項顯示的檔案清單。 因為這個教學課程中數列著重於表單驗證、 授權、 使用者帳戶和角色，我不想花費太多討論 GridView 控制項的內部運作的時間。 雖然本教學課程提供特定的此頁面所設定的逐步指示，它不會不深入為什麼進行特定選擇，或轉譯的輸出上有的效果的特定屬性的詳細資料。 GridView 控制項詳盡，請參閱我*[在 ASP.NET 2.0 中使用資料](../../data-access/index.md)*教學課程系列。


先開啟`UserBasedAuthorization.aspx`檔案`Membership`資料夾，然後將 GridView 控制項加入至名為頁面`FilesGrid`。 從 GridView 的智慧標籤，按一下 編輯資料行連結以啟動 欄位 對話方塊。 從這裡，取消核取自動產生欄位核取方塊在左下角。 接著，將選取的按鈕、 刪除 按鈕和兩個 BoundFields 從左上角 （CommandField 類型之下可以找到的 選取和刪除按鈕）。 設定選取的按鈕`SelectText`屬性，以檢視與第一個的 BoundField`HeaderText`和`DataField`屬性名稱。 設定第二個的 BoundField`HeaderText`屬性大小 （位元組），其`DataField`屬性的長度，以其`DataFormatString`{0: n0} 的屬性和其`HtmlEncode`屬性設定為 False。

設定後 GridView 的資料行，按一下 [確定] 關閉 [欄位] 對話方塊。 從 屬性 視窗中，設定 GridView`DataKeyNames`屬性`FullName`。 此時 GridView 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](user-based-authorization-vb/samples/sample9.aspx)]

建立 GridView 的標記，我們就可以撰寫的程式碼會擷取特定的目錄中的檔案，並將其繫結至 GridView。 將下列程式碼加入至網頁的`Page_Load`事件處理常式：

[!code-vb[Main](user-based-authorization-vb/samples/sample10.vb)]

上述程式碼會使用[`DirectoryInfo`類別](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx)取得應用程式的根資料夾中的檔案清單。 [ `GetFiles()`方法](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx)陣列形式傳回目錄中的所有檔案[`FileInfo`物件](https://msdn.microsoft.com/library/system.io.fileinfo.aspx)，然後繫結至 GridView。 `FileInfo`物件具有各種屬性，例如`Name`， `Length`，和`IsReadOnly`，和其他項目。 您可以看到從其宣告的標記，只顯示 GridView`Name`和`Length`屬性。

> [!NOTE]
> `DirectoryInfo`和`FileInfo`類別位於[`System.IO`命名空間](https://msdn.microsoft.com/library/system.io.aspx)。 因此，您必須以其命名空間名稱的這些類別名稱的前面上或匯入類別檔案的命名空間 (透過`Imports System.IO`)。


請花一點時間造訪此頁透過瀏覽器。 它會顯示位於應用程式的根目錄中的檔案清單。 按一下任何檢視或刪除 LinkButtons 將會導致回傳，但會發生任何動作，因為我們尚未以建立必要的事件處理常式。


[![在 GridView 列出 Web 應用程式的根目錄中的檔案](user-based-authorization-vb/_static/image20.png)](user-based-authorization-vb/_static/image19.png)

**圖 7**: GridView 列出 Web 應用程式的根目錄中的檔案 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image21.png))


我們需要方法來顯示所選檔案的內容。 返回 Visual Studio，並加入名為文字方塊`FileContents`GridView 上方。 設定其`TextMode`屬性`MultiLine`及其`Columns`和`Rows`屬性到 95%和 10，分別。

[!code-aspx[Main](user-based-authorization-vb/samples/sample11.aspx)]

接下來，建立事件處理常式的 GridView [ `SelectedIndexChanged`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx)並加入下列程式碼：

[!code-vb[Main](user-based-authorization-vb/samples/sample12.vb)]

此程式碼使用 GridView`SelectedValue`屬性來判斷所選檔案的完整檔案名稱。 就內部而言，`DataKeys`為了取得參考集合`SelectedValue`，因此務必在您設定的 GridView`DataKeyNames`屬性名稱，如稍早在此步驟中所述。 [ `File`類別](https://msdn.microsoft.com/library/system.io.file.aspx)用來讀取選取的檔案內容到字串，然後指派給`FileContents`文字方塊的`Text`屬性，藉以顯示在頁面上選取的檔案的內容。


[![在文字方塊中顯示選取之檔案的內容](user-based-authorization-vb/_static/image23.png)](user-based-authorization-vb/_static/image22.png)

**圖 8**: 選取檔案的內容會顯示在文字方塊中 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image24.png))


> [!NOTE]
> 如果您檢視包含 HTML 標記檔案的內容，然後嘗試檢視或刪除檔案時，您會收到`HttpRequestValidationException`錯誤。 會發生這種情況是因為在回傳文字方塊的內容會傳送至 web 伺服器。 根據預設，ASP.NET 就會引發`HttpRequestValidationException`每當有潛在危險回傳的內容，例如 HTML 標記中，偵測到的錯誤。 若要停用這項錯誤發生，請關閉要求驗證頁面加入`ValidateRequest="false"`至`@Page`指示詞。 如需詳細資訊之優點的要求驗證，以及哪些措施，您應該採取時為停用，讀取[要求驗證-防止指令碼攻擊](https://asp.net/learn/whitepapers/request-validation/)。


最後，加入下列程式碼的事件處理常式的 GridView 的[`RowDeleting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-vb[Main](user-based-authorization-vb/samples/sample13.vb)]

程式碼只會顯示在刪除檔案的完整名稱`FileContents`文字方塊*沒有*實際刪除該檔案。


[![按一下 [刪除] 按鈕並不會實際刪除檔案](user-based-authorization-vb/_static/image26.png)](user-based-authorization-vb/_static/image25.png)

**圖 9**： 按一下 刪除按鈕不會實際刪除檔案 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image27.png))


在步驟 1 中，我們設定 URL 授權規則，來禁止從檢視中的頁面的匿名使用者`Membership`資料夾。 若要進一步展現微調驗證，讓我們來允許匿名使用者造訪`UserBasedAuthorization.aspx` 頁面上，但功能有限。 若要開啟此頁面可供所有使用者存取，加入下列`<location>`項目`Web.config`檔案`Membership`資料夾：

[!code-xml[Main](user-based-authorization-vb/samples/sample14.xml)]

加入這之後`<location>`項目，從站台的記錄，以測試新的 URL 授權規則。 為匿名使用者應該允許造訪`UserBasedAuthorization.aspx`頁面。

目前，任何已驗證或匿名使用者可以瀏覽`UserBasedAuthorization.aspx`頁面上，檢視或刪除檔案。 讓我們來產生它，以便只有驗證的使用者可以檢視檔案的內容並僅 Tito 才能刪除檔案。 可以套用這類細部授權規則，以宣告方式，透過程式設計方式，或這兩種方法的組合。 讓我們來限制誰可以檢視檔案的內容中使用宣告式方法我們會使用以程式設計的方式，才能刪除檔案的限制。

### <a name="using-the-loginview-control"></a>使用 LoginView 控制項

如同我們在過去的教學課程中，LoginView 控制項適用於顯示不同的介面，針對已驗證和匿名使用者，並提供簡單的方式來隱藏不是匿名使用者存取的功能。 匿名使用者無法檢視或刪除檔案，因為我們只需要顯示`FileContents`時已驗證使用者瀏覽頁面時的文字方塊。 若要達成此目的，LoginView 控制項加入網頁中，它`LoginViewForFileContentsTextBox`，並移動`FileContents`LoginView 控制項的文字方塊中的宣告式標記`LoggedInTemplate`。

[!code-aspx[Main](user-based-authorization-vb/samples/sample15.aspx)]

LoginView 範本中的 Web 控制項不再直接從程式碼後置類別存取。 例如， `FilesGrid` GridView 的`SelectedIndexChanged`和`RowDeleting`目前參考的事件處理常式`FileContents`TextBox 控制項的程式碼類似：

[!code-aspx[Main](user-based-authorization-vb/samples/sample16.aspx)]

不過，這段程式碼不再有效。 藉由移動`FileContents`文字方塊到`LoggedInTemplate`文字方塊中無法直接存取。 相反地，我們必須使用`FindControl("controlId")`方法來以程式設計方式參考控制項。 更新`FilesGrid`參考文字方塊中的事件處理常式就像這樣：

[!code-vb[Main](user-based-authorization-vb/samples/sample17.vb)]

將文字方塊中移動到 LoginView 之後`LoggedInTemplate`及文字方塊中使用的參考更新網頁的程式碼`FindControl("controlId")`模式，請瀏覽與匿名使用者在頁面。 如圖 10 所示，`FileContents`則不會顯示文字方塊。 不過，仍會顯示檢視 LinkButton。


[![LoginView 控制項只會呈現 FileContents 文字方塊中，已驗證的使用者](user-based-authorization-vb/_static/image29.png)](user-based-authorization-vb/_static/image28.png)

**圖 10**: LoginView 控制項只會呈現`FileContents`文字方塊中，已驗證的使用者 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image30.png))


將 GridView 欄位轉換為 TemplateField 為其中一種方式隱藏匿名使用者的 [檢視] 按鈕。 這會產生範本，包含檢視 LinkButton 宣告式標記。 我們可以將 LoginView 控制項加入 TemplateField 並放置在 LoginView LinkButton `LoggedInTemplate`，藉此隱藏匿名訪客的 [檢視] 按鈕。 若要完成此動作，按一下編輯資料行中的連結來啟動 [欄位] 對話方塊的 GridView 的智慧標籤。 接下來，從左上角清單中選取 選取 按鈕，然後按一下 轉換為 TemplateField 連結此欄位。 如此一來，將會修改欄位的宣告式標記的來源：

[!code-aspx[Main](user-based-authorization-vb/samples/sample18.aspx)]

 收件者: 

[!code-aspx[Main](user-based-authorization-vb/samples/sample19.aspx)]

 此時，我們可以將 LoginView TemplateField 加入。 下列標記會顯示檢視 LinkButton，僅適用於已驗證使用者。 

[!code-aspx[Main](user-based-authorization-vb/samples/sample20.aspx)]

圖 11 顯示，因為最後的結果不是，很與檢視資料行仍會顯示即使隱藏資料行中檢視 LinkButtons。 下一節中，我們將探討如何隱藏整個 GridView 資料行 （或不只是 LinkButton）。


[![LoginView 控制項隱藏檢視 LinkButtons 匿名訪客](user-based-authorization-vb/_static/image32.png)](user-based-authorization-vb/_static/image31.png)

**圖 11**: LoginView 控制項隱藏檢視 LinkButtons 匿名訪客 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>以程式設計的方式限制功能

在某些情況下，則限制至頁面的功能不足，無法宣告式的技術。 例如，特定頁面功能的可用性可能相依於超出使用者瀏覽頁面是否為匿名或已驗證的準則。 在這種情況下，不同的使用者介面項目可以顯示或隱藏透過程式設計的方式。

若要以程式設計的方式限制功能，我們需要執行兩項工作：

1. 決定瀏覽頁面的使用者是否可以存取這些功能，以及
2. 以程式設計方式修改根據使用者是否擁有存取的功能有問題的使用者介面。

若要示範這兩個工作的應用程式，讓我們僅允許 Tito 從 GridView 刪除檔案。 我們第一項工作，然後是以判斷它是否 Tito 瀏覽頁面。 一旦已決定的我們需要隱藏 （或者顯示） 的 GridView 刪除資料行。 GridView 的資料行是透過可存取其`Columns`屬性; 如果資料行只會呈現其`Visible`屬性設定為`True`（預設值）。

將下列程式碼加入`Page_Load`之前的資料繫結至 GridView 的事件處理常式：

[!code-vb[Main](user-based-authorization-vb/samples/sample21.vb)]

如我們所述[*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-vb.md)教學課程中，`User.Identity.Name`傳回的識別名稱。 這會對應至登入控制項中輸入的使用者名稱。 如果它是 Tito 瀏覽頁面、 GridView 的第二個資料行的`Visible`屬性設定為`True`，否則會設為`False`。 最後結果就是，當有人以外 Tito 瀏覽 頁面上，另一個已驗證的使用者或匿名使用者，刪除資料行則不會呈現 （請參閱圖 12）。不過，當 Tito 造訪的網頁時，刪除資料行存在於 （請參閱圖 13）。


[![刪除資料行是未呈現時瀏覽由有人以外 Tito （例如 Bruce)](user-based-authorization-vb/_static/image35.png)](user-based-authorization-vb/_static/image34.png)

**圖 12**: 刪除的資料行是未呈現時瀏覽由有人以外 Tito （例如 Bruce) ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image36.png))


[![刪除資料行是針對 Tito 呈現](user-based-authorization-vb/_static/image38.png)](user-based-authorization-vb/_static/image37.png)

**圖 13**: Tito 呈現刪除的資料行 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>步驟 4： 將授權規則套用至類別和方法

在步驟 3 中我們會不允許匿名使用者無法檢視檔案的內容，並禁止所有使用者，但 Tito 刪除檔案。 這是藉由隱藏未經授權的訪客透過宣告方式和程式設計技術的相關聯的使用者介面項目以完成。 簡單範例中，適當地隱藏使用者介面項目已直接，但呢更複雜的站台其中可能有許多不同的方式來執行相同的功能嗎？ 限制使用該功能，未經授權的使用者，萬一我們忘記來隱藏或停用所有適用的使用者介面項目怎麼辦？

以確保未經授權的使用者無法存取一項特定功能的簡單方法是該類別或方法具有裝飾[`PrincipalPermission`屬性](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)。 當.NET 執行階段會使用類別或執行其中一個方法時，它會檢查以確認目前的安全性內容有使用該類別或執行方法的權限。 `PrincipalPermission`屬性提供一個機制，我們可以透過定義這些規則。

讓我們將示範如何使用`PrincipalPermission`屬性在 GridView 的`SelectedIndexChanged`和`RowDeleting`分別禁止匿名使用者與 Tito，以外的使用者所執行的事件處理常式。 我們要做的就是將加入每個函式定義在適當的屬性：

[!code-vb[Main](user-based-authorization-vb/samples/sample22.vb)]

屬性`SelectedIndexChanged`事件處理常式指定唯有經過驗證的使用者可以執行事件處理常式上的屬性為`RowDeleting`事件處理常式限制 Tito 執行。

> [!NOTE]
> 屬性可以套用至類別、 方法、 屬性或事件。 當加入屬性，它必須是類別、 方法、 屬性或事件宣告陳述式的一部分。 Visual Basic 使用換行，做為陳述式分隔符號，因為屬性必須出現在同一行的值宣告，或直接上使用行接續字元 （底線）。 在上述程式碼片段中，行接續字元用來將屬性放在一行和方法宣告在另一台。


如果由於某種原因而，Tito 以外的使用者嘗試執行`RowDeleting`事件處理常式或未驗證的使用者嘗試執行`SelectedIndexChanged`.NET 執行階段將會引發的事件處理常式， `SecurityException`。


[![如果安全性內容未獲授權執行方法，會擲回安全性例外狀況](user-based-authorization-vb/_static/image41.png)](user-based-authorization-vb/_static/image40.png)

**圖 14**： 若未經授權的安全性內容執行方法，`SecurityException`就會擲回 ([按一下以檢視完整大小的影像](user-based-authorization-vb/_static/image42.png))


> [!NOTE]
> 若要允許存取類別或方法的多個安全性內容，來裝飾類別或方法有`PrincipalPermission`針對每個安全性內容的屬性。 也就是說，若要讓 Tito 以及 Bruce 執行`RowDeleting`事件處理常式，加入*兩個*`PrincipalPermission`屬性：


[!code-vb[Main](user-based-authorization-vb/samples/sample23.vb)]

除了 ASP.NET 網頁中，許多應用程式也會有架構，其中包括各種層，例如商務邏輯和資料存取層級。 這些圖層通常實作為類別庫，並提供類別和方法來執行商務邏輯與資料相關的功能。 `PrincipalPermission`屬性可用於將授權規則套用至這些層級。

如需有關使用`PrincipalPermission`屬性來定義類別和方法上的授權規則，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格項目[企業和資料層使用新增授權規則`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>總結

在此教學課程中我們討論了如何套用使用者為基礎的授權規則。 我們會查看 ASP 已開始。網路的 URL 授權架構。 每個要求的 ASP.NET 引擎的`UrlAuthorizationModule`會檢查以判斷是否要將身分識別授權存取要求之資源的應用程式的組態中定義的 URL 授權規則。 簡單地說，URL 授權可輕鬆指定特定目錄中的特定頁面或所有頁面的授權規則。

URL 授權架構套用授權規則，在頁面的頁面。 使用 URL 授權的要求識別授權或無法存取特定資源。 許多案例中，不過，需要更多的細部授權規則。 而不是定義誰可以存取頁面，我們可能需要讓每個人存取的網頁，但是若要顯示不同的資料，或提供根據使用者瀏覽頁面的不同功能。 頁面層級的授權通常牽涉到隱藏特定的使用者介面項目，以避免未經授權的使用者存取所禁止的功能。 此外，也會使用屬性來限制存取類別和其方法，針對特定使用者的執行。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [加入商務和資料層級使用的授權規則`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET 授權](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [IIS6 與 IIS7 安全性之間的變更](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [設定特定的檔案和子目錄](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [限制使用者為基礎的資料修改功能](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb.md)
- [LoginView 控制項快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [了解 IIS7 URL 授權](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule`技術文件](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [使用 ASP.NET 2.0 中的資料](../../data-access/index.md)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一頁](validating-user-credentials-against-the-membership-user-store-vb.md)
[下一頁](storing-additional-user-information-vb.md)
