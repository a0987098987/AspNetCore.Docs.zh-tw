---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: 成員資格 |Microsoft Docs
author: microsoft
description: ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。 ASP.NET 表單驗證會提供便利的方式來 incorp...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: f776ed628e206c06543589767ba364af3c76ae16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818213"
---
<a name="membership"></a>成員資格
====================
by [Microsoft](https://github.com/microsoft)

> ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。 ASP.NET 表單驗證會提供便利的方式來併入您的 ASP.NET 應用程式的登入表單，並驗證使用者針對資料庫或其他資料存放區。


ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。 ASP.NET 表單驗證會提供便利的方式來併入您的 ASP.NET 應用程式的登入表單，並驗證使用者針對資料庫或其他資料存放區。 FormsAuthentication 類別的成員可使您能夠處理驗證的 cookie、 檢查有有效的登入、 記錄將使用者登出等。不過，在 ASP.NET 1.x 應用程式中實作表單驗證，可能需要相當多的程式碼。

在 ASP.NET 2.0 中的成員資格是實現了重大改進，透過使用單獨的表單驗證。 （成員資格是與表單驗證，搭配最強大，但使用表單驗證，就不需要）。您很快就會看到，您可以使用 ASP.NET Membership 與 login 控制項在 ASP.NET 2.0 中實作功能強大的成員資格系統，而不需要完全撰寫太多程式碼。

## <a name="implementing-membership-in-aspnet-20"></a>在 ASP.NET 2.0 中實作成員資格

成員資格是由下列四個步驟來實作。 請記住，有許多可以一併實作的選擇性組態以及所需的子步驟。 這些步驟是為了說明設定成員資格的全貌。

1. 建立成員資格資料庫 （如果 SQL Server 做為成員資格儲存區。）
2. 在您的應用程式組態檔中指定的成員資格選項。 （預設被啟用的成員資格）。
3. 決定您想要使用的成員資格存放區的類型。 選項包括： 

    - Microsoft SQL Server （版本 7.0 或更新版本）
    - Active Directory 存放區
    - 自訂成員資格提供者
4. 設定 ASP.NET 表單驗證的應用程式。 同樣地，成員資格可利用表單驗證，但使用表單驗證，就不需要。
5. 定義成員資格的使用者帳戶及設定角色，如有需要。

## <a name="creating-the-membership-database"></a>建立成員資格資料庫

如果您正在使用 SQL Server 7.0 或更新版本為您的成員資格儲存區中，您可以使用 aspnet\_regsql 公用程式 （可輕易從 Visual Studio.NET 2005年命令提示字元） 來設定您的資料庫。 Aspnet\_regsql 公用程式可用的命令提示字元工具或透過經由 GUI 精靈。 精靈的方法是最簡單的方式來設定您的資料庫。 若要存取精靈，只要執行下列命令：

`aspnet_regsql W`

一旦您執行該命令時，您會看到使用 ASP.NET SQL Server 安裝精靈，如下所示。


![](membership/_static/image1.jpg)

**圖 1**


ASP.NET SQL Server 安裝精靈會建立網站，您在精靈中指定的執行個體中。 不過，ASP.NET 會使用連接字串在 machine.config 檔案中連接到您的資料庫。 根據預設，此連接字串會指向的 SQL Server 2005 執行個體，因此如果您使用的 SQL Server 2000 或 SQL Server 7.0 執行個體，您必須修改 machine.config 檔案中的連接字串。 該連接字串可以在以下：

[!code-xml[Main](membership/samples/sample1.xml)]

不幸的是，如果您未修改連接字串，ASP.NET 將不會提供描述性的錯誤。 它只會繼續抱怨說，您還沒有建立資料庫。 在上述案例中，我已經修改連接字串以指向我的本機 SQL Server 2000 執行個體。

## <a name="specifying-configuration-and-adding-users-and-roles"></a>指定的組態並新增使用者和角色

設定成員資格的下一個步驟是將所需的資訊新增至應用程式的 web.config 檔案。 在 ASP.NET 1.x 中，修改 web.config 檔案難有時由於 lowerCamelCase 使用，其中沒有 Intellisense。 Visual Studio.NET 2005年讓工作更容易使用 Intellisense 的組態檔，但 ASP.NET 2.0 則進一步編輯組態檔中提供的 Web 介面。

您可以按一下 [方案總管] 工具列，如下所示的 [ASP.NET 組態] 按鈕來啟動 Web 介面。 您也可以啟動 Web 介面，透過插入登入控制項時顯示快顯視窗。


![](membership/_static/image2.jpg)

**圖 2**


這會啟動 ASP.NET 網站管理工具如下所示。 ASP.NET 網站管理是四個索引標籤介面，可讓您輕鬆地管理應用程式設定。 可使用下列索引標籤：

- **Home**
- **安全性**設定使用者、 角色和存取。
- **應用程式**設定應用程式設定。
- **提供者**設定及測試您的應用程式成員資格提供者。

Web Site Administration Tool 可讓您輕鬆地建立新的使用者，建立新的角色，以及管理使用者和角色。 在 Windows 介面中，不提供此功能。 Windows 介面可讓您輕鬆地定義授權設定，以及新增、 刪除和管理提供者，在 Web Site Administration Tool 所沒有的功能。

若要啟動的 Windows 介面，開啟 Internet Information Services 嵌入式管理單元，以滑鼠右鍵按一下您的應用程式，並選擇屬性。 按一下 [ASP.NET] 索引標籤，然後按一下 [編輯設定] 按鈕。 （應用程式必須在啟用 [編輯設定] 按鈕的 ASP.NET 2.0 下執行。 您可以設定的 ASP.NET 版本在 [ASP.NET] 對話方塊。）[ASP.NET 組態設定] 對話方塊隨即出現，如下所示。


![](membership/_static/image3.jpg)

**圖 3**


在 [一般] 索引標籤上會列出連接字串和應用程式設定。 斜體的任何設定父組態檔 （machine.config 或更高層級的 web.config） 中定義及設定不會以斜體顯示從應用程式組態檔。 如果設定已加入，已移除或編輯應用程式層級，ASP.NET 會新增、 移除或修改在應用程式層級 web.config，而不是從組態檔，從中繼承移除設定的設定。

[驗證] 索引標籤如下所示。 這是您將在其中設定您的成員資格設定。 表單驗證設定，成員資格提供者，，和角色提供者可以在此設定。


![](membership/_static/image4.jpg)

**圖 4**


## <a name="implementing-membership-in-your-application"></a>在 您的應用程式中實作成員資格

實作 ASP.NET 2.0 成員資格應用程式中的最簡單方式是使用提供的登入控制項。 這個方法可讓您不需要撰寫任何程式碼完全實作 ASP.NET 2.0 成員資格的基本概念。

ASP.NET 2.0 中提供下列登入控制項︰

## <a name="login-control"></a>Login 控制項

Login 控制項提供有人登入您的成員資格系統的介面。 它會提供您使用者名稱和密碼 textboxt] 和 [登入按鈕。 許多其他常見功能，例如連結的人註冊尚未這樣做，核取方塊，可讓使用者自動登入在後續造訪時，密碼提示，等等的連結。Login 控制項的所有功能都都可透過控制項的屬性自訂項目。

在 ASP.NET 1.x 中，開發人員必須撰寫一大堆程式碼來執行查詢，使用表單驗證時。 使用 ASP.NET 2.0 成員資格，也可以驗證使用者，而不需要撰寫任何程式碼完全。 ASP.NET 會自動為您上進行使用者執行的查閱。 (如果您使用 Login 控制項，而不使用 ASP.NET 成員資格，您可以使用**OnAuthenticate**方法以驗證使用者。)

## <a name="loginview-control"></a>LoginView 控制項

LoginView 控制項是預設的情況下，提供兩個範本的樣板化控制項AnonymousTemplate 和 LoggedInTemplate。 顯示的範本取決於使用者已登入您的成員資格系統。 這個控制項通常用來顯示登入控制項時，使用者有尚未登入和 LoginStatus 控制項和/或其他登入控制項，當使用者已登入。 如果您使用角色管理 ASP.NET 應用程式中，LoginView 控制項可以顯示特定的範本，根據使用者角色。 （更多有關 ASP.NET 角色管理後續將說明。）

## <a name="passwordrecovery-control"></a>Provider 控制項

Provider 控制項可讓使用者會收到一封電子郵件與他或她的目前密碼或重設其密碼。 純文字和加密的密碼可復原，並在電子郵件傳送給使用者。 如果密碼已雜湊，就無法復原。 而是使用者必須執行密碼重設。

## <a name="loginstatus-control"></a>LoginStatus 控制項

LoginStatus 控制項用來顯示目前已登入的使用者未登入的使用者登入指標和登出指標。 Request.IsAuthenticated 屬性用來判斷哪一個標記，來顯示。 LoginStatus 控制項所顯示的指標可以是文字 (透過實作**LoginText**並**LogoutText**屬性) 或映像 (可透過實作**LoginImageUrl**並**LogoutImageUrl**屬性。)

當使用者登出透過 LoginStatus 控制項時，他或她會重新導向至所指定的 URL **LogoutPageUrl**屬性。 如果未設定該屬性，會重新整理目前的頁面。 由於網站可能受到表單驗證，目前頁面的重新整理會將使用者重新導向網站的登入頁面。

## <a name="loginname-control"></a>LoginName 控制項

LoginName 控制項顯示之使用者的目前登入網站的使用者名稱。

## <a name="createuserwizard-control"></a>CreateUserWizard 控制項

CreateUserWizard 控制項提供便利的方式來註冊您的成員資格系統使用者。 您可以透過如下所示的介面新增 （實作為 v kolekci WizardSteps 的集合） 的步驟。


![](membership/_static/image5.jpg)

**圖 5**


CreateUserWizard 是樣板化控制項衍生自精靈類別，並提供下列範本：

- **HeaderTemplate**此範本會控制在精靈的標頭的外觀。
- **SidebarTemplate**此範本會控制在精靈的資訊看板 的外觀。
- **StartNavigationTemplate**導覽的外觀會在開始步驟在精靈的這個樣板控制項。
- **StepNavigationTemplate**此範本會控制瀏覽區，不在開始 或 完成 步驟時的外觀。
- **FinishNavigationTemplate**此範本會控制瀏覽區，在 [完成] 步驟上的外觀。

此外，您將加入至精靈每個步驟中，ASP.NET 會建立自訂範本，其中包含一個 ContentTemplate 和 CustomNavigationTemplate 該步驟。 如需自訂 CreateUserWizard 的完整詳細資訊，請參閱 VS.NET 2005 文件：

## <a name="changepassword-control"></a>ChangePassword 控制項

ChangePassword 控制項允許使用者變更其密碼。 如果 DisplayUserName 屬性為 true （它是預設為 false），使用者可以在未登入時變更他或她的密碼。 如果使用者*是*已經登入和 DisplayUserName 屬性為 true 時，使用者將能夠變更未登入時提供他們知道該使用者的使用者識別碼的另一位使用者的密碼。

請記住，如果您想要能夠變更密碼，而不需要登入的使用者，您必須確保 ChangePassword 控制項顯示的頁面允許匿名存取。 很明顯地，使用者必須提供舊密碼，才能變更其密碼。

## <a name="role-management"></a>角色管理

角色管理可讓您將使用者指派給特定的角色，並接著限制存取特定檔案或該角色為基礎的資料夾。 角色管理也會提供 API，讓您能夠以程式設計方式判斷地角色或判斷特定角色中的所有使用者並據以回應。

角色管理，就不需要在 ASP.NET 成員資格，也不是成員資格，才能使用角色管理的需求。 不過，這兩個補充彼此得很好，而且很可能，開發人員會互相搭配使用。

若要啟用您的應用程式中的角色管理，請在 web.config 檔案進行下列變更：

[!code-xml[Main](membership/samples/sample2.xml)]

當**cacheRolesInCookie**屬性設為 true，ASP.NET 會快取在 cookie 中的用戶端上的使用者角色成員資格。 這可讓角色查閱 RoleProvider 呼叫的情況下發生。 當使用這個屬性時，開發人員都以確保**cookieProtection**屬性會設定為 All。 （這是預設值）。這可確保 cookie 資料會加密，並協助確保 cookie 內容未遭到變更。 可以使用 Web Site Administration Tool 加入角色。 它可讓您輕鬆地定義角色、 設定這些角色為基礎的站台的組件的存取，並將使用者指派給角色。


![](membership/_static/image6.jpg)

**圖 6**


如上所示，可以加入新的角色，只要輸入角色的名稱，然後按一下 新增角色。 可以管理或在現有的角色清單中的適當連結，即可刪除現有的角色。

當您管理角色時，您可以新增或移除使用者，如下所示。


![](membership/_static/image7.jpg)

**圖 7**


藉由檢查使用者在角色的核取方塊，您可以輕鬆地將使用者加入特定的角色。 ASP.NET 會自動使用適當的項目更新成員資格資料庫。 您也會想要設定您的應用程式的存取規則。 透過這項操作的 ASP.NET 1.x 開發人員熟悉&lt;授權&gt;在 web.config 檔案中，以及該選項的項目是 ASP.NET 2.0 中仍然可用。 不過，其更容易管理的存取規則使用 Web Site Administration Tool 所示下方。


![](membership/_static/image8.jpg)

**圖 8**


在此情況下，管理資料夾會反白顯示 （難以查看，因為此工具會反白顯示它以淺灰色） 和系統管理員角色授與存取權。 會拒絕所有其他使用者。 您可以按一下標頭的圖示，以選取的規則，然後使用往上移和下移按鈕排列的規則。 使用 ASP.NET&lt;授權&gt;項目，以其出現的順序處理規則。 換句話說，如果在上述螢幕擷取規則的順序反轉，沒有人必須存取管理資料夾因為 ASP.NET 會遇到的第一個規則的規則，拒絕每個人的資料夾。

ASP.NET 2.0 會將您指定的存取規則的資料夾中的 web.config 檔案。 透過組態檔或透過 Web Site Administration Tool，就可以編輯的存取規則。 換句話說，Web Site Administration Tool 是只可以在方便使用的環境中編輯組態檔透過此介面。

## <a name="using-roles-in-code"></a>在程式碼中使用角色

角色管理的 API 版本之後並未變更 1.x。 **IsInRole**方法用來判斷使用者是否具有特定角色。

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET 也會建立目前內容的成員身分的 RolePrincipal 執行個體。 RolePrincipal 物件可用來取得所有角色給使用者所屬，如下所示：

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>LoginView 控制項搭配使用 kolekci RoleGroups

既然您已了解角色管理和成員資格，可讓簡短討論 LoginView 控制項如何在 ASP.NET 2.0 會充分利用這項功能。 如先前所述，LoginView 控制項是包含兩個範本預設情況下，樣板化控制項AnonymousTemplate 和 LoggedInTemplate。 內 LoginView 工作對話方塊是連結 （如下所示），可讓您 upravit kolekci RoleGroups。


![](membership/_static/image9.jpg)

**圖 9**


每個 RoleGroup 物件包含字串陣列，定義 RoleGroup 適用於哪些角色。 若要加入新的 RoleGroup LoginView 控制項，按一下 編輯 kolekci RoleGroups 連結。 在上圖中，您可以看到我已新增新的 RoleGroup 系統管理員。 選取該 RoleGroup （RoleGroup[0]) 檢視下拉式清單中的，我可以設定的範本，則只會顯示系統管理員角色的成員。 在下圖中，我已新增新的 RoleGroup 適用於 「 銷售 」 角色和發佈角色的成員。 這會將第二個 RoleGroup 加入 LoginView 工作對話方塊中的 檢視 下拉式清單和任何項目加入至該範本會顯示 Sales 或 通訊群組中的任何使用者角色。


