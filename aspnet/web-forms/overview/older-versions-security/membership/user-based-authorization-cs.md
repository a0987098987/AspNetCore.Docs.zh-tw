---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: 使用者為基礎的授權 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中我們將探討限制存取頁面，並限制透過各種技術的頁面層級功能。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ab8fb2782cf9d29c9912fa81158c50a9d8d8af5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396773"
---
<a name="user-based-authorization-c"></a>使用者為基礎的授權 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip)或[下載 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> 在本教學課程中我們將探討限制存取頁面，並限制透過各種技術的頁面層級功能。


## <a name="introduction"></a>簡介

大部分的 web 應用程式提供使用者帳戶執行這項操作來限制特定的訪客存取網站內的特定網頁的組件中。 最線上的留言板站台，比方說，-匿名和已驗證的所有使用者都都能夠檢視的留言板文章，但只有經過驗證的使用者可以瀏覽網頁，以建立新的貼文。 而且可能只是供特定使用者 （或一組特定的使用者） 存取的系統管理頁面。 此外，使用者以使用者為基礎的頁面層級功能都不一樣。 檢視文章的清單，已驗證的使用者會顯示一個介面來評比每個文章中，而此介面不是匿名的訪客可用。

ASP.NET 可讓您輕鬆地定義使用者為基礎的授權規則。 中的標記只需要稍微`Web.config`，特定的網頁或整個目錄可以鎖定，使其只能存取指定的使用者子集。 頁面層級功能可以開啟或關閉根據目前登入的使用者，透過程式設計的方式和宣告式的方式。

在本教學課程中我們將探討限制存取頁面，並限制透過各種技術的頁面層級功能。 讓我們開始吧 ！

## <a name="a-look-at-the-url-authorization-workflow"></a>了解 URL 授權工作流程

中所述[*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程中，當 ASP.NET 執行階段會處理要求的 ASP.NET 資源要求在其生命週期期間引發的事件數目。 *HTTP 模組*會執行其程式碼以回應要求的生命週期中的特定事件的 managed 的類別。 ASP.NET 隨附於多個執行基本工作，在幕後的 HTTP 模組。

這類 HTTP 模組是[ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)。 在上一個教學課程的主要功能中所述`FormsAuthenticationModule`是要判斷目前要求的身分識別。 這是藉由檢查表單驗證票證，位於 cookie 或內嵌在 URL 內完成。 此識別發生期間[`AuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)。

另一個重要的 HTTP 模組[ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)，這會引發以回應[`AuthorizeRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)(之後恰好`AuthenticateRequest`事件)。 `UrlAuthorizationModule`檢查中的組態標記`Web.config`來判斷目前的身分識別是否有權瀏覽指定的頁面。 此程序指*URL 授權*。

我們將檢驗在步驟 1 中的 URL 授權規則的語法但首先讓我們看看什麼`UrlAuthorizationModule`根據要求是否已獲授權或不會。 如果`UrlAuthorizationModule`決定要求已獲得授權，則它不會執行任何動作，以及其整個生命週期的要求會繼續。 不過，如果要求*未*獲授權，則`UrlAuthorizationModule`中止生命週期，並指示`Response`傳回的物件[HTTP 401 未經授權](http://www.checkupdown.com/status/E401.html)狀態。 使用表單驗證時此 HTTP 401 狀態永遠不傳回給用戶端因為如果`FormsAuthenticationModule`偵測到狀態是 HTTP 401 修改它[HTTP 302 重新導向](http://www.checkupdown.com/status/E302.html)登入頁面。

[圖 1] 說明的 ASP.NET 管線中的工作流程`FormsAuthenticationModule`，而`UrlAuthorizationModule`送達的未經授權的要求時。 特別是，[圖 1] 顯示匿名訪客要求`ProtectedPage.aspx`，這是給匿名使用者，拒絕存取的頁面。 造訪者是匿名的因為`UrlAuthorizationModule`中止要求，並傳回 HTTP 401 未經授權的狀態。 `FormsAuthenticationModule`然後 401 狀態轉換為 302 重新導向至登入頁面。 透過登入頁面會驗證使用者之後，他會重新導向至`ProtectedPage.aspx`。 這次`FormsAuthenticationModule`識別讓使用者根據其驗證票證。 既然已經過驗證的訪客，`UrlAuthorizationModule`允許存取該網頁。


[![URL 授權工作流程與表單驗證](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**圖 1**: 表單驗證和 URL 授權工作流程 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image3.png))


圖 1 說明匿名的造訪者嘗試存取不適用於匿名使用者的資源時，就會發生的互動。 在此情況下，會重新導向她嘗試瀏覽於 querystring 中指定頁面的 [登入] 頁面的匿名訪客。 使用者已成功登入之後，她將會自動重新導向回到她一開始嘗試檢視的資源。

由匿名使用者進行未經授權的要求時，此工作流程非常簡單，很容易了解發生了什麼事的訪客和原因。 但請記住`FormsAuthenticationModule`將會重新導向*任何*未經授權的使用者登入 頁面中，即使已驗證的使用者提出要求。 如果已驗證的使用者嘗試瀏覽的頁面，讓她缺少授權單位，這可以導致令人困惑的使用者體驗。

想像我們的網站必須設定其 URL 授權規則，ASP.NET 網頁`OnlyTito.aspx`是順著只 Tito。 現在，想像一下 Sam 瀏覽網站、 登入，並再嘗試瀏覽`OnlyTito.aspx`。 `UrlAuthorizationModule`將會暫止要求的生命週期，並傳回 HTTP 401 未經授權的狀態，其中`FormsAuthenticationModule`會偵測並再將 Sam 重新導向至登入頁面。 由於 Sam 已登入，不過，她可能會想知道為什麼她已傳送到登入頁面。 她可能原因，她登入認證已遺失不知怎麼的或她輸入認證無效。 如果 Sam 成日自己的認證從登入頁面會 （再次） 登入她資料並重新導向至`OnlyTito.aspx`。 `UrlAuthorizationModule`會偵測 Sam 無法瀏覽此頁面，並將她將會傳回登入頁面。

圖 2 說明此令人困惑的工作流程。


[![預設的工作流程可能會導致令人困惑的循環](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**圖 2**: 預設工作流程可能會導致令人困惑的循環 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image6.png))


[圖 2] 所示的工作流程可以快速 befuddle 即使大部分電腦精通訪客。 我們將探討如何防止這種混淆在步驟 2 中的循環。

> [!NOTE]
> ASP.NET 會使用兩種機制來判斷目前使用者是否可以存取特定網頁： URL 授權和檔案授權。 檔案授權由實作[ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx)，以決定授權單位 consulting 要求的檔案的 Acl。 檔案授權通常會用於使用 Windows 驗證，因為設定的 Acl 會套用到 Windows 帳戶的權限。 使用表單驗證時，所有的作業系統和檔案系統層級要求會執行相同的 Windows 帳戶，無論使用者瀏覽的網站。 因為本教學課程系列，著重於表單驗證，我們將不討論檔案授權。


### <a name="the-scope-of-url-authorization"></a>URL 授權範圍

`UrlAuthorizationModule`是 managed 程式碼是在 ASP.NET 執行階段的一部分。 在 microsoft 的第 7 版之前[Internet Information Services (IIS)](https://www.iis.net/) web 伺服器沒有 IIS 的 HTTP 管線及 ASP.NET 執行階段的管線之間的相異屏障。 簡單地說，在 IIS 6 和更早版本，ASP。NET 的`UrlAuthorizationModule`只會要求從 IIS ASP.NET 執行階段委派時執行。 根據預設，IIS 處理像是 HTML 頁面和 CSS、 JavaScript 和影像檔-本身-靜態內容和才遞交給 ASP.NET 執行階段時使用的延伸模組頁面的要求`.aspx`， `.asmx`，或`.ashx`要求。

IIS 7 中，不過，用來整合的 IIS 和 ASP.NET 管線。 有一些組態設定中，您可以設定要叫用的 IIS 7 `UrlAuthorizationModule` for*所有*要求，這表示，可以定義任何類型的檔案的 URL 授權規則。 此外，IIS 7 包含它自己的 URL 授權引擎。 如需有關整合 ASP.NET 和 IIS 7 的原生的 URL 授權功能的詳細資訊，請參閱[了解 IIS7 URL 授權](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)。 如 ASP.NET 和 IIS 7 的整合更深入探討，挑選 Shahram Khosravi 活頁簿，一份*Professional IIS 7 和 ASP.NET 整合式程式設計*(ISBN: 978 0470152539)。

簡單的說，在 IIS 7 之前的版本，URL 授權規則只適用於由 ASP.NET 執行階段的資源。 但是，使用 IIS 7 就可以使用 IIS 的原生的 URL 授權功能或整合的 ASP。NET 的`UrlAuthorizationModule`至 IIS 的 HTTP 管線，藉此擴充這項功能的所有要求。

> [!NOTE]
> 有一些細微但很重要的差異方式 ASP。NET 的`UrlAuthorizationModule`和 IIS 7 處理授權規則的 URL 授權功能。 本教學課程不會檢查 IIS 7 的 URL 授權功能或它如何剖析相較於授權規則的差異`UrlAuthorizationModule`。 如需有關這些主題的詳細資訊，請參閱 MSDN 上或在 IIS 7 文件[www.iis.net](https://www.iis.net/)。


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>步驟 1： 定義中的 URL 授權規則`Web.config`

`UrlAuthorizationModule`決定是否要授與或拒絕存取的應用程式的組態中定義的 URL 授權規則為基礎的特定身分識別要求的資源。 授權規則會在拼[`<authorization>`項目](https://msdn.microsoft.com/library/8d82143t.aspx)的形式`<allow>`和`<deny>`子項目。 每個`<allow>`和`<deny>`子項目可以指定：

- 特定使用者
- 以逗號分隔的使用者清單
- 所有匿名使用者，表示由問號 （？）
- 以星號表示的所有使用者 (\*)

下列標記示範如何使用 URL 授權規則以允許使用者 Tito 和 Scott 並拒絕其他所有人：

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

`<allow>`項目會定義哪些使用者可以-Tito 以及 Scott-雖然`<deny>`元素會指示所*所有*使用者被拒絕。

> [!NOTE]
> `<allow>`和`<deny>`項目也可以指定角色的授權規則。 我們將檢視在未來的教學課程中的角色為基礎的授權。


下列設定會授與 Sam （包括匿名訪客） 以外的任何人的存取權：

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

若要允許已驗證的使用者，請使用下列的設定中，拒絕所有匿名使用者的存取：

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

授權規則定義內`<system.web>`中的項目`Web.config`並套用至所有 web 應用程式中的 ASP.NET 資源。 有時候，應用程式會有不同的授權規則的不同區段。 比方說，在電子商務網站上，所有訪客可能產品閒時仔細研究，請參閱產品評論、 搜尋目錄，等等。 不過，只有已驗證的使用者可能會達到簽出或要管理其中的出貨歷程記錄的頁面。 此外，可能會有網站的僅供選取的使用者，例如站台系統管理員的部分。

ASP.NET 可讓您輕鬆地在網站定義不同的檔案和資料夾的不同的授權規則。 指定根資料夾中的授權規則`Web.config`檔案將套用到站台中的所有 ASP.NET 資源。 不過，這些預設授權設定可以覆寫特定的資料夾加`Web.config`與`<authorization>`一節。

讓我們更新我們的網站，讓只有經過驗證的使用者可以瀏覽中的 ASP.NET 網頁`Membership`資料夾。 若要這麼做我們需要加入`Web.config`檔案`Membership`資料夾並設定它拒絕匿名使用者的授權設定。 以滑鼠右鍵按一下`Membership`資料夾中的在 [方案總管] 中，從操作功能表中，選擇 [加入新項目] 功能表，然後加入新的 Web 組態檔，名為`Web.config`。


[![將 Web.config 檔案新增至 [成員資格] 資料夾](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**圖 3**： 新增`Web.config`的檔案`Membership`資料夾 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image9.png))


此時您的專案應該包含兩個`Web.config`檔案： 一個在目錄的根目錄，一個在`Membership`資料夾。


[![您的應用程式現在應該包含兩個 Web.config 檔案](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**圖 4**： 您的應用程式應該現在包含兩個`Web.config`檔案 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image12.png))


更新組態檔中的`Membership`資料夾，讓它禁止匿名使用者的存取權。

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

這樣就完成了 ！

若要測試這項變更，請瀏覽在瀏覽器首頁，請確定您登出。由於 ASP.NET 應用程式的預設行為是要讓所有的訪客，而且因為我們沒有製作根目錄的任何授權修改`Web.config`檔案中，我們就能夠瀏覽為匿名的訪客的根目錄中的檔案。

按一下左邊的資料行中找到的 [建立使用者帳戶] 連結。 這會帶您前往`~/Membership/CreatingUserAccounts.aspx`。 由於`Web.config`檔案中`Membership`資料夾定義的授權規則，以禁止匿名存取，`UrlAuthorizationModule`中止要求，並傳回 HTTP 401 未經授權的狀態。 `FormsAuthenticationModule`會加以修改以傳送給我們登入頁面 302 重新導向狀態。 請注意，頁面我們已嘗試存取 (`CreatingUserAccounts.aspx`) 傳遞至透過登入頁面`ReturnUrl`querystring 參數。


[![URL 授權規則禁止匿名存取，因為我們會被重新導向至登入頁面](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**圖 5**： 因為 URL 授權規則禁止匿名存取，我們會被重新導向至登入頁面 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image15.png))


在成功登入時，我們會重新導向至`CreatingUserAccounts.aspx`頁面。 這次`UrlAuthorizationModule`允許存取該網頁，因為我們不會再為匿名。

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>套用至特定位置的 URL 授權規則

中定義的授權設定`<system.web>`一節`Web.config`套用至該目錄及其子目錄中的 ASP.NET 資源的所有 (否則覆寫另一個直到`Web.config`檔案)。 不過，在某些情況下，我們可能想要有特定的授權設定，除了一個或兩個特定的頁面指定目錄中的所有 ASP.NET 資源。 這可藉由新增`<location>`中的項目`Web.config`指向不同的授權規則，該檔案，其中定義其唯一的授權規則。

若要說明如何使用`<location>`若要覆寫特定資源的組態設定，讓我們自訂的授權設定，以便只 Tito 可以瀏覽的項目`CreatingUserAccounts.aspx`。 若要達成此目的，新增`<location>`項目`Membership`資料夾的`Web.config`檔案並更新其標記，使它看起來如下所示：

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

`<authorization>`中的項目`<system.web>`定義中的 ASP.NET 資源的預設 URL 授權規則`Membership`資料夾及其子資料夾。 `<location>`項目可讓我們來覆寫這些規則的特定資源。 在上述的標記`<location>`項目參考`CreatingUserAccounts.aspx`頁面上，並指定其授權規則，例如允許 Tito，但拒絕其他所有人。

若要測試這項授權變更，先瀏覽的網站為匿名使用者。 如果您嘗試瀏覽中的任何頁面`Membership`資料夾，例如`UserBasedAuthorization.aspx`，則`UrlAuthorizationModule`將拒絕要求，您將會被重新導向至登入頁面。 為說 Scott，登入之後，您可以造訪中的任何網頁`Membership`資料夾*除了*如`CreatingUserAccounts.aspx`。 嘗試瀏覽`CreatingUserAccounts.aspx`登入的任何人但 Tito 將會導致未經授權的存取嘗試，將您重新導向回到登入頁面。

> [!NOTE]
> `<location>`項目必須出現在組態之外`<system.web>`項目。 您必須使用個別`<location>`針對您想要覆寫其授權設定每個資源的項目。


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>了解如何`UrlAuthorizationModule`使用授權規則授與或拒絕存取

`UrlAuthorizationModule`決定是否要為特定的 URL 授權特定的身分識別，藉由分析 URL 授權規則一次一個，從第一個啟動並開始一路往下。 只要找到相符項目，使用者會授與或拒絕存取，視在找到相符項`<allow>`或`<deny>`項目。 <strong>如果找到相符項目，則使用者會獲得存取。</strong> 因此，如果您想要限制存取，因此務必您使用`<deny>`URL 授權組態中的最後一個元素的項目。 <strong>如果您省略</strong><strong>`<deny>`</strong><strong>項目中，所有使用者會都獲授與存取權。</strong>

若要深入了解所使用的程序`UrlAuthorizationModule`我們稍早在此步驟中討論過的 URL 授權規則時判斷授權單位，請考量的範例。 第一個規則是`<allow>`允許存取 Tito 與 Scott 的項目。 第二個規則是`<deny>`每個人都拒絕存取的項目。 如果匿名使用者造訪，`UrlAuthorizationModule`是匿名的要求，啟動 Scott 或 Tito 嗎？ 答案很明顯地，為 [否]，讓它繼續進行第二個規則。 是匿名的在集中的每個人嗎？ 因為答案以下是 [是]，`<deny>`作用中放入規則，並造訪者會重新導向至登入頁面。 同樣地，如果 Jisun 就瀏覽，`UrlAuthorizationModule`一開始會詢問您，是 Jisun Scott 或 Tito 嗎？ 因為她是不是，`UrlAuthorizationModule`繼續進行第二個問題，是在集中的每個人的 Jisun 嗎？ 她是，因此，也就無法存取。 最後，如果 Tito 造訪時，第一個問題所造成`UrlAuthorizationModule`是確定的回應，因此 Tito 授與存取權。

因為`UrlAuthorizationModule`授權規則從上而下，停止在任何相符項目，就一定要有更特定的規則前面較不特定的處理程序。 亦即，若要定義禁止 Jisun 和匿名使用者，但允許所有其他已驗證的使用者的授權規則，您會開始使用最明確的規則-一個影響 Jisun-，然後繼續進行較不特定的規則-允許所有其他的已驗證的使用者，但拒絕所有匿名使用者。 下列的 URL 授權規則實作此原則的方式，先拒絕 Jisun，，然後再拒絕任何匿名使用者。 Jisun 以外的任何已驗證的使用者會獲授與存取權因為都`<deny>`陳述式會比對。

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>步驟 2： 修正的未經授權、 驗證使用者的工作流程

如我們稍早在本教學課程中查看的 URL 授權工作流程 」 一節中所討論的隨時瓿未經授權的要求，`UrlAuthorizationModule`中止要求，並傳回 HTTP 401 未經授權的狀態。 修改此 401 狀態`FormsAuthenticationModule`到 302 重新導向至登入頁面會將使用者的狀態。 即使使用者經過驗證，此工作流程就會發生任何未經授權的要求。

可能將它們搞混，因為他們已經登入，系統會傳回已驗證的使用者登入頁面。 加上一些工作的我們可以改善此工作流程重新導向到頁面，其中說明他們已嘗試存取受限制的頁面進行未經授權的要求已驗證的使用者。

名為 web 應用程式的根資料夾中建立新的 ASP.NET 網頁著手`UnauthorizedAccess.aspx`; 別忘了將使用此頁面`Site.master`主版頁面。 建立此頁面之後, 移除參考的內容控制項`LoginContent`ContentPlaceHolder 以便主版頁面的預設內容將會顯示。 接下來，新增訊息，說明這種情況，也就是使用者嘗試存取受保護的資源。 新增這類訊息之後`UnauthorizedAccess.aspx`頁面的宣告式標記看起來應該如下所示：

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

我們現在需要變更工作流程，因此，如果已驗證的使用者執行未經授權的要求，則會傳送至`UnauthorizedAccess.aspx`頁面而不是登入頁面。 將未經授權的要求重新導向至登入頁面的邏輯內嵌在私用方法的`FormsAuthenticationModule`類別中，因此我們無法自訂此行為。 我們可以做什麼，不過，若要將使用者重新導向的登入頁面加入我們自己的邏輯是`UnauthorizedAccess.aspx`，如有需要。

當`FormsAuthenticationModule`未經授權的訪客會將重新導向至登入頁面附加至查詢字串名稱的要求、 未經授權的 URL `ReturnUrl`。 例如，如果未經授權的使用者嘗試瀏覽`OnlyTito.aspx`，則`FormsAuthenticationModule`會將他們重新導向至`Login.aspx?ReturnUrl=OnlyTito.aspx`。 因此，如果查詢字串，其中包含與已驗證的使用者登入頁面會觸達`ReturnUrl`參數，則我們知道此未經驗證的使用者只嘗試來瀏覽的網頁，她未獲授權檢視。 在此情況下，我們想要重新導向她`UnauthorizedAccess.aspx`。

若要這麼做，將下列程式碼新增至登入頁面的`Page_Load`事件處理常式：

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

上述程式碼會將已驗證、 未經授權的使用者重新導向`UnauthorizedAccess.aspx`頁面。 若要查看作用中的此邏輯，請瀏覽網站的匿名訪客並按一下左側的資料行中的 [建立使用者帳戶] 連結。 這會帶您前往`~/Membership/CreatingUserAccounts.aspx`頁面，其中在步驟 1 中我們設定為僅允許 Tito 存取。 因為匿名使用者被禁止，`FormsAuthenticationModule`將我們重新導向回到登入頁面。

此時我們是匿名的因此`Request.IsAuthenticated`會傳回`false`與我們未重新導向至`UnauthorizedAccess.aspx`。 相反地，登入頁面隨即出現。 Tito，例如 Bruce 以外的使用者身分登入。 輸入適當的認證之後, 的登入網頁我們回到的重新導向`~/Membership/CreatingUserAccounts.aspx`。 不過，此頁面才可存取 Tito，因為我們未經授權檢視它，並會立即傳回登入頁面。 這次，不過，`Request.IsAuthenticated`會傳回`true`(並`ReturnUrl`querystring 參數存在)，因此我們會重新導向至`UnauthorizedAccess.aspx`頁面。


[![通過驗證，未經授權的使用者重新導向至 UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**圖 6**： 通過驗證，未經授權的使用者重新導向至`UnauthorizedAccess.aspx`([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image18.png))


這個自訂的工作流程會縮短 圖 2 所述的循環，以呈現更合理且簡單的使用者體驗。

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>步驟 3： 限制目前登入使用者為基礎的功能

URL 授權可讓您輕鬆地指定粗略的授權規則。 如我們所見在步驟 1 中，使用 URL 授權我們可以簡潔狀態允許哪些身分識別，以及哪些拒絕檢視資料夾中的特定頁面或所有頁面。 在某些情況下，不過，我們可能會想要允許所有使用者，請瀏覽 頁面上，但限制瀏覽它的使用者為基礎的網頁的功能。

請考慮電子商務網站，可讓已驗證的訪客，若要檢閱其產品的情況。 當匿名使用者造訪的產品 頁面上時，他們會看到的產品資訊，並不會提供機會讓發表評論。 不過，瀏覽相同的頁面已驗證的使用者會看到檢閱的介面。 如果已驗證的使用者有尚未檢閱這項產品，介面會讓它們能夠送出檢閱;否則，它會顯示它們其先前提交檢閱。 若要採取這種情況下步驟此外，[產品] 頁面可能會顯示其他資訊，提供適用於電子商務公司的使用者擴充的功能。 比方說，產品頁面可能會列出庫存清查，並包含編輯產品的價格和描述，當某員工瀏覽的選項。

可以實作這類精細的幅度授權規則，是以宣告方式或透過程式設計方式 （或兩者的某種組合）。 下一節中，我們將了解如何實作透過 LoginView 控制項的微調授權。 接下來，我們將探討程式設計技術。 我們來看看套用微調授權規則之前，不過，我們必須先建立的頁面，其功能取決於使用者瀏覽它。

讓我們建立頁面，其中列出 GridView 內的特定目錄中的檔案。 GridView 會列出每個檔案的名稱、 大小和其他資訊，以及包含 Linkbutton 的兩個資料行： 其中一個標題為檢視和一個標題為的刪除。 如果檢視 LinkButton 已按下，將會顯示所選檔案的內容;如果刪除 LinkButton 已按下，就會刪除檔案。 讓我們一開始建立，此頁面使其檢視和刪除功能可供所有使用者。 < 使用 LoginView 控制項和以程式設計的方式限制功能章節我們將了解如何啟用或停用這些功能，以瀏覽頁面的使用者。

> [!NOTE]
> 我們即將建置的 ASP.NET 網頁會使用 GridView 控制項來顯示檔案清單。 本教學課程中 > 系列聚焦於表單驗證、 授權、 使用者帳戶和角色，因為我不想花太多時間討論 GridView 控制項的內部運作方式。 雖然本教學課程提供特定的逐步指示，設定此頁面，它不會不探討為什麼做特定選擇，或轉譯的輸出上有的效果的特定屬性的詳細資料。 如 GridView 控制項徹底的檢查，請參閱我*[使用 ASP.NET 2.0 中的資料](../../data-access/index.md)* 教學課程系列。


首先開啟`UserBasedAuthorization.aspx`檔案中`Membership`資料夾，然後將 GridView 控制項新增至名為頁面`FilesGrid`。 從 GridView 的智慧標籤上，按一下 編輯資料行連結以啟動 欄位 對話方塊。 從這裡開始，取消核取自動產生欄位核取方塊左下角。 接下來，新增 選取 按鈕、 刪除 按鈕和兩個 BoundFields 從左上角 （CommandField 類型之下可以找到 選取和刪除按鈕）。 設定 [選取] 按鈕`SelectText`屬性，以檢視與第一個的 BoundField`HeaderText`和`DataField`屬性名稱。 設定第二個的 BoundField`HeaderText`屬性，以位元組為單位的大小及其`DataField`屬性的長度，以其`DataFormatString`屬性設{0:N0}及其`HtmlEncode`屬性設定為 False。

設定 GridView 的資料行之後, 按一下 [確定] 關閉 [欄位] 對話方塊。 從 [屬性] 視窗中，設定 GridView`DataKeyNames`屬性設`FullName`。 此時 GridView 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

使用 GridView 的標記建立，我們可以撰寫的程式碼會擷取特定的目錄中的檔案，並將其繫結至 GridView。 將下列程式碼新增至網頁的`Page_Load`事件處理常式：

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

上述程式碼會使用[`DirectoryInfo`類別](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx)取得一份應用程式的根資料夾中的檔案。 [ `GetFiles()`方法](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx)傳回的所有檔案的目錄中，為陣列[`FileInfo`物件](https://msdn.microsoft.com/library/system.io.fileinfo.aspx)，它會接著繫結至 GridView。 `FileInfo`物件具有各種屬性，例如`Name`， `Length`，和`IsReadOnly`，其他項目。 如您所見，從其宣告的標記，只顯示 GridView`Name`和`Length`屬性。

> [!NOTE]
> `DirectoryInfo`並`FileInfo`類別位於[`System.IO`命名空間](https://msdn.microsoft.com/library/system.io.aspx)。 因此，您將其命名空間名稱使用這些類別名稱前加上或匯入的類別檔案的命名空間需要 (透過`using System.IO`)。


請花一點時間瀏覽此頁面，透過瀏覽器。 它會顯示位於應用程式的根目錄中檔案的清單。 按一下 檢視 或 刪除的 Linkbutton 的任何會導致回傳，但會發生任何動作，因為我們至今還建立必要的事件處理常式。


[![GridView 列出 Web 應用程式的根目錄中的檔案](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**圖 7**: GridView 列出 Web 應用程式的根目錄中的檔案 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image21.png))


我們需要一個方法來顯示所選檔案的內容。 返回 Visual Studio，並新增名為 TextBox `FileContents` GridView 上方。 設定其`TextMode`屬性，以`MultiLine`及其`Columns`和`Rows`屬性到 95%和 10，分別。

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

接下來，建立事件處理常式，如 GridView [ `SelectedIndexChanged`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx)並加入下列程式碼：

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

此程式碼使用 GridView 的`SelectedValue`屬性來判斷所選檔案的完整檔案名稱。 就內部而言，`DataKeys`以取得參考集合`SelectedValue`，因此請務必設定 GridView 的`DataKeyNames`屬性名稱，如稍早在此步驟中所述。 [ `File`類別](https://msdn.microsoft.com/library/system.io.file.aspx)用來選取的檔案內容讀取到字串，然後指派給`FileContents`TextBox 的`Text`屬性，藉此顯示的頁面上選取的檔案內容。


[![會出現在文字方塊中選取之檔案的內容](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**圖 8**: 選取檔案的內容會顯示在文字方塊中 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> 如果您檢視檔案，其中包含 HTML 標記的內容，並再嘗試檢視或刪除檔案，您會收到`HttpRequestValidationException`時發生錯誤。 會發生這種情況是因為在回傳文字方塊的內容會傳送至 web 伺服器。 根據預設，ASP.NET 會引發`HttpRequestValidationException`每當有潛在危險的回傳內容，例如 HTML 標記，偵測到的錯誤。 若要停用這項錯誤發生，請關閉要求驗證頁面加上`ValidateRequest="false"`至`@Page`指示詞。 如需詳細資訊，以及以何種預防措施，您應該採取時的要求驗證的 「 優點先停用，讀取[要求驗證-防止指令碼攻擊](https://asp.net/learn/whitepapers/request-validation/)。


最後，新增 GridView 的事件處理常式的下列程式碼[`RowDeleting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

程式碼只會顯示要刪除之檔案的完整名稱`FileContents`TextBox*而不需要*實際刪除該檔案。


[![按一下 [刪除] 按鈕並不會實際刪除檔案](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**圖 9**： 按一下刪除按鈕不會實際刪除的檔案 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image27.png))


在步驟 1 中，我們設定 URL 授權規則，來禁止從檢視中的頁面的匿名使用者`Membership`資料夾。 若要進一步展現微調驗證，讓我們來允許匿名使用者造訪`UserBasedAuthorization.aspx` 頁面上，但功能受限。 若要開啟此頁面的所有使用者存取，新增下列`<location>`項目`Web.config`檔案中`Membership`資料夾：

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

新增這項之後`<location>`項目，從站台的記錄，以測試新的 URL 授權規則。 為匿名使用者應該允許前往`UserBasedAuthorization.aspx`頁面。

目前，任何已驗證或匿名使用者可以瀏覽`UserBasedAuthorization.aspx`頁面並檢視或刪除檔案。 讓它，讓只有經過驗證的使用者可以檢視檔案的內容，並只 Tito 才能刪除檔案。 可以套用這類精細的幅度授權規則，以宣告方式，透過程式設計方式，或這兩種方法的組合。 讓我們來限制誰可以檢視檔案; 的內容中使用宣告式方法我們將使用以程式設計的方式，才能刪除檔案的限制。

### <a name="using-the-loginview-control"></a>使用 LoginView 控制項

因為我們在過去的教學課程中所見，LoginView 控制項適用於顯示不同的介面，針對已驗證和匿名使用者，並提供簡單的方式，來隱藏不是可供匿名使用者存取的功能。 因為匿名使用者無法檢視或刪除檔案，我們只需要顯示`FileContents`文字方塊中，當已驗證的使用者瀏覽頁面時。 若要這麼做，請將 LoginView 控制項加入頁面，將其命名`LoginViewForFileContentsTextBox`，並移動`FileContents`LoginView 控制項的文字方塊中的宣告式標記`LoggedInTemplate`。

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

LoginView 的範本中的 Web 控制項不再直接從程式碼後置類別存取。 比方說， `FilesGrid` GridView 的`SelectedIndexChanged`並`RowDeleting`事件處理常式目前參考`FileContents`喜歡使用程式碼的 TextBox 控制項：

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

不過，此程式碼不再有效。 藉由移動`FileContents`到 TextBox`LoggedInTemplate`文字方塊無法直接存取。 相反地，我們必須使用`FindControl("controlId")`方法來以程式設計方式參考的控制項。 更新`FilesGrid`事件處理常式，以參考文字方塊就像這樣：

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

之後將文字方塊移至 LoginView`LoggedInTemplate`和更新網頁的程式碼參考文字方塊使用`FindControl("controlId")`模式中，瀏覽的頁面為匿名使用者。 如 [圖 10] 所示，`FileContents`則不會顯示文字方塊。 不過，仍會顯示檢視 LinkButton。


[![LoginView 控制項只會呈現 FileContents 文字方塊中，已驗證的使用者](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**圖 10**: LoginView 控制項只會呈現`FileContents`文字方塊中已驗證的使用者 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image30.png))


一種隱藏匿名使用者的 [檢視] 按鈕的方式是將 GridView 欄位轉換為 TemplateField。 這會產生包含檢視 LinkButton 的宣告式標記的範本。 我們可以加入 TemplateField LoginView 控制項並放置在 LoginView LinkButton `LoggedInTemplate`，藉此隱藏之匿名訪客的 [檢視] 按鈕。 若要這麼做，按一下 GridView 的智慧標籤，以啟動 [欄位] 對話方塊中的 [編輯資料行] 連結。 接下來，從左下角中的清單中選取 選取 按鈕，然後按一下 轉換為 TemplateField 連結此欄位。 如此一來，將會修改欄位的宣告式標記的來源：

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 收件者: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

到目前為止，我們可以將 LoginView 加入 TemplateField。 下列標記會顯示檢視 LinkButton 僅適用於已驗證的使用者。

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

如 [圖 11] 所示，最終結果不是，很與檢視資料行仍會顯示即使隱藏的資料行中檢視 Linkbutton。 我們將探討如何隱藏整個的 GridView 資料行 （和不只是 LinkButton） 下, 一節。


[![LoginView 控制項隱藏檢視 Linkbutton 匿名訪客](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**圖 11**: LoginView 控制項隱藏檢視 Linkbutton 匿名訪客 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>以程式設計的方式限制功能

在某些情況下，宣告式技巧會不足，無法限制至頁面的功能。 例如，某些頁面功能的可用性可能相依於超出使用者瀏覽頁面是否為匿名或已驗證的準則。 在此情況下，各種使用者介面項目可以顯示或隱藏透過程式設計的方式。

若要以程式設計的方式限制功能，我們必須執行兩項工作：

1. 判斷瀏覽頁面的使用者是否可以存取這些功能，以及
2. 以程式設計方式修改根據使用者是否能夠存取的功能有問題的使用者介面。

為了示範這兩項工作的應用程式，讓我們只允許 Tito GridView 從刪除檔案。 然後，我們第一項工作，是判斷它是否為 Tito 瀏覽的頁面。 之後，已判斷出，我們要隱藏 （或顯示） GridView 的刪除資料行。 GridView 資料行是可透過存取其`Columns`屬性; 如果資料行只會呈現其`Visible`屬性設定為`true`（預設值）。

將下列程式碼加入`Page_Load`之前將資料繫結至 GridView 的事件處理常式：

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

如我們所述[*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程中，`User.Identity.Name`傳回身分識別的名稱。 這會對應至登入控制項中輸入的使用者名稱。 如果它是 Tito 瀏覽網頁，GridView 的第二個資料行的`Visible`屬性設定為`true`; 否則設定為`false`。 最後結果就是，當 Tito 以外的其他人造訪的頁面上，另一個已驗證的使用者或匿名使用者，刪除資料行則不會呈現 （請參閱 圖 12）;不過，當 Tito 造訪網頁時，刪除資料行已存在 （請參閱 圖 13）。


[![刪除資料行是不呈現時瀏覽過的人以外 Tito （例如 Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**圖 12**: 刪除的資料行是不呈現時瀏覽過的人以外 Tito （例如 Bruce) ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image36.png))


[![刪除資料行呈現 Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**圖 13**: 刪除的資料行呈現 Tito ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>步驟 4： 將授權規則套用至類別和方法

在步驟 3 中我們不允許匿名使用者無法檢視檔案的內容，並禁止所有使用者，但 Tito 刪除檔案。 這是藉由隱藏相關聯的使用者介面項目給未經授權的訪客，透過宣告式和程式設計技術來達成。 簡單範例中，適當地隱藏使用者介面項目已直接了當，但什麼是更複雜的網站可能是許多不同的方式來執行相同的功能嗎？ 限制使用該功能以未經授權的使用者，萬一我們忘了隱藏或停用所有適用的使用者介面項目怎麼辦？

簡單的方法，以確保未經授權的使用者無法存取特定的一種功能是該類別或方法的裝飾[`PrincipalPermission`屬性](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)。 當.NET 執行階段會使用的類別，或執行其中一個方法時，它會檢查以確保目前的安全性內容有使用該類別或執行方法的權限。 `PrincipalPermission`屬性提供一個機制，讓我們可以定義這些規則。

讓我們將示範如何使用`PrincipalPermission`屬性上的 GridView`SelectedIndexChanged`和`RowDeleting`分別禁止匿名使用者和 Tito，以外的使用者所執行的事件處理常式。 我們要做的只是新增在每個函式定義適當的屬性：

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

屬性`SelectedIndexChanged`事件處理常式指定只有經過驗證的使用者可以執行的事件處理常式中，在做為屬性`RowDeleting`事件處理常式會限制 Tito 執行。

如果以某種方式，Tito 以外的使用者嘗試執行`RowDeleting`事件處理常式或未驗證的使用者嘗試執行`SelectedIndexChanged`.NET 執行階段將會引發的事件處理常式， `SecurityException`。


[![如果安全性內容未獲授權執行方法，會擲回安全性例外狀況](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**圖 14**： 如果安全性內容無權執行此方法中，`SecurityException`就會擲回 ([按一下以檢視完整大小的影像](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> 若要允許存取類別或方法的多個安全性內容，裝飾的類別或方法具有`PrincipalPermission`針對每個安全性內容的屬性。 也就是說，若要讓 Tito 以及 Bruce 執行`RowDeleting`事件處理常式，加入*兩個*`PrincipalPermission`屬性：


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

除了 ASP.NET 頁面中，許多應用程式還包含各種層級，例如商務邏輯和資料存取層的架構。 這些層級通常會實作為類別庫，並提供類別和方法來執行商務邏輯與資料相關的功能。 `PrincipalPermission`屬性可用於將授權規則套用至這些層級。

如需有關使用`PrincipalPermission`屬性來定義類別和方法的授權規則，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格項目[的商務和資料層使用新增授權規則`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>總結

在本教學課程中，我們討論過如何套用使用者為基礎的授權規則。 我們開始著手了解 ASP。NET 的 URL 授權架構。 每個要求，ASP.NET 引擎的`UrlAuthorizationModule`會檢查以判斷身分識別是否獲得授權存取所要求的資源的應用程式的組態中定義的 URL 授權規則。 簡單地說，URL 授權讓您輕鬆地指定特定目錄中的特定頁面或所有頁面的授權規則。

URL 授權架構會套用授權規則 頁面的頁面為基礎。 使用 URL 授權時，已獲授權存取特定資源，或不的要求識別。 許多案例中，不過，呼叫多個精細的幅度授權規則。 而不是定義誰可以存取的頁面，我們可能需要讓每個人存取的頁面，但會顯示不同的資料，或提供不同的功能，根據使用者瀏覽的頁面。 頁面層級的授權通常牽涉到隱藏特定的使用者介面項目，以防止未經授權的使用者存取禁止的功能。 此外，就可以使用屬性來限制存取類別和其方法，針對特定使用者執行。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [將授權規則新增至商務和資料層使用 `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET 授權](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [IIS6 和 IIS7 安全性之間的變更](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [設定特定的檔案和子目錄](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [根據使用者限制資料修改功能](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [LoginView 控制項快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [了解 IIS7 URL 授權](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` 技術文件](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [使用 ASP.NET 2.0 中的資料](../../data-access/index.md)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [上一頁](validating-user-credentials-against-the-membership-user-store-cs.md)
> [下一頁](storing-additional-user-information-cs.md)
