---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: 解除鎖定並核准使用者帳戶 (C#) |Microsoft 文件
author: rick-anderson
description: 本教學課程示範如何建置網頁，讓系統管理員管理使用者的鎖定，核准狀態。 我們也會看到如何核准新使用者 o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b3d69e96513192bc73a2a6a7cbb4c6e33eb610b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="unlocking-and-approving-user-accounts-c"></a>解除鎖定和核准的使用者帳戶 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> 本教學課程示範如何建置網頁，讓系統管理員管理使用者的鎖定，核准狀態。 我們也會看到如何確認電子郵件地址時，才核准新的使用者。


## <a name="introduction"></a>簡介

使用者名稱、 密碼及電子郵件，以及每個使用者帳戶具有兩個指定使用者是否可以登入網站的狀態欄位： 鎖定及核准。 使用者會自動鎖定如果他們提供認證不正確指定的次數內指定的 （預設設定鎖定使用者在 5 無效的登入嘗試在 10 分鐘內） 的分鐘數。 其中某些動作必須經過後新的使用者才能登入站台的情況下有用核准的狀態。 例如，使用者可能需要先確認電子郵件地址，或由系統管理員核准才能登入。

因為鎖定 out 或未經使用者無法登入，它是唯一的自然猜想可以重設這些狀態的方式。 ASP.NET 不包含任何內建的功能或 Web 控制項，來管理使用者的鎖定，部分核准狀態，因為這些決策需要處理站台的站台為基礎。 某些網站可能會自動核准所有新的使用者帳戶 （預設行為）。 其他人擁有系統管理員核准新的帳戶，或直到他們造訪的連結傳送給他們註冊時提供的電子郵件地址，請勿核許使用者。 同樣地，某些網站可能會鎖定使用者直到系統管理員重設其狀態，而其他站台傳送電子郵件對 URL 鎖定使用者他們可以瀏覽至解除鎖定其帳戶。

本教學課程示範如何建置網頁，讓系統管理員管理使用者的鎖定，核准狀態。 我們也會看到如何確認電子郵件地址時，才核准新的使用者。

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>步驟 1： 管理使用者的鎖定，核准狀態

在<a id="Tutorial12"> </a> [*建置介面來選取一個使用者帳戶，從許多*](building-an-interface-to-select-one-user-account-from-many-cs.md)教學課程中我們建構列出每個使用者帳戶，在分頁的頁面篩選 GridView。 此方格會列出每個使用者名稱和電子郵件、 其核准及鎖定狀態，是否目前在線上，以及使用者相關的任何註解。 若要管理使用者核准，因此鎖定狀態，我們無法讓此方格可供編輯。 若要變更使用者的核准的狀態，系統管理員會先找出使用者帳戶，並選取或取消選取已核准的核取方塊，然後編輯對應的 GridView 資料列。 或者，我們無法管理的已核准及鎖定狀態透過個別的 ASP.NET 頁面。

此教學課程中我們使用兩個 ASP.NET 網頁：`ManageUsers.aspx`和`UserInformation.aspx`。 此處的想法是`ManageUsers.aspx`在系統中，列出的使用者帳戶時`UserInformation.aspx`讓系統管理員管理特定使用者的核准及鎖定狀態。 我們第一件事是加強在 GridView`ManageUsers.aspx`包含 HyperLinkField，會轉譯成連結的資料行。 我們想要每個連結以指向`UserInformation.aspx?user=UserName`，其中*UserName*是要編輯之使用者的名稱。

> [!NOTE]
> 如果您已下載的程式碼<a id="Tutorial13"> </a> [*復原及變更密碼*](recovering-and-changing-passwords-cs.md)教學課程，您可能已注意到，`ManageUsers.aspx`頁面已經包含一系列 」管理 」 的連結和`UserInformation.aspx`頁面會提供介面變更所選的使用者的密碼。 決定不複寫該功能，因為它已運作規避成員資格 API 和操作直接與 SQL Server 資料庫若要變更使用者的密碼，本教學課程與相關聯的程式碼中。 與從頭開始進行本教學課程`UserInformation.aspx`頁面。


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>新增 「 管理 」 的連結`UserAccounts`GridView

開啟`ManageUsers.aspx`頁面上，並加入至 HyperLinkField `UserAccounts` GridView。 設定 HyperLinkField `Text` 「 管理 」 的屬性和其`DataNavigateUrlFields`和`DataNavigateUrlFormatString`屬性`UserName`和"UserInformation.aspx?user={0}"，分別。 這些設定會使得所有超連結顯示的文字 「 管理 」，但每個連結會傳入適當設定 HyperLinkField *UserName*值傳入查詢字串。

加入之後 HyperLinkField GridView，請花一點時間檢視`ManageUsers.aspx`透過瀏覽器的頁面。 如圖 1 所示，每個 GridView 資料列現在包含 「 管理 」 連結。 Bruce 的 「 管理 」 連結指向`UserInformation.aspx?user=Bruce`，而 Dave 的 「 管理 」 連結指向`UserInformation.aspx?user=Dave`。


[![HyperLinkField 新增](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**圖 1**: HyperLinkField 新增每個使用者帳戶的 「 管理 」 連結 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image3.png))


我們將建立的使用者介面和程式碼`UserInformation.aspx`頁面時刻，但首先讓我們 talk 有關如何以程式設計方式變更使用者的鎖定，並且核准狀態。 [ `MembershipUser`類別](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)具有[ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)和[`IsApproved`屬性](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx)。 `IsLockedOut`屬性是唯讀的。 沒有任何機制來以程式設計方式鎖定使用者。若要解除鎖定使用者，請使用`MembershipUser`類別的[`UnlockUser`方法](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)。 `IsApproved`屬性可讀取和寫入。 若要儲存任何變更這個屬性，我們必須呼叫`Membership`類別的[`UpdateUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)，並傳入修改`MembershipUser`物件。

因為`IsApproved`屬性為讀取和寫入，核取方塊控制項可能是最佳的使用者介面項目來設定這個屬性。 不過，核取方塊並不適用於`IsLockedOut`屬性因為系統管理員無法鎖定使用者，她可能只能解除鎖定使用者。 合適的使用者介面`IsLockedOut`屬性是一個按鈕，按一下時，會解除鎖定使用者帳戶。 只有當使用者遭到鎖定，才應該啟用這個按鈕。

### <a name="creating-theuserinformationaspxpage"></a>建立`UserInformation.aspx`頁面

我們已經準備好要實作中的使用者介面`UserInformation.aspx`。 開啟此頁面，並加入下列 Web 控制項：

- 超連結控制項，按一下時，系統管理員會傳回`ManageUsers.aspx`頁面。
- 顯示選取的使用者名稱的標籤 Web 控制項。 設定此標籤`ID`至`UserNameLabel`和清除其`Text`屬性。
- 核取方塊控制項，名為`IsApproved`。 設定其`AutoPostBack`屬性`true`。
- Label 控制項來顯示使用者的上次鎖定的日期。 此標籤`LastLockedOutDateLabel`和清除其`Text`屬性。
- 解除鎖定使用者的按鈕。 這個按鈕命名為`UnlockUserButton`並設定其`Text`「 解除鎖定使用者 」 的屬性。
- Label 控制項來顯示狀態訊息，例如 「 使用者的核准的狀態已更新。 」 命名這個控制項`StatusMessage`，清除出其`Text`屬性，並設定其`CssClass`屬性`Important`。 ( `Important` CSS 類別定義於`Styles.css`樣式表檔案; 它會相對應的文字顯示在大型的紅色字型。)

加入這些控制項之後, 在 Visual Studio 中的 [設計] 檢視看起來應該類似螢幕擷取畫面在圖 2 中。


[![建立 UserInformation.aspx 的使用者介面](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**圖 2**： 建立使用者介面`UserInformation.aspx`([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image6.png))


完整的使用者介面下, 一步是將`IsApproved`核取方塊和其他控制項根據所選的使用者的資訊。 建立網頁的事件處理常式`Load`事件並加入下列程式碼：

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

上述程式碼一開始會確保這第一次造訪的網頁和不後續回傳。 接著會讀取傳遞的使用者名稱`user`querystring 欄位擷取透過該使用者帳戶的相關資訊和`Membership.GetUser(username)`方法。 如果沒有使用者名稱提供透過查詢字串，或如果找不到指定的使用者，系統管理員會傳送回`ManageUsers.aspx`頁面。

`MembershipUser`物件的`UserName`值接著會顯示在`UserNameLabel`和`IsApproved`核取方塊根據`IsApproved`屬性值。

`MembershipUser`物件的[`LastLockoutDate`屬性](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx)傳回`DateTime`值，指出使用者上次鎖定。如果使用者永遠不會被鎖定，傳回的值會取決於成員資格提供者。 建立新的帳戶時，`SqlMembershipProvider`設定`aspnet_Membership`資料表的`LastLockoutDate`欄位設為`1754-01-01 12:00:00 AM`。 上述程式碼會顯示中的空字串`LastLockoutDateLabel`如果`LastLockoutDate`屬性出現在年份的前 2000年，否則的日期部分`LastLockoutDate`標籤中顯示屬性。 `UnlockUserButton'` s`Enabled`屬性設定為使用者的鎖定狀態，這表示當使用者遭到鎖定，才會啟用此按鈕。

請花一點時間來測試`UserInformation.aspx`透過瀏覽器的頁面。 當然，會需要在啟動`ManageUsers.aspx`和選取要管理的使用者帳戶。 在到達`UserInformation.aspx`，請注意，`IsApproved`如果使用者核准，才會檢查核取方塊。 如果使用者曾已經鎖定時，會顯示鎖定日期其最後一個。 只有當使用者目前已鎖定，會啟用 [解除鎖定使用者] 按鈕。選取或取消選取`IsApproved`核取方塊，或按一下 [解除鎖定使用者] 按鈕會導致回傳，但是因為我們尚未以使用者帳戶進行任何修改建立這些事件的事件處理常式。

返回 Visual Studio 並建立事件處理常式`IsApproved`核取方塊的`CheckedChanged`事件和`UnlockUser`按鈕的`Click`事件。 在`CheckedChanged`事件處理常式中，設定使用者的`IsApproved`屬性`Checked`屬性的核取方塊，然後儲存變更，透過對呼叫`Membership.UpdateUser`。 在`Click`事件處理常式，只需呼叫`MembershipUser`物件的`UnlockUser`方法。 在這兩個事件處理常式，會顯示適當的訊息中`StatusMessage`標籤。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>測試`UserInformation.aspx`頁面

備妥這些事件處理常式，以再次瀏覽頁面和未核准的使用者。 如圖 3 所示，您應該會看到訊息，指出頁面上使用者的簡短`IsApproved`已成功修改屬性。


[![Chris 已未核准](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**圖 3**: Chris 已未核准 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image9.png))


接下來，登出，然後嘗試使用者身分，登入的帳戶是只未經核准。 使用者未獲得核准，因為它們無法登入。 根據預設，登入控制項顯示相同的訊息，如果使用者無法登入，不論原因為何。 但在<a id="Tutorial6"> </a> [*驗證使用者認證對成員資格使用者存放區*](../membership/validating-user-credentials-against-the-membership-user-store-cs.md)教學課程中我們探討了增強的登入控制項來顯示更適當的訊息。 如圖 4 所示，Chris 會顯示訊息，說明他無法登入因為他的帳戶尚未核准。


[![Chris 無法登入因為 His 帳戶是未核准](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**圖 4**: Chris 無法登入因為 His 帳戶是未核准 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image12.png))


若要測試鎖定的功能，請嘗試將核准的使用者身分登入，但使用不正確的密碼。 重複此程序的必要次數直到使用者的帳戶已被鎖定。登入控制項也已更新以顯示自訂訊息，如果嘗試從鎖定的帳戶登入。 您知道該帳戶已經鎖定時一旦您開始看到登入頁面的下列訊息: 「 您的帳戶已被鎖定由於嘗試太多次無效登入。 請連絡系統管理員，讓您的帳戶解除鎖定。 」

返回`ManageUsers.aspx`頁面上，按一下 鎖定使用者的管理連結。 如圖 5 所示，您應該會看到中的值`LastLockedOutDateLabel`解除鎖定使用者按鈕應會啟用。 按一下 [解除鎖定的使用者] 按鈕，以解除鎖定使用者帳戶。 一旦您解除鎖定使用者，他們將能夠登入一次。


[![Dave 已被鎖定系統](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**圖 5**: Dave 已被鎖定出系統的 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>步驟 2： 指定新使用者的核准狀態

已核准的狀態為適用於您想要執行後新的使用者才能登入並存取使用者特定功能站台的某些動作的情況。 比方說，您可能會執行所有頁面，除了登入和註冊頁面，其中都是只有經過驗證的使用者能夠存取私用的網站。 但要是陌生人到達您的網站，尋找註冊 頁面上，並建立帳戶嗎？ 若要避免這種情況中，您無法移動註冊頁面，即可`Administration`資料夾，並要求系統管理員以手動方式建立的每個帳戶。 或者，您可以允許任何人註冊，但禁止站台存取，直到系統管理員核准的使用者帳戶。

根據預設，適用於 CreateUserWizard 控制項核准新的帳戶。 您可以使用控制項的這個行為設定[`DisableCreatedUser`屬性](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)。 將此屬性設定為`true`未核准新的使用者帳戶。

> [!NOTE]
> 預設適用於 CreateUserWizard 控制項自動登入新的使用者帳戶。 此行為取決於控制項的[`LoginCreatedUser`屬性](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)。 未核准的使用者無法登入站台，因為當`DisableCreatedUser`是`true`新的使用者帳戶未登入站台的值為何`LoginCreatedUser`屬性。


如果您要以程式設計方式建立新的使用者帳戶，透過`Membership.CreateUser`方法，建立未經核准的使用者帳戶使用其中一個多載會接受新使用者的`IsApproved`做為輸入參數的屬性值。

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>步驟 3： 核准使用者藉由驗證電子郵件地址

支援使用者帳戶的許多網站請勿核准新的使用者，直到他們確認它們在註冊時提供的電子郵件地址。 這個驗證程序通常用於需要唯一的、 已驗證的電子郵件地址和註冊程序中加入額外的步驟，來防堵 bot、 垃圾郵件，以及其他 ne'er-do-wells。 使用此模型中，當新的使用者註冊它們就會傳送電子郵件訊息，其中包含驗證頁面的連結。 瀏覽連結使用者證明他們收到電子郵件，因此，電子郵件地址，前提是有效的。 [驗證] 頁面會負責核准使用者。 這可能會自動發生，藉此核准到達此頁面上，任何使用者或使用者提供一些額外的資訊，例如之後才[CAPTCHA](http://en.wikipedia.org/wiki/Captcha)。

為了符合此工作流程，我們需要先更新帳戶建立頁面，讓新使用者都是未核准 」。 開啟`EnhancedCreateUserWizard.aspx`頁面`Membership`資料夾及設定適用於 CreateUserWizard 控制項的`DisableCreatedUser`屬性`true`。

接下來，我們必須設定傳送電子郵件給新的使用者，其中包含有關如何驗證其帳戶的指示適用於 CreateUserWizard 控制項。 特別是，我們將會以電子郵件中包含的連結`Verification.aspx`頁面 (其中我們尚未以建立)，並傳入新使用者的`UserId`透過查詢字串。 `Verification.aspx`頁面將會查詢指定的使用者，並標示核准。

### <a name="sending-a-verification-email-to-new-users"></a>驗證電子郵件傳送給新使用者

若要從適用於 CreateUserWizard 控制項傳送一封電子郵件，請設定其`MailDefinition`屬性適當地。 中所述<a id="Tutorial13"> </a>[上一個教學課程](recovering-and-changing-passwords-cs.md)，包含 「 變更密碼 」 和 「 Provider 控制項[`MailDefinition`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)的運作方式與適用於 CreateUserWizard 控制項。

> [!NOTE]
> 若要使用`MailDefinition`中的屬性，您必須指定郵件傳遞選項`Web.config`。 如需詳細資訊，請參閱[ASP.NET 中的 傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)。


開始建立新的電子郵件範本名為`CreateUserWizard.txt`中`EmailTemplates`資料夾。 使用下列文字範本：

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

設定`MailDefinition'`s`BodyFileName`屬性為"~ / EmailTemplates/CreateUserWizard.txt"及其`Subject`屬性 歡迎使用我的網站 ！ 請啟動您的帳戶。 」

請注意，`CreateUserWizard.txt`電子郵件範本包含`<%VerificationUrl%>`預留位置。 這適用於 URL`Verification.aspx`會置於頁面。 適用於 CreateUserWizard 會自動取代`<%UserName%>`和`<%Password%>`預留位置取代新的帳戶使用者名稱和密碼，但是沒有任何內建`<%VerificationUrl%>`預留位置。 我們需要手動將它取代適當的驗證 URL。

若要達成此目的，建立適用於 CreateUserWizard 的事件處理常式[`SendingMail`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx)並加入下列程式碼：

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

`SendingMail`事件引發之後`CreatedUser`事件，這表示，依時間上述事件處理常式都會執行新的使用者帳戶已建立。 我們可以存取新使用者的`UserId`藉由呼叫值`Membership.GetUser`方法，傳入`UserName`輸入適用於 CreateUserWizard 控制項。 接下來，會形成驗證 URL。 陳述式`Request.Url.GetLeftPart(UriPartial.Authority)`傳回`http://yourserver.com`部分 URL。`Request.ApplicationPath`傳回路徑的應用程式的根位置。 然後定義為 URL 的驗證`Verification.aspx?ID=userId`。 然後這些兩個字串串連來形成完整的 URL。 最後，電子郵件訊息內文 (`e.Message.Body`) 具有的所有項目`<%VerificationUrl%>`取代完整的 URL。

最後的結果是，新使用者都是未核准 」，這表示它們無法登入網站。 此外，它們會自動傳送電子郵件連結至驗證 URL （請參閱圖 6）。


[![新的使用者會收到一封電子郵件驗證 URL 的連結](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**圖 6**： 新的使用者會收到一封電子郵件驗證 URL 的連結 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> 適用於 CreateUserWizard 控制項的預設值適用於 CreateUserWizard 步驟會顯示訊息，通知的使用者他們的帳戶已建立並顯示 [繼續] 按鈕。 按一下此按鈕會將使用者帶至控制項所指定的 URL`ContinueDestinationPageUrl`屬性。 在適用於 CreateUserWizard`EnhancedCreateUserWizard.aspx`已傳送新的使用者`~/Membership/AdditionalUserInfo.aspx`，這時系統會提示使用者輸入他們的出生地、 首頁 URL 和簽章。 由於這項資訊可以只加入登入的使用者，它才會更新這個屬性，以將使用者傳送回站台的首頁 (`~/Default.aspx`)。 此外，`EnhancedCreateUserWizard.aspx`頁面或適用於 CreateUserWizard 步驟應該當做引數來通知使用者已傳送驗證電子郵件，並不會啟動其帳戶，直到它們遵循下列這封電子郵件中的指示。 我將這些修改保留為一項工作讀取器。


### <a name="creating-the-verification-page"></a>建立 [驗證] 頁面

我們最後一項工作是建立`Verification.aspx`頁面。 將此頁面新增至根資料夾中，將它與相關聯`Site.master`主版頁面。 因為我們已經完成大部分先前的內容頁面，新增站台之後，請移除參考的內容控制項`LoginContent`ContentPlaceHolder，讓內容頁面使用主版頁面的預設內容。

將標籤 Web 控制項加入`Verification.aspx`頁面上，將其`ID`至`StatusMessage`和清除其 text 屬性。 接下來，建立`Page_Load`事件處理常式並加入下列程式碼：

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

上述程式碼大量確認`UserId`提供透過查詢字串存在，它是有效`Guid`值和參考現有的使用者帳戶。 如果通過所有這些檢查，已核准的使用者帳戶。否則，就會顯示適當的狀態訊息。

圖 7 顯示`Verification.aspx`頁面上，當透過瀏覽器瀏覽。


[![新使用者的帳戶是立即核准](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**圖 7**: 新使用者的帳戶是立即核准 ([按一下以檢視完整大小的影像](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>總結

所有的成員資格使用者帳戶擁有判斷使用者是否可以登入網站的兩個狀態：`IsLockedOut`和`IsApproved`。 這兩個屬性必須是`true`登入使用者。

使用者的鎖定狀態為安全性量值可用來降低透過暴力方法分成多站台駭客的可能性。 具體來說，使用者便會鎖定了特定的時間範圍內無效的登入嘗試數目時。 可透過在成員資格提供者設定來設定這些界限`Web.config`。

已核准的狀態通常用於作為禁止新的使用者登入，直到發生某些動作。 可能是站台都需要系統管理員先核准新的帳戶，或如我們所見在步驟 3 中，確認電子郵件地址。

祝您程式設計 ！

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝...

許多有用的檢閱者已檢閱本教學課程系列。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](recovering-and-changing-passwords-cs.md)
> [下一頁](building-an-interface-to-select-one-user-account-from-many-vb.md)