![](membership/_static/image10.jpg)

**圖 10**


## <a name="overriding-the-existing-membership-provider"></a>覆寫現有的成員資格提供者

有幾種方式，您可以擴充功能的 ASP.NET 成員資格。 首先，您可以從它繼承並覆寫其方法很明顯地變更 SqlMembershipProvider 類別的現有功能。 比方說，如果您想要實作您自己的功能，建立使用者時，您可以建立您自己的類別繼承從 SqlMembershipProvider，如下所示：

[!code-csharp[Main](membership/samples/sample5.cs)]

如果相反地，您會想要建立您自己的提供者 （例如將成員資格資訊儲存在 Access 資料庫），您可以建立自己的提供者。

## <a name="creating-your-own-membership-provider"></a>建立您自己的成員資格提供者

若要建立您自己的成員資格提供者，您必須建立繼承自 MembershipProvider 類別的類別。 如果您使用的 VB.NET，Visual Studio 2005 會將所有您需要覆寫的方法虛設常式。 如果您使用的 C# 中，您將新增虛設常式。

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

Thats 身為 C# 開發人員實作相當的清單。 您可能會發現它在 VB.NET 中建立類別，而不需要任何實作，然後使用.NET 反射程式或類似工具來轉換成 C# 的程式碼變得更加容易。

連接字串和其他屬性應該設定為其 Initialize 方法中的預設值。 （只有當提供者會在執行階段載入時會引發的 Initialize 方法）。Initialize 方法的第二個參數是型別 System.Collections.Specialized.NameValueCollection，且具備的參考&lt;新增&gt;與您的 web.config 檔案中的自訂提供者相關聯的項目。 該項目看起來如下所示：

[!code-xml[Main](membership/samples/sample6.xml)]

Initialize 方法的範例如下。

[!code-csharp[Main](membership/samples/sample7.cs)]

若要驗證使用者，當您登入表單送出時，您必須使用 ValidateUser 的方法。 當使用者按一下登入控制項中的 [登入] 按鈕，就會引發這個方法。 您會將您會執行使用者查閱，在此方法的程式碼。

如您所見，撰寫您自己的成員資格提供者並不困難，並可讓您擴充這個強大的功能，ASP.NET 2.0。
