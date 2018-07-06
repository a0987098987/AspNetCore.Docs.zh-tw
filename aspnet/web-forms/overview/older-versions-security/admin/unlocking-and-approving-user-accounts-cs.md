---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: 解除鎖定及核准使用者帳戶 (C#) |Microsoft Docs
author: rick-anderson
description: 本教學課程示範如何建置網頁，以供系統管理員管理使用者的鎖定，且已核准狀態。 我們也將了解如何核准新使用者 o...
ms.author: aspnetcontent
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: ab6fa1b460de671559dc16ca85f2a4df1e3d5922
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818300"
---
<a name="unlocking-and-approving-user-accounts-c"></a>解除鎖定及核准使用者帳戶 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> 本教學課程示範如何建置網頁，以供系統管理員管理使用者的鎖定，且已核准狀態。 我們也將了解如何在他們驗證電子郵件地址時，才核准新使用者。


## <a name="introduction"></a>簡介

使用者名稱、 密碼和電子郵件，以及每個使用者帳戶有兩個的 [狀態] 欄位，以指定使用者是否可以登入網站： 鎖定及核准。 使用者會自動會鎖定時，如果他們提供無效的認證 （預設設定鎖定的使用者在 10 分鐘內的 5 個無效的登入嘗試之後） 的分鐘數內指定的次數。 已核准的狀態是在新的使用者能夠登入至站台之前，某些動作，必須經過的情況下很有用。 例如，使用者可能需要先確認電子郵件地址，或由系統管理員核准，才能登入。

因為一部鎖定或未核准使用者無法登入，它是唯一的自然想知道可以重設這些狀態的方式。 ASP.NET 不包含任何內建的功能或 Web 控制項，來管理使用者的鎖定，且部分核准狀態，因為這些決策必須就站對站個別處理。 某些網站可能會自動核准所有新的使用者帳戶 （預設行為）。 其他讓系統管理員核准新的帳戶，或請勿核准使用者，直到他們瀏覽連結傳送給他們註冊時所提供的電子郵件地址。 同樣地，某些網站可能會鎖定使用者直到系統管理員重設其狀態，而其他站台傳送電子郵件給鎖定使用者的 url 可以瀏覽解除鎖定其帳戶。

本教學課程示範如何建置網頁，以供系統管理員管理使用者的鎖定，且已核准狀態。 我們也將了解如何在他們驗證電子郵件地址時，才核准新使用者。

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>步驟 1： 管理使用者的鎖定及核准狀態

在  <a id="Tutorial12"> </a> [*選取一個使用者帳戶來建置介面從許多*](building-an-interface-to-select-one-user-account-from-many-cs.md)教學課程，我們已建構的頁面，列出每個使用者帳戶，在分頁，篩選 GridView。 此方格會列出每個使用者的名稱和電子郵件、 其已核准與鎖定的狀態，無論是目前在線上，和任何使用者相關的註解。 若要管理使用者核准，並鎖定狀態，我們可以把這個方格編輯。 若要變更使用者的核准的狀態，系統管理員會先找出的使用者帳戶，並檢查，或取消選取 已核准的核取方塊，然後編輯對應的 GridView 資料列。 或者，我們無法管理的已核准與鎖定的狀態，透過不同的 ASP.NET 頁面。

本教學課程中我們使用兩個 ASP.NET 網頁：`ManageUsers.aspx`和`UserInformation.aspx`。 這意思是，`ManageUsers.aspx`列出的使用者帳戶在系統中，雖然`UserInformation.aspx`可讓系統管理員管理特定使用者的核准及鎖定狀態。 我們第一要務是加強在 GridView`ManageUsers.aspx`包含 HyperLinkField，會轉譯為連結的資料行。 我們希望每個連結以指向`UserInformation.aspx?user=UserName`，其中*使用者名稱*是要編輯之使用者的名稱。

> [!NOTE]
> 如果您已下載的程式碼<a id="Tutorial13"> </a> [*復原及變更密碼*](recovering-and-changing-passwords-cs.md)教學課程，您可能已經注意到，`ManageUsers.aspx`頁面已經包含一組"管理 」 的連結和`UserInformation.aspx`頁面會提供介面變更所選的使用者的密碼。 我決定不要複寫這項功能的程式碼，本教學課程與相關聯，因為它由規避 Membership API 和操作直接與 SQL Server 資料庫，若要變更使用者的密碼。 本教學課程會使用從頭開始`UserInformation.aspx`頁面。


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>新增 「 管理 」 連結`UserAccounts`GridView

開啟`ManageUsers.aspx`頁面上，並新增至 HyperLinkField `UserAccounts` GridView。 設定 HyperLinkField `Text` [管理] 的屬性並將其`DataNavigateUrlFields`和`DataNavigateUrlFormatString`屬性，以`UserName`和 「 UserInformation.aspx?user={0}"分別。 這些設定會使所有的超連結顯示文字 「 管理 」，但每個連結會將 $ 中適當設定 HyperLinkField *UserName*成查詢字串的值。

加入之後 HyperLinkField GridView，請花一點時間檢視`ManageUsers.aspx`透過瀏覽器的頁面。 如 [圖 1] 所示，每個 GridView 資料列現在會包含 [管理] 連結。 Bruce 的 「 管理 」 連結指向`UserInformation.aspx?user=Bruce`，而 Dave 的 「 管理 」 連結指向`UserInformation.aspx?user=Dave`。


[![HyperLinkField 新增](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**圖 1**: HyperLinkField 新增每個使用者帳戶的 [管理] 連結 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image3.png))


我們將建立的使用者介面，並為撰寫程式碼`UserInformation.aspx`頁面目前為止，但第一個讓我們來討論有關如何以程式設計方式變更使用者的鎖定，且已核准狀態。 [ `MembershipUser`類別](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)具有[ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)並[`IsApproved`屬性](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx)。 `IsLockedOut`屬性是唯讀的。 沒有任何機制來以程式設計方式鎖定使用者;若要解除鎖定使用者，請使用`MembershipUser`類別的[`UnlockUser`方法](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)。 `IsApproved`屬性可讀取和寫入。 若要儲存這個屬性的任何變更，我們必須呼叫`Membership`類別的[`UpdateUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)，並傳入已修改`MembershipUser`物件。

因為`IsApproved`屬性讀取和寫入，核取方塊控制項可能是最佳的使用者介面項目來設定這個屬性。 不過，核取方塊並不適用於`IsLockedOut`屬性因為系統管理員無法鎖定使用者，她可能只能解除鎖定使用者。 適當的使用者介面，讓`IsLockedOut`屬性是一個按鈕，按一下時，會解除鎖定使用者帳戶。 如果使用者遭到鎖定，才應該啟用此按鈕。

### <a name="creating-theuserinformationaspxpage"></a>建立`UserInformation.aspx`頁面

現在，我們已準備好要實作中的使用者介面`UserInformation.aspx`。 開啟此頁面，並加入下列的 Web 控制項：

- 超連結控制項，按一下時，會傳回系統管理員`ManageUsers.aspx`頁面。
- 顯示選取的使用者名稱標籤 Web 控制項。 設定此標籤`ID`要`UserNameLabel`並清除其`Text`屬性。
- 核取方塊控制項，名為`IsApproved`。 設定其`AutoPostBack`屬性設`true`。
- Label 控制項來顯示使用者上次的鎖定的日期。 此標籤`LastLockedOutDateLabel`並清除其`Text`屬性。
- 解除鎖定使用者的按鈕。 此按鈕命名`UnlockUserButton`並設定其`Text`為 「 解除鎖定使用者 」 的屬性。
- Label 控制項用於顯示狀態訊息，例如 「 已更新使用者的核准的狀態 」。 此控制項`StatusMessage`，清除出其`Text`屬性，並設定其`CssClass`屬性設`Important`。 ( `Important` CSS 類別定義於`Styles.css`樣式表檔案，在大型的紅色字型顯示相對應的文字。)

之後加入這些控制項，在 Visual Studio 中的 [設計] 檢視看起來應該類似於圖 2 螢幕擷取畫面。


[![建立 UserInformation.aspx 的使用者介面](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**圖 2**： 建立的使用者介面`UserInformation.aspx`([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image6.png))


使用者介面完成後下, 一步是設定`IsApproved`核取方塊和其他控制項根據所選的使用者的資訊。 建立網頁的事件處理常式`Load`事件，並新增下列程式碼：

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

上述程式碼一開始會確保這第一次造訪的網頁和不後續的回傳。 它接著會讀取傳遞的使用者名稱`user`查詢字串欄位，並擷取透過該使用者帳戶的相關資訊`Membership.GetUser(username)`方法。 如果不需要使用者名稱已提供透過查詢字串，或如果找不到指定的使用者，系統管理員會送回到`ManageUsers.aspx`頁面。

`MembershipUser`物件的`UserName`值，即會顯示在`UserNameLabel`並`IsApproved`核取方塊已根據`IsApproved`屬性值。

`MembershipUser`物件的[`LastLockoutDate`屬性](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx)傳回`DateTime`值，指出使用者上次遭鎖定。如果使用者被鎖定時，傳回的值取決於成員資格提供者。 建立新的帳戶時，`SqlMembershipProvider`設定`aspnet_Membership`資料表的`LastLockoutDate`欄位到`1754-01-01 12:00:00 AM`。 上述程式碼會顯示中的空字串`LastLockoutDateLabel`如果`LastLockoutDate`屬性，就會發生年份的前 2000年，否則的日期部分`LastLockoutDate`屬性會顯示在標籤中。 `UnlockUserButton'` s`Enabled`屬性設定為使用者的鎖定狀態，這表示如果使用者遭到鎖定，才會啟用此按鈕。

請花一點時間測試`UserInformation.aspx`透過瀏覽器的頁面。 當然，必須在開始`ManageUsers.aspx`，然後選取要管理的使用者帳戶。 在抵達`UserInformation.aspx`，請注意，`IsApproved`如果使用者核准，才會檢查核取方塊。 如果使用者曾被鎖定時，會顯示其最後一個鎖定的日期。 只有當使用者目前已鎖定，則會啟用 解除鎖定使用者 按鈕。檢查或取消選取 `IsApproved`核取方塊，或按一下 解除鎖定使用者 按鈕會導致回傳，但不修改的使用者帳戶因為我們至今還建立這些事件的事件處理常式。

返回 Visual Studio 並建立事件處理常式`IsApproved`核取方塊的`CheckedChanged`事件並`UnlockUser` 按鈕的`Click`事件。 在 `CheckedChanged`事件處理常式中，設定使用者的`IsApproved`屬性設`Checked`屬性的核取方塊，然後儲存變更，透過對呼叫`Membership.UpdateUser`。 在 `Click`事件處理常式，只需呼叫`MembershipUser`物件的`UnlockUser`方法。 在這兩個事件處理常式中，會顯示在適當的訊息`StatusMessage`標籤。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>測試`UserInformation.aspx`頁面

在發生這些事件處理常式，重新瀏覽的頁面和未經核准的使用者。 如 [圖 3] 所示，您應該會看到訊息指出此頁面上使用者的簡短`IsApproved`已成功修改屬性。


[![Chris 已經未核准](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**圖 3**: Chris 已經未核准 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image9.png))


接下來，登出，然後嘗試使用者身分，登入帳戶是只要未經核准。 使用者未獲得核准，因為它們無法登入。 根據預設，登入控制項可顯示相同的訊息，如果使用者無法登入，不論原因為何。 但在<a id="Tutorial6"> </a> [*驗證使用者認證對成員資格使用者存放區*](../membership/validating-user-credentials-against-the-membership-user-store-cs.md)教學課程中我們探討了增強的登入控制項來顯示更適當的訊息。 如 [圖 4] 所示，Chris 會顯示訊息，說明他無法登入因為他的帳戶尚未核准。


[![Chris 無法登入因為 His 帳戶是未核准](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**圖 4**: Chris 無法登入因為 His 帳戶是未核准 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image12.png))


若要測試鎖定的功能，請嘗試已核准的使用者身分登入，但使用不正確的密碼。 重複此程序所需的次數之前使用者的帳戶已被鎖定。Login 控制項也已更新以顯示自訂訊息，如果嘗試從鎖定的帳戶登入。 您知道該帳戶已被鎖定之後您會開始看到登入頁面的下列訊息: 「 您的帳戶已經鎖定時因為無效的登入嘗試次數過多。 請連絡系統管理員將您的帳戶解除鎖定。 」

返回`ManageUsers.aspx`頁面上，按一下 鎖定使用者的 管理 連結。 如 [圖 5] 所示，您應該會看到中的值`LastLockedOutDateLabel`解除鎖定使用者按鈕應會啟用。 按一下 [解除鎖定使用者] 按鈕，以解除鎖定使用者帳戶。 一旦您已解鎖使用者，他們將能夠再次登入。


[![Dave 已被鎖定系統](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**圖 5**: Dave 有已鎖定的系統 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>步驟 2： 指定新使用者的核准狀態

已核准的狀態是在您想要新的使用者就能夠將登入並存取該網站的使用者特定功能之前要執行某些動作的情況下很有用。 例如，您可能會執行私用網站所有頁面，除了登入和註冊頁面，其中都是僅供已驗證的使用者存取。 但要是完全達到您的網站，尋找 [註冊] 頁面中，會建立一個帳戶？ 若要避免這種情況中，您無法移動註冊頁面以`Administration`資料夾中，而且需要系統管理員手動建立每個帳戶。 或者，您可以允許任何人註冊，但禁止網站存取權，直到系統管理員核准的使用者帳戶。

根據預設，CreateUserWizard 控制項核准新的帳戶。 您可以設定此控制項的行為[`DisableCreatedUser`屬性](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)。 將此屬性設定為`true`不核准新的使用者帳戶。

> [!NOTE]
> 預設 CreateUserWizard 控制項自動登入新的使用者帳戶。 此行為取決於控制項的[`LoginCreatedUser`屬性](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)。 因為未核准的使用者無法登入網站時`DisableCreatedUser`已`true`新的使用者帳戶未登入站台，不論值`LoginCreatedUser`屬性。


如果您要以程式設計方式建立新的使用者帳戶，透過`Membership.CreateUser`方法中，建立未經核准的使用者帳戶使用其中一個多載會接受新使用者的`IsApproved`做為輸入參數的屬性值。

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>步驟 3： 確認電子郵件地址核准使用者

支援使用者帳戶的許多網站請勿核准新使用者，直到他們會驗證他們在註冊時提供的電子郵件地址。 這個驗證程序常用來阻止 bot、 濫發垃圾郵件者和其他 ne'er-do-wells，因為它需要唯一的、 已驗證的電子郵件地址，並在註冊程序中新增額外的步驟。 使用此模型中，新的使用者註冊時傳送電子郵件訊息，其中包含驗證頁面的連結。 瀏覽連結使用者證明他們有收到電子郵件，因此，電子郵件地址，前提是有效。 [驗證] 頁面會負責核准的使用者。 這可能會自動發生，藉此核准到達此頁面上，任何使用者或使用者提供一些額外的資訊，例如之後，才[CAPTCHA](http://en.wikipedia.org/wiki/Captcha)。

為了符合此工作流程，我們需要先更新帳戶的 [建立] 頁面，以便讓新的使用者未經核准。 開啟`EnhancedCreateUserWizard.aspx`頁面中`Membership`資料夾，然後設定 CreateUserWizard 控制項的`DisableCreatedUser`屬性設`true`。

接下來，我們需要設定 CreateUserWizard 控制項傳送給新的使用者，以驗證其帳戶指示的電子郵件。 特別是，我們將電子郵件中包含的連結`Verification.aspx`頁面上 (這我們至今還建立)，並傳入新使用者的`UserId`透過查詢字串。 `Verification.aspx`會查閱指定的使用者 頁面，並將其標示核准它們。

### <a name="sending-a-verification-email-to-new-users"></a>將驗證電子郵件傳送給新使用者

若要從 CreateUserWizard 控制項傳送一封電子郵件，設定其`MailDefinition`屬性適當地。 中所述<a id="Tutorial13"> </a>[先前的教學課程](recovering-and-changing-passwords-cs.md)，包含 「 變更密碼 」 和 「 Provider 控制項[`MailDefinition`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)的運作方式與CreateUserWizard 控制項。

> [!NOTE]
> 若要使用`MailDefinition`中的屬性，您必須指定郵件的傳遞選項`Web.config`。 如需詳細資訊，請參閱[在 ASP.NET 中的 傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)。


建立名為的新電子郵件範本著手`CreateUserWizard.txt`在`EmailTemplates`資料夾。 範本中使用下列文字：

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

設定`MailDefinition'`s`BodyFileName`屬性，以"~ / EmailTemplates/CreateUserWizard.txt"並將其`Subject`屬性，以 「 歡迎使用我的網站 ！ 請啟用您的帳戶。 」

請注意，`CreateUserWizard.txt`電子郵件範本包含`<%VerificationUrl%>`預留位置。 這正是 URL`Verification.aspx`會置於頁面。 會自動取代 CreateUserWizard`<%UserName%>`並`<%Password%>`預留位置取代為新的帳戶使用者名稱和密碼，但沒有任何內建`<%VerificationUrl%>`預留位置。 我們需要以手動方式將它取代適當的驗證 URL。

若要達成此目的，建立事件處理常式 CreateUserWizard [ `SendingMail`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx)並加入下列程式碼：

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

`SendingMail`事件引發之後`CreatedUser`事件，這表示，上述的事件處理常式執行新的使用者時的帳戶已經存在。 我們可以存取新的使用者`UserId`值，藉由呼叫`Membership.GetUser`方法並傳入`UserName`CreateUserWizard 控制項中輸入。 接下來，會形成驗證 URL。 陳述式`Request.Url.GetLeftPart(UriPartial.Authority)`傳回`http://yourserver.com`一部分 URL;`Request.ApplicationPath`傳回路徑的應用程式的根位置。 驗證 URL 定義為`Verification.aspx?ID=userId`。 然後這兩個字串串連來形成完整的 URL。 最後，電子郵件訊息內文 (`e.Message.Body`) 具有所有發生次數`<%VerificationUrl%>`取代完整的 URL。

實質效果是新的使用者未經核准，這表示它們無法登入網站。 此外，它們會自動傳送電子郵件，內含連結至驗證 URL （請參閱 圖 6）。


[![新的使用者會收到一封電子郵件驗證 URL 的連結](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**圖 6**： 新的使用者會收到一封電子郵件驗證 URL 的連結 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> CreateUserWizard 控制項的預設 CreateUserWizard 步驟會顯示訊息，通知的使用者他們的帳戶已建立，並顯示 [繼續] 按鈕。 按一下這會將使用者帶至由控制項的指定的 URL`ContinueDestinationPageUrl`屬性。 在 CreateUserWizard`EnhancedCreateUserWizard.aspx`設定為傳送給新使用者`~/Membership/AdditionalUserInfo.aspx`，這時系統會提示使用者輸入他們的出生地、 首頁 URL 和簽章。 因為這項資訊可以只新增登入的使用者，所以合理更新這個屬性，以將使用者傳送回站台的首頁 (`~/Default.aspx`)。 此外，`EnhancedCreateUserWizard.aspx`頁面或 CreateUserWizard 步驟應該予以增加以通知使用者已傳送驗證電子郵件，以及它們遵循這封電子郵件中的指示之前，不會啟用其帳戶。 我可以作為練習，保留這些修改來讀取器。


### <a name="creating-the-verification-page"></a>建立 [驗證] 頁面

最後一項工作是建立`Verification.aspx`頁面。 將此頁面新增至根資料夾中，將它與`Site.master`主版頁面。 我們已在先前的內容頁面，新增至網站的大部分完成之後，移除參考的內容控制項`LoginContent`ContentPlaceHolder 使 [內容] 頁面會使用主版頁面的預設內容。

加入標籤 Web 控制項來`Verification.aspx`頁面上，將其`ID`到`StatusMessage`和清除其 text 屬性。 接下來，建立`Page_Load`事件處理常式，並新增下列程式碼：

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

大部分的上述程式碼會確認`UserId`提供透過查詢字串存在，它是有效`Guid`值，而且它會參考現有的使用者帳戶。 如果所有檢查都通過，已核准的使用者帳戶;否則，會顯示適當的狀態訊息。

[圖 7] 顯示`Verification.aspx`頁面上，當透過瀏覽器瀏覽。


[![新的使用者帳戶是現在核准](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**圖 7**: 新使用者的帳戶是現在核准 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>總結

成員資格的所有使用者帳戶都有兩個決定使用者是否可以登入網站的狀態：`IsLockedOut`和`IsApproved`。 這兩個屬性必須是`true`登入使用者。

使用者的鎖定狀態使用基於安全性考量，以減少駭客透過暴力密碼破解方法分成多站台的可能性。 具體而言，將使用者鎖定時，如果有特定數目的特定時段內無效的登入嘗試。 這些範圍會透過成員資格提供者的設定可設定`Web.config`。

已核准的狀態通常用於做為禁止新的使用者登入，直到發生某些動作。 可能是站台都需要由系統管理員第一次核准新的帳戶或如我們在步驟 3 中所見驗證電子郵件地址。

快樂地寫程式 ！

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝...

本教學課程系列是由許多實用的檢閱者檢閱。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](recovering-and-changing-passwords-cs.md)
> [下一頁](building-an-interface-to-select-one-user-account-from-many-vb.md)
