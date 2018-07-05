---
uid: web-pages/overview/security/16-adding-security-and-membership
title: 將安全性及成員資格新增至 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本章將說明如何保護您的網站，使某些頁面僅適用於登入的人。 （您也會看到如何建立網頁 tha...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 468a6661df6442705c2e2e178c01aad01bf5765c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383460"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>加入 ASP.NET Web Pages (Razor) 網站上的安全性及成員資格
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何保護 ASP.NET Web Pages (Razor) 網站，使某些頁面僅適用於登入的人。 （您也會看到如何建立的任何人都可以存取網頁，）。
> 
> **您將學到什麼：** 
> 
> - 如何建立的註冊頁面，然後登入頁面，以便針對某些頁面中，您可以限制只有成員存取的網站。
> - 如何建立公開和僅限會員的頁面。
> - 如何定義角色，也就是在您的網站有不同的安全性權限的群組，以及如何將使用者指派給角色。
> - 如何使用 CAPTCHA 避免自動化的程式 (bot) 建立成員的帳戶。
>   
> 
> 以下是所引進的發行項 ASP.NET 功能：
> 
> - WebMatrix**入門網站**範本。
> - `WebSecurity`協助程式和`Roles`類別。
> - `ReCaptcha`協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library


您可以將您的網站設定，以使用者即可登入&#8212;也就是，讓站台支援*成員資格*。 這適合用於有許多原因。 例如，您的網站可能有應該只能供成員的頁面。 在某些情況下，您可能需要使用者能夠登入以傳送意見反應或留言。

即使您的網站支援成員資格，則使用者會不一定需要登入，才在站台上使用的部分頁面。 未登入的使用者稱為*匿名使用者*。

使用者可以註冊您的網站上，並可以再登入網站。 網站需要使用者名稱 （電子郵件地址） 和密碼以確認使用者是他們所宣告的人。 此程序的登入，並確認使用者的身分識別稱為*驗證*。

您可以設定安全性及成員資格以不同的方式：

- 如果您使用 WebMatrix，是建立為新站台上輕鬆**入門網站**範本。 此範本已設定的安全性及成員資格，並已註冊頁面、 登入頁面上，等等。

    範本所建立的站台也有選項可讓使用者登入使用外部網站，例如 Facebook、 Google 或 Twitter。
- 如果您想要將安全性新增至現有的網站，或如果您不想要使用**入門網站**範本，您可以建立您自己註冊頁面、 登入頁面上，等等。

本文著重於第一個選項&mdash;如何藉由新增安全性**入門網站**範本。 它也會提供有關如何實作您自己的安全性的一些基本資訊，並接著提供有關如何執行該動作的詳細資訊連結。 另外還有關於如何啟用外部登入，在個別的文章中的更多詳細資料中所述的資訊。

## <a name="creating-website-security-using-the-starter-site-template"></a>使用入門網站範本建立網站安全性

在 WebMatrix 中，您可以使用**入門網站**範本來建立包含下列網站：

- 用來儲存使用者名稱和密碼為成員資料庫。
- 註冊頁面，其中匿名 （新） 的使用者可以註冊。
- 登入和登出頁面。
- 密碼復原和重設頁面。

下列程序描述如何建立網站，並將其設定。

