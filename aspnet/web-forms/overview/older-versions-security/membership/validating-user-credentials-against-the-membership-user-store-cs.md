---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: 正在驗證使用者認證，針對成員資格使用者存放區 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將檢查如何驗證使用者的認證，對使用程式設計的方式和 Login 控制項的成員資格使用者存放區...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: e8c46d09a7ebab19204f7c439ec4333e0c36b73e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828951"
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>針對成員資格使用者存放區 (C#) 驗證使用者認證
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip)或[下載 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> 在本教學課程中，我們將檢查如何驗證使用者的認證，對使用程式設計的方式和 Login 控制項的成員資格使用者存放區。 我們也將探討如何自訂登入控制項的外觀和行為。


## <a name="introduction"></a>簡介

在 <a id="Tutorial05"></a>[前述教學課程](creating-user-accounts-cs.md)我們探討了如何在成員資格架構中建立新的使用者帳戶。 我們先探討了以程式設計方式建立使用者帳戶，透過`Membership`類別的`CreateUser`方法，並檢查 使用 CreateUserWizard Web 控制項。 不過，目前登入頁面會驗證提供的認證，對使用者名稱和密碼組的硬式編碼清單。 我們需要更新登入頁面的邏輯，使它會針對成員資格架構的使用者存放區的認證來驗證。

更像是建立使用者帳戶，可以驗證認證以程式設計方式或以宣告方式。 成員資格 API 包含用於以程式設計方式驗證使用者的認證，對使用者存放區的方法。 和 ASP.NET 隨附登入 Web 控制項，會呈現使用者介面使用的使用者名稱、 密碼及登入按鈕的文字方塊。

在本教學課程中，我們將檢查如何驗證使用者的認證，對使用程式設計的方式和 Login 控制項的成員資格使用者存放區。 我們也將探討如何自訂登入控制項的外觀和行為。 讓我們開始吧 ！

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>步驟 1： 驗證認證，對成員資格使用者存放區

使用表單驗證的網站，使用者登入網站瀏覽登入頁面，並輸入其認證。 這些認證，然後會比較針對使用者存放區。 如果它們是有效的使用者會授與表單驗證票證，也就是安全性語彙基元，表示身分識別和訪客的真確性。

若要驗證的使用者成員資格架構，請使用`Membership`類別的[`ValidateUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)。 `ValidateUser`方法會採用兩個輸入參數- *`username`* 並*`password`* -，並傳回布林值，指出是否已有效認證。 如同`CreateUser`方法，在上一個教學課程中，我們檢查`ValidateUser`方法會委派設定的成員資格提供者的實際驗證。

`SqlMembershipProvider`取得指定的使用者密碼，透過驗證提供的認證`aspnet_Membership_GetPasswordWithFormat`預存程序。 請記得，`SqlMembershipProvider`儲存使用者的密碼使用三種格式之一： 清除、 加密或雜湊。 `aspnet_Membership_GetPasswordWithFormat`預存程序傳回的密碼，以其原始格式。 加密或雜湊密碼`SqlMembershipProvider`轉換*`password`* 傳入值`ValidateUser`成其對等的方法來加密或雜湊狀態及再比較它與從所傳回的內容資料庫。 如果儲存在資料庫中的密碼符合格式化輸入使用者的密碼，是有效的認證。

讓我們更新我們的登入頁面 (~ /`Login.aspx`)，因此它會針對成員資格架構的使用者存放區提供的認證來驗證。 我們建立此登入頁面年代<a id="Tutorial02"></a>[*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程中，兩個文字方塊，使用者名稱和密碼，以建立介面記住我] 核取方塊，並登入按鈕 （請參閱 [圖 1）。 程式碼會驗證輸入的認證，對硬式編碼 （Scott/密碼、 Jisun/密碼和 Sam/密碼） 的使用者名稱和密碼組的清單。 在  <a id="Tutorial03"></a>[*表單驗證組態和進階主題*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)我們更新了表單中儲存其他資訊的登入網頁的程式碼的教學課程驗證票證`UserData`屬性。


[![登入頁面的介面包含兩個文字方塊、 CheckBoxList 和按鈕](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**圖 1**: 登入頁面的介面包含兩個文字方塊、 CheckBoxList 和一個按鈕 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


登入頁面使用者介面可維持不變，但我們需要將登入按鈕的`Click`事件處理常式，以驗證使用者是否有成員資格架構的使用者存放區的程式碼。 更新的事件處理常式，使其程式碼看起來像這樣：

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

此程式碼是非常簡單。 我們會先呼叫`Membership.ValidateUser`方法並傳入提供的使用者名稱和密碼。 如果該方法會傳回 true，則使用者登入透過網站`FormsAuthentication`類別的 RedirectFromLoginPage 方法。 (如我們所述<a id="Tutorial2"></a>[*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程中，`FormsAuthentication.RedirectFromLoginPage`建立表單驗證票證，並將使用者重新導向以適當的頁面。）如果認證不正確的不過，`InvalidCredentialsMessage`標籤會顯示通知使用者他們的使用者名稱或密碼不正確。

這樣就完成了 ！

若要測試登入頁面會在如預期般運作，請嘗試使用其中一項在前述教學課程中所建立的使用者帳戶登入。 或者，如果您尚未建立帳戶，請繼續並建立一個從`~/Membership/CreatingUserAccounts.aspx`頁面。

> [!NOTE]
> 當使用者輸入認證，並送出登入頁面表單時，認證，包括密碼，會透過網際網路到 web 伺服器中傳輸*純文字*。 這表示任何探查的網路流量的駭客可以看到使用者名稱和密碼。 若要避免這個問題，務必使用加密的網路流量[安全通訊端層 (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)。 這可確保加密的認證 （以及整個網頁的 HTML 標記），從目前的使用者會離開瀏覽器，但尚未接收由 web 伺服器。


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>成員資格架構如何處理無效的登入嘗試

當訪客達到登入頁面，並提交其認證時，則其瀏覽器會將 HTTP 要求對登入頁面。 如果認證有效，則 HTTP 回應會包含驗證票證在 cookie 中。 因此，駭客嘗試入侵您的站台可以建立可徹底登入頁面，以有效的使用者名稱和密碼猜出傳送 HTTP 要求的程式。 如果密碼猜測值正確無誤，請登入頁面就會傳回驗證票證 cookie，此時程式知道它有偶然的有效使用者名稱/密碼組。 透過暴力密碼破解，只有弱式密碼時，尤其是可能能夠樂使用者的密碼，這類程式。

若要避免這類的暴力密碼破解攻擊，成員資格架構鎖定使用者如果有一段時間內的登入失敗嘗試數目。 確切的參數是可透過下列兩個成員資格提供者組態設定進行設定：

- `maxInvalidPasswordAttempts` -指定多少的無效密碼的使用者帳戶遭到鎖定之前的時間週期內，便允許嘗試。預設值為 5。
- `passwordAttemptWindow` -表示的時間週期，以分鐘為單位的期間指定的無效的登入嘗試次數會被鎖定的帳戶。預設值為 10。

如果使用者已經鎖定時，她無法登入，直到系統管理員解除鎖定其帳戶。 當使用者遭到鎖定時，`ValidateUser`方法將*一律*傳回`false`，即使提供有效的認證。 雖然此行為可減少駭客會中斷並進入您的網站透過暴力密碼破解方法的可能性，它可能會結束鎖定的有效使用者只要忘記其密碼或不小心對 Caps Lock，或有不正確的輸入日期。

不幸的是，沒有任何內建的工具，用於解除鎖定使用者帳戶。 若要解除鎖定帳戶，您可以修改資料庫直接-變更`IsLockedOut`欄位中`aspnet_Membership`適用於適當的使用者帳戶集的資料表，或建立 web 型介面，其中列出與選項，來解除鎖定其帳戶被鎖定。 我們將檢視建立的系統管理介面，在未來的教學課程中完成一般使用者帳戶與角色相關工作。

> [!NOTE]
> 一項缺點`ValidateUser`方法是，當提供的認證無效，它不提供任何說明為何。 認證可能無效，因為沒有不相符的使用者名稱/密碼組，在使用者存放區中，或因為尚未核准的使用者，或使用者已被鎖定。在步驟 4 中，我們將了解如何向使用者顯示更詳細的訊息，其登入嘗試失敗時。


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>步驟 2： 收集的認證，透過登入 Web 控制項

[登入 Web 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)呈現非常類似於我們在上一步所建立的預設使用者介面<a id="SKM5"> </a> [*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程。 使用登入控制項，節省我們需要建立的介面來收集的訪客的認證的工作。 此外，登入控制會自動登入使用者 （假設已提交的認證有效），藉此儲存我們不必撰寫任何程式碼。

讓我們更新`Login.aspx`、 取代手動建立的介面和程式碼的登入控制項。 啟動移除現有的標記，並在程式碼`Login.aspx`。 您可能會刪除徹底，或只需加以註解化。若要標記為註解宣告式標記，把它與`<%--`和`--%>`分隔符號。 您可以手動輸入這些分隔符號，或如 圖 2 所示，您可以選取要標記為註解，然後按一下 標記為註解選取的行圖示，在工具列中的文字。 同樣地，您可以使用標記為註解選取的行圖示標記為註解程式碼後置類別中選取的程式碼。


[![標記為註解的現有宣告式標記和程式碼置於 Login.aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**圖 2**： 註解出現有宣告式標記和程式碼置於`Login.aspx`([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> 檢視 Visual Studio 2005 中的宣告式標記時，標記為註解選取的行圖示不提供。 如果您不想要使用 Visual Studio 2008 您必須手動新增`<%--`和`--%>`分隔符號。


接下來，將登入控制項從工具箱拖曳到頁面上，並設定其`ID`屬性設`myLogin`。 此時您的畫面應該看起來類似 圖 3。 請注意，登入控制項的預設介面包含文字方塊控制項的使用者名稱和密碼，請記得我下一次核取方塊，並登入 按鈕。 另外還有`RequiredFieldValidator`的兩個文字方塊控制項。


[![將登入控制項新增至頁面](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**圖 3**： 將登入控制項新增至網頁 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


大功告成 ！ 按一下 登入控制項的 登入 按鈕時，就會發生回傳，並且呼叫 Login 控制項`Membership.ValidateUser`方法並傳入輸入使用者名稱和密碼。 如果認證無效，登入控制項就會顯示訊息，說明如下。 如果，不過，是有效的認證，登入控制項建立表單驗證票證，並將使用者重新導向到適當的網頁。

Login 控制項使用四項因素，來判斷適當的頁面，將使用者重新導向至成功的登入：

- 此登入控制項是否在登入頁面上所定義`loginUrl`設定表單驗證設定; 中，此設定的預設值是 `Login.aspx`
- 是否存在`ReturnUrl`querystring 參數
- 登入控制項的值[`DestinationUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl`指定在表單驗證組態設定的值，此設定的預設值是 `Default.aspx`

圖 4 說明如何登入控制項以達成其適當的頁面決定使用這四個參數。


[![將登入控制項新增至頁面](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**圖 4**： 將登入控制項新增至網頁 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


請花一點時間瀏覽的網站，透過瀏覽器，並以成員資格架構中的現有使用者身分登入測試登入控制項。

登入控制項的轉譯的介面進行大幅設定。 有一些會影響它的外觀; 的屬性不僅如此，登入控制項可以轉換成精確地控制版面配置範本的使用者介面項目。 此步驟的其餘部分會檢驗如何自訂外觀和版面配置。

### <a name="customizing-the-login-controls-appearance"></a>自訂登入控制項的外觀

登入控制項的預設屬性設定轉譯使用者介面 （登入） 的標題、 使用者名稱和密碼輸入，記住我的 TextBox 和 Label 控制項下, 一次的核取方塊，並登入 按鈕。 這些項目的外觀都可透過登入控制項的多個屬性進行所有設定。 此外，可以新增額外的使用者介面項目-例如連結的頁面，即可建立新的使用者帳戶-設定屬性或兩個。

讓我們花一些時間才能 gussy 註冊我們的登入控制項的外觀。 因為`Login.aspx`頁面已經有一個指出登入頁面頂端的文字、 登入控制項的標題是多餘。 因此，清除[`TitleText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx)值，以便移除登入控制項的標題。

使用者名稱： 和密碼： 左邊的兩個 TextBox 控制項的標籤可以透過自訂[ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx)並[`PasswordLabelText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)分別。 讓我們變更使用者名稱： 讀取 Username 的標籤:。 標籤和文字方塊樣式是透過[ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx)並[`TextBoxStyle`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)分別。

記住我下一步 時間核取方塊的文字屬性可以設定透過登入控制項[ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)，和其預設已核取狀態是透過可設定[ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) （其中預設值為 False）。 請繼續進行，並設定`RememberMeSet`預設會勾選屬性設為 True，讓 記住我下一次核取方塊。

Login 控制項提供兩個屬性來調整其使用者介面控制項的版面配置。 [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx)指出是否將使用者名稱： 和密碼： 標籤會顯示在左邊，其對應的文字方塊 （預設值），或其上。 [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx)指出是否以垂直方式提出的使用者名稱和密碼輸入 （一個在彼此上方） 或水平。 我要保留其預設值，這兩個屬性，但建議您嘗試將這兩個屬性設定為非預設值以查看產生的效果。

> [!NOTE]
> 在下一步 區段中，設定登入控制項的版面配置中，我們會探討使用範本來定義版面配置控制項的使用者介面項目的精確的版面配置。


藉由設定登入控制項的屬性設定包裝[ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx)並[`CreateUserUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx)為 Not 尚未註冊嗎？ 建立帳戶 ！ 和`~/Membership/CreatingUserAccounts.aspx`分別。 這會將超連結加入登入控制項的介面指向頁面中建立<a id="SKM6"> </a>[前述教學課程](creating-user-accounts-cs.md)。 登入控制項[ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx)並[`HelpPageUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx)並[ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx)和[`PasswordRecoveryUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx)工作以相同的方式，呈現 [說明] 頁面和密碼復原頁面的連結。

進行這些屬性變更之後，您的登入控制項宣告式標記和外觀看起來應該類似於 [圖 5] 所示。


[![登入控制項的屬性值決定它的外觀](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**圖 5**: 登入控制項的屬性值決定其外觀 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>設定登入控制項的版面配置

登入 Web 控制項的預設使用者介面配置的介面，在 HTML `<table>`。 但是，如果我們需要更精細地控制轉譯的輸出？ 我們想要取代的或許`<table>`透過一系列的`<div>`標記。 或者，如果我們的應用程式會需要額外的認證進行驗證？ 許多財務網站中，比方說，要求使用者提供不只一個使用者名稱和密碼，但也個人識別碼 (PIN) 或其他識別資訊。 無論原因可能是，就可以轉換成範本，讓我們可以明確地定義介面的宣告式標記的 Login 控制項。

我們需要做兩件事，以更新到收集額外的認證登入控制項：

1. 更新登入控制項的介面，以加入 Web 控制項來收集其他的認證。
2. 覆寫登入控制項的內部驗證邏輯，使其使用者名稱和密碼有效，且其額外的認證有效，也只驗證使用者。

若要完成第一項工作，我們需要將登入控制項轉換成範本，並新增必要的 Web 控制項。 至於第二個工作中，登入控制項的驗證邏輯可以取代藉由建立控制項的事件處理常式[`Authenticate`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)。

讓我們更新登入控制項，會提示使用者輸入其使用者名稱、 密碼和電子郵件地址，使其只會驗證使用者，如果提供的電子郵件地址符合檔案的電子郵件地址。 我們首先要將登入控制項的介面轉換成範本。 從登入控制項的智慧標籤上，選擇 [轉換成範本] 選項。


[![將登入控制項轉換成範本](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**圖 6**： 將登入控制項轉換成範本 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> 若要還原為其預先 template 版本登入控制項，按一下重設連結，從控制項的智慧標籤。


將登入控制項轉換成範本加入`LayoutTemplate`與 HTML 項目和定義使用者介面的 Web 控制項的控制項的宣告式標記。 如 [圖 7] 所示，將控制項轉換為範本中移除數個屬性從 [屬性] 視窗中，這類`TitleText`， `CreateUserUrl`，依此類推，因為使用的範本時，會忽略這些屬性值。


[![較少的屬性都可用時登入控制項轉換為範本](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**圖 7**： 較少的屬性都可用時登入控制項轉換成範本 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


中的 HTML 標記`LayoutTemplate`可能依需要修改。 同樣地，請隨意新增任何新的 Web 控制項範本。 不過，很重要，該登入控制項的核心 Web 控制項保留在範本中，並保留其指派`ID`值。 特別是，請勿移除或重新命名`UserName`或是`Password`文字方塊中，`RememberMe`核取方塊， `LoginButton`  按鈕，`FailureText`標籤，或`RequiredFieldValidator`控制項。

若要收集的造訪者的電子郵件地址，我們需要將文字方塊加入至範本。 新增下列宣告式標記之間的資料表資料列 (`<tr>`)，其中包含`Password`TextBox 和資料表資料列 [記住我接下來的保留時間] 核取方塊：

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

在新增之後`Email`文字方塊中，瀏覽透過瀏覽器頁面。 如 [圖 8] 所示，登入控制項的使用者介面現在會包含第三個文字方塊中。


[![Login 控制項現在包含一個文字方塊，使用者的電子郵件地址](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**圖 8**： 登入控制項現在包含使用者的電子郵件地址文字方塊 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


此時，登入控制項仍在使用`Membership.ValidateUser`方法以驗證提供的認證。 同樣地，此值輸入到`Email`文字方塊無關的使用者可以登入。 在步驟 3 中我們將探討如何覆寫登入控制項的驗證邏輯，以便在認證才會被視為有效的使用者名稱和密碼有效，且提供的電子郵件地址個比對檔案上的電子郵件地址。

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>步驟 3： 修改登入控制項的驗證邏輯

當訪客提供她認證並點選 [登入] 按鈕，回傳是兩邊彼此乾瞪眼和登入控制其驗證工作流程進行。 工作流程一開始會引發[`LoggingIn`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx)。 任何與此事件相關聯的事件處理常式可以取消作業中的記錄檔，藉由設定`e.Cancel`屬性設`true`。

如果未取消作業中的記錄檔，則工作流程進展藉由引發[`Authenticate`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)。 如果沒有的事件處理常式`Authenticate`事件，然後它會負責判斷提供的認證是否有效。 如果未不指定任何事件處理常式，則會使用登入控制項`Membership.ValidateUser`方法，以判斷有效性的認證。

如果提供的認證是否有效，則建立表單驗證票證時， [ `LoggedIn`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx)引發，而且使用者已重新導向到適當的網頁。 如果，不過，認證會被視為無效，則[`LoginError`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx)，就會引發，並顯示訊息通知使用者他們的認證無效。 根據預設，在失敗登入控制項只是設定其`FailureText`標籤控制項 Text 屬性 （登入嘗試未成功失敗訊息。 請再試一次）。 不過，如果登入控制項[`FailureAction`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx)設定為`RedirectToLoginPage`，然後登入控制問題`Response.Redirect`附加查詢字串參數的登入頁面`loginfailure=1`（這會導致登入控制項以顯示失敗的訊息）。

圖 9 提供的驗證工作流程的流程圖。


[![登入控制項的驗證工作流程](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**圖 9**: 登入控制項的驗證工作流程 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> 如果您想知道當您把`FailureAction`的`RedirectToLogin`頁面選項，請考慮下列案例。 現在我們`Site.master`主版頁面目前沒有顯示的文字，Hello，陌生人時由匿名使用者，瀏覽左側資料行中，但假設我們想要使用的登入控制項取代該文字。 這可讓匿名使用者從任何頁面中的站台，而不需要他們直接瀏覽登入頁面登入。 不過，如果使用者無法登入，透過主版頁面所呈現的 Login 控制項，它可能會讓合理地重新導向至登入頁面 (`Login.aspx`) 因為該頁面可能包含其他指示、 連結及其他說明-例如若要建立的連結新的帳戶，或是擷取遺失的密碼-未新增至主版頁面。


### <a name="creating-theauthenticateevent-handler"></a>建立`Authenticate`事件處理常式

若要插入在我們的自訂驗證邏輯中，我們要建立登入控制項的事件處理常式`Authenticate`事件。 建立事件處理常式`Authenticate`事件將會產生下列事件處理常式定義：

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

如您所見，`Authenticate`事件處理常式傳遞類型的物件[ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx)做為其第二個輸入參數。 `AuthenticateEventArgs`類別包含名為的布林值屬性`Authenticated`用來指定提供的認證是否有效。 然後，我們的工作，是以撰寫程式碼會判斷提供的認證是否有效，並設定`e.Authenticate`屬性據此。

### <a name="determining-and-validating-the-supplied-credentials"></a>判斷及驗證提供的認證

使用登入控制項[ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx)並[`Password`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx)來判斷使用者所輸入的使用者名稱和密碼認證。 若要判斷輸入任何其他的 Web 控制項的值 (例如`Email`我們在上一個步驟中新增的文字方塊)，使用 *`LoginControlID`* `.FindControl`(「*`controlID`*") 來取得以程式設計方式參考範本中的 Web 控制項其`ID`屬性等於*`controlID`*。 例如，若要取得的參考`Email`文字方塊中，使用下列程式碼：

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

若要驗證使用者的認證，我們需要做兩件事：

1. 請確定提供的使用者名稱和密碼有效
2. 請確認輸入的電子郵件地址符合檔案上的使用者嘗試登入電子郵件地址

若要完成第一次檢查，我們可以直接使用`Membership.ValidateUser`像我們在步驟 1 中所見的方法。 第二個檢查，我們要判斷使用者的電子郵件地址，以便我們可以比較它與它們在 TextBox 控制項中輸入的電子郵件地址。 若要取得特定使用者的相關資訊，請使用`Membership`類別的[`GetUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)。

`GetUser`方法有幾個多載。 如果不傳遞任何參數使用，它會傳回目前登入使用者的相關資訊。 若要取得特定使用者的相關資訊，請呼叫`GetUser`傳入其使用者名稱。 無論`GetUser`會傳回[`MembershipUser`物件](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)，其中包含屬性，例如`UserName`， `Email`， `IsApproved`， `IsOnline`，依此類推。

下列程式碼會實作這些兩個檢查。 如果兩者都通過，然後`e.Authenticate`設定為`true`，否則它會指派`false`。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

使用此程式碼就緒之後，嘗試為有效的使用者，並輸入正確的使用者名稱、 密碼和電子郵件地址登入。 再試一次，但這次特意不使用不正確的電子郵件地址 （請參閱 圖 10）。 最後，試試看使用虛構的使用者名稱的第三次。 在第一種情況下您應該已成功登入至站台，但在最後兩個情況下您應該會看到登入控制項的認證無效訊息。


[![提供不正確的電子郵件地址時，將無法登入的 Tito](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**圖 10**: Tito 無法記錄檔中時提供不正確的電子郵件地址 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> 在步驟 1 中的 [如何成員資格架構會處理登入嘗試無效] 區段中所述當`Membership.ValidateUser`方法呼叫並傳遞無效的認證，它會持續追蹤的無效的登入嘗試，並鎖定使用者，如果在超過特定在指定的時間範圍內的無效嘗試的臨界值。 因為我們的自訂驗證邏輯呼叫`ValidateUser`方法，針對有效的使用者名稱不正確的密碼會遞增無效的登入的嘗試計數器，但此計數器不會遞增，萬一其中使用者名稱和密碼是否有效，但電子郵件地址不正確。 關鍵就在於，此行為是適合，因為它是不太可能，駭客會知道使用者名稱和密碼，但必須使用暴力密碼破解技術來判斷使用者的電子郵件地址。


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>步驟 4： 改善登入控制項的認證不正確訊息

當使用者嘗試使用無效的認證登入時，登入控制項就會顯示訊息，說明登入嘗試成功。 控制項會顯示指定的訊息的特別的是，其[`FailureText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx)，其具有預設值是您登入嘗試未成功。 請重試。

您應該記得，有許多使用者的認證可能無效的原因：

- 使用者名稱可能不存在
- 使用者名稱已存在，但密碼無效
- 使用者名稱和密碼不正確，但尚未核准的使用者
- 使用者名稱和密碼不正確，但使用者已鎖定外 （最有可能因為它們超過指定的時間範圍內無效的登入嘗試次數）

可能有其他原因時使用自訂驗證邏輯。 比方說，我們所撰寫的程式碼在步驟 3 中，使用者名稱和密碼可能有效，但電子郵件地址可能不正確。

不論原因是無效的認證，登入控制項顯示相同的錯誤訊息。 缺乏此種意見反應可以是使用者尚未核准的帳戶，或使用者已經鎖定時造成混淆。不過，加上一些的工作，我們可以讓登入控制項，顯示更適當的訊息。

每當使用者嘗試登入控制項所引發的不正確認證登入其`LoginError`事件。 請繼續並建立此事件，事件處理常式，並新增下列程式碼：

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

上述程式碼一開始會設定登入控制項的`FailureText`屬性為預設值 （登入嘗試未成功。 請再試一次）。 它接著會檢查以查看是否所提供的名稱會對應至現有的使用者帳戶。 因此，它會查詢所產生的如果`MembershipUser`物件的`IsLockedOut`和`IsApproved`以判斷帳戶是否已被鎖定，或者尚未核准的屬性。 在任一情況下，`FailureText`屬性更新成對應的值。

若要測試此程式碼，特意不嘗試將現有的使用者身分登入，但使用不正確的密碼。 在 10 分鐘的時間範圍內的資料列中執行這五次，並會鎖定帳戶。如 圖 11 所示，後續的登入嘗試將一律失敗 （即使具有正確的密碼），但現在會顯示更具描述性已經鎖定您的帳戶時因為無效的登入嘗試次數過多。 請連絡系統管理員將您的帳戶解除鎖定的訊息。


[![Tito 執行無效的登入嘗試次數過多，而且已被鎖定](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**圖 11**: Tito 執行太多無效的登入嘗試，並有已鎖定 ([按一下以檢視完整大小的影像](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>總結

先前本教學課程中，我們的登入頁面進行驗證所提供的認證，對使用者名稱/密碼組的硬式編碼清單。 在本教學課程中，我們已更新頁面，即可驗證認證，對成員資格架構。 在步驟 1 中我們探討使用`Membership.ValidateUser`方法以程式設計的方式。 在步驟 2 中我們以 Login 控制項取代我們的手動建立的使用者介面和程式碼。

Login 控制項轉譯標準登入的使用者介面，並會自動驗證使用者的認證，對成員資格架構。 此外，如果有效的認證，登入控制項將使用者登入透過表單驗證。 簡單地說，提供功能完整的登入的使用者體驗，請只將登入控制項拖曳至頁面、 沒有額外的宣告式標記或所需程式碼。 更甚者，登入控制項是控制的高度可自訂，以便進行適度能力的轉譯的使用者介面和驗證邏輯。

此時我們的網站訪客可以建立新的使用者帳戶和記錄至站台，但我們還沒有看看頁面根據已驗證的使用者限制存取。 目前，已驗證或匿名的任何使用者可以檢視我們的網站上的任何頁面。 以及控制我們的網站頁面，以在使用者的使用者為基礎的存取權，我們可能會有特定的功能取決於使用者的網頁。 下一個教學課程會探討如何限制存取和登入的使用者為基礎的頁面功能。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [顯示自訂訊息鎖定，且未經核准的使用者](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [檢查 ASP.NET 2.0 的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [如何： 建立 ASP.NET 登入頁面](https://msdn.microsoft.com/library/ms178331.aspx)
- [登入控制項的技術文件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [使用 Login 控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa Murphy 和 Michael Olivero。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com)。

> [!div class="step-by-step"]
> [上一頁](creating-user-accounts-cs.md)
> [下一頁](user-based-authorization-cs.md)
