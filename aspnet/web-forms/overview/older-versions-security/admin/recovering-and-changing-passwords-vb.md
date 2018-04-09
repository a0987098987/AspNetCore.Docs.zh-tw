---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: 復原，以及變更密碼 (VB) |Microsoft 文件
author: rick-anderson
description: ASP.NET 包含兩個 Web 控制項的協助復原，以及變更密碼。 Provider 控制項可讓訪客復原他遺失的 pa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: cffe07eaea5144df82e56c989b0cde7cfd3d194a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="recovering-and-changing-passwords-vb"></a>復原，以及變更密碼 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET 包含兩個 Web 控制項的協助復原，以及變更密碼。 Provider 控制項可讓您復原密碼遺失的訪客。 ChangePassword 控制項可讓使用者更新其密碼。 如同其他登入相關的 Web 控制項中，我們已看到在這個教學課程系列的 Provider 和 ChangePassword 控制成員資格 framework 來重設或修改使用者的密碼在幕後的工作。


## <a name="introduction"></a>簡介

之間我銀行、 公用程式的公司、 電話公司，電子郵件帳戶及個人化的 web 入口網站的網站，我，大多數人來說，例如有數十個記住的密碼不同。 記下這些天這麼多的認證，是不常見的人員忘記其密碼。 若要將此列入考量，提供使用者帳戶的網站必須包含使用者可以復原其密碼的方式。 這個程序通常會包含產生新的隨機密碼，並以電子郵件傳送檔案的使用者的電子郵件地址。 接收新的密碼之後大部分使用者回到網站，並變更其密碼的隨機產生更好的一個。

ASP.NET 包含兩個 Web 控制項的協助復原，以及變更密碼。 Provider 控制項可讓您復原密碼遺失的訪客。 ChangePassword 控制項可讓使用者更新其密碼。 如同其他登入相關的 Web 控制項中，我們已看到在這個教學課程系列的 Provider 和 ChangePassword 控制成員資格 framework 來重設或修改使用者的密碼在幕後的工作。

在本教學課程中，我們會檢查使用這兩個控制項。 我們也將了解如何以程式設計方式變更和重設使用者的密碼，透過`MembershipUser`類別的`ChangePassword`和`ResetPassword`方法。

## <a name="step-1-helping-users-recover-lost-passwords"></a>步驟 1： 幫助使用者復原遺失的密碼

支援使用者帳戶的所有網站都需要為使用者提供某種機制以復原忘記的密碼。 好消息是在 ASP.NET 中實作這類功能是幫助您輕鬆這點受惠 Provider Web 控制項。 Provider 控制項呈現的介面會提示使用者輸入其使用者名稱，並在必要時，其安全性問題的答案。 它然後以電子郵件的使用者密碼。

> [!NOTE]
> 因為電子郵件訊息會以純文字方式透過網路傳輸有安全性風險與傳送使用者的密碼，透過電子郵件。


Provider 控制項包含三種檢視：

- **使用者名稱**-會提示他們的使用者名稱的訪客。 這是初始檢視。
- **問題**位使用者的使用者名稱和安全性問題顯示為文字，以及使用者輸入其安全性問題的答案的文字方塊。
- **成功**-顯示訊息，通知寄出其密碼的使用者。

顯示檢視，Provider 控制項所執行的動作取決於下列成員資格的組態設定：

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

成員資格架構`RequiresQuestionAndAnswer`設定指出是否使用者必須指定安全性問題與解答時的帳戶註冊。 如我們所述<a id="_msoanchor_1"> </a> [*建立使用者帳戶*](../membership/creating-user-accounts-vb.md)教學課程，如果`RequiresQuestionAndAnswer`則適用於 CreateUserWizard 的介面包含文字方塊中，則為 True （預設值）控制項的新使用者的安全性問題與解答。如果`RequiresQuestionAndAnswer`為 False 時，會收集任何這類資訊。 同樣地，如果`RequiresQuestionAndAnswer`是 True，則 Provider 控制項會顯示問題檢視的使用者輸入其使用者名稱後，只有當使用者輸入正確的安全性解答，會復原密碼。 如果`RequiresQuestionAndAnswer`為 False，不過，Provider 控制項直接從使用者檢視表移到順利完成檢視表。