1. 啟動 WebMatrix 和在**快速入門**頁面上，選取**站台從範本**。
2. 選取 **入門網站**範本，然後按一下 **確定**。 WebMatrix 會建立新的站台。
3. 在左窗格中，按一下**檔案**工作區選取器。
4. 在 您的網站的 根 資料夾中，開啟 *\_AppStart.cshtml*檔案，這是用來包含全域設定的特殊檔案。 它包含標記為註解使用的部分陳述式`//`字元：

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    這些陳述式設定`WebMail`協助程式，可用來傳送電子郵件。 成員資格系統可以使用電子郵件來傳送確認訊息，使用者在註冊時，或在想要變更其密碼時。 （例如，使用者註冊之後，他們取得電子郵件，包括他們可以點選來完成註冊程序的連結）。

    傳送電子郵件中所述，需要存取 SMTP 伺服器，[新增至 ASP.NET Web Pages 網站的電子郵件](https://go.microsoft.com/fwlink/?LinkId=202899)。 您會將電子郵件設定儲存在此中央 *\_AppStart.cshtml*檔案，讓您不必撰寫程式碼加以重複成可以傳送電子郵件的每個頁面。 （您不需要設定 SMTP 設定，以設定註冊資料庫，您只需要 SMTP 設定，如果您想要驗證使用者從其電子郵件別名，並讓使用者重設遺忘的密碼）。
5. 取消註解陳述式移除`//`每一個前面。

    如果您不想設定電子郵件確認，您可以略過此步驟和下一個步驟。 如果未設定的 SMTP 值，新的帳戶，立即毋需確認電子郵件。
6. 修改程式碼中的下列電子郵件相關設定：

   - 設定`WebMail.SmtpServer`您具有存取權的 SMTP 伺服器的名稱。
   - 離開`WebMail.EnableSsl`設定為`true`。 此設定可保護可藉由加密傳送至 SMTP 伺服器的認證。
   - 設定`WebMail.UserName`SMTP 伺服器帳戶的使用者名稱。
   - 設定`WebMail.Password`SMTP 伺服器帳戶的密碼。
   - 設定`WebMail.From`您自己的電子郵件地址。 這是從訊息傳送的電子郵件地址。

     > [!NOTE] 
     > 
     > **祕訣**如需有關這些屬性值的詳細資訊，請參閱[設定電子郵件設定](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings)中[自訂全網站行為適用於 ASP.NET 網頁](https://go.microsoft.com/fwlink/?LinkID=202906)。
7. 儲存並關閉 *\_AppStart.cshtml*。
8. 執行*Default.cshtml*瀏覽器中的頁面。

    ![安全性-成員資格-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > 如果您看到錯誤，告訴您屬性必須是執行個體`ExtendedMembershipProvider`，網站可能未設定為使用 ASP.NET Web Pages 成員資格系統 (SimpleMembership)。 如果裝載提供者的伺服器設定方式不同於您的本機伺服器時，這有時會發生。 若要修正此問題，將下列項目新增至站台的*Web.config*檔案：
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > 將這個項目新增為子系`<configuration>`項目，做為對等的`<system.web>`項目。
9. 在頁面右上角，按一下**註冊**連結。 *Register.cshtml*頁面隨即顯示。
10. 輸入使用者名稱和密碼，然後按一下**註冊**。

    ![安全性-成員資格-3](16-adding-security-and-membership/_static/image2.png)

    當您建立網站，從**入門網站**範本，一個名為資料庫*StarterSite.sdf*的網站中建立*應用程式\_資料*資料夾。 在註冊期間，您的使用者資訊會加入至資料庫。 如果您設定 SMTP 值時，訊息會傳送電子郵件地址讓您可以完成註冊，您使用。

    ![安全性-成員資格-4](16-adding-security-and-membership/_static/image3.png)
11. 請移至您的電子郵件程式，並尋找訊息，其中將包含您的確認碼和超連結至網站。
12. 按一下此超連結可啟用您的帳戶。 [確認] 超連結會開啟註冊確認頁。

    ![5-安全性-成員資格](16-adding-security-and-membership/_static/image4.png)
13. 按一下 **登入**連結，然後使用您註冊的帳戶登入。

      登入之後,**登入**並**註冊**連結由取代**登出**連結。 您登入名稱會顯示為連結。 （此連結可移至的頁面，您可以在這裡變更您的密碼。）

      ![6-安全性-成員資格](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > 根據預設，ASP.NET 網頁的認證以傳送至伺服器以純文字 （人類看得懂的文字）。 生產網站應該使用安全 HTTP (https://，也稱為*安全通訊端層*或 SSL) 來加密伺服器與交換的機密資訊。 您可以需要電子郵件傳送訊息給設定使用 SSL`WebMail.EnableSsl=true`如同先前的範例。 如需有關 SSL 的詳細資訊，請參閱 <<c0> [ 保護的 Web 通訊： 憑證、 SSL 及 https://](https://go.microsoft.com/fwlink/?LinkId=208660)。

## <a name="additional-membership-functionality-in-the-site"></a>在站台的其他成員資格功能

您的網站包含其他功能，可讓使用者管理他們的帳戶。 使用者可以執行下列作業：

- 變更其密碼。 登入之後，他們可以按一下 使用者名稱 （這是連結）。 這會讓它們進入頁面他們可以在其中建立新的密碼 (*Account/ChangePassword.cshtml*)。
- 復原遺忘的密碼。 在 [登入] 頁面中，沒有連結 (**您是否忘記您的密碼？**)，會帶領使用者前往一個頁面 (*Account/ForgotPassword.cshtml*) 可以在其中輸入電子郵件地址。 站台將它們傳送電子郵件訊息，他們可以點選來設定新密碼的連結 (*Account/PasswordReset.cshtml*)。

您也可以讓使用者也可以登入使用外部網站，如稍後所說明。

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>建立僅限會員的頁面

目前，任何人都可以瀏覽至您的網站中的任何頁面。 但您可能想要有僅適用於已登入的人的頁面 (也就是成員)。 ASP.NET 可讓您建立只能由登入的成員可以存取的頁面。 一般而言，如果匿名使用者嘗試存取僅限會員的頁面，您將他們重新導向至登入頁面。

在此程序中，您將建立的資料夾將包含僅適用於登入使用者的頁面。

1. 在站台的根目錄中，建立新的資料夾。 (在功能區中，按一下下方的箭號**的新**，然後選擇**新資料夾**。)
2. 將新資料夾命名*成員*。
3. 內部*成員*資料夾中，建立新的頁面，並將它命名為*MembersInformation.cshtml*。
4. 下列程式碼和標記取代現有的內容：

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    此程式碼測試`IsAuthenticated`的屬性`WebSecurity`物件，它會傳回`true`如果使用者已登入。 如果使用者未登入，程式碼會呼叫`Response.Redirect`傳送至使用者*Login.cshtml*頁面*帳戶*資料夾。

    重新導向 URL 會包含`returnUrl`查詢字串值，會使用`Request.Url.LocalPath`設定目前頁面的路徑。 如果您將設定`returnUrl`像這樣的查詢字串中的值 （和傳回的 URL 是否為本機路徑），登入頁面將會回到使用者這個頁面之後在登入時。

    程式碼也會設定 *\_SiteLayout.cshtml*作為其版面配置頁的頁面。 (如需有關版面配置頁的詳細資訊，請參閱[在 ASP.NET Web Pages 網站中建立一致的版面配置](https://go.microsoft.com/fwlink/?LinkId=202891)。)
5. 執行站台。 如果您仍登入，請按一下**登出**在頁面頂端的按鈕。
6. 在瀏覽器中要求頁面 */成員/MembersInformation*。 例如，URL 可能如下所示：

    `http://localhost:38366/Members/MembersInformation`

    （連接埠號碼 (38366) 可能會在您的 URL 不同。）

    若要將您重新導向*Login.cshtml*頁面上，因為您未登入。
7. 使用您稍早建立的帳戶登入。 將您重新導向回到*MembersInformation*頁面。 因為您已登入，此時您會看到頁面內容。

若要保護多個頁面的存取權，您可以這樣做：

- 加入每個頁面中的安全性檢查。
- 建立 *\_PageStart.cshtml*供您保留受保護的頁面，以及新增的安全性檢查的資料夾中的頁面。 *\_PageStart.cshtml*頁面做為一種通用資料夾中的所有頁面的頁面。 這項技術會更詳細地說明[自訂全網站行為適用於 ASP.NET 網頁](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access)。

## <a name="creating-security-for-groups-of-users-roles"></a>建立群組的使用者 （角色） 的安全性

如果您的網站有大量成員，它不有效率前您讓他們看到的頁面，個別檢查每個使用者的權限。 改為用途是要建立群組，或*角色*，個別成員隸屬於。 然後，您可以檢查角色為基礎的權限。 在本節中，您將建立&quot;admin&quot;角色，然後再建立的使用者可以存取的頁面中 （屬於） 該角色。

ASP.NET 成員資格系統設定為支援的角色。 不過，不同於成員資格註冊和登入**入門網站**範本未包含頁面，幫助您管理角色。 （管理角色是系統管理工作，而不是使用者的工作）。不過，您可以將群組直接成員資格資料庫中在 WebMatrix 中。

1. 在 WebMatrix 中，按一下**資料庫**工作區選取器。
2. 在左窗格中，開啟*StarterSite.sdf*節點，開啟**資料表**節點，然後再按兩下*網頁\_角色*資料表。

    ![7-安全性-成員資格](16-adding-security-and-membership/_static/image6.png)
3. 新增名為的角色&quot;admin&quot;。 *RoleId*欄位會自動填入。 (它是主索引鍵，而且已設定為身分識別] 欄位中中, 所述[使用 [ASP.NET Web Pages 網站中的資料庫簡介](https://go.microsoft.com/fwlink/?LinkId=202893)。)
4. 請記下的值是什麼*RoleId*欄位。 （如果這是您定義的第一個角色時，它會是 1）。

    ![8-安全性-成員資格](16-adding-security-and-membership/_static/image7.png)
5. 關閉*網頁\_角色*資料表。
6. 開啟*UserProfile*資料表。
7. 請記下的*UserId*的一或多個資料表，然後關閉資料表中的使用者值。
8. 開啟*網頁\_UserInRoles*資料表，並輸入*UserID*並*RoleID*值插入資料表。 例如，若要將放入的使用者 2 &quot;admin&quot;角色，您會輸入這些值：

    ![安全性成員資格 9](16-adding-security-and-membership/_static/image8.png)
9. 關閉*網頁\_為 UsersInRoles*資料表。

    既然您已定義的角色，您可以設定該角色中的使用者可以存取的頁面。
10. 在網站根資料夾中，建立名為的新頁面*AdminError.cshtml*並以下列程式碼取代現有的內容。 這會是使用者會重新導向至如果它們不允許存取 頁面的頁面。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. 在網站根資料夾中，建立名為的新頁面*AdminOnly.cshtml*並以下列程式碼取代現有的程式碼：

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole`方法會傳回`true`目前使用者是否屬於指定的角色 （在此案例中，「 系統管理員 」 角色）。
12. 執行*Default.cshtml*瀏覽器，但未登入。 （如果您已經登入，登出。）
13. 在瀏覽器的網址列中加入*AdminOnly*在 URL 中。 (亦即，要求*AdminOnly.cshtml*檔案。)若要將您重新導向*AdminError.cshtml*頁面上，因為您不目前登入中的使用者身分&quot;管理員&quot;角色。
14. 返回*Default.cshtml*並以您要新增的使用者登入&quot;管理員&quot;角色。
15. 瀏覽至*AdminOnly.cshtml*頁面。 此時您會看到的頁面。

## <a name="preventing-automated-programs-from-joining-your-website"></a>加入您的網站時，防止自動的程式

登入頁面將不會停止自動的程式 (有時稱為*web 機器人*或*bot*) 從向您的網站。 此程序描述如何啟用 [註冊] 頁面的 ReCaptcha 測試。

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. 註冊您的網站在 ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net))。 當您完成註冊之後時，您會取得公開金鑰和私密金鑰。
2. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
3. 在 *帳號*資料夾中，開啟的檔案命名為*Register.cshtml*。
4. 在頁面頂端的程式碼，找出下列幾行，並取消其註解藉由移除`//`註解字元：

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. 取代`PRIVATE_KEY`ReCaptcha 私用金鑰。
6. 在頁面標記中，移除`@*`和`*@`註解字元周圍頁面標記中的下列行：

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. 取代`PUBLIC_KEY`使用您的金鑰。
8. 如果您還沒有已移除，移除`<div>`元素，其中包含開頭為 「 若要啟用 CAPTCHA 驗證...」 的文字。 (移除整個`<div>`項目和其內容。)

9. 執行*Default.cshtml*瀏覽器中。 如果您登入網站，按一下**登出**連結。
10. 按一下 **註冊**連結與測試使用 CAPTCHA 測試註冊。

     ![安全性-成員資格-10](16-adding-security-and-membership/_static/image9.png)

如需詳細資訊`ReCaptcha`協助程式，請參閱 <<c2> [ 避免自動化程式 (Bot) 從使用您的 ASP.NET 網站使用 CATPCHA](https://go.microsoft.com/fwlink/?LinkId=251967)。

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>讓使用者能夠使用外部網站登入

**入門網站**範本包含程式碼和標記，可讓使用者使用 Facebook、 Windows Live、 Twitter、 Google 或 Yahoo 登入。 根據預設，未啟用這項功能。 一般的程序的使用讓使用者使用這些外部提供者登入如下：

- 決定您想要支援哪些外部網站。
- 如有需要，請移至該網站，並將登入應用程式設定。 （例如，您必須執行這項操作，以便在 Facebook 登入。）
- 在您的網站，設定提供者。 在大部分情況下，您只需要在一些程式碼取消註解 *\_AppStart.cshtml*檔案。
- 將標記新增至註冊頁面，讓使用者能夠登入的外部網站的連結。 通常，您可以複製您需要而且稍有變更文字的標記。

您可以在主題找到逐步指示[啟用從 ASP.NET Web Pages 網站中的外部站台的登入](https://go.microsoft.com/fwlink/?LinkId=251969)。

在使用者登入從另一個站台後，使用者會傳回您的站台並*產生關聯*該登入您的網站。 作用中，這會在使用者的外部登入您的網站中建立成員資格項目。 這可讓您使用外部登入 （例如角色） 的成員資格的一般功能。

## <a name="adding-security-to-an-existing-website"></a>將安全性新增至現有的網站

稍早在本文中的程序會依賴使用**入門網站**範本做為網站安全性的基礎。 如果不是適合您要當作起點**入門網站**範本或複製該範本為基礎的站台的相關頁面，您可以相同的安全性類型中實作您自己的網站透過自行撰寫。 建立相同類型的頁面，註冊、 登入等等 — 然後使用協助程式和類別，來設定成員資格。

基本程序所述的部落格文章[最基本的方式來實作 ASP.NET Razor 安全性](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)。 大部分的工作是使用下列方法和屬性`WebSecurity`協助程式：

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx)， [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx)。 這些方法可讓您判斷是否有人已註冊，以及如何註冊它們。
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx)。 這個屬性可讓您決定是否要將目前的使用者登入。 這非常有用，如果使用者沒有已登入，將使用者重新導向至登入頁面。
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx)， [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx)。 縮小或相應放大，這些方法會記錄使用者。
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx)。 這個屬性就適用於顯示目前使用者的登入的名稱 （如果使用者已登入）。
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx)。 這個方法很有用，如果您設定註冊的電子郵件確認。 (詳細資料所述的部落格文章[確認功能用於 ASP.NET Web Pages 安全性](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)。)

若要管理角色，您可以使用[角色](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx)並[的成員資格](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx)類別，如部落格文章中所述。

## <a name="additional-resources"></a>其他資源

- [自訂全網站行為](https://go.microsoft.com/fwlink/?LinkId=202906)
- [保護的 Web 通訊： 憑證、 SSL 及 https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [最基本的方式來實作 ASP.NET Razor 安全性](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)並[確認功能用於 ASP.NET Web Pages 安全性](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)。 這些是說明如何實作 ASP.NET 成員資格功能，而不使用的部落格文章**入門網站**範本。
- [在 ASP.NET Web Pages 網站中啟用外部網站的登入](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity 類別 API 參考](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99))(MSDN)
- [SimpleRoleProvider 類別 API 參考](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99))(MSDN)
- [SimpleMembershipProvider 類別 API 參考](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99))(MSDN)
