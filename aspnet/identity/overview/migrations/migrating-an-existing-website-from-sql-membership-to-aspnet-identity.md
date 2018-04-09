---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: 從 SQL 成員資格移轉現有的網站，以 ASP.NET Identity |Microsoft 文件
author: Rick-Anderson
description: 本教學課程說明如何移轉現有的 web 應用程式與使用者和角色建立使用新的 ASP.NET Identity 的 SQL 成員資格的資料...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2790f32bc74cecf450f5a258fc1ff5b280a63923
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>從 SQL 成員資格移轉現有的網站，以 ASP.NET Identity
====================
由[Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)

> 此教學課程說明如何移轉現有的 web 應用程式與使用者角色資料建立新的 ASP.NET 識別系統中使用 SQL 成員資格的步驟。 這種方法牽涉到將現有的資料庫結構描述變更為 ASP.NET Identity 及舊/新增至該類別中的勾點所需的一個。 您採用這種方式，一旦移轉之後，身分識別的未來更新將毫不費力地處理。


此教學課程中，我們會使用 Visual Studio 2010 建立使用者和角色的資料所建立的 web 應用程式範本 (Web Form)。 然後我們將使用 SQL 指令碼將現有的資料庫移轉至所需的身分識別系統資料表。 接下來我們將安裝必要的 NuGet 套件，並加入新的帳戶管理頁面的成員資格管理會使用身分識別系統。 移轉的測試，以建立使用 SQL 成員資格的使用者應該能夠登入和新的使用者應該能夠註冊。 您可以找到完整的範例[這裡](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)。 另請參閱[從 ASP.NET 成員資格移轉至 ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html)。

## <a name="getting-started"></a>使用者入門

### <a name="creating-an-application-with-sql-membership"></a>建立應用程式使用 SQL 成員資格

