---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: 驗證使用者認證，針對成員資格使用者存放區 (C#) |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們將檢查如何驗證使用者的認證，使用程式設計的方式和登入控制項的成員資格使用者存放區...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: 484a0f16265ee2d887ee08f6ae7ada47047f1f04
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "30891802"
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>驗證使用者認證，針對成員資格使用者存放區 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip)或[下載 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> 在本教學課程中，我們將檢查如何驗證使用者的認證，使用程式設計的方式和登入控制項的成員資格使用者存放區。 我們也將探討如何自訂登入控制項的外觀和行為。


## <a name="introduction"></a>簡介

在<a id="Tutorial05"> </a>[前述教學課程](creating-user-accounts-cs.md)我們探討了如何在成員資格架構中建立新的使用者帳戶。 我們先看過以程式設計方式建立使用者帳戶透過`Membership`類別的`CreateUser`方法，然後使用並檢查適用於 CreateUserWizard Web 控制項。 不過，目前登入頁面會驗證針對使用者名稱和密碼組的硬式編碼清單所提供的認證。 我們需要更新登入網頁的邏輯，使它會驗證成員資格架構的使用者存放區的認證。

更像是建立使用者帳戶，可以驗證認證以程式設計方式或以宣告方式。 成員資格應用程式開發介面包含以程式設計方式驗證使用者的認證，針對使用者存放區的方法。 和 ASP.NET 隨附登入 Web 控制項，其會呈現使用者介面的使用者名稱和密碼來登入按鈕的文字方塊。

在本教學課程中，我們將檢查如何驗證使用者的認證，使用程式設計的方式和登入控制項的成員資格使用者存放區。 我們也將探討如何自訂登入控制項的外觀和行為。 讓我們開始吧 ！

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>步驟 1： 驗證成員資格使用者存放區的認證，

使用表單驗證的網站，使用者登入網站瀏覽登入頁面，並輸入其認證。 這些認證，然後會針對使用者存放區進行比較。 如果有效，然後授與使用者表單驗證票證，即表示身分識別和真實性的造訪者的安全性權杖。

若要驗證使用者，以針對成員資格 framework，請使用`Membership`類別的[`ValidateUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)。 `ValidateUser`方法會採用兩個輸入參數-在*`username`* 和*`password`* -並傳回布林值，指出是否已有效認證。 例如`CreateUser`方法我們在上一個教學課程中，檢查`ValidateUser`方法會委派實際驗證設定的成員資格提供者。

`SqlMembershipProvider`取得指定之使用者的密碼，透過驗證提供的認證`aspnet_Membership_GetPasswordWithFormat`預存程序。 請記得，`SqlMembershipProvider`儲存使用者的密碼使用三種格式之一： 清除、 加密，或雜湊。 `aspnet_Membership_GetPasswordWithFormat`預存程序傳回其原始格式的密碼。 加密或雜湊密碼`SqlMembershipProvider`轉換*`password`* 傳入值`ValidateUser`到它的對等的方法加密或雜湊狀態，並再比較它與從傳回什麼資料庫。 如果儲存在資料庫中的密碼符合格式化的使用者所輸入的密碼，是有效的認證。

讓我們更新我們的登入頁面 (~ /`Login.aspx`)，讓它會驗證成員資格 framework 使用者存放區提供的認證。 我們建立此登入頁面中<a id="Tutorial02"> </a> [*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程中，兩個文字方塊的使用者名稱和密碼，以建立介面記住我核取方塊，並登入按鈕 （請參閱圖 1）。 程式碼會驗證輸入的認證，硬式編碼的使用者名稱和密碼組清單 （Scott/密碼、 Jisun/密碼，與 Sam/密碼）。 在<a id="Tutorial03"> </a> [*表單驗證設定和進階主題*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)教學課程中我們已更新儲存在表單的其他資訊的登入網頁的程式碼驗證票證`UserData`屬性。


[![登入頁面的介面包含兩個文字方塊、 CheckBoxList 和一個按鈕](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**圖 1**: 登入頁面的介面包含兩個文字方塊、 CheckBoxList 和一個按鈕 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


登入頁面使用者介面可以維持不變，但我們需要將登入按鈕`Click`事件處理常式和程式碼，驗證使用者對成員資格 framework 使用者存放區。 更新事件處理常式，使其程式碼會出現，如下所示：

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

此程式碼是非常簡單。 我們一開始呼叫`Membership.ValidateUser`方法，傳入提供的使用者名稱和密碼。 如果該方法會傳回 true，則在使用者登入透過網站`FormsAuthentication`類別的 RedirectFromLoginPage 方法。 (如我們所述<a id="Tutorial2"> </a> [*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程中，`FormsAuthentication.RedirectFromLoginPage`建立表單驗證票證，並將使用者重新導向以適當的頁面。）如果您的認證不正確的不過，`InvalidCredentialsMessage`標籤隨即顯示，通知使用者他們的使用者名稱或密碼不正確。

這就是這麼簡單 ！

若要測試登入頁面會在如預期般運作，請嘗試使用您在前述教學課程中建立的使用者帳戶之一登入。 或者，如果您尚未建立帳戶，請繼續建立一個從`~/Membership/CreatingUserAccounts.aspx`頁面。

> [!NOTE]
> 當使用者輸入其認證，並將登入頁面表單送出時，到 web 伺服器在網際網路上傳輸的認證，包括其密碼，*純文字*。 這表示使用者名稱和密碼，可以看到任何駭客探查網路流量。 若要避免這個問題，務必要使用加密的網路流量[安全通訊端層 (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)。 這可確保加密認證 （以及整個網頁的 HTML 標記），直到收到 web 伺服器離開瀏覽器的時間。


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>成員資格 Framework 如何處理無效的登入嘗試次數

當訪客到達登入頁面，並將其認證提交時，則其瀏覽器進行登入頁面 HTTP 要求。 如果認證有效，HTTP 回應會包含在 cookie 中的驗證票證。 因此，駭客嘗試入侵您的站台無法建立徹底將 HTTP 要求傳送到具有有效的使用者名稱和密碼在猜測的登入頁面的程式。 如果密碼猜測是否正確，登入頁面將會傳回驗證票證 cookie，此時程式知道它是有效的使用者名稱/密碼組有誤。 透過暴力，特別是如果密碼是弱式可能可以遇到使用者的密碼，這類程式。

若要防止暴力攻擊，成員資格 framework 鎖定使用者是否有一段時間內失敗的登入嘗試數目。 精確參數是可透過下列兩個成員資格提供者組態設定：

- `maxInvalidPasswordAttempts` -指定多少的無效密碼的使用者帳戶遭到鎖定之前的時間週期內，便允許嘗試。預設值為 5。
- `passwordAttemptWindow` -表示以分鐘為單位的期間指定的無效的登入嘗試次數將導致帳戶遭到鎖定的時間週期。預設值為 10。

如果使用者已經鎖定時，她無法登入，直到系統管理員解除鎖定其帳戶。 當使用者遭到鎖定時，`ValidateUser`方法將*一律*傳回`false`，即使提供有效的認證。 雖然這種行為會減少，駭客將至您的網站透過暴力方法中斷的可能性，它可以得到鎖定的有效使用者只需忘記其密碼或不小心具有 Caps Lock 或屬於具有不正確的輸入日期。

很遺憾，目前沒有內建的工具，以解除鎖定使用者帳戶。 若要解除鎖定帳戶，您可以修改資料庫直接-變更`IsLockedOut`欄位`aspnet_Membership`適用於適當的使用者帳戶的資料表，或建立 web 型介面，其中列出的選項來解除鎖定其帳戶被鎖定。 我們將檢驗完成一般使用者帳戶和角色相關的工作，在未來的教學課程中建立系統管理介面。

> [!NOTE]
> 一個缺點是`ValidateUser`方法是提供的認證無效時，其並不提供任何說明有關為何。 憑證可能無效，因為使用者存放區中沒有任何相符的使用者名稱/密碼組或因為尚未核准的使用者，或使用者已被鎖定。步驟 4 中，我們會看到如何對使用者顯示更詳細的訊息，其登入嘗試失敗時。


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>步驟 2： 收集認證透過登入 Web 控制項

[登入 Web 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)呈現預設使用者介面非常類似於我們在建立<a id="SKM5"> </a> [*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程。 使用登入控制項，我們將工作儲存不必建立介面，以收集訪客的認證。 此外，登入控制項自動登入使用者 （假設已提交的認證有效），藉此儲存我們不需要撰寫任何程式碼。

讓我們更新`Login.aspx`、 取代手動建立的介面和程式碼與登入控制項。 開始移除現有的標記和程式碼中`Login.aspx`。 您可能會刪除徹底，或只需加以註解化。若要宣告式標記為註解，把它與`<%--`和`--%>`分隔符號。 您可以手動輸入這些分隔符號，或如圖 2 所示，您可以選取要標記為註解，然後按一下 [標記為註解選取的行中的圖示] 工具列的文字。 同樣地，您可以使用註解選取的行圖示標記為註解程式碼後置類別中選取的程式碼。


[![註解的現有的宣告式標記和程式碼置於 Login.aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**圖 2**： 註解出現有宣告式標記和程式碼置於`Login.aspx`([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> 在 Visual Studio 2005 中檢視的宣告式標記時，不使用註解選取的行圖示。 如果您不想要使用 Visual Studio 2008 您必須手動新增`<%--`和`--%>`分隔符號。


接下來，將登入控制項從 [工具箱] 頁面上，並設定其`ID`屬性`myLogin`。 此時您的畫面看起來應該類似於圖 3。 核取方塊，並登入 按鈕下, 一次登入控制項的預設介面，包含文字方塊控制項的使用者名稱和密碼，請記住我的附註。 另外還有`RequiredFieldValidator`的兩個文字方塊控制項。


[![將登入控制項加入頁面](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**圖 3**: 登入控制項加入至網頁 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


就大功告成 ！ 按一下 [登入控制項的登入] 按鈕時，會發生回傳，且登入控制項將會呼叫`Membership.ValidateUser`方法，傳入輸入使用者名稱和密碼。 如果是無效的認證，登入控制項就會顯示訊息，說明如下。 不過，認證是否有效，如果登入控制項建立表單驗證票證，並將使用者重新導向至適當的頁面。

登入控制項使用四個因素，來決定適當的頁面，即可重新導向使用者成功登入：

- 登入控制項是否登入頁面上所定義的`loginUrl`表單驗證設定; 中設定此設定的預設值是 `Login.aspx`
- 與否`ReturnUrl`querystring 參數
- 登入控制項的值[`DestinationUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl`指定在表單驗證組態設定的值，此設定的預設值是 `Default.aspx`

圖 4 說明如何登入控制項使用這四個參數來到達其適當的頁面的決策。


[![將登入控制項加入頁面](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**圖 4**: 登入控制項加入至網頁 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


請花一點時間瀏覽的網站，透過瀏覽器，並以成員資格架構中的現有使用者登入測試輸出登入控制。

登入控制項的轉譯的介面是可高度設定。 有許多屬性會影響它的外觀。不僅如此，登入控制項可以轉換成範本，以精確控制配置的使用者介面項目。 此步驟中的其餘部分會檢查自訂外觀和版面配置方式。

### <a name="customizing-the-login-controls-appearance"></a>自訂登入控制項的外觀

登入控制項的預設屬性值呈現使用者介面 （記錄檔中） 的標題、 使用者名稱和密碼輸入，記住我的文字方塊和標籤控制項下, 一次的核取方塊，並登入 按鈕。 所有可透過登入控制項的多個屬性來設定這些項目的外觀。 此外，可以加入額外的使用者介面項目-連結的頁面，即可建立新的使用者帳戶-例如，藉由設定屬性或兩個。

讓我們花幾分鐘的時間 gussy 總我們登入控制項的外觀。 因為`Login.aspx`頁面已經有指出登入頁面頂端的文字，是多餘的登入控制項的標題。 因此，以清除[`TitleText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx)值，以便移除登入控制項的標題。

使用者名稱： 和密碼： 左邊的兩個文字方塊控制項的標籤可以透過自訂[ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx)和[`PasswordLabelText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)分別。 讓我們來變更使用者名稱： 讀取使用者名稱的標籤:。 標籤和文字方塊的樣式，則為可透過設定[ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx)和[`TextBoxStyle`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)分別。

記住我可以透過登入控制項的設定下一個時間核取方塊的文字屬性[ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)，且其預設檢查狀態都可透過設定[ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) （其中預設為 False）。 請繼續進行，並設定`RememberMeSet`依預設會核取屬性設為 True，讓 記住我下一次核取方塊。

登入控制項提供了兩個屬性，以便調整其使用者介面控制項的配置。 [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx)指出是否將使用者名稱： 和密碼： 標籤會出現在左側的其對應的文字方塊 （預設值），或其上方。 [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx)表示是否垂直定位的使用者名稱和密碼的輸入 （一個在彼此上方） 或水平方向。 我要保留其預設值，這兩個屬性，但建議您將再試一次將這兩個屬性設定為非預設值以查看產生的效果。

> [!NOTE]
> 在下一步 區段中，設定登入控制項的版面配置中，我們將探討使用範本定義確切的版面配置控制項的使用者介面項目配置。


藉由設定登入控制項的屬性設定包裝[ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx)和[`CreateUserUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx)成 Not 尚未登錄嗎？ 建立帳戶 ！ 和`~/Membership/CreatingUserAccounts.aspx`分別。 這會將超連結加入登入控制項的介面指向頁面中建立<a id="SKM6"> </a>[前述教學課程](creating-user-accounts-cs.md)。 登入控制項[ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx)和[`HelpPageUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx)和[ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx)和[ `PasswordRecoveryUrl` 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx)同樣的方式，呈現一份說明網頁和密碼復原頁面的連結中運作。

這些屬性變更後，登入控制項的宣告式標記和外觀看起來應該類似於圖 5 所示。


[![登入控制項的屬性值會指示其外觀](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**圖 5**: 登入控制項的屬性值指示其外觀 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>設定登入控制項的版面配置

登入 Web 控制項的預設使用者介面會配置在 HTML 介面`<table>`。 但我們需要更細微地控制轉譯的輸出該怎麼辦？ 我們想要取代的或許`<table>`含有一系列`<div>`標記。 或者，如果我們的應用程式會要求額外的認證進行驗證？ 許多財務網站中，例如，要求使用者提供不只一個使用者名稱及密碼，但也個人識別碼 (PIN) 或其他識別資訊。 無論原因可能是，便可將登入控制項轉換成範本，讓我們可以明確定義的介面宣告式標記。

我們需要如何做兩件事，才能更新登入控制，以收集其他的認證：

1. 更新包含網頁的控制項來收集其他的認證登入控制項的介面。
2. 覆寫登入控制項的內部驗證邏輯，使其使用者名稱和密碼有效，且其額外的認證有效，太只驗證使用者。

若要完成的第一個工作，我們需要將登入控制項轉換成範本，並新增必要的 Web 控制項。 可以藉由建立控制項的事件處理常式取代與第二項工作，登入控制項的驗證邏輯[`Authenticate`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)。

提示使用者輸入其使用者名稱、 密碼及電子郵件地址，使其只會驗證使用者，如果提供電子郵件地址符合檔案上的電子郵件地址，讓我們先更新登入控制項。 首先，我們需要將登入控制項的介面轉換為範本。 從登入控制項的智慧標籤上，選擇 [轉換成範本] 選項。


[![將登入控制項轉換為範本](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**圖 6**： 將登入控制項轉換為範本 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> 若要還原為其預先 template 版本登入控制項，在按一下重設連結控制項的智慧標籤。


將登入控制項轉換為範本加入`LayoutTemplate`至 HTML 項目和定義使用者介面的 Web 控制項與控制項的宣告式標記。 如圖 7 所示，將控制項轉換為範本中移除一些屬性從 [屬性] 視窗中，例如`TitleText`， `CreateUserUrl`，依此類推，因為使用範本時，會忽略這些屬性值。


[![較少的屬性都可用時登入控制項轉換為範本](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**圖 7**： 較少的屬性都可用時登入控制項轉換為範本 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


中的 HTML 標記`LayoutTemplate`可能會依需要修改。 同樣地，您將任何新的 Web 控制項加入至範本。 不過，很重要，該登入控制項的核心 Web 控制項範本中保留，並保留其指派`ID`值。 特別是，請勿移除或重新命名`UserName`或`Password`文字方塊`RememberMe`核取方塊， `LoginButton`  按鈕，`FailureText`標籤，或`RequiredFieldValidator`控制項。

若要收集的造訪者的電子郵件地址，我們需要將文字方塊加入至範本。 加入下列宣告式標記資料表的資料列之間 (`<tr>`)，其中包含`Password`文字方塊中，保留 記住我接下來的資料表資料列時間核取方塊：

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

在新增之後`Email`文字方塊中，瀏覽的頁面，透過瀏覽器。 如圖 8 所示，登入控制項的使用者介面現在包含第三個文字方塊。


[![登入控制項現在包含文字方塊中，使用者的電子郵件地址](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**圖 8**： 登入控制項現在包含文字方塊中，使用者的電子郵件地址 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


此時，登入控制項仍在使用`Membership.ValidateUser`方法以驗證提供的認證。 相對地，值輸入`Email`文字方塊具有使用者是否可以登入上的並無任何影響。 在步驟 3 中我們將探討如何覆寫登入控制項的驗證邏輯，使認證才會被視為有效的使用者名稱和密碼有效，而且提供的電子郵件地址符合檔案上的電子郵件地址。

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>步驟 3： 修改登入控制項的驗證邏輯

當訪客提供其認證並點選 [登入] 按鈕，回傳展示和登入控制其驗證工作流程進展。 藉由引發啟動工作流程[`LoggingIn`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx)。 此事件相關聯的任何事件處理常式可能會取消作業中的記錄檔，藉由設定`e.Cancel`屬性`true`。

如果在作業記錄不會取消，工作流程進展藉由引發[`Authenticate`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)。 如果沒有的事件處理常式`Authenticate`事件，然後它會負責決定所提供的認證是否為有效。 如果未不指定任何事件處理常式，則會使用登入控制項`Membership.ValidateUser`方法，以判斷憑證的有效性。

如果提供的認證有效，則表單驗證票證建立時， [ `LoggedIn`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx)引發時，使用者會重新導向到適當的頁面。 如果，不過，認證會被視為無效，則[`LoginError`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx)，就會引發，則會顯示訊息通知使用者他們的認證不正確。 根據預設，在失敗登入控制項只會設定其`FailureText`標籤控制項文字屬性 （登入嘗試不成功失敗訊息。 請再試一次）。 不過，如果登入控制項[`FailureAction`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx)設`RedirectToLoginPage`，然後登入控制問題`Response.Redirect`附加 querystring 參數將登入頁面`loginfailure=1`（這會導致登入控制項以顯示失敗的訊息）。

圖 9 提供的驗證工作流程的流程圖。


[![登入控制項的驗證工作流程](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**圖 9**: 登入控制項的驗證工作流程 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> 如果您想知道使用時機`FailureAction`的`RedirectToLogin`選項頁面上，請考慮下列案例。 現在我們`Site.master`主版頁面目前顯示的文字，Hello，陌生人左側的資料行，當瀏覽由匿名使用者，但假設我們想要以登入控制項取代該文字。 這樣可讓匿名使用者登入的任何頁面上的站台，而不需直接瀏覽登入頁面。 不過，如果使用者無法透過主版頁面所呈現的登入控制項登入，可能會更有意義將它們重新導向至登入頁面 (`Login.aspx`) 因為該頁面可能包含其他指示、 連結和其他說明-例如，若要建立的連結新的帳戶或擷取遺失的密碼-不是新增至主版頁面。


### <a name="creating-theauthenticateevent-handler"></a>建立`Authenticate`事件處理常式

若要插入在我們的自訂驗證邏輯時，我們要建立登入控制項的事件處理常式`Authenticate`事件。 建立事件處理常式`Authenticate`事件將會產生下列事件處理常式定義：

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

如您所見，`Authenticate`事件處理常式會傳遞類型的物件[ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx)做為其第二個輸入參數。 `AuthenticateEventArgs`類別包含名為的布林值屬性`Authenticated`用來指定所提供的認證是否有效。 我們的工作，則以撰寫程式碼會判斷所提供的認證是否有效，並設定`e.Authenticate`屬性據此。

### <a name="determining-and-validating-the-supplied-credentials"></a>判斷及驗證提供的認證

使用登入控制項的[ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx)和[`Password`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx)來判斷使用者所輸入的使用者名稱和密碼認證。 若要判斷任何額外的 Web 控制項中輸入的值 (例如`Email`文字方塊中，我們加入上一個步驟中)，使用*`LoginControlID`* `.FindControl`("*`controlID`*") 來取得程式設計參考範本中的 Web 控制項的`ID`屬性等於*`controlID`*。 例如，若要取得的參考`Email`文字方塊中，使用下列程式碼：

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

若要驗證使用者的認證，我們需要如何做兩件事：

1. 請確定提供的使用者名稱和密碼有效
2. 請確認輸入的電子郵件地址相符使用者嘗試登入的檔案上的電子郵件地址

若要完成第一次檢查，我們可以直接使用`Membership.ValidateUser`像我們了解在步驟 1 中的方法。 第二個核取，我們要判斷使用者的電子郵件地址，因此，我們可以將它比較所輸入的文字方塊控制項的電子郵件地址。 若要取得特定使用者的相關資訊，請使用`Membership`類別的[`GetUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)。

`GetUser`方法有數個多載。 如果使用此選項，而未傳入任何參數，它會傳回目前登入使用者的相關資訊。 若要取得特定使用者的相關資訊，請呼叫`GetUser`傳入其使用者名稱。 無論如何，`GetUser`傳回[`MembershipUser`物件](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)，其中包含屬性，例如`UserName`， `Email`， `IsApproved`， `IsOnline`，依此類推。

下列程式碼會實作這些檢查兩個項目。 如果兩者都通過，然後`e.Authenticate`設`true`，否則它會指派`false`。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

此位置的程式碼，嘗試登入為有效的使用者，並輸入正確的使用者名稱、 密碼及電子郵件地址。 再試一次，但這次故意使用不正確的電子郵件地址 （請參閱圖 10）。 最後，再試一次它使用不存在使用者名稱的第三次。 在第一種情況下您應該已成功登入至站台，但在最後兩個情況下您應該會看到登入控制項的認證無效訊息。


[![提供不正確的電子郵件地址時，將無法登入的 Tito](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**圖 10**: Tito 無法記錄檔中時提供不正確的電子郵件地址 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> 在步驟 1 中，如何成員資格架構會處理無效的登入嘗試 > 一節中所述當`Membership.ValidateUser`方法呼叫並傳遞無效的認證，它會追蹤無效的登入嘗試並鎖定超出特定的使用者不正確的嘗試在指定的時間間隔內的臨界值。 因為我們的自訂驗證邏輯呼叫`ValidateUser`方法，不正確的密碼為有效的使用者名稱會遞增無效的登入嘗試計數器，但此計數器不會遞增，在案例中，使用者名稱和密碼不正確，但電子郵件地址不正確。 關鍵就在於，此行為適合，因為它是不太可能，駭客會知道的使用者名稱和密碼，但必須使用暴力技術來判斷使用者的電子郵件地址。


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>步驟 4： 改善登入控制項的認證不正確的訊息

當使用者嘗試使用無效的認證登入時，登入控制項就會顯示訊息，說明的登入嘗試不成功。 特別是，控制項會顯示由指定的訊息其[`FailureText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx)，其具有預設值是您登入嘗試不成功。 請重試。

前文提過，有許多使用者的認證可能無效的原因：

- 使用者名稱可能不存在
- 使用者名稱存在，但是密碼不正確
- 使用者名稱和密碼不正確，但尚未核准的使用者
- 使用者名稱和密碼不正確，但使用者已鎖定超出 （很可能是因為它們超過指定時間範圍內無效的登入嘗試次數）

和可能有其他原因時使用自訂驗證邏輯。 例如，我們已寫入以程式碼在步驟 3 中，使用者名稱和密碼可能有效，但電子郵件地址可能不正確。

無論原因是無效的認證，登入控制項顯示相同的錯誤訊息。 這種欠缺的意見反應反而會混淆使用者尚未核准的帳戶或使用者已被鎖定。不過，利用最少的工作，我們可以有登入控制項顯示更適當的訊息。

每當使用者嘗試登入具有無效的認證登入控制項引發其`LoginError`事件。 請繼續並建立此事件，事件處理常式，加入下列程式碼：

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

上述程式碼會藉由設定登入控制項的啟動`FailureText`屬性為預設值 （登入嘗試不成功。 請再試一次）。 接著會檢查以查看是否提供的使用者名稱會對應至現有的使用者帳戶。 因此，查詢產生如果`MembershipUser`物件的`IsLockedOut`和`IsApproved`來判斷帳戶是否已經鎖定時，或尚未核准的屬性。 在任一情況下，`FailureText`屬性會更新成對應的值。

若要測試這段程式碼，故意嘗試將現有的使用者身分登入，但使用不正確的密碼。 在 10 分鐘的時間範圍內的資料列中執行這五次，並會鎖定帳戶。圖 11 顯示，後續的登入嘗試將一律失敗 （即使是具有正確的密碼），但現在會顯示更具描述性，為您的帳戶已被鎖定因為無效的登入嘗試次數過多。 請連絡系統管理員，讓您的帳戶解除鎖定的訊息。


[![Tito 執行無效的登入嘗試次數過多，而且已被鎖定](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**圖 11**: Tito 執行太多無效的登入嘗試和已被鎖定 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>總結

先前本教學課程中，我們的登入頁面會驗證提供的認證，針對使用者名稱/密碼組的硬式編碼清單。 在本教學課程中，我們已更新頁面，即可驗證成員資格架構的認證。 在步驟 1 中，我們還在使用`Membership.ValidateUser`方法以程式設計的方式。 在步驟 2 中取代我們以手動方式建立的使用者介面和程式碼與登入控制項。

登入控制項轉譯標準登入使用者介面，並會自動驗證使用者的認證，針對成員資格 framework。 此外，萬一有效的認證，登入控制項簽署使用者透過表單驗證。 簡單地說，完全正常運作的登入使用者經驗，請只需登入控制項拖曳到頁面、 沒有額外的宣告式標記或必要的程式碼。 什麼是控制的多個，登入控制項是控制的高度可自訂，讓良好能力的呈現的使用者介面和驗證邏輯。

此時我們的網站訪客可以建立新的使用者帳戶和記錄到站台，但我們還沒有查看限制的存取權頁面根據已驗證的使用者。 目前已驗證或匿名的任何使用者可以檢視在網站上的任何頁面。 控制對我們的網站頁面以使用者的使用者為基礎的存取，以及我們可能會有特定的功能取決於使用者的網頁。 下一個教學課程會查看如何登入的使用者為基礎的頁面中功能和限制。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [顯示自訂訊息以使鎖定且未經核准的使用者](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [檢查 ASP.NET 2.0 的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [如何： 建立 ASP.NET 登入頁面](https://msdn.microsoft.com/library/ms178331.aspx)
- [登入控制項的技術文件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [使用登入控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已本文菲和 Michael Olivero。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com)。

> [!div class="step-by-step"]
> [上一頁](creating-user-accounts-cs.md)
> [下一頁](user-based-authorization-cs.md)
