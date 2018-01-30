---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: "成員資格 |Microsoft 文件"
author: microsoft
description: "ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。 ASP.NET 表單驗證會提供便利的方式來 incorp..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 1a5a495845b60f9aac51c9776311af67f5dc8767
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="membership"></a>成員資格
====================
by [Microsoft](https://github.com/microsoft)

> ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。 ASP.NET 表單驗證會提供便利的方式來併入您的 ASP.NET 應用程式的登入表單，並驗證使用者針對資料庫或其他資料存放區。


ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。 ASP.NET 表單驗證會提供便利的方式來併入您的 ASP.NET 應用程式的登入表單，並驗證使用者針對資料庫或其他資料存放區。 FormsAuthentication 類別的成員可使其可處理驗證的 cookie、 檢查有有效的登入、 記錄等出使用者。但是，在 ASP.NET 1.x 應用程式中實作表單驗證可能需要大量的程式碼。

ASP.NET 2.0 中的成員資格是主要的進階透過使用單獨的表單驗證。 （成員資格是最健全的表單驗證，結合時，但使用表單驗證，就不需要）。您很快就會發現，您可以使用 ASP.NET 成員資格和登入控制項在 ASP.NET 2.0 中實作功能強大的成員資格系統，而不需要完全撰寫太多程式碼。

## <a name="implementing-membership-in-aspnet-20"></a>在 ASP.NET 2.0 中實作的成員資格

成員資格是由下列四個步驟來實作。 請注意，有許多可一併實作的選擇性組態以及所涉及的子步驟。 這些步驟是為了說明設定成員資格的大圖片。

1. 建立成員資格資料庫 （如果 SQL Server 會使用做為成員資格存放區）。
2. 在您的應用程式組態檔中指定的成員資格選項。 （依預設已啟用的成員資格）。
3. 決定您想要使用的成員資格儲存區的類型。 選項如下： 

    - Microsoft SQL Server （版本 7.0 或更新版本）
    - Active Directory Store
    - 自訂成員資格提供者
4. 設定 ASP.NET 表單驗證的應用程式。 同樣地，成員資格設計成利用表單驗證，但使用表單驗證，就不需要。
5. 定義成員資格的使用者帳戶，並視需要設定角色。

## <a name="creating-the-membership-database"></a>建立成員資格資料庫

如果 youre 使用 SQL Server 7.0 或更新版本作為您的成員資格儲存區中，您可以使用 aspnet\_regsql 公用程式 （可取得最容易從 Visual Studio.NET 2005年命令提示字元） 來設定您的資料庫。 Aspnet\_regsql 公用程式可用的命令提示字元工具或是透過 GUI 精靈。 精靈方法是設定您的資料庫最簡單的方式。 若要存取此精靈，只要執行下列命令：

`aspnet_regsql W`

一旦您執行該命令，您會使用 ASP.NET SQL Server 安裝精靈如下所示。


![](membership/_static/image1.jpg)

**圖 1**


ASP.NET SQL Server 安裝精靈會在您在精靈中指定的執行個體中建立的網站。 不過，ASP.NET 會使用連接字串，machine.config 檔案中連接到您的資料庫。 根據預設，此連接字串將會為非 SQL Server 2005 執行個體，因此如果您使用的 SQL Server 2000 或 SQL Server 7.0 執行個體，您必須修改 machine.config 檔案中的連接字串。 此處的連接字串可以是位於：

[!code-xml[Main](membership/samples/sample1.xml)]

不幸的是，如果您不需要修改連接字串，ASP.NET 將不會提供描述性的錯誤。 它只會繼續抱怨指出您尚未建立資料庫。 在上述情況中，我已經修改為指向 我的本機 SQL Server 2000 執行個體的連接字串。

## <a name="specifying-configuration-and-adding-users-and-roles"></a>指定的組態和新增使用者和角色

設定成員資格的下一個步驟是將所需的資訊加入至應用程式的 web.config 檔案。 在 ASP.NET 中 1.x，修改 web.config 檔案發生有時困難 lowerCamelCase 使用，因為缺少的 Intellisense。 Visual Studio.NET 2005年，使得工作更為容易，因為 Intellisense 組態檔，但 ASP.NET 2.0 更進一步編輯組態檔中提供 Web 介面。

您可以依序按一下 [ASP.NET 組態] 按鈕，如下所示的 [方案總管] 工具列上，啟動 Web 介面。 您也可以啟動 Web 介面，透過快顯視窗會顯示插入登入控制項時。


![](membership/_static/image2.jpg)

**圖 2**


這會啟動 ASP.NET 網站管理工具如下所示。 ASP.NET 網站管理是四個索引標籤介面，以簡化管理應用程式設定。 可用的下列索引標籤如下：

- **首頁**
- **安全性**設定使用者、 角色和存取。
- **應用程式**設定應用程式設定。
- **提供者**設定和測試您的應用程式成員資格提供者。

網站管理工具可讓您輕鬆地建立新的使用者，建立新的角色，以及管理使用者和角色。 Windows 介面中，不提供此功能。 Windows 介面可讓您輕鬆地定義授權設定，以及加入、 刪除和管理提供者，不在網站管理工具的功能。

若要啟動 Windows 介面，開啟 [網際網路資訊服務] 嵌入式管理單元，以滑鼠右鍵按一下您的應用程式，然後選擇屬性。 按一下 [ASP.NET] 索引標籤，然後按一下 [編輯設定] 按鈕。 （應用程式必須啟用 編輯組態按鈕 ASP.NET 2.0 底下執行。 您可以設定 ASP.NET 版本在 [ASP.NET] 對話方塊。）[ASP.NET 組態設定] 對話方塊隨即出現如下所示。


![](membership/_static/image3.jpg)

**圖 3**


在 [一般] 索引標籤會列出連接字串和應用程式設定。 任何以斜體設定父組態檔 （machine.config 或更高層級的 web.config） 中定義及設定不會以斜體從應用程式組態檔。 新增設定，如果已移除或編輯應用程式層級，ASP.NET 會新增、 移除或修改應用程式層級 web.config，而不是從它繼承的組態檔中移除此設定的設定。

[驗證] 索引標籤如下所示。 這是您將在其中設定成員資格設定。 表單驗證設定，成員資格提供者，並在此處設定角色提供者。


![](membership/_static/image4.jpg)

**圖 4**


## <a name="implementing-membership-in-your-application"></a>在您的應用程式中實作的成員資格

在您的應用程式中實作 ASP.NET 2.0 的成員資格的最簡單方式是使用提供的登入控制項。 這個方法可讓您不需要撰寫任何程式碼完全實作 ASP.NET 2.0 的成員資格的基本概念。

下列的登入控制項可在 ASP.NET 2.0:

## <a name="login-control"></a>登入控制項

登入控制項提供身分登入的成員資格系統的使用者介面。 它提供使用者名稱和密碼 textboxt] 和 [登入按鈕。 例如若要註冊的使用者連結的其他許多常用功能尚未這樣做，核取方塊，可讓使用者自動登入在後續造訪時，密碼提示等等的連結。登入控制項的所有功能都皆透過控制項的屬性可自訂的。

在 ASP.NET 中 1.x，開發人員必須撰寫牽涉到大量的程式碼來執行查詢，使用表單驗證時。 使用 ASP.NET 2.0 的成員資格，也可以驗證使用者，而不需要撰寫任何程式碼完全。 ASP.NET 自動會為您執行查詢的使用者。 (如果您在使用登入控制項，而不需要使用 ASP.NET 成員資格，您可以使用**OnAuthenticate**方法以驗證使用者。)

## <a name="loginview-control"></a>LoginView 控制項

LoginView 控制項是根據預設，會提供兩個範本樣板化控制項AnonymousTemplate 和 LoggedInTemplate。 會顯示範本取決於使用者登入您的成員資格系統。 這個控制項通常用於顯示當使用者未擁有登入的登入控制項和 LoginStatus 控制項和/或其他登入控制項，當使用者登入。 如果您使用角色管理 ASP.NET 應用程式中，LoginView 控制項可以顯示特定使用者角色為基礎的範本。 （更 asp.net 角色管理會涵蓋更新版本。）

## <a name="passwordrecovery-control"></a>Provider 控制項

Provider 控制項可讓使用者收到電子郵件，與目前密碼或重設其密碼。 純文字和加密的密碼可復原，並在電子郵件傳送給使用者。 如果密碼已雜湊，就無法復原。 改為使用者就必須執行密碼重設。

## <a name="loginstatus-control"></a>LoginStatus 控制項

LoginStatus 控制項用來顯示目前登入的使用者的登入指標使用者未登入和登出指標。 Request.IsAuthenticated 屬性用來判斷哪些標記，來顯示。 LoginStatus 控制項所顯示的指標可以是文字 (透過實作**LoginText**和**LogoutText**屬性) 或映像 (透過實作**LoginImageUrl**和**LogoutImageUrl**屬性。)

當使用者登出透過 LoginStatus 控制項時，他或她會重新導向至所指定的 URL **LogoutPageUrl**屬性。 如果未設定該屬性，會重新整理目前頁面。 站台可能受到表單驗證，因為目前頁面的重新整理將使用者重新導向至網站的登入頁面。

## <a name="loginname-control"></a>LoginName 控制項

LoginName 控制項顯示之使用者的目前登入網站的使用者名稱。

## <a name="createuserwizard-control"></a>適用於 CreateUserWizard 控制項

適用於 CreateUserWizard 控制項提供便利的方式來註冊您的成員資格系統使用者。 您可以新增 （實作為的 WizardSteps 集合） 的步驟，透過如下所示的介面。


![](membership/_static/image5.jpg)

**圖 5**


適用於 CreateUserWizard 是樣板化控制項衍生自精靈類別，並提供下列範本：

- **HeaderTemplate**這個範本就會控制在精靈的標頭的外觀。
- **SidebarTemplate**這個範本就會控制在精靈的 [資訊看板] 的外觀。
- **StartNavigationTemplate**所巡覽的外觀會開始步驟在精靈的這個範本控制項。
- **StepNavigationTemplate**此範本控制導覽區域中，不在開始或完成步驟時的外觀。
- **FinishNavigationTemplate**此範本控制導覽區域中，在 [完成] 步驟上的外觀。

此外，您將加入至精靈的每個步驟，ASP.NET 將建立自訂範本，其中包含 ContentTemplate 和 CustomNavigationTemplate 該步驟。 如需自訂適用於 CreateUserWizard 完整的詳細資訊，請參閱 VS.NET 2005 文件：

## <a name="changepassword-control"></a>ChangePassword 控制項

ChangePassword 控制項可讓使用者變更其密碼。 如果 DisplayUserName 屬性為 true （它是預設為 false），使用者可以在未登入時變更密碼。 如果使用者*是*已經登入和 DisplayUserName 屬性為 true 時，使用者將無法變更不會記錄在提供他們知道該使用者的使用者識別碼的另一位使用者的密碼。

請記住，如果您想要能夠變更密碼，而不需要登入的使用者，您必須確定 ChangePassword 控制項顯示的頁面允許匿名存取。 很明顯地，使用者必須提供舊密碼，才能變更其密碼。

## <a name="role-management"></a>角色管理

角色管理可讓您將使用者指派至特定角色，然後限制存取特定檔案或資料夾，根據該角色。 角色管理也會提供應用程式開發介面，讓您能夠以程式設計方式判斷 abv 角色或判斷特定角色中的所有使用者並據以回應。

角色管理，就不需要 ASP.NET 成員資格，也不是成員資格才能使用角色管理的需求。 不過，這兩個精美補充彼此，而且很可能的開發人員相互搭配使用。

若要啟用應用程式中的角色管理，請在 web.config 檔案中進行以下變更：

[!code-xml[Main](membership/samples/sample2.xml)]

當**cacheRolesInCookie**屬性設為 true，ASP.NET 會快取在 cookie 中用戶端上的使用者角色成員資格。 這可讓角色查閱 RoleProvider 呼叫的情況下發生。 當使用這個屬性時，開發人員都以確保**cookieProtection**屬性將設為全部。 （這是預設值）。這可確保 cookie 資料加密，並可協助確保 cookie 內容未遭到變更。 您可以使用網站管理工具新增角色。 它可讓您輕鬆地定義角色、 設定這些角色為基礎的站台的組件的存取，並將使用者指派至角色。


![](membership/_static/image6.jpg)

**圖 6**


如上所示，可以加入新的角色只需輸入角色的名稱，然後按一下 加入角色。 可以管理或現有的角色清單中的適當連結，即可刪除現有的角色。

當您管理角色時，您可以新增或移除使用者，如下所示。


![](membership/_static/image7.jpg)

**圖 7**


藉由檢查使用者在角色中的核取方塊，您可以輕鬆加入使用者至特定角色。 ASP.NET 會自動使用適當的項目更新成員資格資料庫。 您也會想要設定您的應用程式的存取規則。 透過這項操作的 ASP.NET 1.x 開發人員熟悉&lt;授權&gt;web.config 檔案中和該選項中的項目是 ASP.NET 2.0 中仍然可用。 不過，其更輕鬆地管理存取規則使用網站管理工具所示下方。


![](membership/_static/image8.jpg)

**圖 8**


在此情況下，管理資料夾會反白顯示 （其難以看見，因為此工具會反白顯示它以淺灰色） 和系統管理員角色授與存取權。 會拒絕所有其他使用者。 您可以按一下前端圖示以選取的規則，然後使用往上移和下移按鈕排列的規則。 搭配 ASP.NET 使用&lt;授權&gt;項目，以它們出現的順序處理規則。 換句話說，如果上述擷取畫面中的規則的順序已顛倒，沒有人必須存取管理資料夾因為 ASP.NET 會遇到的第一個規則的規則，拒絕每個人的資料夾。

ASP.NET 2.0 會將 web.config 檔案加入至您指定的存取規則的資料夾。 透過組態檔或透過網站管理工具，可以編輯存取規則。 換句話說，網站管理工具是只要透過此組態檔可以編輯好記的環境中的介面。

## <a name="using-roles-in-code"></a>在程式碼中使用角色

角色管理的 API 版本後尚未變更 1.x。 **IsInRole**方法用來判斷使用者是否為特定角色。

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET 也會建立目前內容的成員身分的 RolePrincipal 執行個體。 RolePrincipal 物件可以用來取得所有的使用者所屬的角色，如下所示：

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>LoginView 控制項搭配使用 RoleGroups

既然您已了解角色管理和成員資格時，可讓簡要討論 LoginView 控制項會利用這項功能在 ASP.NET 2.0 的方式。 如先前所討論，LoginView 控制項是樣板化控制項，其中包含兩個範本的預設值;AnonymousTemplate 和 LoggedInTemplate。 內 LoginView 工作對話方塊的連結 （如下所示），可讓您編輯 RoleGroups。


![](membership/_static/image9.jpg)

**圖 9**


每個 RoleGroup 物件包含字串的陣列，定義 RoleGroup 適用於哪些角色。 若要加入新 RoleGroup LoginView 控制項中，按一下 編輯 RoleGroups 連結。 在上圖中，您可以查看我已新增為系統管理員的新 RoleGroup。 選取該 RoleGroup （RoleGroup[0]) 檢視下拉式清單中的，我可以設定的範本，則只會顯示系統管理員角色的成員。 在下面的影像中，我已新增適用於 「 銷售 」 角色和角色的成員發佈新 RoleGroup。 這會將第二個 RoleGroup 加入 LoginView 工作對話方塊中檢視下拉式清單中，任何項目加入至該範本就可以看到在 Sales 或發佈中的任何使用者角色。