1. 我們需要使用現有的應用程式，使用 SQL 成員資格及使用者和角色的資料來啟動。 為了本文中，我們來建立 Visual Studio 2010 中的 web 應用程式。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. 使用 ASP.NET 組態工具，建立 2 位使用者： **oldAdminUser**和**oldUser。**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. 建立名為系統管理員的角色，並將 'oldAdminUser' 新增為該角色的使用者。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. 建立站台的系統管理區段 Default.aspx。 若要啟用系統管理員角色中的使用者存取的 web.config 檔案中設定授權標記。 可以在這裡找到更多資訊 [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. 在 伺服器總管，以了解 SQL 成員資格系統所建立的資料表檢視的資料庫。 使用者登入的資料儲存在 aspnet\_使用者和 aspnet\_成員資格資料表，而角色資料會儲存在 aspnet\_Roles 資料表。 有關哪些使用者可在哪些角色儲存在 aspnet 資訊\_為 UsersInRoles 資料表。 基本成員資格管理便可移植到 ASP.NET Identity 系統了上述資料表中的資訊。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>移轉至 Visual Studio 2013

1. 安裝適用於 Web 或搭配 Visual Studio 2013 的 Visual Studio Express 2013[最新更新](https://www.microsoft.com/download/details.aspx?id=44921)。
2. 您已安裝的 Visual Studio 版本中開啟上述的專案。 如果電腦上未安裝 SQL Server Express，當您開啟專案，因為連接字串使用 SQL Express 時，會顯示提示。 您可以選擇要安裝 SQL Express 或做為解決變更 LocalDb 的連接字串。 我們將會為 LocalDb 變更這個發行項。
3. 開啟 web.config，然後變更的連接字串。以 (LocalDb) v11.0 SQLExpess。 移除 ' 使用者執行個體 = true' 從連接字串。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. 開啟 伺服器總管，並確認可以觀察的資料表結構描述和資料。
5. ASP.NET Identity 系統搭配 4.5 或更高版本的 framework 版本。 將目標重定為 4.5 或更高的應用程式。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    建置專案，以確認沒有任何錯誤。

### <a name="installing-the-nuget-packages"></a>正在安裝 Nuget 封裝

1. 在 [方案總管] 中，以滑鼠右鍵按一下專案&gt;**管理 NuGet 封裝**。 在 [搜尋] 方塊中，輸入 「 Asp.net 識別 」。 在結果清單中選取的封裝，並按一下 [安裝]。 按一下 [我接受] 按鈕，接受授權合約。 請注意，此套件會安裝相依性套件： EntityFramework 和 Microsoft ASP.NET Identity Core。 同樣地，請安裝下列封裝 （如果您不想要啟用 OAuth 登入，略過最後 4 OWIN 封裝）：

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>將資料庫移轉至新的身分識別系統

下一個步驟是將現有的資料庫移轉至 ASP.NET Identity 系統所需的結構描述。 若要達到此我們執行 SQL 指令碼具有一組命令，建立新的資料表，並將現有的使用者資訊移轉到新的資料表。 指令碼檔案可以找到[這裡](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql)。

此指令碼檔案是針對此範例。 如果資料表的結構描述建立使用 SQL 成員資格已自訂或修改指令碼需要隨之變更。

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>如何產生結構描述移轉的 SQL 指令碼

ASP.NET Identity 類別現成可用的資料的現有使用者，我們需要將資料庫結構描述移轉至 ASP.NET 識別所需的一個。 加入新的資料表，並將現有的資訊複製到這些資料表，我們可以執行這項操作。 根據預設 ASP.NET Identity 會使用 EntityFramework 將身分識別模型類別對應至要存放區擷取資訊的資料庫。 這些模型類別會實作定義使用者和角色物件的識別核心介面。 資料表和資料庫中的資料行取決於這些模型類別。 EntityFramework 模型中的類別識別 v2.1.0 以及其屬性會定義如下

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| ID | 字串 | ID | RoleId | ProviderKey | ID |
| 使用者名稱 | 字串 | 名稱 | UserId | UserId | ClaimType |
| PasswordHash | 字串 |  |  | LoginProvider | ClaimValue |
| SecurityStamp | 字串 |  |  |  | 使用者\_識別碼 |
| Email | 字串 |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | 字串 |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

我們需要資料表的每個這些模型具有資料行對應至屬性。 中所定義的類別與資料表之間的對應`OnModelCreating`方法`IdentityDBContext`。 這就所謂的設定 fluent 應用程式開發的應用程式開發介面方法，可以找到更多資訊[這裡](https://msdn.microsoft.com/data/jj591617.aspx)。 如下所述為類別設定

| **類別** | **資料表** | **主索引鍵** | **外部索引鍵** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | ID |  |
| IdentityRole | AspnetRoles | ID |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | 使用者\_識別碼-&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | ID | 使用者\_識別碼-&gt;AspnetUsers |

我們可以使用這項資訊來建立 SQL 陳述式來建立新的資料表。 我們可以個別寫入每個陳述式，或產生整個視需要使用 EntityFramework PowerShell 命令，然後我們可以編輯的指令碼。 若要這樣做，請在 VS 開啟**Package Manager Console**從**檢視**或**工具**功能表

- 執行命令"Enable-migrations 」 來啟用 EntityFramework 移轉。
- 執行命令"add-migration 初始 」 建立初始設定程式碼，在 C# 中建立資料庫 / VB
- 最後一個步驟是執行 「 更新資料庫 – 指令碼 」 產生 SQL 指令碼的命令為基礎的模型類別。

這個資料庫產生指令碼可用來當做開頭，其中我們將會進行其他變更以加入新的資料行，並將資料複製。 的優點就是我們產生`_MigrationHistory`供 EntityFramework 模型類別未來版本的身分識別的版本變更時，修改資料庫結構描述的資料表。 

SQL 成員資格使用者資訊有其他內容以及識別使用者模型類別也就是電子郵件中的密碼嘗試次數、 上次登入日期、 上次鎖定日期等。這是有用的資訊，而且我們想要套用至身分識別系統上。 作法是將額外屬性加入至使用者模型，並將它們對應回資料庫的資料表資料行。 我們可以這樣做將類別加入該子類別`IdentityUser`模型。 我們可以將屬性加入至這個自訂類別，並編輯 SQL 指令碼，建立資料表時，加入相對應的資料行。 這個類別的程式碼後面會進一步說明文章中。 用於建立 SQL 指令碼`AspnetUsers`資料表之後會加入新的屬性

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

接下來，我們需要將現有的資訊從 SQL 成員資格資料庫複製到新加入的資料表，身分識別。 這可以透過 SQL 會將資料直接從一個資料表複製到另一個。 若要將資料加入至資料表的資料列中，我們使用`INSERT INTO [Table]`建構。 我們可以使用另一個資料表複製`INSERT INTO`陳述式，連同`SELECT`陳述式。 若要取得我們需要查詢的所有使用者資訊*aspnet\_使用者*和*aspnet\_成員資格*資料表，並將資料複製到*AspNetUsers*資料表。 我們使用`INSERT INTO`和`SELECT`連同`JOIN`和`LEFT OUTER JOIN`陳述式。 如需有關查詢和資料表之間複製資料的詳細資訊，請參閱[這](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx)連結。 此外在 AspnetUserLogins 和 AspnetUserClaims 資料表是空的開始因為沒有預設會對應至此的 SQL 成員資格資訊。 複製的唯一資訊是針對使用者和角色。 在先前步驟中所建立的專案，將會是 SQL 查詢來將資訊複製到使用者資料表

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

在上述的 SQL 陳述式，從每個使用者的相關資訊*aspnet\_使用者*和*aspnet\_成員資格*資料表複製的資料行*AspnetUsers*資料表。 只有在這裡的修改時，我們要複製密碼。 SQL 成員資格中的密碼的加密演算法會使用 'PasswordSalt' 和 'PasswordFormat'，因為我們要複製的太以及雜湊的密碼，讓它可以由身分識別用來解密密碼。 說明文章中取得進一步的自訂密碼 hasher 連結時。 

此指令碼檔案是針對此範例。 對於應用程式有更多的資料表，開發人員可以依照類似的方法，加入使用者模型類別上的其他屬性，並將它們對應至 AspnetUsers 資料表中的資料行。 若要執行指令碼，

1. 開啟 伺服器總管。 展開以顯示資料表的 'ApplicationServices' 連接。 以滑鼠右鍵按一下 [資料表] 節點，然後選取 [新增查詢] 選項

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. 在查詢視窗中，複製並貼上整個 Migrations.sql 檔案中的 SQL 指令碼。 請按 [執行] 箭號按鈕執行指令碼檔案。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    重新整理 [伺服器總管] 視窗。 資料庫中建立五個新的資料表。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    以下是如何將 SQL 成員資格資料表中的資訊對應至新的身分識別系統。

    aspnet\_角色-&gt; AspNetRoles

    asp\_netUsers 和 asp\_netMembership-&gt; AspNetUsers

    aspnet\_UserInRoles-&gt; AspNetUserRoles

    上一節所述，在 AspNetUserClaims 和 AspNetUserLogins 資料表是空的。 '鑑別子' 資料表中的欄位 AspNetUser 應該符合模型類別名稱的定義為下一個步驟。 PasswordHash 資料行也為格式 ' 加密的密碼 | 密碼 salt | 密碼格式 '。 這可讓您使用特殊 SQL 成員資格加密邏輯，以便您可以重複使用舊密碼。 在本文稍後說明中。

### <a name="creating-models-and-membership-pages"></a>建立模型和成員資格頁面

如先前所述，身分識別功能會使用 Entity Framework 來溝通之資料庫的預設儲存帳戶資訊。 若要使用資料表中的現有資料，我們需要建立模型類別將對應至資料表，並將它們連結身分識別系統中。 識別合約的一部分，模型類別應實作 Identity.Core dll 中定義的介面，或者也可以擴充現有 Microsoft.AspNet.Identity.EntityFramework 用於這些介面的實作。

在我們的範例，AspNetRoles、 AspNetUserClaims、 AspNetLogins 和 AspNetUserRole 資料表會有類似於識別系統的現有實作的資料行。 因此我們可以重複使用現有的類別對應至這些資料表。 AspNetUser 資料表有一些額外的資料行用來儲存 SQL 成員資格資料表中的其他資訊。 這可以藉由建立模型類別擴充 'IdentityUser' 的現有實作，並加入其他屬性對應。

1. 在專案中的建立 Models 資料夾並加入使用者的類別。 類別的名稱應該符合 'AspnetUsers' 資料表的 '鑑別子' 資料行中加入的資料。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    使用者類別應該擴充 IdentityUser 類別中找到*Microsoft.AspNet.Identity.EntityFramework* dll。 宣告類別中的對應回 AspNetUser 資料行的屬性。 屬性識別碼、 使用者名稱、 PasswordHash 和 SecurityStamp IdentityUser 中所定義，因此會省略。 以下是具有屬性的所有使用者類別的程式碼

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Entity Framework DbContext 類別，才能保存在模型資料表中的資料，以及從資料表以填入模型擷取資料。 *Microsoft.AspNet.Identity.EntityFramework* dll 定義的 IdentityDbContext 類別會擷取和儲存資訊的身分識別資料表進行互動。 IdentityDbContext&lt;tuser&gt;採用 'TUser' 類別可以是任何擴充 IdentityUser 類別的類別。

    建立新類別擴充 IdentityDbContext '模型' 資料夾下，傳遞步驟 1 中建立的 「 使用者 」 類別中的 ApplicationDBContext

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. 新的身分識別系統中的使用者管理是使用 UserManager&lt;tuser&gt;類別中定義*Microsoft.AspNet.Identity.EntityFramework* dll。 我們需要建立可擴充 UserManager，傳遞步驟 1 中建立的 「 使用者 」 類別中的自訂類別。

    在 [模型] 資料夾中建立新的類別延伸 UserManager UserManager&lt;使用者&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. 應用程式的使用者的密碼都會經過加密並儲存在資料庫中。 使用 SQL 成員資格中的密碼編譯演算法與不同的新識別系統中的一個。 若要重複使用舊密碼，我們需要舊的使用者登入時使用 SQL 成員資格演算法在新使用者的身分識別使用的密碼編譯演算法時，選擇性地解密密碼。

    UserManager 類別具有屬性 'PasswordHasher'，其中儲存的實作 'IPasswordHasher' 介面的類別執行個體。 這用來加密/解密密碼驗證的使用者交易期間。 在步驟 3 中所定義的 UserManager 類別，建立新類別 SQLPasswordHasher 並複製下列程式碼。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    藉由匯入的 System.Text 和 System.Security.Cryptography 命名空間解析編譯錯誤。

    EncodePassword 方法加密的密碼，根據預設 SQL 成員資格的密碼編譯實作。 這是來自 System.Web dll。 如果舊的應用程式的自訂實作則應該要反映在這裡使用。 我們必須定義兩種其他方法*HashPassword*和*VerifyHashedPassword*使用*EncodePassword*雜湊指定的密碼或確認純文字的方法與一個資料庫中現有的密碼。

    SQL 成員資格系統用 PasswordHash、 PasswordSalt 和 PasswordFormat 來雜湊時註冊，或變更其密碼，使用者所輸入的密碼。 在移轉期間 PasswordHash 中的資料行分隔 AspNetUser 資料表中儲存這三個欄位 ' |' 字元。 當使用者登入和密碼具有這些欄位時，我們會使用 SQL 成員資格 crypto 檢查密碼;否則我們會使用身分識別系統的預設密碼編譯來驗證密碼。 這種方式舊的使用者就不需要變更其密碼一旦移轉應用程式。
5. 宣告 UserManager 類別的建構函式，並將這個當做 SQLPasswordHasher 傳遞給建構函式中的屬性。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>建立新的帳戶管理頁面

移轉的下一個步驟是新增可讓使用者註冊並登入的帳戶管理頁面。 從 SQL 成員資格的舊帳戶頁面會使用不會用新的身分識別系統的控制項。 若要加入新的使用者管理頁面請依照教學課程中的，在以下連結[ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)步驟 「 加入要用於註冊您的應用程式的使用者 Web 表單 」 從開始因為我們已經建立專案並加入 NuGet封裝。

我們需要進行一些變更，才能使用我們在這裡有的專案範例。

- Register.aspx.cs 和 Login.aspx.cs 後置程式碼類別可以使用這個`UserManager`從建立使用者的身分識別封裝。 此範例使用 UserManager 在 [模型] 資料夾中加入先前所述的步驟。
- 使用而不是 IdentityUser Register.aspx.cs 和 Login.aspx.cs 程式碼後置類別中建立的使用者類別。 這在我們的自訂使用者類別中攔截到身分識別系統。
- 若要建立資料庫的部分可以略過。
- 開發人員必須設定新使用者 ApplicationId，以符合目前的應用程式識別碼。 這可以查詢此應用程式 ApplicationId Register.aspx.cs 類別中建立使用者物件之前，並將它設定建立使用者之前完成。 

    範例：

    定義方法來查詢 aspnet Register.aspx.cs 頁面中\_應用程式資料表，並根據應用程式名稱來取得應用程式識別碼

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    現在取得此物件上設定使用者

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

使用舊的使用者名稱和密碼來登入現有的使用者。 若要建立新的使用者使用 [註冊] 頁面。 同時確認使用者已在角色中如預期般。

移植至身分識別系統可協助使用者將開放驗證 (OAuth) 加入至應用程式。 此範例，請參閱[這裡](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)具有啟用 OAuth。

## <a name="next-steps"></a>後續步驟

在本教學課程介紹如何移植 ASP.NET Identity SQL 成員資格的使用者，但我們沒有連接埠設定檔資料。 在下一個教學課程中我們會查移植到新的身分識別系統的設定檔資料從 SQL 成員資格。

您可以保留這篇文章底部的意見反應。

*感謝您 Tom Dykstra 和 Rick Anderson 檢閱發行項。*
