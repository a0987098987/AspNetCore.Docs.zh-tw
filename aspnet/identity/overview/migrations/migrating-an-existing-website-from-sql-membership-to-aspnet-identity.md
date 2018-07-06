---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: 從 SQL 成員資格的現有網站移轉至 ASP.NET Identity |Microsoft Docs
author: Rick-Anderson
description: 本教學課程中說明的步驟來移轉現有的 web 應用程式與使用者建立使用新的 ASP.NET Identity 的 SQL 成員資格的角色資料...
ms.author: aspnetcontent
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4acb8c82c9b05de9d587466170f8fac4ef9b6dde
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812247"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>從 SQL 成員資格的現有網站移轉至 ASP.NET Identity
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)

> 本教學課程中說明的步驟來移轉現有的 web 應用程式與使用者和角色的資料建立新的 ASP.NET 身分識別系統中使用 SQL 成員資格。 這種方法涉及將現有的資料庫結構描述變更為 ASP.NET 身分識別及舊/新增至該類別中的勾點所需的一個。 您採用這種方法，一旦移轉您的資料庫之後，會讓您輕鬆處理識別未來的更新。


本教學課程中，我們要使用 Visual Studio 2010 來建立使用者和角色的資料建立 web 應用程式範本 (Web Form)。 我們接著將使用 SQL 指令碼，若要將現有的資料庫移轉至身分識別系統所需的資料表。 接下來我們將安裝必要的 NuGet 套件，並新增新的帳戶管理頁面用於成員資格管理的身分識別系統。 為移轉的測試，建立使用 SQL 成員資格的使用者應該能夠登入，而且新的使用者應該無法註冊。 您可以找到完整的範例[此處](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)。 另請參閱[從 ASP.NET 成員資格移轉至 ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html)。

## <a name="getting-started"></a>使用者入門

### <a name="creating-an-application-with-sql-membership"></a>建立具有 SQL 成員資格應用程式

