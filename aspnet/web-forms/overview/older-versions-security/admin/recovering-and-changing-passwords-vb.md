---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: 復原及變更密碼 (VB) |Microsoft Docs
author: rick-anderson
description: ASP.NET 包含兩個 Web 控制項，可協助復原及變更密碼。 Provider 控制項可讓造訪者得以復原他遺失的 pa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: 68b70708904a46e31639331bbfd0dd31240ea421
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393992"
---
<a name="recovering-and-changing-passwords-vb"></a>復原及變更密碼 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET 包含兩個 Web 控制項，可協助復原及變更密碼。 Provider 控制項可讓造訪者得以復原遺失的密碼。 ChangePassword 控制項可讓使用者更新其密碼。 在此教學課程系列，Provider 所見如同其他的登入相關的 Web 控制項和 ChangePassword 控制項與成員資格架構，來重設或修改使用者的密碼在幕後的工作。


## <a name="introduction"></a>簡介

之間的我的銀行、 公共事業公司、 電話公司、 電子郵件帳戶和個人化的 web 入口網站的網站，我，大多數人來說，例如有數十種不同的密碼，請記住。 記住這幾天內這麼多的認證，並不是常有人將忘記其密碼。 為了說明這一點，提供使用者帳戶的網站需要包含讓使用者能夠復原其密碼。 此程序通常需要產生新的隨機密碼，並以電子郵件傳送檔案的使用者的電子郵件地址。 接收到其新的密碼後大部分使用者回到網站，並變更其密碼，從更令人印象深刻的一個隨機產生的一個。

ASP.NET 包含兩個 Web 控制項，可協助復原及變更密碼。 Provider 控制項可讓造訪者得以復原遺失的密碼。 ChangePassword 控制項可讓使用者更新其密碼。 在此教學課程系列，Provider 所見如同其他的登入相關的 Web 控制項和 ChangePassword 控制項與成員資格架構，來重設或修改使用者的密碼在幕後的工作。

在本教學課程中，我們會檢查使用這兩個控制項。 我們也將了解如何以程式設計方式變更和重設使用者的密碼，透過`MembershipUser`類別的`ChangePassword`和`ResetPassword`方法。

## <a name="step-1-helping-users-recover-lost-passwords"></a>步驟 1： 可協助使用者復原遺失的密碼

支援使用者帳戶的所有網站都需要為使用者提供一些機制來修復遺忘的密碼。 好消息是，在 ASP.NET 中實作這類功能是由於 Provider Web 控制項變得輕而易舉。 Provider 控制項呈現的介面會提示使用者輸入其使用者名稱，並在必要時，其安全性問題的答案。 它然後傳送電子郵件給使用者其密碼。

> [!NOTE]
> 因為透過網路以純文字傳送電子郵件訊息有安全性風險相關傳送使用者的密碼，透過電子郵件。


Provider 控制項是由三個檢視所組成：

- **使用者名稱**-會提示他們的使用者名稱的訪客。 這是初始檢視。
- **問題**-文字，以及使用者輸入他的安全性問題的答案文字方塊以顯示使用者的使用者名稱和安全性問題。
- **成功**-會顯示訊息，通知已傳送電子郵件密碼的使用者。

顯示檢視和 Provider 控制項所執行的動作取決於下列成員資格的組態設定：

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

成員資格架構`RequiresQuestionAndAnswer`設定指出是否時，使用者必須指定安全性問題與解答的帳戶註冊。 如我們所述<a id="_msoanchor_1"> </a> [*建立使用者帳戶*](../membership/creating-user-accounts-vb.md)教學課程中，如果`RequiresQuestionAndAnswer`則 CreateUserWizard 介面包含文字方塊中，則為 True （預設值）新使用者的安全性問題和答案; 控制項如果`RequiresQuestionAndAnswer`為 False 時，會收集任何這類資訊。 同樣地，如果`RequiresQuestionAndAnswer`是 True，則問題檢視使用者輸入其使用者名稱後 Provider 控制項顯示; 只有當使用者輸入正確的安全性解答復原密碼。 如果`RequiresQuestionAndAnswer`為 False，不過，Provider 控制項直接從使用者名稱 檢視將移至 成功 檢視。

