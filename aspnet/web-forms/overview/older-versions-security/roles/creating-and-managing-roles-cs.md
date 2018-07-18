---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: 建立及管理角色 (C#) |Microsoft Docs
author: rick-anderson
description: 本教學課程會檢查角色架構中設定所需的步驟。 接下來，我們將建置 web 頁面來建立和刪除角色。
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: 84b9624aaae0cc98f1908b4521cee43bce6e856d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806868"
---
<a name="creating-and-managing-roles-c"></a>建立及管理角色 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> 本教學課程會檢查角色架構中設定所需的步驟。 接下來，我們將建置 web 頁面來建立和刪除角色。


## <a name="introduction"></a>簡介

在  <a id="_msoanchor_1"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-cs.md)教學課程中我們示範了使用 URL 授權來限制某些使用者一組頁面，然後瀏覽的宣告式和適用於調整造訪的使用者為基礎的 ASP.NET 頁面的功能程式設計技術。 授與權限存取頁面或使用者的使用者為基礎的功能，不過，可能會維護工作的夢魘案例中有許多使用者帳戶或經常變更使用者的權限。 每當使用者獲得或失去授權來執行特定工作中，系統管理員必須更新適當的 URL 授權規則、 宣告式標記和程式碼。

它通常有助於將使用者分類成群組或*角色*然後套用權限角色的角色為基礎。 例如，大部分的 web 應用程式有一組特定的頁面或保留只適用於系統管理使用者的工作。 在中使用的技術所學*使用者為基礎的授權*教學課程中，我們會新增適當的 URL 授權規則、 宣告式標記和程式碼，以允許指定的使用者帳戶，以執行系統管理工作。 但如果已加入新的系統管理員，或現有的系統管理員需要能夠撤銷她的系統管理權限，我們必須傳回並更新組態檔和網頁。 使用角色，不過，我們無法建立名為系統管理員角色並將這些信任的使用者指派給系統管理員角色。 接下來，我們會新增適當的 URL 授權規則、 宣告式標記和程式碼，以允許系統管理員角色，才能執行各種系統管理工作。 使用此基礎結構，將新的系統管理員新增至站台，或移除現有的是簡單，只要加入或移除系統管理員角色的使用者。 任何設定、 宣告式標記或變更程式碼是必要的。

ASP.NET 提供了定義角色，並將它們與使用者帳戶建立關聯的角色架構。 使用角色架構我們可以建立和刪除角色，將使用者新增或移除角色中的使用者，請判斷一組屬於特定的角色，並告知使用者是否屬於特定角色的使用者。 設定角色架構之後, 我們可以限制網頁 URL 授權規則透過以角色的角色為基礎的存取權及顯示或隱藏其他資訊或根據目前登入使用者的角色頁面上的功能。

本教學課程會檢查角色架構中設定所需的步驟。 接下來，我們將建置 web 頁面來建立和刪除角色。 在  <a id="_msoanchor_2"> </a> [*將角色指派給使用者*](assigning-roles-to-users-cs.md)教學課程會探討如何新增和移除角色的使用者。 然後在<a id="_msoanchor_3"> </a> [ *Role-based Authorization* ](role-based-authorization-cs.md)教學課程中我們將了解如何限制存取權的角色的角色為基礎，以及如何調整頁面的功能而定的頁面造訪的使用者角色。 讓我們開始吧 ！

## <a name="step-1-adding-new-aspnet-pages"></a>步驟 1： 加入新的 ASP.NET 網頁

在本教學課程，後面兩個我們將檢查各種角色相關的函式和功能。 我們需要一系列的 ASP.NET 頁面，來實作檢查在這些教學課程的主題。 讓我們來建立這些頁面，並更新站台對應。

藉由建立新的資料夾中名為專案啟動`Roles`。 接下來，新增四個以新的 ASP.NET 網頁`Roles`資料夾中，連結每個頁面`Site.master`主版頁面。 名稱的頁面：

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

