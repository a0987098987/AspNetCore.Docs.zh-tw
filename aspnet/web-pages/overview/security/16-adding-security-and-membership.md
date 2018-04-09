---
uid: web-pages/overview/security/16-adding-security-and-membership
title: 安全性與成員資格中加入 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 這一章會示範如何保護您的網站，這樣的部分頁面則只提供給登入的人。 （您也會看到如何建立頁面 tha...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 351368a356a71e85d4abfdceac8d4f84e0b217f4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>加入 ASP.NET Web Pages (Razor) 站台的安全性和成員資格
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何保護 ASP.NET Web Pages (Razor) 網站，這樣的部分頁面則只提供給登入的人。 （您也會看到如何建立的任何人都可以存取的網頁）。
> 
> **您將學習：** 
> 
> - 如何建立具有註冊頁面和登入頁面，讓某些頁中，您可以限制為只有成員存取的網站。
> - 如何建立公用和成員專用頁面。
> - 如何定義角色是在您的網站具有不同的安全性權限的群組，以及如何將使用者指派至角色。
> - 如何使用 CAPTCHA 可防止自動的程式 (bot) 建立成員帳戶。
>   
> 
> 這些是發行項中所引進的 ASP.NET 功能：
> 
> - WebMatrix**入門網站**範本。
> - `WebSecurity` Helper 和`Roles`類別。
> - `ReCaptcha`協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library


您可以設定您的網站，讓使用者可以登入&#8212;也就是，讓站台支援*成員資格*。 這非常適合有許多原因。 例如，您的網站可能應該只能用於成員的頁面。 在某些情況下，您可能需要使用者登入，才能傳送意見反應，或保留註解。

即使您的網站支援的成員資格，使用者不一定需要登入，才在站台上使用的部分頁面。 未登入的使用者稱為*匿名使用者*。

使用者可以註冊您的網站上，而且可以網站然後登入。 網站需要使用者名稱 （電子郵件地址） 和密碼以確認使用者是他們所宣告的身分。 這個程序的登入，並確認使用者的身分識別稱為*驗證*。

您可以在不同的方式設定安全性和成員資格：

- 如果您使用的 WebMatrix，一個簡單的方法是建立為基礎的新站台**入門網站**範本。 此範本已設定的安全性和成員資格，且已註冊頁面、 登入頁面上，等等。

    範本所建立的站台也會有的選項，讓使用者使用 Facebook、 Google、 像外部網站登入或 Twitter。
- 如果您想要將安全性新增至現有的網站，或如果您不想要使用**入門網站**範本，您可以建立您自己註冊 頁面上，登入頁面上，以及等等。

本文著重在第一個選項&mdash;如何藉由新增安全性**入門網站**範本。 它也會提供有關如何實作您自己的安全性的一些基本資訊，並提供有關如何執行此作業的詳細資訊連結。 也是有關如何啟用外部登入，個別的文件中的更多詳細資料中所述的資訊。

## <a name="creating-website-security-using-the-starter-site-template"></a>使用入門網站範本建立網站安全性

在 WebMatrix 中，您可以使用**入門網站**範本來建立包含下列網站：

- 用來儲存使用者名稱和密碼為成員資料庫。
- 註冊頁面 （新增） 的匿名使用者可以註冊其中。
- 登入和登出的頁面。
- 密碼復原和重設頁面。

下列程序描述如何建立站台並加以設定。