使用者提供他的使用者名稱-或其使用者名稱和安全性的解答，如果之後`RequiresQuestionAndAnswer`Provider 是 True-傳送電子郵件給使用者其密碼。 如果`EnablePasswordRetrieval`選項設為 True，則使用者傳送電子郵件，其目前的密碼。 如果設定為 False 和`EnablePasswordReset`設為 True，則 Provider 控制項產生新隨機密碼對於使用者而言，與這個新的密碼，給他們的電子郵件。 如果兩個`EnablePasswordRetrieval`和`EnablePasswordReset`為 False，Provider 控制會擲回的例外狀況。

> [!NOTE]
> 請記得，`SqlMembershipProvider`將使用者的密碼儲存在三種格式之一： 清除、 雜湊 （預設值） 或加密。 使用的儲存體機制取決於成員資格的設定;示範應用程式使用 Hashed 密碼格式。 使用 Hashed 密碼格式時`EnablePasswordRetrieval`選項必須設定為 False，因為系統無法判斷使用者的實際密碼雜湊儲存在資料庫中的版本。


[圖 1] 說明 Provider 的介面和行為方式會影響成員資格設定。


[![RequiresQuestionAndAnswer、 EnablePasswordRetrieval，以及 EnablePasswordReset 影響 Provider 控制項的外觀和行為](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**圖 1**: `RequiresQuestionAndAnswer`， `EnablePasswordRetrieval`，以及`EnablePasswordReset`影響 Provider 控制項的外觀和行為 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> 在  <a id="_msoanchor_2"> </a> [*在 SQL Server 中建立成員資格結構描述*](../membership/creating-the-membership-schema-in-sql-server-vb.md)我們透過設定來設定成員資格提供者的教學課程`RequiresQuestionAndAnswer`設為 True，`EnablePasswordRetrieval`來為 false，和`EnablePasswordReset`設為 True。


### <a name="using-the-passwordrecovery-control"></a>使用 Provider 控制項

讓我們看看使用 ASP.NET 網頁中的 Provider 控制項。 開啟`RecoverPassword.aspx`並拖曳和置放 Provider 控制項從 [工具箱] 拖曳至設計工具; 設定其`ID`至`RecoverPwd`。 登入及 CreateUserWizard Web 控制項，例如 Provider 控制項的檢視會呈現豐富的複合介面包含標籤、 文字方塊、 按鈕和驗證控制項。 您可以自訂檢視透過控制項的樣式屬性，或藉由將檢視轉換成範本的外觀。 我不要更動此練習中，有興趣的讀者。

當使用者造訪此頁面她會輸入其使用者名稱，然後按一下 [提交] 按鈕。 因為我們設定`RequiresQuestionAndAnswer`屬性設為 True，在我們的成員資格組態設定，Provider 控制項將接著顯示 [問題] 檢視。 使用者輸入其正確的安全性解答，並按一下 送出之後，將使用者的密碼更新為隨機產生一個 Provider 控制項，並將其電子郵件傳送此電子郵件地址，檔案的密碼。 所有這些都是可能沒有我們不必撰寫一行程式碼 ！

在測試此頁面之前，沒有要容易的組態的最後一個項目： 我們需要指定中的郵件傳遞設定`Web.config`。 Provider 控制項要依賴這些設定來傳送電子郵件。

透過所指定的郵件傳遞組態[`<system.net>`項目](https://msdn.microsoft.com/library/6484zdc1.aspx)的[`<mailSettings>`項目](https://msdn.microsoft.com/library/w355a94k.aspx)。 使用[`<smtp>`項目](https://msdn.microsoft.com/library/ms164240.aspx)來指出傳遞方法和地址的預設值。 下列標記會設定為使用名為網路 SMTP 伺服器的郵件設定`smtp.example.com`連接埠 25 上且擁有的使用者名稱和密碼的使用者名稱/密碼認證。

> [!NOTE]
> `<system.net>` 是根的子元素`<configuration>`項目和的同層級`<system.web>`。 因此，請勿`<system.net>`內的項目`<system.web>`項目; 相反地，將它放在相同的層級。


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

除了在網路上使用 SMTP 伺服器，您也可以指定應該均存放要傳送的電子郵件訊息的收取目錄。

一旦您已設定的 SMTP 設定，請瀏覽`RecoverPassword.aspx`透過瀏覽器的頁面。 第一次嘗試輸入在使用者存放區不存在的使用者名稱。 如 圖 2 所示，Provider 控制項就會顯示訊息，指出 無法存取使用者資訊。 您可以透過控制項的自訂訊息的文字[`UserNameFailureText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)。


[![如果輸入無效的使用者名稱，就會顯示一則錯誤訊息](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**圖 2**： 如果您輸入使用者名稱無效，會顯示錯誤訊息 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image6.png))


現在，請輸入使用者名稱。 知道使用者的電子郵件地址，您可以存取，以及其安全性回答您的系統中的帳戶名稱使用。 輸入使用者名稱，然後按一下 [送出之後, Provider 控制項會顯示其問題] 檢視。 為使用使用者名稱 檢視中，如果您輸入不正確回答 Provider 控制項會顯示的錯誤訊息 （請參閱 圖 3）。 使用[`QuestionFailureText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx)來自訂此錯誤訊息。


[![如果使用者輸入無效的安全性解答，就會顯示一則錯誤訊息](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**圖 3**： 如果使用者輸入無效的安全性解答，就會顯示錯誤訊息 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image9.png))


最後，輸入正確的安全性解答，然後按一下 提交。 在幕後，Provider 控制產生的隨機密碼，將它指派給使用者帳戶，傳送電子郵件通知使用者的新密碼 （請參閱 圖 4），然後顯示 成功 檢視。


[![使用者會傳送一封電子郵件與他的新密碼](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**圖 4**： 傳送電子郵件與他的新密碼的使用者 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>自訂電子郵件

Provider 控制項所傳送的預設電子郵件是而晦暗，而且 （請參閱 圖 4）。 從指定的帳戶傳送訊息`<smtp>`項目的`from`主旨為密碼的屬性和純文字主體：

請返回該網站，然後使用下列資訊來登入。

使用者名稱：*使用者名稱*

密碼：*密碼*

此訊息可以透過程式設計方式自訂透過 Provider 控制項的事件處理常式[`SendingMail`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)，或以宣告方式透過[`MailDefinition`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx)。 讓我們來探索這兩種選項。

`SendingMail`電子郵件訊息傳送，且最後一個機會來以程式設計方式調整電子郵件訊息之前，引發事件。 型別的物件時，引發這個事件時，要傳遞事件處理常式[ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx)，其`Message`屬性包含傳送電子郵件的參考。

建立事件處理常式`SendingMail`事件，並新增下列程式碼，以程式設計方式將`webmaster@example.com`CC 清單。

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

也可以透過宣告式的方式設定電子郵件訊息。 Provider`MailDefinition`屬性是物件型別的[ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx)。 `MailDefinition`類別提供了一堆電子郵件相關的屬性，包括`From`， `CC`， `Priority`， `Subject`， `IsBodyHtml`， `BodyFileName`，和其他人。 首先，設定[`Subject`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx)為更具描述性，比使用預設 （密碼），例如您的密碼已重設...

若要自訂我們需要建立個別的電子郵件範本檔案的電子郵件訊息的主體包含主體的內容。 開始建立新的資料夾中名為網站`EmailTemplates`。 接下來，將新的文字檔新增至名為此資料夾`PasswordRecovery.txt`並新增下列內容：

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

請注意，預留位置`<%UserName%>`和`<%Password%>`。 Provider 控制會自動取代這些兩個預留位置，使用者的使用者名稱和復原的密碼，再傳送電子郵件。

最後，指向`MailDefinition`的[`BodyFileName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)我們剛剛建立的電子郵件範本 (`~/EmailTemplates/PasswordRecovery.txt`)。

進行這些變更重新審視之後`RecoverPassword.aspx`頁面，然後輸入您使用者名稱和安全性的答案。 您會收到應該看起來像圖 5 中的電子郵件。 請注意， `webmaster@example.com` CC 會和，已更新的主旨和本文。


[![更新主旨、 主體和 [副本] 清單](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**圖 5**: 主旨、 主體和 [副本] 已更新清單 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image15.png))


若要傳送 HTML 格式的電子郵件設定[ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) True （預設值為 False） 和更新電子郵件範本，以包含 HTML。

`MailDefinition`屬性不是唯一的 Provider 類別。 我們將在步驟 2 中所看到的 ChangePassword 控制項也提供`MailDefinition`屬性。 此外，CreateUserWizard 控制項包含這類屬性，您可以設定自動傳送給新使用者的 歡迎使用電子郵件訊息。

> [!NOTE]
> 目前沒有任何連結在左側導覽中用來觸達`RecoverPassword.aspx`頁面。 如果她無法成功登入此網站，瀏覽此頁面只會在意使用者。 因此，更新`Login.aspx`頁面，即可包含的連結`RecoverPassword.aspx`頁面。


### <a name="programmatically-resetting-a-users-password"></a>以程式設計方式重設使用者的密碼

當重設使用者的密碼 Provider 控制呼叫`MembershipUser`物件的[`ResetPassword`方法](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)。 這個方法有兩個多載：

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -重設使用者的密碼。 如果使用這個多載`RequiresQuestionAndAnswer`為 False。
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -重設使用者的密碼才提供*securityAnswer*正確無誤。 如果使用這個多載`RequiresQuestionAndAnswer`為 True。

這兩個多載會傳回新的隨機產生密碼。

使用其他方法，在 成員資格架構，例如`ResetPassword`方法會委派到設定的提供者。 `SqlMembershipProvider`叫用`aspnet_Membership_ResetPassword`預存程序，並傳遞使用者的使用者名稱、 新的密碼和提供的密碼答案，在其他欄位。 預存程序可確保密碼解答比對，，然後更新 使用者的密碼。

幾個低層級的實作附註：

- 鎖定的使用者無法重設其密碼。 不過，有可能會在未經核准的使用者。 我們將討論出鎖定，並核准狀態更詳細地<a id="_msoanchor_3"> </a> [ *Unlocking 和核准使用者*](unlocking-and-approving-user-accounts-vb.md)帳戶教學課程。
- 如果不正確的密碼解答，使用者的密碼解答嘗試計數會遞增。 如果指定的多個無效的安全性解答嘗試就會發生在指定的時間範圍內，使用者遭到鎖定。

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>產生方式的隨機密碼文字

電子郵件中的訊息數字 4 和 5 所示的隨機產生密碼藉由 Membership 類別[`GeneratePassword`方法](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)。 這個方法會接受兩個整數輸入的參數-*長度*並*numberOfNonAlphanumericCharacters* -並至少傳回一個字串*長度*在長時間使用字元至少*numberOfNonAlphanumericCharacters*非英數的字元數。 當這個方法從呼叫內的成員資格類別或登入相關的 Web 控制項時，這兩個參數的值取決於成員資格的組態`MinRequiredPasswordLength`和`MinRequiredNonalphanumericCharacters`我們分別設定為 7 和 1 的屬性。

`GeneratePassword`方法會使用強式密碼編譯亂數產生器，以確保在已選取哪些隨機字元所組成，沒有任何偏差。 此外，`GeneratePassword`是`Public`，也就是說，您可以使用它直接從您的 ASP.NET 應用程式若要產生隨機字串或密碼。

> [!NOTE]
> `SqlMembershipProvider`類別一律會產生隨機密碼，至少 14 個字元，因此如果`MinRequiredPasswordLength`是小於 14，則會忽略其值。


## <a name="step-2-changing-passwords"></a>步驟 2： 變更密碼

隨機產生密碼難以記住。 [圖 4] 所示的密碼，請考慮： `WWGUZv(f2yM:Bd`。 嘗試認可的記憶體 ！ 不用說，這類的隨機產生密碼傳送給使用者之後，她會想要將密碼變更為更令人印象深刻的項目。

若要建立要變更其密碼的使用者介面使用 ChangePassword 控制項。 更 Provider 控制項，例如 ChangePassword 控制項包含的兩個檢視： 變更密碼] 和 [成功。 變更密碼 檢視會提示使用者輸入其舊和新密碼。 在提供正確的舊密碼和新的密碼符合最小長度以及非英數的字元需求，ChangePassword 控制項更新使用者的密碼，並顯示 [成功] 檢視。

> [!NOTE]
> ChangePassword 控制項修改使用者的密碼，藉由叫用`MembershipUser`物件的[`ChangePassword`方法](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)。 ChangePassword 方法接受兩個`String`輸入參數- *oldPassword*並*newPassword*-並更新使用者的帳戶具有*newPassword*，假設所提供*oldPassword*正確無誤。


開啟`ChangePassword.aspx`頁面上，並將 ChangePassword 控制項新增至頁面上，將它命名為`ChangePwd`。 此時，設計 檢視應該會顯示 變更密碼 （請參閱 圖 6） 的檢視。 例如與 Provider 控制項，您可以切換透過控制項的智慧標籤的檢視。 此外，這些檢視的外觀會透過各種的樣式屬性，或將它們轉換成範本，將可自訂的。


[![ChangePassword 控制項加入頁面](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**圖 6**: ChangePassword 控制項加入網頁 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image18.png))


ChangePassword 控制項可以更新目前登入使用者的密碼*或*另一個，指定使用者的密碼。 如 [圖 6] 所示，預設的 [變更密碼] 檢視會呈現只需要三個文字方塊控制項： 一個用於舊的密碼，兩個新的密碼。 此預設介面用來更新目前登入使用者的密碼。

若要使用 ChangePassword 控制項更新另一位使用者的密碼，將控制項的[`DisplayUserName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx)設為 True。 如此一來，是在頁面上，若要變更其密碼提示使用者的使用者名稱中加入第四個文字方塊中。

設定`DisplayUserName`要適合在您想要讓出記錄的使用者變更其密碼，而不需要登入則為 True。 我個人認為沒有問題需要登入使用者，然後才允許她在變更其密碼。 因此，保留`DisplayUserName`設為 False （預設值）。 在進行這項決策，不過，我們基本上均無法到達此頁面的匿名使用者。 更新站台的 URL 授權規則以拒絕匿名使用者造訪`ChangePassword.aspx`。 如果您需要重新整理您的 URL 授權規則語法上的記憶體，請參閱上一步<a id="_msoanchor_4"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-vb.md)教學課程。

> [!NOTE]
> 看起來好像沒，`DisplayUserName`屬性可用於讓系統管理員能夠變更其他使用者的密碼。 不過，即使`DisplayUserName`設為 True，必須知道並輸入正確的舊密碼。 我們會討論可讓系統管理員變更使用者的密碼，在步驟 3 中的技術。


請瀏覽`ChangePassword.aspx`透過瀏覽器頁面，並變更您的密碼。 請注意，是否您輸入新的密碼無法滿足密碼長度和成員資格設定中指定的非英數的字元需求，會顯示一則錯誤訊息 （請參閱 圖 7）。


[![ChangePassword 控制項加入頁面](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**圖 7**: ChangePassword 控制項加入網頁 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image21.png))


在輸入正確的舊密碼和有效新密碼，登入使用者的密碼變更，並顯示 [成功] 檢視。

### <a name="sending-a-confirmation-email"></a>傳送確認電子郵件

根據預設，ChangePassword 控制項不會對剛更新其密碼的使用者傳送電子郵件訊息。 如果您想要傳送電子郵件，直接設定控制項的`MailDefinition`屬性。 讓我們設定 ChangePassword 控制項，讓使用者傳送 HTML 格式的電子郵件，其中包含新的密碼。

藉由建立新檔案中，啟動`EmailTemplates`名為資料夾`ChangePassword.htm`。 加入下列標記：

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

接下來，將 ChangePassword 控制項的`MailDefinition`屬性的`BodyFileName`， `IsBodyHtml`，和`Subject`屬性 ~ EmailTemplates/ChangePassword.htm，True，且您的密碼已變更 / ！ 分別。

進行這些變更之後，重新瀏覽的頁面，並再次變更您的密碼。 此時，ChangePassword 控制項就會將自訂的 HTML 格式的電子郵件傳送檔案的使用者的電子郵件地址 （請參閱 圖 8）。


[![電子郵件訊息，通知使用者，其密碼已經變更](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**圖 8**: 電子郵件通知使用者，其密碼已經變更 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>步驟 3： 可讓系統管理員變更使用者的密碼

在支援使用者帳戶的應用程式中的常見功能是針對系統管理使用者，若要變更其他使用者的密碼的能力。 有時候這項功能是必要，因為系統缺乏可讓使用者變更其自己的密碼。 在此情況下，使用者復原遺忘的密碼的唯一方式是將它們指派新密碼管理員。 Provider 和 ChangePassword 控制項，不過，系統管理使用者需要不忙碌本身與變更使用者的密碼，因為使用者都能執行此本身。

但是，如果您的用戶端必須將系統管理使用者應該能夠變更其他使用者的密碼？ 不幸的是，新增這項功能，可以是一些工作。 若要變更使用者的密碼，必須提供舊的和新密碼給`MembershipUser`物件的`ChangePassword`方法，但系統管理員應該要知道使用者的密碼，才能修改它。

一個解決方法是先重設使用者的密碼，然後將它變更為新的密碼，使用程式碼，如下所示：

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

此程式碼一開始會擷取相關資訊*username*，這是系統管理員想要變更其密碼的使用者。 接下來，`ResetPassword`叫用方法時，哪些指派和新的隨機密碼的使用者。 是由方法傳回此隨機產生的密碼，並將其儲存在變數`resetPwd`。 既然我們已經知道使用者的密碼，我們就可以將它變更，透過對呼叫`ChangePassword`。

問題是，此程式碼僅適用於成員資格系統組態設定，`RequiresQuestionAndAnswer`為 False。 如果`RequiresQuestionAndAnswer`為 True，因為它是我們的應用程式，則`ResetPassword`方法必須傳送的安全性解答，否則它將會擲回例外狀況。

如果成員資格架構已設定為需要的安全性問題與解答，且您的用戶端尚未要求系統管理員可以變更使用者的密碼，您會有三個選項：

- 在空中飛行時擲回您手裡，告訴您的用戶端，這是無法完成的只是一件事。
- 設定`RequiresQuestionAndAnswer`設為 False。 這會導致較不安全的應用程式。 想像一下老道的惡意使用者能夠存取另一位使用者的電子郵件收件匣。 遭盜用的使用者可能離開辦公桌去吃午餐，且未鎖定自己的工作站，或可能從公用的終端機存取其電子郵件及未登出。在任一情況下，惡意使用者可以瀏覽`RecoverPassword.aspx`頁面，然後輸入使用者的使用者名稱。 系統會然後傳送電子郵件已復原的密碼不會提示輸入安全性解答。
- 略過成員資格架構和直接與 SQL Server 資料庫的工作所建立的抽象層。 成員資格結構描述包含名為預存程序`aspnet_Membership_SetPassword`，設定使用者的密碼，而且不需要的安全性解答或舊的密碼以完成其工作。

這些選項都特別吸引人，但這生命週期的開發人員有時會的方式。

我把它，並實作第三個方法，撰寫程式碼，會略過`Membership`並`MembershipUser`類別，並直接對其進行`SecurityTutorials`資料庫。

> [!NOTE]
> 藉由直接使用資料庫，是大家所提供的成員資格架構的封裝。 這項決定繫結在我們`SqlMembershipProvider`，讓我們較可攜式的程式碼。 此外，此程式碼可能無法如預期般在未來版本的 ASP.NET 成員資格結構描述變更。 這個方法會因應措施，和大部分的因應措施，例如不是最佳作法的範例。


程式碼有一些不討喜的位元，並會相當冗長。 因此，我不想干擾本教學課程，深入檢驗它。 如果您有興趣進一步了解更多資訊，請針對此教學課程和瀏覽下載程式碼`~/Administration/ManageUsers.aspx`頁面。 此頁面上，我們在中建立<a id="_msoanchor_5"> </a>[前述教學課程](building-an-interface-to-select-one-user-account-from-many-vb.md)，列出每位使用者。 我已更新要包含的連結 GridView`UserInformation.aspx`頁面上，將傳遞查詢字串透過所選的使用者的使用者名稱。 `UserInformation.aspx`頁面會顯示選取的使用者和文字方塊的相關資訊，變更其密碼 （請參閱 圖 9）。

之後，輸入新的密碼、 確認在第二個文字方塊中，然後按一下更新的 [使用者] 按鈕，回傳是兩邊彼此乾瞪眼和`aspnet_Membership_SetPassword`預存程序會叫用，更新使用者的密碼。 我非常鼓勵以更加熟悉程式碼，然後再試擴充功能，以包括傳送電子郵件給使用者的密碼已變更這項功能有興趣的讀者。


[![系統管理員可以變更使用者的密碼](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**圖 9**: 系統管理員可以變更使用者的密碼 ([按一下以檢視完整大小的影像](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx`頁面目前僅適用於如果成員資格架構設定為清除或雜湊格式儲存密碼。 雖然我們誠邀您加入這項功能，它會缺少加密新的密碼，程式碼。 建議您將必要的程式碼的方式是使用為解編程式，例如[Reflector](http://www.aisto.com/roeder/dotnet/)來檢查在.NET Framework 中; 方法的原始程式碼先檢查`SqlMembershipProvider`類別的`ChangePassword`方法。 這是密碼的我用來撰寫的程式碼建立雜湊的技術。


## <a name="summary"></a>總結

ASP.NET 提供了兩個控制項，可協助使用者管理他們的密碼。 Provider 控制項可用於使用者忘記其密碼。 根據成員資格架構的設定，使用者可以傳送電子郵件，其現有的密碼或隨機產生新密碼。 ChangePassword 控制項可讓使用者更新其密碼。

登入及 CreateUserWizard 控制項，例如密碼與 ChangePassword 控制項會呈現豐富的使用者介面，而不需要撰寫可靠的宣告式標記或程式碼行。 如果預設的使用者介面不符合您的需求，您可以自訂它透過各種不同的樣式屬性。 或者，控制項的介面可能會轉換成範本，控制更精細的細微程度。 在幕後，這些控制項使用 Membership API 中，叫用`MembershipUser`物件的`ResetPassword`和`ChangePassword`方法。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ChangePassword 控制項快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Provider 控制項快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [在 ASP.NET 中傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` 常見問題集](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者加 Michael Emmings Suchi Banerjee。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [下一頁](unlocking-and-approving-user-accounts-vb.md)