此時您專案的方案總管] 看起來應該類似螢幕擷取畫面的 [圖 1 所示。


[![[角色] 資料夾已新增四個新的頁面](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**圖 1**： 四個新頁面已加入至`Roles`資料夾 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image3.png))


每個頁面，到目前為止，有兩個內容控制項，一個用於每個主版頁面的 ContentPlaceHolders:`MainContent`和`LoginContent`。

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

請記得， `LoginContent` ContentPlaceHolder 的預設標記會顯示登入或登出的站台，根據使用者是否已驗證的連結。 是否存在`Content2`內容控制項在 ASP.NET 頁面中，不過，會覆寫的主版頁面的預設標記。 如我們所述<a id="_msoanchor_4"> </a> [*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程中，覆寫預設標記可用於，我們不想要顯示登入相關的頁面在左側的資料行中的選項。

這些四個頁面，不過，我們想要顯示的主版頁面的預設標記`LoginContent`ContentPlaceHolder。 因此，移除的宣告式標記`Content2`內容控制項。 完成後，每四個頁面的標記應包含一個內容控制項。

最後，讓我們更新站台對應 (`Web.sitemap`) 以包含這些新的網頁。 新增下列 XML 程式碼之後`<siteMapNode>`我們新增的成員資格教學課程。

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

更新站台對應，請瀏覽的網站，透過瀏覽器。 如 [圖 2] 所示，在左側的導覽現在會包含項目角色教學課程。


[![[角色] 資料夾已新增四個新的頁面](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**圖 2**： 四個新頁面已加入至`Roles`資料夾 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>步驟 2： 指定及設定角色 Framework 提供者

成員資格架構，像是角色 framework 是建置在提供者模型之上。 中所述<a id="_msoanchor_5"> </a> [*安全性基本概念和 ASP.NET 支援*](../introduction/security-basics-and-asp-net-support-cs.md)教學課程中，.NET Framework 隨附三個內建的角色提供者： [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx)[ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx)，以及[ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx)。 本系列教學課程著重於`SqlRoleProvider`，做為角色存放區使用 Microsoft SQL Server 資料庫。

基本上角色架構並`SqlRoleProvider`如同成員資格架構工作及`SqlMembershipProvider`。 .NET Framework 包含`Roles`類別，可做為角色架構的 API。 `Roles`類別具有類似的靜態方法`CreateRole`， `DeleteRole`， `GetAllRoles`， `AddUserToRole`， `IsUserInRole`，依此類推。 叫用其中一種方法時，`Roles`類別會委派呼叫設定的提供者。 `SqlRoleProvider`適用於特定角色的資料表 (`aspnet_Roles`和`aspnet_UsersInRoles`) 回應。

若要使用`SqlRoleProvider`我們的應用程式中的提供者，我們需要指定哪些資料庫做為存放區。 `SqlRoleProvider`預期要有特定的資料庫資料表、 檢視和預存程序的指定的角色存放區。 可以使用新增這些必要的資料庫物件[`aspnet_regsql.exe`工具](https://msdn.microsoft.com/library/ms229862.aspx)。 我們已經有具有所需的結構描述的資料庫此時`SqlRoleProvider`。 回到<a id="_msoanchor_6"> </a> [ *SQL Server 中建立成員資格結構描述*](../membership/creating-the-membership-schema-in-sql-server-cs.md)教學課程中我們建立一個名為資料庫`SecurityTutorials.mdf`並用`aspnet_regsql.exe`以新增應用程式包含所需的資料庫物件的服務`SqlRoleProvider`。 我們就只需要告訴啟用角色的支援，並將角色架構`SqlRoleProvider`與`SecurityTutorials.mdf`作為角色存放區的資料庫。

透過設定角色架構&lt; `roleManager` &gt;應用程式的項目`Web.config`檔案。 根據預設，會停用角色支援。 若要啟用它，您必須設定[ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx)項目的`enabled`屬性設定為`true`就像這樣：

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

根據預設，所有的 web 應用程式具有名為角色提供者`AspNetSqlRoleProvider`型別的`SqlRoleProvider`。 此預設提供者會在中註冊`machine.config`(位於`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

提供者的`connectionStringName`屬性會指定角色存放區使用。 `AspNetSqlRoleProvider`提供者會將這個屬性設定為`LocalSqlServer`，也定義於`machine.config`和點，根據預設，在 SQL Server 2005 Express Edition 資料庫`App_Data`名為資料夾`aspnet.mdf`。

因此，如果我們只需但未指定任何提供者資訊，在我們的應用程式中啟用角色架構`Web.config`檔案中，應用程式會使用已註冊的預設角色提供者， `AspNetSqlRoleProvider`。 如果`~/App_Data/aspnet.mdf`資料庫不存在時，ASP.NET 執行階段會自動加以建立並新增應用程式服務結構描述。 不過，我們不想要使用`aspnet.mdf`資料庫; 相反地，我們想要使用`SecurityTutorials.mdf`我們已建立並新增至應用程式服務結構描述的資料庫。 在下列其中一種，可以完成這項修改：

- <strong>指定的值</strong><strong>`LocalSqlServer`</strong><strong>中的連接字串名稱</strong><strong>`Web.config`</strong><strong>。</strong> 藉由覆寫`LocalSqlServer`中的連接字串名稱值`Web.config`，我們可以使用已註冊的預設角色提供者 (`AspNetSqlRoleProvider`)，並讓它正確使用`SecurityTutorials.mdf`資料庫。 如需有關這項技術的詳細資訊，請參閱 < [Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格文章[設定 ASP.NET 2.0 應用程式服務使用 SQL Server 2000 或 SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)。
- <strong>加入新的已註冊提供者的型別</strong><strong>`SqlRoleProvider`</strong><strong>並設定其</strong><strong>`connectionStringName`</strong><strong>指向設定</strong><strong>`SecurityTutorials.mdf`</strong><strong>資料庫。</strong> 這是我建議，並使用中的方法<a id="_msoanchor_7"> </a> [ *SQL Server 中建立成員資格結構描述*](../membership/creating-the-membership-schema-in-sql-server-cs.md)教學課程中，而且這是我將在本教學課程使用的方法。

下列角色的組態將標記新增至`Web.config`檔案。 此標記會註冊新的提供者，名為`SecurityTutorialsSqlRoleProvider`。

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

上述標記會定義`SecurityTutorialsSqlRoleProvider`做為預設提供者 (透過`defaultProvider`屬性中`<roleManager>`項目)。 它也會設定`SecurityTutorialsSqlRoleProvider`的`applicationName`設為`SecurityTutorials`，相當於`applicationName`成員資格提供者所使用的設定 (`SecurityTutorialsSqlMembershipProvider`)。 雖然未顯示在這裡， [ `<add>`項目](https://msdn.microsoft.com/library/ms164662.aspx)如`SqlRoleProvider`也可能包含`commandTimeout`屬性來指定資料庫逾時持續期間，以秒為單位。 預設值為 30。

使用此組態中標記的地方，我們已準備好開始使用我們的應用程式中的角色功能項目。

> [!NOTE]
> 上述組態標記說明如何使用&lt; `roleManager` &gt;項目的`enabled`和`defaultProvider`屬性。 有多個角色架構將以使用者的使用者為基礎的角色資訊所產生的關聯會影響其他屬性。 我們將檢視中的這些設定<a id="_msoanchor_8"> </a> [ *Role-based Authorization* ](role-based-authorization-cs.md)教學課程。


## <a name="step-3-examining-the-roles-api"></a>步驟 3： 檢查角色 API

角色架構的功能可透過公開[`Roles`類別](https://msdn.microsoft.com/library/system.web.security.roles.aspx)，其中包含 13 個靜態方法，來執行以角色為基礎的作業。 當我們看看建立和刪除角色，在步驟 4 和 6 我們將使用[ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx)並[ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx)方法，可以新增或移除系統中的角色。

若要在系統中取得的所有角色清單，請使用[`GetAllRoles`方法](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)（請參閱步驟 5）。 [ `RoleExists`方法](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx)傳回布林值，指出指定的角色是否存在。

在下一個教學課程中，我們將檢查如何將使用者與角色產生關聯。 `Roles`類別的[ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx)， [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx)， [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx)，與[ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx)方法會將一個或多個使用者新增至一或多個角色。 若要從角色移除使用者，請使用[ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)， [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx)， [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx)，或[ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx)方法。

在  <a id="_msoanchor_9"> </a> [ *Role-based Authorization* ](role-based-authorization-cs.md)教學課程中我們將探討如何以程式設計的方式顯示或隱藏目前登入使用者的角色為基礎的功能。 若要達成此目的，我們可以使用`Role`類別的[ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx)， [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx)， [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)，或[ `IsUserInRole`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)方法。

> [!NOTE]
> 請記住，每當其中一種方法會叫用，`Roles`類別會委派呼叫設定的提供者。 在本例中，這表示呼叫正在傳送給`SqlRoleProvider`。 `SqlRoleProvider`然後執行適當的資料庫作業叫用的方法為基礎。 比方說，程式碼`Roles.CreateRole("Administrators")`會導致`SqlRoleProvider`執行`aspnet_Roles_CreateRole`預存程序，會將新記錄到`aspnet_Roles`名為 Administrators 的資料表。


本教學課程的其餘部分會探討使用`Roles`類別的`CreateRole`， `GetAllRoles`，和`DeleteRole`方法來管理系統中的角色。

## <a name="step-4-creating-new-roles"></a>步驟 4： 建立新的角色

角色所提供的一種任意群組使用者，和此分組最常用於套用授權規則的更便利方式。 但是為了做授權機制，我們必須先定義應用程式中有哪些角色的角色。 不幸的是，ASP.NET 不包含 CreateRoleWizard 控制項。 若要新增新的角色，我們需要建立適當的使用者介面，並叫用角色 API 自己。 好消息是，這是很容易就能完成。

> [!NOTE]
> 雖然沒有 CreateRoleWizard Web 控制項，還有[ASP.NET Web Site Administration Tool](https://msdn.microsoft.com/library/ms228053.aspx)，這是本機的 ASP.NET 應用程式，目的是要協助檢視和管理 web 應用程式的組態。 不過，我並不熱衷於 ASP.NET Web Site Administration Tool 的原因有二。 首先，它正在稍微有錯誤，且使用者體驗離開令人滿意。 第二，ASP.NET Web Site Administration Tool 可只在本機工作，這表示，您必須建置您自己的角色管理網頁，如果您需要從遠端管理即時網站上的角色。 這兩個原因，本教學課程，下一步 將著重於建置必要的角色管理工具，在網頁上，而不是依賴 ASP.NET Web Site Administration Tool。


開啟`ManageRoles.aspx`頁面中`Roles`資料夾並將文字方塊和按鈕 Web 控制項新增至頁面。 將文字方塊控制項的`ID`屬性，以`RoleName`和按鈕的`ID`並`Text`屬性，以`CreateRoleButton`和建立的角色，分別。 此時，您的頁面宣告式標記看起來應該如下所示：

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

接下來，按兩下`CreateRoleButton`按鈕在設計工具中建立的控制項`Click`事件處理常式，然後加入下列程式碼：

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

上述程式碼一開始會指派已修剪的角色名稱中輸入`RoleName`文字方塊`newRoleName`變數。 下一步`Roles`類別的`RoleExists`呼叫方法來判斷角色`newRoleName`已經存在系統中。 如果角色不存在，則會建立透過呼叫`CreateRole`方法。 如果`CreateRole`角色名稱已存在於系統中，傳遞給方法`ProviderException`擲回例外狀況。 這就是為什麼程式碼會先檢查以確保該角色已不存在在系統中，然後再呼叫`CreateRole`。 `Click`事件處理常式結束時，會清除`RoleName`TextBox 的`Text`屬性。

> [!NOTE]
> 您可能會發生什麼事會好奇如果使用者未輸入任何值，轉換`RoleName`文字方塊中。 如果值傳入`CreateRole`方法是`null`或空字串，發生例外狀況，就會引發。 同樣地，如果角色名稱包含逗號就會引發例外狀況。 因此，頁面應該包含驗證控制項，以確保使用者輸入的角色，而且它不包含任何逗號。 我保留作為練習的讀取器。


讓我們建立名為 Administrators 的角色。 請瀏覽`ManageRoles.aspx`透過瀏覽器頁面上，在文字方塊中輸入 以系統管理員 （請參閱 圖 3），然後按一下 建立角色 按鈕。


[![建立系統管理員角色](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**圖 3**： 建立系統管理員角色 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image9.png))


會發生什麼事？ 回傳，但沒有任何角色存在已有的視覺提示新增至系統。 我們將會更新以包含視覺化回饋的步驟 5 中的此頁面。 現在，不過，您可以確認角色已建立前往`SecurityTutorials.mdf`資料庫，並顯示從資料`aspnet_Roles`資料表。 如 [圖 4] 所示，`aspnet_Roles`資料表包含剛加入系統管理員角色的記錄。


[![Aspnet_Roles 資料表有一個資料列，系統管理員](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**圖 4**:`aspnet_Roles`資料表有一個資料列，系統管理員 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>步驟 5： 顯示在系統中的角色

讓我們擴大`ManageRoles.aspx`頁面，以納入系統中的目前角色的清單。 若要這麼做，將 GridView 控制項新增至頁面並設定其`ID`屬性設`RoleList`。 接下來，將方法新增至名為頁面的程式碼後置類別`DisplayRolesInGrid`使用下列程式碼：

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

`Roles`類別的`GetAllRoles`方法會傳回所有角色的系統中的字串陣列。 此字串陣列，則會繫結至 GridView。 若要將角色清單繫結至 GridView，第一次載入頁面時，我們要呼叫`DisplayRolesInGrid`方法，從頁面的`Page_Load`事件處理常式。 當第一次瀏覽的頁面，但不是會在後續回傳時，下列程式碼會呼叫這個方法。

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

使用此程式碼就緒之後，請瀏覽透過瀏覽器頁面。 如 [圖 5] 所示，您應該會看到一個方格，具有單一資料行標示為項目。 方格包含我們在步驟 4 中新增為系統管理員角色的資料列。


[![GridView 會顯示單一資料行中的角色](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**圖 5**: GridView 會顯示單一資料行中的角色 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image15.png))


GridView 會顯示單一資料行標示為項目，因為 GridView`AutoGenerateColumns`屬性設定為 True （預設值），這會導致 GridView，以自動建立資料行中每一個屬性及其`DataSource`。 陣列具有單一屬性，表示項目，因此陣列中的的 GridView 內的單一資料行。

顯示與 GridView 的資料時，我願意明確定義我的資料行，而非讓它們隱含產生 GridView。 藉由明確定義就能輕鬆將資料格式化的資料行，重新排列資料行，並執行其他一般工作。 因此，讓我們更新 GridView 的宣告式標記，讓其資料行明確定義。

先是設定 GridView 的`AutoGenerateColumns`屬性設定為 False。 接下來，為 TemplateField 加入方格中，設定其`HeaderText`角色的屬性，並設定其`ItemTemplate`，讓它顯示陣列的內容。 若要達成此目的，新增名為 Label Web 控制項`RoleNameLabel`要`ItemTemplate`，並繫結其`Text`屬性設`Container.DataItem`。

這些屬性和`ItemTemplate`的內容可以宣告方式或透過設定 GridView 的 [欄位] 對話方塊和編輯樣板介面。 若要連線到 [欄位] 對話方塊中，按一下 GridView 的智慧標籤中的 [編輯資料行] 連結。 接下來，取消核取 自動產生欄位核取方塊來設定`AutoGenerateColumns`屬性設定為 False，並新增為 TemplateField 至 GridView，設定其`HeaderText`角色的屬性。 若要定義`ItemTemplate`的內容，從 GridView 的智慧標籤中選擇 [編輯範本] 選項。 將標籤 Web 控制項拖曳至`ItemTemplate`，將其`ID`屬性設`RoleNameLabel`，並設定其資料繫結設定，讓其`Text`屬性的繫結至`Container.DataItem`。

無論您使用哪種方法，GridView 的產生的宣告式標記看起來應該類似下面的完成時。

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> 陣列的內容會顯示使用資料繫結語法`<%# Container.DataItem %>`。 時顯示陣列的內容繫結至 GridView，為什麼要使用此語法的完整描述已超出本教學課程的範圍。 如需有關此問題的詳細資訊，請參閱[繫結至資料 Web 控制項的純量陣列](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)。


目前， `RoleList` GridView 只會繫結至角色的清單時第一次瀏覽的頁面。 我們需要新增新的角色時，請重新整理方格。 若要這麼做，更新`CreateRoleButton`按鈕的`Click`事件處理常式，因此它會呼叫`DisplayRolesInGrid`方法如果在建立新的角色。

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

現在當使用者將新角色時，才`RoleList`GridView 會顯示剛加入的角色在回傳時，提供角色已成功建立的視覺化回饋。 為了說明這點，請瀏覽`ManageRoles.aspx`透過瀏覽器頁面，然後新增名為監督員的角色。 時按一下 [建立角色] 按鈕，將發生回傳與方格會更新以包含系統管理員，以及新的角色，監督員。


[![監督員角色可讓您擁有已加入](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**圖 6**： 監督員角色可讓您有已加入 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>步驟 6： 刪除角色

使用者可以在此時建立新的角色，並檢視所有現有的角色，從`ManageRoles.aspx`頁面。 讓我們允許使用者也會刪除角色。 `Roles.DeleteRole`方法有兩個多載：

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -刪除的角色*roleName*。 如果角色包含一個或多個成員，則會擲回例外狀況。
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -刪除的角色*roleName*。 如果*throwOnPopulateRole*是`true`，則如果角色包含一個或多個成員，會擲回例外狀況。 如果*throwOnPopulateRole*是`false`，則是否包含任何成員或不刪除角色。 就內部而言，`DeleteRole(roleName)`方法呼叫`DeleteRole(roleName, true)`。

`DeleteRole`方法也會擲回例外狀況如果*roleName*是`null`或空字串或如果*roleName*包含逗號。 如果*roleName*不存在於系統`DeleteRole`失敗時以無訊息模式，而不會引發例外狀況。

讓我們擴大在 GridView`ManageRoles.aspx`包含刪除按鈕，按一下時，會刪除選取的角色。 藉由將 GridView 中的 [刪除] 按鈕，移至 [欄位] 對話方塊中，並新增 [刪除] 按鈕，位於 [CommandField] 選項啟動。 請刪除按鈕最左側資料行，然後將其`DeleteText`屬性，以刪除角色。


[![將 [刪除] 按鈕新增至 RoleList GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**圖 7**： 新增至 [刪除] 按鈕`RoleList`GridView ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image21.png))


新增 [刪除] 按鈕之後, 您 GridView 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

接下來，建立事件處理常式，如 GridView 的`RowDeleting`事件。 這是在按一下 [刪除的角色] 按鈕時，會將在回傳時引發的事件。 將下列程式碼加入事件處理常式。

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

程式碼開始是以程式設計方式參考`RoleNameLabel`Web 控制項中按下的 [刪除的角色] 按鈕的資料列。 `Roles.DeleteRole`再叫用方法，傳入`Text`的`RoleNameLabel`和`false`，藉此刪除的角色，不論是否有任何與角色相關聯的使用者。 最後， `RoleList` GridView 會重新整理，以便只是已刪除的角色不會再出現在方格中。

> [!NOTE]
> [刪除的角色] 按鈕不需要任何一種使用者確認之前刪除的角色。 其中一個最簡單的方式，可確認動作是透過用戶端確認對話方塊。 如需有關這項技術的詳細資訊，請參閱 <<c0> [ 正在刪除時新增用戶端確認](https://asp.net/learn/data-access/tutorial-42-cs.aspx)。


## <a name="summary"></a>總結

許多 web 應用程式有特定的授權規則或只會提供給使用者的特定類別的頁面層級功能。 例如，可能有一份只有系統管理員可以存取的網頁。 而不是以使用者的使用者為基礎，定義這些授權規則，通常很多有用來定義規則，根據角色。 亦即，而非明確地讓使用者 Scott 以及 Jisun 存取系統管理網頁，以更容易維護的方法是以允許系統管理員角色的成員，來存取這些頁面，然後代表使用者屬於 Scott 和 Jisun系統管理員角色。

角色架構可讓您輕鬆地建立及管理角色。 在本教學課程中，我們檢查如何設定要使用的角色 framework `SqlRoleProvider`，做為角色存放區使用 Microsoft SQL Server 資料庫。 此外，我們也會建立一個網頁，列出系統中現有的角色，並允許新的角色，以建立和刪除現有的。 在後續教學課程中，我們會看到如何將使用者指派給角色，以及如何套用以角色為基礎的授權。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [檢查 ASP.NET 2.0 的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [如何： 使用 ASP.NET 2.0 中的 角色管理員](https://msdn.microsoft.com/library/ms998314.aspx)
- [角色提供者](https://msdn.microsoft.com/library/aa478950.aspx)
- [復原您自己的網站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [技術文件`<roleManager>`項目](https://msdn.microsoft.com/library/ms164660.aspx)
- [使用成員資格和角色管理員 Api](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者包括 Alicja Maziarz、 Suchi Banerjee 和 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](assigning-roles-to-users-cs.md)