1. 我們要開始使用現有的應用程式會使用 SQL 成員資格，且具有使用者和角色的資料。 針對本文目的，讓我們在 Visual Studio 2010 中建立 web 應用程式。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. 使用 [ASP.NET 組態] 工具中，建立 2 個使用者： **oldAdminUser**和**oldUser。**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. 建立名為系統管理員的角色，並將 'oldAdminUser' 新增為該角色的使用者。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. 使用 Default.aspx 中建立網站的 [管理] 區段。 以系統管理員角色的使用者存取的 web.config 檔案中設定授權標記。 可以在這裡找到更多的資訊 [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. 若要了解 SQL 成員資格系統所建立的資料表的 [伺服器總管] 中檢視的資料庫。 使用者登入資料會儲存在 aspnet\_使用者和 aspnet\_成員資格資料表，而角色資料會儲存在 aspnet\_Roles 資料表。 有關哪些使用者可在哪些角色會儲存在 aspnet 資訊\_為 UsersInRoles 資料表。 基本成員資格管理便足以移植到 ASP.NET 身分識別系統了上述資料表中的資訊。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>移轉至 Visual Studio 2013

1. 安裝 Visual Studio Express 2013 for Web 或搭配 Visual Studio 2013[最新的更新](https://www.microsoft.com/download/details.aspx?id=44921)。
2. 在您安裝的版本的 Visual Studio 中開啟上述的專案。 如果在電腦上未安裝 SQL Server Express，當您開啟專案，因為連接字串會使用 SQL Express 時，就是會顯示提示。 您可以選擇安裝 SQL Express 或做為因應措施變更 LocalDb 的連接字串。 本文中我們會將它變更 LocalDb。
3. 開啟 web.config，然後變更連接字串。到 (LocalDb) v11.0 SQLExpess。 移除 ' 的使用者執行個體 = true' 從連接字串。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. 開啟 伺服器總管，並確認可以觀察的資料表結構描述和資料。
5. ASP.NET 身分識別系統的運作方式與 4.5 或更新版本的 framework 版本。 將目標重定為 4.5 或更高的應用程式。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    建置專案以確認沒有任何錯誤。

### <a name="installing-the-nuget-packages"></a>安裝 Nuget 套件

1. 在 [方案總管] 中，以滑鼠右鍵按一下專案&gt;**管理 NuGet 套件**。 在 [搜尋] 方塊中，輸入 「 Asp.net 身分識別 」。 在結果清單中選取的封裝，並按一下 [安裝]。 藉由按一下 [我接受] 按鈕，接受授權合約。 請注意，此套件會安裝相依性套件： EntityFramework 和 Microsoft Asp.net 身分識別。 同樣地，請安裝下列封裝 （如果您不想要啟用 OAuth 登入，略過最後 4 個 OWIN 套件）：

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>將資料庫移轉至新的身分識別系統

下一個步驟是將現有的資料庫移轉至 ASP.NET 身分識別系統所需的結構描述。 若要達到這我們執行 SQL 指令碼他有一組命令，以建立新的資料表，並將現有的使用者資訊移轉到新的資料表。 可以找到指令碼檔案[此處](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql)。

此指令碼檔案是此範例中的特定項目。 如果資料表的結構描述建立使用 SQL 成員資格自訂或修改指令碼需要隨之變更。

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>如何產生結構描述移轉的 SQL 指令碼

ASP.NET 身分識別類別都是現成的現有使用者的資料，我們要將資料庫結構描述移轉至 ASP.NET 身分識別所需的一個。 藉由新增新的資料表，並將現有的資訊複製到這些資料表，我們可以執行這項操作。 根據預設 ASP.NET 身分識別會使用 EntityFramework 將身分識別模型類別對應至要儲存/擷取資訊的資料庫。 這些模型類別會實作核心身分識別介面，定義使用者和角色物件。 資料表和資料庫中的資料行根據這些模型類別。 EntityFramework 模型中的類別識別 v2.1.0 以及其屬性會定義如下

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| ID | 字串 | ID | 物件的 RoleId | ProviderKey | ID |
| 使用者名稱 | 字串 | 名稱 | 使用者識別碼 | 使用者識別碼 | ClaimType |
| PasswordHash | 字串 |  |  | LoginProvider | ClaimValue |
| SecurityStamp | 字串 |  |  |  | 使用者\_識別碼 |
| Email | 字串 |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| 電話號碼 | 字串 |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

我們需要為每個模型中具有對應至屬性的資料行的資料表。 類別與資料表之間的對應定義於`OnModelCreating`方法的`IdentityDBContext`。 這也稱為組態的 fluent API 方法，以及可以找到詳細資訊[此處](https://msdn.microsoft.com/data/jj591617.aspx)。 類別的組態是下面所述

| **類別** | **資料表** | **主索引鍵** | **外部索引鍵** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | ID |  |
| IdentityRole | AspnetRoles | ID |  |
| IdentityUserRole | AspnetUserRole | 使用者識別碼 + 物件的 RoleId | 使用者\_識別碼-&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | ID | 使用者\_識別碼-&gt;AspnetUsers |

我們可以使用這項資訊建立 SQL 陳述式來建立新的資料表。 我們可以個別寫入每個陳述式，或產生完整的指令碼，視需要使用 EntityFramework PowerShell 命令，我們接著可以編輯。 若要這樣做，在 VS 開啟**Package Manager Console**從**檢視**或是**工具**功能表

- 若要啟用 EntityFramework 移轉的執行的命令"Enable-migrations 」。
- 執行命令 」 新增移轉初始 」 建立初始設定程式碼，以建立資料庫 C# / vb。
- 最後一個步驟是執行 「 更新資料庫 – 指令碼 」 會產生 SQL 指令碼的命令為基礎的模型類別。

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

此資料庫產生指令碼可用來當做開頭，其中我們將會進行其他變更來加入新的資料行，並複製資料。 這個優點是我們產生`_MigrationHistory`供 EntityFramework 模型類別變更為身分識別發行前版本的未來版本時，修改資料庫結構描述的資料表。 

SQL 成員資格使用者資訊有其他屬性，除了在身分識別使用者模型類別中也就是電子郵件、 密碼嘗試次數、 上次登入日期、 最後一個鎖定日期等。這是很有用的資訊，我們希望它結轉到身分識別系統。 做法是將額外的屬性加入至使用者模型，並將它們對應回資料庫中的資料表資料行。 我們這樣可以新增類別子類別化`IdentityUser`模型。 我們可以將此自訂類別的屬性，並編輯 SQL 指令碼，以建立資料表時新增對應的資料行。 此類別的程式碼會進一步說明，文件中。 建立的 SQL 指令碼`AspnetUsers`資料表之後會新增新的屬性

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

接下來，我們需要將現有的資訊從 SQL 成員資格資料庫複製到新加入的資料表，身分識別。 這可以透過 SQL 將資料直接從一個資料表複製到另一個。 若要將資料加入至資料表的資料列中，我們使用`INSERT INTO [Table]`建構。 若要從另一個資料表，我們可以使用複製`INSERT INTO`陳述式，連同`SELECT`陳述式。 若要取得我們要查詢的所有使用者資訊*aspnet\_使用者*並*aspnet\_成員資格*資料表，並將資料複製到*AspNetUsers*資料表。 我們會使用`INSERT INTO`並`SELECT`連同`JOIN`和`LEFT OUTER JOIN`陳述式。 如需有關查詢和資料表之間複製資料的詳細資訊，請參閱[這](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx)連結。 此外 AspnetUserLogins 和 AspnetUserClaims 資料表是空的一開始由於沒有對應至這個預設的 SQL 成員資格資訊。 複製的唯一資訊是對使用者和角色。 在先前步驟中建立的專案，會將資訊複製到使用者資料表的 SQL 查詢

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

在上述的 SQL 陳述式，從每個使用者的相關資訊*aspnet\_使用者*並*aspnet\_成員資格*資料表已複製到的資料行*AspnetUsers*資料表。 此處作法僅修改時，我們將複製的密碼。 由於 SQL 成員資格中的密碼的加密演算法會使用 'PasswordSalt' 和 'PasswordFormat'，所以我們複製，太以及雜湊的密碼，讓它可以識別所用來解密密碼。 自訂密碼雜湊程式連結時，文件中進一步說明。 

此指令碼檔案是此範例中的特定項目。 有其他資料表的應用程式，開發人員可以依照類似的方法，使用者模型類別上加上額外的屬性，並將它們對應至在 AspnetUsers 資料表中的資料行。 若要執行指令碼，

1. 開啟 伺服器總管。 展開以顯示資料表的 'ApplicationServices' 連接。 以滑鼠右鍵按一下 [資料表] 節點，然後選取 [新增查詢] 選項

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. 在查詢視窗中，複製並貼上整個 Migrations.sql 檔案中的 SQL 指令碼。 按下 [執行] 箭號按鈕，以執行指令碼檔案。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    重新整理 [伺服器總管] 視窗。 在資料庫中建立五個新的資料表。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    以下是如何將 SQL 成員資格資料表中的資訊對應到新的身分識別系統。

    aspnet\_角色-&gt; AspNetRoles

    asp\_netUsers 和 asp\_netMembership-&gt; AspNetUsers

    aspnet\_UserInRoles-&gt; [aspnetuserroles]

    上一節所述，在 AspNetUserClaims 和 AspNetUserLogins 資料表是空的。 '鑑別子' 資料表中的欄位 AspNetUser 應該符合下一個步驟中所定義的模型類別名稱。 PasswordHash 資料行也的格式為 ' 加密的密碼 | 密碼 salt | 密碼格式 '。 這可讓您使用特殊 SQL 成員資格密碼編譯的邏輯，以便您可以重複使用舊密碼。 在本文稍後說明中。

### <a name="creating-models-and-membership-pages"></a>建立模型和成員資格頁面

如先前所述，身分識別功能會使用 Entity Framework 與資料庫中的預設儲存帳戶資訊。 若要使用資料表中的現有資料，我們要建立模型類別將對應至資料表，並將它們連結在身分識別系統中。 身分識別合約的一部分，模型類別應實作 Identity.Core dll 中定義的介面，或可以擴充現有 Microsoft.AspNet.Identity.EntityFramework 這些介面的實作。

在我們的範例，AspNetRoles、 AspNetUserClaims、 AspNetLogins 和 AspNetUserRole 資料表會有類似於現有的身分識別系統實作的資料行。 因此我們可以重複使用現有的類別，將對應至這些資料表。 AspNetUser 資料表會有一些額外的資料行用來儲存 SQL 成員資格資料表中的其他資訊。 這可以藉由建立模型類別來擴充現有的 'IdentityUser' 實作，並加入其他屬性對應。

1. 建立模型專案中的資料夾，並新增使用者的類別。 類別的名稱應該符合 'AspnetUsers' 資料表的 '鑑別子' 資料行中新增的資料。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    使用者類別應該擴充 IdentityUser 類別中找到*Microsoft.AspNet.Identity.EntityFramework* dll。 宣告類別中對應至 AspNetUser 資料行的屬性。 屬性識別碼、 使用者名稱、 PasswordHash 和 SecurityStamp IdentityUser 中所定義，並因此會省略。 以下是具有所有屬性的使用者類別的程式碼

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Entity Framework DbContext 類別，才能保存在模型資料表中的資料，以及從表格來擴展模型擷取資料。 *Microsoft.AspNet.Identity.EntityFramework* dll 定義互動來擷取與儲存資訊的身分識別資料表的 IdentityDbContext 類別。 IdentityDbContext&lt;tuser&gt;會將 'TUser' 類別可以是任何擴充 IdentityUser 類別的類別。

    建立新的類別延伸 IdentityDbContext 'Models' 資料夾中，傳遞步驟 1 中建立的 「 使用者 」 類別下的 ApplicationDBContext

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. 在新的身分識別系統中的使用者管理工作是使用 UserManager&lt;tuser&gt;類別中定義*Microsoft.AspNet.Identity.EntityFramework* dll。 我們需要建立可擴充 UserManager，傳遞步驟 1 中建立的 「 使用者 」 類別中的自訂類別。

    在 Models 資料夾中建立新的類別延伸 UserManager UserManager&lt;使用者&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. 應用程式的使用者的密碼會加密，並儲存在資料庫中。 使用 SQL 成員資格的密碼編譯演算法與新的身分識別系統中不同。 若要重複使用舊密碼，我們需要舊的使用者登入身分識別中使用的密碼編譯演算法，為新的使用者時所使用的 SQL 成員資格演算法時，選擇性地將解密密碼。

    UserManager 類別都有屬性 'PasswordHasher' 儲存類別可實作 'IPasswordHasher' 介面的執行個體。 這用來加密/解密密碼驗證的使用者交易期間。 在步驟 3 中所定義的 UserManager 類別，建立新類別 SQLPasswordHasher 並複製下列程式碼。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    匯入 System.Text 和 System.Security.Cryptography 命名空間，以解決編譯錯誤。

    根據預設 SQL 成員資格的密碼編譯實作密碼加密 EncodePassword 方法。 這是取自 System.Web dll。 如果舊的應用程式使用自訂的實作則應該會反映這裡。 我們必須定義兩種其他方法*HashPassword*並*VerifyHashedPassword*使用*EncodePassword*雜湊指定的密碼或確認純文字的方法與一個資料庫中現有的密碼。

    SQL 成員資格系統用 PasswordHash、 PasswordSalt 和 PasswordFormat 來雜湊註冊或變更其密碼時，使用者所輸入的密碼。 在移轉期間，所有的三個欄位會儲存在 PasswordHash 資料表中資料行 AspNetUser 分隔 ' |' 字元。 當使用者登入且密碼有這些欄位時，我們會使用 SQL 成員資格 crypto 檢查密碼;否則我們可以使用身分識別系統的預設密碼編譯驗證的密碼。 這種方式舊使用者就不需要變更其密碼，一旦移轉應用程式。
5. 宣告 UserManager 類別的建構函式，並將這個當做 SQLPasswordHasher 傳遞給建構函式中的屬性。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>建立新的帳戶管理頁面

移轉的下一個步驟是新增可讓使用者註冊和登入的帳戶管理頁面。 從 SQL 成員資格的舊帳戶頁面會使用不適用於新的身分識別系統的控制項。 若要新增新的使用者管理頁面會遵循此連結的教學課程[ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)從步驟 '加入要用於註冊您的應用程式的使用者 Web Form' 因為我們已建立專案並新增 NuGet封裝。

我們需要進行一些變更來處理我們在這裡有專案範例。

- Register.aspx.cs 和 Login.aspx.cs 背後的程式碼類別使用`UserManager`從身分識別的封裝建立的使用者。 此範例使用 UserManager Models 資料夾中新增先前所述的步驟。
- 使用而不是 IdentityUser Register.aspx.cs 和 Login.aspx.cs 類別背後的程式碼中建立的使用者類別。 這在我們的自訂使用者類別連結到 身分識別系統。
- 若要建立資料庫的部分可略過。
- 開發人員必須設定新使用者 ApplicationId，以符合目前的應用程式識別碼。 這可以藉由查詢這個應用程式 ApplicationId Register.aspx.cs 類別中建立使用者物件之前，並將它設定建立使用者之前完成。 

    範例：

    定義方法來查詢 aspnet Register.aspx.cs 頁面中\_應用程式資料表，並根據應用程式名稱中取得的應用程式識別碼

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    立即取得此物件上設定使用者

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

使用舊的使用者名稱和密碼來登入現有的使用者。 若要建立新的使用者使用 [註冊] 頁面。 也請確認使用者正在角色如預期般運作。

移植到身分識別系統，可協助使用者將開放驗證 (OAuth) 新增至應用程式。 此範例，請參閱[此處](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)具有啟用 OAuth。

## <a name="next-steps"></a>後續步驟

在本教學課程中，我們示範如何移植到 ASP.NET 身分識別，從 SQL 成員資格使用者，但我們並未連接埠設定檔資料。 在下一個教學課程中我們會查移植到新的身分識別系統的設定檔資料從 SQL 成員資格。

您可以保留在這篇文章底部提供意見反應。

*感謝 Tom Dykstra 和 Rick Anderson 檢閱文件。*