![](membership/_static/image10.jpg)

**圖 10**


## <a name="overriding-the-existing-membership-provider"></a>覆寫現有的成員資格提供者

有幾種方式，您可以擴充功能的 ASP.NET 成員資格。 首先，您可以從它繼承和覆寫其方法很明顯地變更 SqlMembershipProvider 類別的既有的功能。 例如，如果您想要實作您自己的功能，建立使用者時，您可以建立您自己的類別繼承從 SqlMembershipProvider，如下所示：

[!code-csharp[Main](membership/samples/sample5.cs)]

如果相反地，您會想要建立您自己的提供者 （例如在存取資料庫中，儲存成員資格資訊），您可以建立自己的提供者。

## <a name="creating-your-own-membership-provider"></a>建立您自己的成員資格提供者

若要建立您自己的成員資格提供者，您必須建立繼承自 MembershipProvider 類別的類別。 如果您使用 VB.NET，Visual Studio 2005 會將所有您需要覆寫的方法虛設常式。 如果您使用的 C# 中，您將新增虛設常式其最多。

您必須覆寫下列：

- ApplicationName 屬性
- ChangePassword 函式
- ChangePasswordQuestionAndAnswer 函式
- CreateUser 函式
- DeleteUser 函式
- EnablePasswordReset 屬性
- EnablePasswordRetrieval 屬性
- FindUsersByEmail 函式
- FindUsersByName 函式
- GetAllUsers 函式
- GetNumberOfUsersOnline 函式
- GetPassword 函式
- GetUser 函式
- GetUserNameByEmail 函式
- MaxInvalidPasswordAttempts 屬性
- MinRequiredNonAlphanumericCharacters 屬性
- MinRequiredPasswordLength 屬性
- PasswordAttemptWindow 屬性
- PasswordFormat 屬性
- PasswordStrengthRegularExpression 屬性
- RequiresQuestionAndAnswer 屬性
- RequiresUniqueEmail 屬性
- ResetPassword 函式
- 解除鎖定使用者函式
- UpdateUser 函式
- ValidateUser 函式

Thats 實作為 C# 開發人員相當多的清單。 您可能會發現 VB.NET 中建立類別，任何實作情況下，然後使用.NET 反映或類似的工具來轉換成 C# 程式碼變得更加容易。

連接字串和其他屬性應該設定為其預設值的 Initialize 方法中。 （只有當提供者是在執行階段載入時會引發的初始化方法）。Initialize 方法的第二個參數的型別 System.Collections.Specialized.NameValueCollection 而且參考&lt;新增&gt;與您在 web.config 檔案中的自訂提供者相關聯的項目。 該項目看起來如下：

[!code-xml[Main](membership/samples/sample6.xml)]

以下是 Initialize 方法的範例。

[!code-csharp[Main](membership/samples/sample7.cs)]

若要驗證使用者，當您登入表單送出時，您必須使用 ValidateUser 方法。 當使用者按一下登入控制項中的 [登入] 按鈕，就會引發這個方法。 您會將您的程式碼會執行使用者查閱，在此方法。

如您所見，撰寫您自己的成員資格提供者並不困難，並可讓您擴充這項功能強大的 ASP.NET 2.0 功能。