使用者提供他的使用者名稱-或其使用者名稱和安全性解答，如果之後`RequiresQuestionAndAnswer`為 True-Provider 電子郵件傳送的使用者密碼。 如果`EnablePasswordRetrieval`選項設為 True，則使用者會以電子郵件傳送其目前的密碼。 如果設定為 False，`EnablePasswordReset`設為 True，則 Provider 控制項產生新隨機密碼對於使用者而言，與此新密碼給他們的電子郵件。 如果兩個`EnablePasswordRetrieval`和`EnablePasswordReset`是 False，Provider 控制擲回例外狀況。

> [!NOTE]
> 請記得，`SqlMembershipProvider`將使用者的密碼儲存在三種格式之一： Clear、 Hashed （預設值） 或已加密。 可用的儲存體機制取決於成員資格的設定。示範應用程式中使用 Hashed 密碼格式。 使用 Hashed 密碼格式時`EnablePasswordRetrieval`選項必須設定為 False，因為系統無法判斷使用者的實際的密碼，從資料庫中儲存的雜湊版本。


圖 1 說明如何 Provider 的介面以及行為會受到成員資格設定。


[![RequiresQuestionAndAnswer、 EnablePasswordRetrieval 和 EnablePasswordReset 影響 Provider 控制項的外觀和行為](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**圖 1**: `RequiresQuestionAndAnswer`， `EnablePasswordRetrieval`，和`EnablePasswordReset`影響 Provider 控制項的外觀和行為 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> 在<a id="_msoanchor_2"> </a> [*在 SQL Server 中建立成員資格結構描述*](../membership/creating-the-membership-schema-in-sql-server-vb.md)教學課程中我們設定成員資格提供者藉由設定`RequiresQuestionAndAnswer`為 True，`EnablePasswordRetrieval`至為 false，和`EnablePasswordReset`為 True。


### <a name="using-the-passwordrecovery-control"></a>使用 Provider 控制項

讓我們看看 Provider 控制項使用 ASP.NET 網頁中。 開啟`RecoverPassword.aspx`拖曳和 drop Provider 控制項從 [工具箱] 拖曳至設計工具之後，設定其`ID`至`RecoverPwd`。 登入和適用於 CreateUserWizard Web 控制項，例如 Provider 控制項的檢視會呈現豐富的複合介面，其中包含標籤、 文字方塊、 按鈕和驗證控制項。 您可以自訂檢視透過控制項的樣式屬性，或將檢視轉換成範本的外觀。 我將保留這為一項工作有興趣的讀取器。

當使用者造訪此頁她會輸入其使用者名稱，然後按一下 [提交] 按鈕。 因為我們設定`RequiresQuestionAndAnswer`屬性設為 True，在我們的成員資格組態設定，Provider 控制會接著顯示 [問題] 檢視。 使用者輸入其正確的安全性解答，並按一下 送出後，將使用者的密碼更新到隨機產生的一個 Provider 控制項，並以電子郵件傳送此電子郵件地址，檔案上的密碼。 這些全部都是可能不需要撰寫一行程式碼 ！

測試此頁面之前，沒有要傾向於組態的最後一個項目： 我們需要指定中的郵件傳遞設定`Web.config`。 Provider 控制項必須這些設定來傳送電子郵件。

郵件傳遞組態透過指定[`<system.net>`元素](https://msdn.microsoft.com/library/6484zdc1.aspx)的[`<mailSettings>`元素](https://msdn.microsoft.com/library/w355a94k.aspx)。 使用[`<smtp>`元素](https://msdn.microsoft.com/library/ms164240.aspx)指出傳遞方法，而且位址的預設值。 下列標記會設定為使用名為網路 SMTP 伺服器的郵件設定`smtp.example.com`連接埠 25 上且擁有的使用者名稱和密碼的使用者名稱/密碼認證。

> [!NOTE]
> `<system.net>` 是根目錄的子項目`<configuration>`項目和的同層級`<system.web>`。 因此，請不要將`<system.net>`內的項目`<system.web>`項目; 相反地，將其放置在相同層級。


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

除了在網路上使用 SMTP 伺服器，您也可以指定應該均存放要傳送的電子郵件訊息的收取目錄。

一旦您已設定 SMTP 設定，請瀏覽`RecoverPassword.aspx`透過瀏覽器的頁面。 第一次嘗試輸入在使用者存放區不存在的使用者名稱。 如圖 2 所示，Provider 控制項就會顯示訊息，指出 無法存取使用者資訊。 您可以透過控制項的自訂訊息的文字[`UserNameFailureText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)。


[![如果未輸入使用者名稱無效，會顯示錯誤訊息](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**圖 2**： 如果未輸入使用者名稱無效，會顯示的錯誤訊息 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image6.png))


現在請輸入使用者名稱。 使用知道帳戶的電子郵件地址，您可以存取其安全性回答您系統中的使用者名稱。 輸入使用者名稱並按下提交之後之後, Provider 控制項會顯示檢視的問題。 為使用使用者名稱 檢視中，如果您輸入了不正確回答 Provider 控制項會顯示錯誤訊息 （請參閱圖 3）。 使用[`QuestionFailureText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx)來自訂此錯誤訊息。


[![如果使用者輸入無效的安全性解答，會顯示錯誤訊息](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**圖 3**： 如果使用者輸入無效的安全性解答，會顯示的錯誤訊息 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image9.png))


最後，請輸入正確的安全性解答，並按一下 [提交]。 在幕後，Provider 控制產生隨機密碼、 將它指派給使用者帳戶、 傳送電子郵件通知使用者他們新的密碼 （請參閱圖 4），然後顯示 [成功] 檢視。


[![使用者會傳送一封電子郵件使用 His 的新密碼](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**圖 4**： 含有 His 的新密碼的電子郵件傳送的使用者 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>自訂電子郵件

Provider 控制項所傳送的預設電子郵件是保持單調，而不是 （請參閱圖 4）。 傳送訊息中指定的帳戶從`<smtp>`項目的`from`主體密碼的屬性和純文字本文：

請返回站台，並使用下列資訊登入。

使用者名稱：*使用者名稱*

密碼：*密碼*

此訊息可透過 Provider 控制項的事件處理常式以程式設計方式自訂[`SendingMail`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)，或以宣告方式透過[`MailDefinition`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx)。 讓我們來探索這兩個選項。

`SendingMail`事件引發之前會傳送電子郵件訊息，並以程式設計方式調整電子郵件訊息的最後機會。 此事件處理常式時引發此事件時，會傳遞給型別的物件[ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx)、 其`Message`屬性包含傳送電子郵件的參考。

建立事件處理常式`SendingMail`事件並加入下列程式碼，以程式設計方式將`webmaster@example.com`[副本] 清單。

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

也可以透過宣告式的方式設定電子郵件訊息。 Provider`MailDefinition`屬性是物件的型別[ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx)。 `MailDefinition`類別提供的電子郵件相關的屬性，包括主機`From`， `CC`， `Priority`， `Subject`， `IsBodyHtml`， `BodyFileName`，和其他人。 首先，設定[`Subject`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx)為比預設 （密碼），例如您的密碼已重設使用更具描述性的項目...

自訂建立個別的電子郵件範本檔案所需的電子郵件訊息的本文，其中包含主體的內容。 藉由建立新的資料夾中名為 「 網站啟動`EmailTemplates`。 接下來，將新的文字檔加入至名為此資料夾`PasswordRecovery.txt`並加入下列內容：

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

請記下的預留位置使用`<%UserName%>`和`<%Password%>`。 Provider 控制項會自動取代這些兩個預留位置使用者的使用者名稱和傳送電子郵件之前的復原的密碼。

最後，點`MailDefinition`的[`BodyFileName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)剛才所建立的電子郵件範本 (`~/EmailTemplates/PasswordRecovery.txt`)。

進行這些變更重新審視之後`RecoverPassword.aspx`頁面上，輸入您使用者名稱和安全性的答案。 您會收到應該看起來類似下面圖 5 中的電子郵件。 請注意，`webmaster@example.com`已副本會且已經過更新的主旨和本文。


[![已更新主旨、 主體和 [副本] 清單](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**圖 5**: 主體、 Body 和 [副本] 已更新清單 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image15.png))


若要傳送 HTML 格式的電子郵件將[ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) True （預設值為 False） 和更新電子郵件範本包含 HTML。

`MailDefinition`屬性不是唯一 Provider 類別。 我們將會看到在步驟 2 中，ChangePassword 控制項也提供`MailDefinition`屬性。 此外，適用於 CreateUserWizard 控制項包含這類屬性，您可以設定自動傳送給新使用者的 歡迎使用電子郵件訊息。

> [!NOTE]
> 目前沒有連結達到左側導覽列`RecoverPassword.aspx`頁面。 使用者只有興趣想要瀏覽此頁面，如果她無法成功登入此網站。 因此，更新`Login.aspx`頁面，即可加入連結`RecoverPassword.aspx`頁面。


### <a name="programmatically-resetting-a-users-password"></a>以程式設計方式重設使用者的密碼

當重設使用者密碼 Provider 控制呼叫`MembershipUser`物件的[`ResetPassword`方法](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)。 這個方法有兩個多載：

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -重設使用者的密碼。 如果使用這個多載`RequiresQuestionAndAnswer`為 False。
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -重設使用者密碼才提供*securityAnswer*正確無誤。 如果使用這個多載`RequiresQuestionAndAnswer`為 True。

這兩個多載會傳回新的隨機產生的密碼。

像使用其他方法中的成員資格 framework`ResetPassword`方法委派給已設定的提供者。 `SqlMembershipProvider`叫用`aspnet_Membership_ResetPassword`預存程序，傳遞使用者的使用者名稱、 新的密碼，並提供的密碼解答，其他欄位之間。 預存程序可確保符合密碼解答，並接著會更新使用者的密碼。

幾個低層級的實作附註：

- 鎖定的使用者無法重設其密碼。 不過，可能未核准的使用者。 我們將討論出鎖定，核准狀態的詳細<a id="_msoanchor_3"> </a> [ *Unlocking 和核准使用者*](unlocking-and-approving-user-accounts-vb.md)帳戶教學課程。
- 如果密碼解答不正確的使用者的失敗的密碼解答嘗試計數會遞增。 如果指定的多個無效的安全性解答嘗試發生指定的時間間隔內，使用者已鎖定。

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>產生 Word 如何的隨機密碼

電子郵件中的訊息數字 4 和 5 所示的隨機產生密碼建立的成員資格類別[`GeneratePassword`方法](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)。 這個方法會接受兩個整數輸入的參數-*長度*和*numberOfNonAlphanumericCharacters* -並至少傳回一個字串*長度*在長時間使用的字元至少*numberOfNonAlphanumericCharacters*非英數的字元數。 當這個方法中呼叫的成員資格類別或登入相關的 Web 控制項時，這兩個參數的值取決於成員資格設定`MinRequiredPasswordLength`和`MinRequiredNonalphanumericCharacters`我們分別設定為 7 和 1 的屬性。

`GeneratePassword`方法會使用強式密碼編譯亂數產生器，請確認有無偏差中有哪些隨機字元會選取。 此外，`GeneratePassword`是`Public`，這表示，您可以直接使用直接從您的 ASP.NET 應用程式如果您需要產生隨機字串或密碼。

> [!NOTE]
> `SqlMembershipProvider`類別一律會產生隨機密碼至少 14 個字元長，因此如果`MinRequiredPasswordLength`是小於 14，則會忽略其值。


## <a name="step-2-changing-passwords"></a>步驟 2： 變更密碼

隨機產生的密碼難以記住。 圖 4 所示的密碼，請考慮： `WWGUZv(f2yM:Bd`。 再試一次認可的記憶體 ！ 不用說，使用者會傳送這類隨機產生的密碼之後，她會想要變更密碼，以方便。

您可以使用 ChangePassword 控制項來建立使用者變更其密碼的介面。 很多 Provider 控制項，例如 ChangePassword 控制項包含兩種檢視： 變更密碼及成功與否。 變更密碼 檢視會提示使用者輸入其舊的和新密碼。 在提供正確的舊密碼和新的密碼符合最小長度以及非英數字元的需求，ChangePassword 控制項以更新使用者的密碼，並顯示順利完成檢視表。

> [!NOTE]
> ChangePassword 控制項由叫用來修改使用者的密碼`MembershipUser`物件的[`ChangePassword`方法](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)。 ChangePassword 方法接受兩個`String`輸入參數- *oldPassword*和*newPassword*-並更新使用者的帳戶與*newPassword*，假設提供*oldPassword*正確無誤。


開啟`ChangePassword.aspx`頁面上，並將 ChangePassword 控制項加入至頁面上，其命名為`ChangePwd`。 此時，設計 檢視應顯示 變更密碼 （請參閱圖 6） 的檢視。 像與 Provider 控制項，您可以切換檢視透過控制項的智慧標籤。 此外，這些檢視的外觀也可透過各種的樣式屬性，或將它們轉換成範本的自訂。


[![ChangePassword 控制項加入至網頁](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**圖 6**: ChangePassword 控制項加入至網頁 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image18.png))


ChangePassword 控制項可以更新目前登入使用者的密碼*或*另一個，指定使用者的密碼。 如圖 6 所示，預設的 [變更密碼] 檢視會呈現只有三個文字方塊控制項： 一個適用於舊密碼，而兩個新的密碼。 這個預設介面用來更新目前登入使用者的密碼。

若要使用 ChangePassword 控制更新其他使用者的密碼，將控制項的[`DisplayUserName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx)為 True。 這樣將第四個文字方塊加入至頁面上，若要變更其密碼提示使用者的使用者名稱。

設定`DisplayUserName`至很有用，如果您想要讓出記錄的使用者變更其密碼，而不需要登入則為 True。 個人，我認為有沒有任何問題然後再讓她變更其密碼，要求使用者必須登入。 因此，保留`DisplayUserName`設為 False （預設值）。 在這項決策，不過，我們基本上會限制匿名使用者，使其無法到達此頁面。 更新站台的 URL 授權規則以拒絕匿名使用者造訪`ChangePassword.aspx`。 如果您需要重新整理您的 URL 授權規則語法上的記憶體，請參閱上一步<a id="_msoanchor_4"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-vb.md)教學課程。

> [!NOTE]
> 看起來好像，`DisplayUserName`屬性可用於讓系統管理員能夠變更其他使用者的密碼。 不過，即使`DisplayUserName`設為的 True，必須知道並輸入正確的舊密碼。 我們將討論可讓系統管理員變更使用者的密碼，在步驟 3 中的技術。


請瀏覽`ChangePassword.aspx`透過瀏覽器頁面，並變更您的密碼。 請注意，是否您輸入新的密碼不符合密碼長度以及非英數字元的成員資格組態中指定的需求，會顯示錯誤訊息 （請參閱圖 7）。


[![ChangePassword 控制項加入至網頁](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**圖 7**: ChangePassword 控制項加入至網頁 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image21.png))


在輸入正確的舊密碼和有效新密碼，登入使用者的密碼變更，並顯示 [成功] 檢視。

### <a name="sending-a-confirmation-email"></a>傳送確認電子郵件

根據預設，ChangePassword 控制項不會傳送電子郵件訊息給剛剛更新其密碼的使用者。 如果您想要傳送電子郵件，只要設定控制項的`MailDefinition`屬性。 讓我們設定 ChangePassword 控制項，讓使用者傳送 HTML 格式的電子郵件，其中包含新的密碼。

藉由建立新的檔案中開始`EmailTemplates`資料夾名為`ChangePassword.htm`。 加入下列標記：

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

接下來，將 ChangePassword 控制項的`MailDefinition`屬性的`BodyFileName`， `IsBodyHtml`，和`Subject`屬性 ~ / EmailTemplates/ChangePassword.htm、 True 和您的密碼已變更 ！ 分別。

進行這些變更之後，再次瀏覽的頁面，並再次變更您的密碼。 這次 ChangePassword 控制項就會將自訂的 HTML 格式的電子郵件傳送至檔案上的使用者的電子郵件地址 （請參閱圖 8）。


[![電子郵件訊息，通知使用者，其密碼已經變更](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**圖 8**: 電子郵件訊息通知使用者，其密碼已經變更 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>步驟 3： 讓系統管理員能夠變更使用者的密碼

中支援使用者帳戶的應用程式的常見功能是為其他使用者的密碼變更的系統管理使用者的能力。 有時候這項功能需要，因為系統缺乏使用者變更他們自己的密碼的能力。 在這種情況下，使用者可以復原忘記的密碼的唯一方式是將它們指派新密碼管理員。 使用新密碼與變更密碼的控制項，不過，系統管理使用者需要不忙碌本身與變更使用者的密碼，因為使用者無法執行此本身。

不過，如果您的用戶端堅持系統管理使用者應該能夠變更其他使用者的密碼嗎？ 不幸的是工作的，加入這項功能，可以是工作的。 若要變更使用者的密碼，必須提供舊的和新密碼給`MembershipUser`物件的`ChangePassword`方法，但系統管理員不應該有知道使用者的密碼，才能修改它。

解決方法之一是第一次重設使用者的密碼，並將它變更為新的密碼類似下列的程式碼：

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

此程式碼會啟動擷取相關資訊*username*，這是系統管理員想要變更其密碼的使用者。 下一步`ResetPassword`叫用方法時，哪些指派及使用者的新的隨機密碼。 是由方法傳回此隨機產生的密碼，並將其儲存在變數`resetPwd`。 既然我們已經知道使用者的密碼，我們可以變更它，透過對呼叫`ChangePassword`。

問題在於，此程式碼只有在成員資格系統組態設定使`RequiresQuestionAndAnswer`為 False。 如果`RequiresQuestionAndAnswer`為 True，因為它是我們的應用程式，然後在`ResetPassword`方法必須傳送的安全性解答，否則會擲回例外狀況。

如果成員資格 framework 設定為需要的安全性問題和解答，且您的用戶端尚未堅持系統管理員可以變更使用者的密碼，您會有三個選項：

- 在空中中擲回雙手，告訴您的用戶端，這是只有一個項目無法完成。
- 設定`RequiresQuestionAndAnswer`設為 False。 這會導致較不安全的應用程式。 想像一下，讓使用者能夠存取另一位使用者的電子郵件收件匣。 可能是盜用的使用者離開辦公桌去用餐，且未鎖定工作站，或可能已經從公用的終端機存取他們的電子郵件及未登出。在任一情況下，讓使用者可以瀏覽`RecoverPassword.aspx`頁面上，輸入使用者的使用者名稱。 系統將然後傳送電子郵件的復原的密碼而不提示的安全性解答。
- 略過成員資格 framework 和直接與 SQL Server 資料庫的工作所建立的抽象層。 成員資格結構描述包含名為預存程序`aspnet_Membership_SetPassword`，設定使用者的密碼，而且不需要的安全性解答或舊的密碼才能完成其工作。

這些選項都特別吸引人，但這是開發人員生活實記如何有時會。

我繼續發生，以及實作第三種方法，撰寫程式碼會略過`Membership`和`MembershipUser`類別，以及直接對其進行`SecurityTutorials`資料庫。

> [!NOTE]
> 藉由直接使用資料庫，是大家所成員資格 framework 所提供的封裝。 這項決策繫結合作對我們`SqlMembershipProvider`，讓我們小於移植的程式碼。 此外，這段程式碼可能無法如預期般在未來版本的 ASP.NET 成員資格結構描述變更。 這個方法會因應措施，並像大部分的因應措施，不是最佳作法的範例。


程式碼已部分不適當的位元並相當冗長。 因此，我不想干擾深入檢查它與本教學課程。 如果您有興趣進一步瞭解，請下載的程式碼，此教學課程和瀏覽`~/Administration/ManageUsers.aspx`頁面。 此頁面上，我們在建立<a id="_msoanchor_5"> </a>[前述教學課程](building-an-interface-to-select-one-user-account-from-many-vb.md)，列出每個使用者。 我已更新加入連結 GridView`UserInformation.aspx`頁面上，將傳遞查詢字串透過所選的使用者的使用者名稱。 `UserInformation.aspx`頁面會顯示選取的使用者和文字方塊的相關資訊變更其密碼 （請參閱圖 9）。

輸入新密碼，確認在第二個文字方塊中，再按一下 [更新使用者] 按鈕之後, 回傳展示和`aspnet_Membership_SetPassword`預存程序叫用時，更新使用者的密碼。 建議這些讀取器這項功能有興趣更熟悉的程式碼，以及擴充功能，以包括傳送電子郵件給使用者變更其密碼再試一次。


[![系統管理員可以變更使用者的密碼](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**圖 9**: 系統管理員可以變更使用者的密碼 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx`頁面上目前只適用於如果成員資格 framework 設定為清除或 Hashed 格式儲存密碼。 雖然系統邀請您加入這項功能，它會缺少將新的密碼加密的程式碼。 建議您將必要的程式碼的方式是使用像是解編程式[Reflector](http://www.aisto.com/roeder/dotnet/)檢查.NET Framework; 中方法的原始程式碼先檢查`SqlMembershipProvider`類別的`ChangePassword`方法。 這是用來撰寫的程式碼建立的雜湊密碼的技巧。


## <a name="summary"></a>總結

ASP.NET 提供了兩個控制項，可協助使用者管理他們的密碼。 Provider 控制項可用於忘記其密碼的人。 根據成員資格架構的設定，使用者可能被電子現有的密碼或新的隨機產生的密碼。 ChangePassword 控制項可讓使用者更新密碼。

登入和適用於 CreateUserWizard 控制項，例如 Provider 和 ChangePassword 控制項轉譯豐富的使用者介面，而不需要撰寫上邊的宣告式標記或程式碼行。 如果預設的使用者介面不符合您的需求，您可以自訂它透過不同的樣式屬性。 或者，將控制項的介面可能會轉換成範本，更進一步控制程度。 在幕後這些控制項使用的成員資格應用程式開發介面，叫用`MembershipUser`物件的`ResetPassword`和`ChangePassword`方法。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ChangePassword 控制項快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Provider 控制項快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [在 ASP.NET 中傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` 常見問題集](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者包括 Michael Emmings 和 Suchi Banerjee。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [下一頁](unlocking-and-approving-user-accounts-vb.md)