1. 啟動 WebMatrix 和**快速入門**頁面上，選取**網站從範本**。
2. 選取**入門網站**範本，然後按一下 **確定**。 WebMatrix 會建立新的站台。
3. 在左窗格中，按一下 **檔案**工作區選取器。
4. 在您的網站的根資料夾中，開啟 *\_AppStart.cshtml*檔案，這是用來包含通用設定的特殊檔案。 它包含標記為註解使用某些陳述式`//`字元：

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    這些陳述式設定`WebMail`協助程式，可以用來傳送電子郵件。 成員資格系統可以使用電子郵件，當使用者註冊，或要變更其密碼時，傳送確認訊息。 （例如，使用者註冊後，它們取得電子郵件，其中包含使用者可以按一下以完成註冊程序的連結。）

    傳送電子郵件中所述，需要存取 SMTP 伺服器，[新增電子郵件給 ASP.NET Web Pages 站台中](https://go.microsoft.com/fwlink/?LinkId=202899)。 這個中央中所儲存的電子郵件設定 *\_AppStart.cshtml*檔案，所以您不必重複碼成可以傳送電子郵件的每個頁面。 （您不需要設定 SMTP 設定，來設定註冊資料庫; 您只需要 SMTP 設定，如果您想要驗證使用者從他們的電子郵件別名，並且讓使用者重設忘記的密碼）。
5. 取消註解陳述式移除`//`每一個前面。

    如果您不想要設定電子郵件確認，您可以略過此步驟和下一個步驟。 如果未設定 SMTP 值，新的帳戶是立即可以在沒有確認電子郵件。
6. 修改程式碼中的下列電子郵件相關設定：

   - 設定`WebMail.SmtpServer`您具有存取權的 SMTP 伺服器的名稱。
   - 保留`WebMail.EnableSsl`設`true`。 這項設定會保護加密將會傳送到 SMTP 伺服器的認證。
   - 設定`WebMail.UserName`至您的 SMTP 伺服器帳戶的使用者名稱。
   - 設定`WebMail.Password`您的 SMTP 伺服器帳戶的密碼。
   - 設定`WebMail.From`您自己的電子郵件地址。 這是從訊息傳送的電子郵件地址。

     > [!NOTE] 
     > 
     > **提示**如需有關這些屬性值的詳細資訊，請參閱[設定電子郵件設定](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings)中[自訂全站台的行為的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906)。
7. 儲存並關閉 *\_AppStart.cshtml*。
8. 執行*Default.cshtml*瀏覽器中的。

    ![security-membership-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > 如果您看到錯誤，告訴您屬性必須是執行個體`ExtendedMembershipProvider`，站台可能不會設定為使用 ASP.NET Web Pages 成員資格系統 (SimpleMembership)。 如果裝載提供者的伺服器設定方式不同於您的本機伺服器時，這有時會發生。 若要修正此問題，將下列項目加入站台的*Web.config*檔案：
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > 將這個項目新增為子系`<configuration>`項目，以及對等`<system.web>`項目。
9. 在頁面的右上角，按一下 **註冊**連結。 *Register.cshtml*頁面隨即顯示。
10. 輸入使用者名稱和密碼，然後按一下**註冊**。

    ![security-membership-3](16-adding-security-and-membership/_static/image2.png)

    當您建立網站，從**入門網站**範本，名為資料庫*StarterSite.sdf*是在站台的*應用程式\_資料*資料夾。 在註冊期間，您的使用者資訊加入至資料庫。 如果您設定的 SMTP 值時，訊息會傳送電子郵件地址使用讓您可以完成註冊。

    ![security-membership-4](16-adding-security-and-membership/_static/image3.png)
11. 移至您的電子郵件程式，並尋找訊息，其中將包含您的確認碼和超連結至網站。
12. 按一下此超連結可啟用您的帳戶。 確認超連結會開啟 [登錄] 確認頁面。

    ![security-membership-5](16-adding-security-and-membership/_static/image4.png)
13. 按一下**登入**連結，然後再登入使用您註冊的帳戶。

      您登入之後,**登入**和**註冊**連結來取代**登出**連結。 登入名稱會顯示為連結。 （此連結可跳到頁面中您可以在其中變更您的密碼。）

      ![security-membership-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > 根據預設，ASP.NET web pages 傳送認證到伺服器以純文字 （做為人類看得懂的文字）。 生產網站應該使用安全 HTTP (https:// 也稱為*安全通訊端層*或 SSL) 來加密機密資訊會與伺服器進行交換。 您可以需要電子郵件傳送訊息給設定使用 SSL`WebMail.EnableSsl=true`如同先前的範例。 如需有關 SSL 的詳細資訊，請參閱[保護 Web 通訊： 憑證、 SSL 及 https://](https://go.microsoft.com/fwlink/?LinkId=208660)。

## <a name="additional-membership-functionality-in-the-site"></a>在站台的其他成員資格功能

您的網站包含其他功能，可讓使用者管理他們的帳戶。 使用者可以執行下列作業：

- 變更密碼。 登入之後，使用者可以按一下使用者名稱 （這是連結）。 這將他們帶往頁面他們可以在其中建立新的密碼 (*Account/ChangePassword.cshtml*)。
- 復原忘記的密碼。 在登入頁面上，會連結 (**您是否忘記您的密碼？**)，會將使用者引導至頁面 (*Account/ForgotPassword.cshtml*) 可以在其中輸入電子郵件地址。 站台傳送的電子郵件，有一個連結，他們就可以按一下以設定新的密碼 (*Account/PasswordReset.cshtml*)。

您也可以讓使用者也可以使用外部網站，登入如稍後所說明。

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>建立僅限成員頁面

時間之外，任何人都可以瀏覽在您的網站。 但您可能想要有僅適用於已登入的人的頁面 (也就是成員)。 ASP.NET 可讓您建立只有登入的成員可存取的頁面。 一般而言，匿名使用者嘗試存取僅限成員頁面時，如果您重新導向至登入頁面。

在此程序，您將建立資料夾將包含僅適用於登入之使用者的頁面。

1. 在站台根目錄中，建立新的資料夾。 (在 功能區中，按一下下方的箭號**新增**，然後選擇 **新資料夾**。)
2. 將新的資料夾命名*成員*。
3. 內部*成員*資料夾中，建立新的頁面，並將它命名為*MembersInformation.cshtml*。
4. 下列程式碼和標記取代現有的內容：

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    此程式碼測試`IsAuthenticated`屬性`WebSecurity`物件，它會傳回`true`如果使用者已登入。 如果使用者未登入程式碼會呼叫`Response.Redirect`傳送使用者*Login.cshtml*頁面*帳戶*資料夾。

    重新導向 URL 包含`returnUrl`查詢所使用的字串值`Request.Url.LocalPath`設定目前頁面的路徑。 如果您設定`returnUrl`像這樣的查詢字串中的值 （和傳回 URL 如果是本機路徑），登入頁面將會傳回使用者至這個頁面之後在登入時。

    程式碼也會設定 *\_SiteLayout.cshtml*其版面配置頁的頁面。 (如需詳細資訊版面配置頁面，請參閱[ASP.NET Web Pages 站台中建立一致的版面配置](https://go.microsoft.com/fwlink/?LinkId=202891)。)
5. 執行站台。 如果您仍登入，請按一下**登出**在頁面頂端的按鈕。
6. 在瀏覽器要求網頁*/成員/MembersInformation*。 例如，URL 看起來可能像這樣：

    `http://localhost:38366/Members/MembersInformation`

    （連接埠號碼 (38366) 可能會在您的 URL 不同。）

    正在重新導向至*Login.cshtml*頁面上，因為您未登入。
7. 使用您稍早建立的帳戶登入。 正在重新導向回到*MembersInformation*頁面。 因為您登入，這次您看到的頁面內容。

若要保護存取多個頁面，您可以：

- 加入至每個頁面的安全性檢查。
- 建立 *\_PageStart.cshtml*您保留受保護的頁面，並新增安全性檢查的資料夾中的頁面。  *\_PageStart.cshtml*頁面做為一種通用資料夾中的所有頁面的頁面。 這項技術會更詳細地說明[自訂全站台的行為的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access)。

## <a name="creating-security-for-groups-of-users-roles"></a>建立群組的使用者 （角色） 的安全性

如果您的網站有大量成員，它不有效率個別檢查每個使用者的權限之前您讓他們會看到一個頁面。 您可以執行是建立群組，或*角色*，個別成員隸屬於。 接著，您可以查看根據角色的權限。 在本節中，您將建立&quot;admin&quot;角色，然後建立的使用者可以存取的頁面中 （屬於） 該角色。

ASP.NET 成員資格系統設定來支援角色。 不過，不同於成員資格註冊和登入，**入門網站**範本未包含頁面來協助您管理角色。 （管理角色是管理工作，而不是使用者工作）。不過，您可以直接在 WebMatrix 中的成員資格資料庫中加入群組。

1. 在 WebMatrix 中，按一下 **資料庫**工作區選取器。
2. 在左窗格中，開啟*StarterSite.sdf*節點，開啟**資料表** 節點，然後再按兩下*網頁\_角色*資料表。

    ![security-membership-7](16-adding-security-and-membership/_static/image6.png)
3. 新增名為角色&quot;admin&quot;。 *RoleId*欄位會自動填入。 (它是主索引鍵，而且已為識別欄位中，設定中所述[簡介使用 ASP.NET Web Pages 站台中的資料庫](https://go.microsoft.com/fwlink/?LinkId=202893)。)
4. 記下這個值是給*RoleId*欄位。 （如果這是您正在定義的第一個角色，它會是 1）。

    ![security-membership-8](16-adding-security-and-membership/_static/image7.png)
5. 關閉*網頁\_角色*資料表。
6. 開啟*UserProfile*資料表。
7. 請記下*UserId*值的一或多個資料表，然後關閉資料表中的使用者。
8. 開啟*網頁\_UserInRoles*資料表，並輸入*UserID*和*RoleID*值插入資料表。 例如，若要將放入的使用者 2 &quot;admin&quot;角色，您會輸入這些值：

    ![security-membership-9](16-adding-security-and-membership/_static/image8.png)
9. 關閉*網頁\_為 UsersInRoles*資料表。

    既然您已定義的角色，您可以設定該角色在使用者存取的頁面。
10. 在網站的根資料夾中，建立名為的新頁面*AdminError.cshtml* ，並以下列程式碼取代現有的內容。 這是使用者重新導向至如果它們不允許存取網頁的網頁。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. 在網站的根資料夾中，建立名為的新頁面*AdminOnly.cshtml* ，並以下列程式碼取代現有的程式碼：

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole`方法會傳回`true`如果目前的使用者是指定之角色 （在此情況下，「 管理員 」 角色） 的成員。
12. 執行*Default.cshtml*瀏覽器，但未登入。 （如果您已經登入，登出。）
13. 在瀏覽器的網址列中加入*AdminOnly*在 URL 中。 (換句話說，要求*AdminOnly.cshtml*檔案。)正在重新導向至*AdminError.cshtml*頁面上，因為您不目前登入中的使用者身分&quot;admin&quot;角色。
14. 返回*Default.cshtml*並以您要新增的使用者登入&quot;admin&quot;角色。
15. 瀏覽至*AdminOnly.cshtml*頁面。 此時您會看到的頁面。

## <a name="preventing-automated-programs-from-joining-your-website"></a>防止自動的程式加入您的網站

登入頁面將不會停止自動的程式 (有時稱為*web robots*或*bot*) 登錄到您的網站。 此程序描述如何啟用 [註冊] 頁面的 ReCaptcha 測試。

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. 註冊您的網站，在 ([http://recaptcha.net](http://recaptcha.net))。 當您已經完成註冊時，您會取得公開金鑰和私密金鑰。
2. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
3. 在*帳戶*資料夾中，開啟的檔案命名為*Register.cshtml*。
4. 在頁面頂端的程式碼，找出下列幾行，並取消其藉由移除註解`//`註解字元：

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. 取代`PRIVATE_KEY`ReCaptcha 私用金鑰。
6. 在網頁標記中，移除`@*`和`*@`從網頁標記中的下列程式周圍的註解字元：

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. 取代`PUBLIC_KEY`與您的金鑰。
8. 如果您沒有已移除，移除`<div>`包含開頭為 「 若要啟用 CAPTCHA 驗證..."的文字項目。 (移除整個`<div>`項目和其內容。)

9. 執行*Default.cshtml*瀏覽器中。 如果您登入網站，按一下**登出**連結。
10. 按一下**註冊**連結和測試使用 CAPTCHA 測試的註冊。

     ![security-membership-10](16-adding-security-and-membership/_static/image9.png)

如需有關`ReCaptcha`協助程式，請參閱[防止自動程式 (Bot) 從使用您的 ASP.NET 網站使用 CATPCHA](https://go.microsoft.com/fwlink/?LinkId=251967)。

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>讓使用者能夠登入使用外部站台

**入門網站**範本包含程式碼和標記，可讓使用者使用 Facebook、 Windows Live、 Twitter、 Google 或 Yahoo 登入。 根據預設，未啟用這項功能。 一般的程序的使用讓使用者使用這些外部提供者登入這是：

- 決定您想要支援哪些外部站台。
- 如有需要，請移至該站台，並設定登入應用程式。 （例如，您需要執行此動作以允許 Facebook 登入。）
- 在您的網站中設定提供者。 在大部分情況下，您只需要取消註解中的某些程式碼 *\_AppStart.cshtml*檔案。
- 將標記加入至 [註冊] 頁面，可讓使用者登入的外部網站的連結。 您通常可以複製的標記，您需要並稍有變更的文字。

您可以找到逐步解說主題中的 <<c0> [ 啟用登入，從 ASP.NET Web Pages 網站中的外部站台](https://go.microsoft.com/fwlink/?LinkId=251969)。

在使用者登入從另一個站台後，使用者就會傳回至您的網站和*關聯*該登入與您的網站。 作用中，這會建立成員資格項目在使用者的外部登入網站。 這可讓您使用具有外部登入 （例如角色） 的成員資格的一般功能。

## <a name="adding-security-to-an-existing-website"></a>將安全性新增至現有的網站

本文章中稍早程序依賴使用**入門網站**範本做為網站安全性的基礎。 如果看不到適合您開始從**入門網站**範本或若要從該範本為基礎的站台複製相關頁面，您可以相同的安全性類型中實作您自己的網站透過編碼自己。 建立相同類型的頁面，註冊、 登入，以及其他，然後使用 helper 和類別，來設定成員資格。

基本程序所述的部落格文章[最基本的方式來實作 ASP.NET Razor 安全性](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)。 大部分的工作是使用下列方法和屬性`WebSecurity`helper:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx)， [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx)。 這些方法可讓您判斷是否有人已登錄並加以註冊。
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). 這個屬性可讓您決定是否要將目前的使用者登入。 這非常有用，如果使用者沒有已登入，將使用者重新導向至登入頁面。
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx)， [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx)。 這些方法，將使用者登入或簽出。
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). 這個屬性是適用於顯示目前使用者的登入名稱 （如果使用者已登入）。
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). 這個方法很有用，如果您設定註冊的電子郵件確認。 (部落格文章中說明的詳細資料[使用 ASP.NET Web Pages 安全性確認功能](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)。)

若要管理角色，您可以使用[角色](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx)和[成員資格](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx)類別，部落格文章中所述。

## <a name="additional-resources"></a>其他資源

- [自訂全網站行為](https://go.microsoft.com/fwlink/?LinkId=202906)
- [保護 Web 通訊： 憑證、 SSL 及 https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [最基本的方式來實作 ASP.NET Razor 安全性](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)和[使用 ASP.NET Web Pages 安全性確認功能](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)。 這些是部落格文章說明如何實作 ASP.NET 成員資格功能，而不使用**入門網站**範本。
- [在 ASP.NET Web Pages 網站中啟用外部網站的登入](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity 類別 API 參考](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99))(MSDN)
- [SimpleRoleProvider 類別 API 參考](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99))(MSDN)
- [SimpleMembershipProvider 類別 API 參考](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99))(MSDN)
