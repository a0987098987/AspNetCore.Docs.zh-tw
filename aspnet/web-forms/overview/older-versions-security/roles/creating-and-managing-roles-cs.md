---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: "建立及管理角色 (C#) |Microsoft 文件"
author: rick-anderson
description: "本教學課程會檢查設定角色架構的必要步驟。 接下來，我們將建立的網頁以建立和刪除角色。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: 0784afb83a8974d514e20261f0f520992a630e9b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-managing-roles-c"></a>建立及管理角色 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> 本教學課程會檢查設定角色架構的必要步驟。 接下來，我們將建立的網頁以建立和刪除角色。


## <a name="introduction"></a>簡介

在<a id="_msoanchor_1"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-cs.md)教學課程中我們探討了一組頁面限制特定使用者使用 URL 授權，因此瀏覽宣告式和以程式設計方式調整正在瀏覽的使用者為基礎的 ASP.NET 網頁的功能的技術。 授與權限存取頁面或使用者的使用者為基礎的功能，不過，可能會變得維護惡夢案例中的有許多使用者帳戶或使用者的權限經常變更時。 使用者獲得或失去的授權，才能執行特定工作時，任何時候，系統管理員必須更新適當的 URL 授權規則、 宣告式的標記和程式碼。

它有助於分類成群組的使用者或*角色*然後再套用以角色的角色為基礎的權限。 例如，大部分 web 應用程式有一組特定的頁面或保留僅供系統管理使用者的工作。 在使用的技術來學習*使用者為基礎的授權*教學課程中，我們會加入適當的 URL 授權規則、 宣告式的標記和程式碼，以允許指定的使用者帳戶來執行管理工作。 但如果加入新的系統管理員，或現有的系統管理員需要具備撤銷其系統管理權限，我們就必須傳回並更新組態檔和網頁。 與角色，不過，我們無法建立一個稱為系統管理員角色並指派這些受信任的使用者至系統管理員角色。 接下來，我們會加入適當的 URL 授權規則、 宣告式的標記和程式碼，以允許系統管理員角色來執行各種系統管理工作。 此基礎結構，以將新的系統管理員新增至網站，或移除現有的很簡單，只包含或移除系統管理員角色的使用者。 沒有設定、 宣告式標記或變更程式碼是必要的。

ASP.NET 提供了角色架構定義的角色和與使用者帳戶建立關聯。 使用角色架構我們可以建立及刪除角色，將使用者加入或移除角色中的使用者，請判斷一組屬於特定角色，並告知使用者是否屬於特定角色的使用者。 一旦已設定角色架構，我們可以限制存取透過 URL 授權規則以角色的角色為基礎的頁面和顯示或隱藏其他資訊或根據目前登入使用者的角色頁面上的功能。

本教學課程會檢查設定角色架構的必要步驟。 接下來，我們將建立的網頁以建立和刪除角色。 在<a id="_msoanchor_2"> </a> [*角色指派使用者給*](assigning-roles-to-users-cs.md)教學課程，我們將探討如何加入和從角色移除使用者。 然後在<a id="_msoanchor_3"> </a> [*角色為基礎的授權*](role-based-authorization-cs.md)教學課程中我們會看到如何限制網頁，以及如何調整頁面的功能視角色由角色為基礎的存取權正在瀏覽的使用者角色。 讓我們開始吧 ！

## <a name="step-1-adding-new-aspnet-pages"></a>步驟 1： 加入新的 ASP.NET 網頁

在本教學課程和後面兩個我們將會檢查各種角色相關的功能與功能。 我們需要一系列的 ASP.NET 頁面實作檢查這些教學課程中的主題。 讓我們來建立這些頁面，並更新站台對應。

藉由建立新的資料夾中名為專案啟動`Roles`。 接下來，加入四個以新的 ASP.NET 網頁`Roles`資料夾中，連結與每個頁面`Site.master`主版頁面。 命名頁面：

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

此時您專案的方案總管 看起來應該類似螢幕擷取畫面中圖 1 所示。


[![四個新的頁面已加入至 [角色] 資料夾](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**圖 1**： 四個新頁面已加入至`Roles`資料夾 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image3.png))


每一頁，此時，應有兩個內容控制項，各供一個主版頁面的 ContentPlaceHolders:`MainContent`和`LoginContent`。

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

請記得， `LoginContent` ContentPlaceHolder 的預設標記會顯示登入或登出的站台，根據是否已驗證使用者的連結。 與否`Content2`內容控制項的 ASP.NET 頁面上，不過，會覆寫主版頁面的預設標記。 如我們所述<a id="_msoanchor_4"> </a> [*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)教學課程中，覆寫預設標記中很有用，我們不想要顯示登入相關的頁面左側資料行中的選項。

這些四個頁面，不過，我們想要顯示在主版頁面的預設標記`LoginContent`ContentPlaceHolder。 因此，移除的宣告式標記`Content2`內容控制項。 之後，請每四個頁面的標記應該包含單一內容控制項。

最後，讓我們來更新站台對應 (`Web.sitemap`) 以包含這些新的網頁。 加入下列 XML 之後`<siteMapNode>`我們加入的成員資格教學課程。

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

與更新站台對應，請瀏覽的網站透過瀏覽器。 如圖 2 所示，現在在左側瀏覽角色教學課程包含項目。


[![四個新的頁面已加入至 [角色] 資料夾](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**圖 2**： 四個新頁面已加入至`Roles`資料夾 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>步驟 2： 指定和設定角色架構提供者

成員資格架構，例如角色 framework 是建置在提供者模型之上。 中所述<a id="_msoanchor_5"> </a> [*安全性基本概念和 ASP.NET 支援*](../introduction/security-basics-and-asp-net-support-cs.md)教學課程中，.NET Framework 隨附三個內建的角色提供者： [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.authorizationstoreroleprovider.aspx)[ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.windowstokenroleprovider.aspx)，和[ `SqlRoleProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.sqlroleprovider.aspx)。 此教學課程系列著重於`SqlRoleProvider`，使用 Microsoft SQL Server 資料庫為角色存放區。

基本上角色架構和`SqlRoleProvider`工作一樣，成員資格架構和`SqlMembershipProvider`。 .NET Framework 包含`Roles`做為角色架構應用程式開發介面的類別。 `Roles`類別具有類似的靜態方法`CreateRole`， `DeleteRole`， `GetAllRoles`， `AddUserToRole`， `IsUserInRole`，依此類推。 叫用其中一種方法時，`Roles`類別委派設定的提供者呼叫。 `SqlRoleProvider`適用於的特定角色的資料表 (`aspnet_Roles`和`aspnet_UsersInRoles`) 回應。

若要使用`SqlRoleProvider`我們的應用程式中的提供者，我們需要指定哪些資料庫做為存放區。 `SqlRoleProvider`需要有特定的資料庫資料表、 檢視和預存程序的指定的角色存放區。 這些必要的資料庫物件可以使用加入[`aspnet_regsql.exe`工具](https://msdn.microsoft.com/en-us/library/ms229862.aspx)。 現在我們已經有具有所需的結構描述的資料庫`SqlRoleProvider`。 回到<a id="_msoanchor_6"> </a> [*在 SQL Server 中建立成員資格結構描述*](../membership/creating-the-membership-schema-in-sql-server-cs.md)教學課程中我們建立一個名為資料庫`SecurityTutorials.mdf`並用`aspnet_regsql.exe`加入應用程式包含所需的資料庫物件的服務`SqlRoleProvider`。 因此我們只需要判斷角色架構以啟用角色的支援，並使用`SqlRoleProvider`與`SecurityTutorials.mdf`做為角色存放區資料庫。

已透過設定角色架構&lt; `roleManager` &gt;在應用程式中的項目`Web.config`檔案。 根據預設，已停用角色的支援。 若要啟用它，您必須設定[ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/en-us/library/ms164660.aspx)項目的`enabled`屬性`true`就像這樣：

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

根據預設，所有的 web 應用程式具有名為角色提供者`AspNetSqlRoleProvider`型別的`SqlRoleProvider`。 此預設提供者會在中註冊`machine.config`(位於`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

提供者的`connectionStringName`屬性會指定角色存放區所使用。 `AspNetSqlRoleProvider`提供者會設定這個屬性`LocalSqlServer`，也會定義在`machine.config`和點，根據預設，在 SQL Server 2005 Express Edition 資料庫`App_Data`資料夾名為`aspnet.mdf`。

因此，如果我們只是未指定任何提供者資訊，在我們的應用程式中啟用角色架構`Web.config`檔案，應用程式使用已註冊的預設角色提供者， `AspNetSqlRoleProvider`。 如果`~/App_Data/aspnet.mdf`資料庫不存在，ASP.NET 執行階段會自動建立它，並新增應用程式服務結構描述。 不過，我們不想使用`aspnet.mdf`資料庫; 相反地，我們想要使用`SecurityTutorials.mdf`我們已經建立並加入至應用程式服務結構描述的資料庫。 其中一種方式可以完成這項修改：

- **指定的值 * * *`LocalSqlServer`* * * 中的連接字串名稱 * * *`Web.config`* * *。** 藉由覆寫`LocalSqlServer`中的連接字串名稱值`Web.config`，我們可以使用已註冊的預設角色提供者 (`AspNetSqlRoleProvider`)，並讓它正確使用`SecurityTutorials.mdf`資料庫。 如需有關這項技術的詳細資訊，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格文章：[設定 ASP.NET 2.0 應用程式服務設定為使用 SQL Server 2000 或 SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)。
- **加入新的已註冊提供者的類型 * * *`SqlRoleProvider`* * * 並設定其 * * *`connectionStringName`* * * 設定為指向 * * *`SecurityTutorials.mdf`* * * 資料庫。** 這是我建議，並在中使用的方式<a id="_msoanchor_7"> </a> [*在 SQL Server 中建立成員資格結構描述*](../membership/creating-the-membership-schema-in-sql-server-cs.md)教學課程中，且會將在本教學課程中使用的方法。

加入至下列角色組態標記`Web.config`檔案。 這個標記會註冊新的提供者，名為`SecurityTutorialsSqlRoleProvider`。

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

上述的標記定義`SecurityTutorialsSqlRoleProvider`做為預設提供者 (透過`defaultProvider`屬性`<roleManager>`項目)。 它也會設定`SecurityTutorialsSqlRoleProvider`的`applicationName`設`SecurityTutorials`，相當於`applicationName`成員資格提供者所使用的設定 (`SecurityTutorialsSqlMembershipProvider`)。 而不顯示在這裡， [ `<add>`元素](https://msdn.microsoft.com/en-us/library/ms164662.aspx)如`SqlRoleProvider`也可能包含`commandTimeout`屬性來指定資料庫逾時持續期間，以秒為單位。 預設值為 30。

與此組態中標記位置，我們會準備好開始使用我們的應用程式中的角色功能。

> [!NOTE]
> 上述組態標記說明如何使用&lt; `roleManager` &gt;項目的`enabled`和`defaultProvider`屬性。 有一些角色架構將使用者的使用者為基礎的角色資訊的連結會影響其他屬性。 我們將檢視中的這些設定<a id="_msoanchor_8"> </a> [*角色為基礎的授權*](role-based-authorization-cs.md)教學課程。


## <a name="step-3-examining-the-roles-api"></a>步驟 3： 檢查應用程式開發介面的角色

角色架構的功能可透過公開[`Roles`類別](https://msdn.microsoft.com/en-us/library/system.web.security.roles.aspx)，其中包含 13 個靜態方法，來執行以角色為基礎的作業。 當我們建立及刪除角色，在步驟 4 和 6 我們將使用[ `CreateRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.createrole.aspx)和[ `DeleteRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.deleterole.aspx)的方法，加入或移除系統中的角色。

若要取得系統中的所有角色的清單，請使用[`GetAllRoles`方法](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getallroles.aspx)（請參閱步驟 5）。 [ `RoleExists`方法](https://msdn.microsoft.com/en-us/library/system.web.security.roles.roleexists.aspx)傳回布林值，指出指定的角色是否存在。

在下一個教學課程中，我們將檢查如何將使用者與角色產生關聯。 `Roles`類別的[ `AddUserToRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.addusertorole.aspx)， [ `AddUserToRoles` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.addusertoroles.aspx)， [ `AddUsersToRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.adduserstorole.aspx)，和[ `AddUsersToRoles` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.adduserstoroles.aspx)方法會將一或多個使用者加入至一或多個角色。 若要從角色移除使用者，請使用[ `RemoveUserFromRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeuserfromrole.aspx)， [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeuserfromroles.aspx)， [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeusersfromrole.aspx)，或[ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeusersfromroles.aspx)方法。

在<a id="_msoanchor_9"> </a> [*角色為基礎的授權*](role-based-authorization-cs.md)教學課程中我們將探討如何以程式設計方式顯示或隱藏目前登入的使用者角色為基礎的功能。 若要達成此目的，我們可以使用`Role`類別的[ `FindUsersInRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.findusersinrole.aspx)， [ `GetRolesForUser` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getrolesforuser.aspx)， [ `GetUsersInRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getusersinrole.aspx)，或[ `IsUserInRole`](https://msdn.microsoft.com/en-us/library/system.web.security.roles.isuserinrole.aspx)方法。

> [!NOTE]
> 請記住，隨時其中一種方法會叫用，`Roles`類別委派設定的提供者呼叫。 在我們的案例，這表示若要傳送的呼叫`SqlRoleProvider`。 `SqlRoleProvider`然後會執行叫用的方法為基礎的適當的資料庫作業。 例如，程式碼`Roles.CreateRole("Administrators")`導致`SqlRoleProvider`執行`aspnet_Roles_CreateRole`預存程序，它會插入新的記錄，到`aspnet_Roles`資料表名為系統管理員。


本教學課程的其餘部分會查看使用`Roles`類別的`CreateRole`， `GetAllRoles`，和`DeleteRole`方法來管理系統中的角色。

## <a name="step-4-creating-new-roles"></a>步驟 4： 建立新的角色

角色提供的方式，將任意使用者分組，以及此群組最常用於較方便的方式，將授權規則套用。 但若要使用角色做為授權機制，我們需要先定義應用程式中有哪些角色。 不幸的是，ASP.NET 不包括 CreateRoleWizard 控制項。 若要加入新的角色，我們需要建立適當的使用者介面，並叫用角色 API 自己。 好消息是，這是很容易就能完成。

> [!NOTE]
> 雖然沒有 CreateRoleWizard Web 控制項，還有[ASP.NET 網站管理工具](https://msdn.microsoft.com/en-us/library/ms228053.aspx)，這是本機的 ASP.NET 應用程式，設計來協助您檢視及管理 web 應用程式的組態。 不過，我不大風扇原因有兩個 ASP.NET 網站管理工具。 首先，它是一個位元錯誤和使用者經驗會讓人滿意。 第二，ASP.NET 網站管理工具被專門用於只在本機，表示您將需要建置您自己的角色管理網頁，如果您要從遠端管理即時網站上的角色。 這兩個原因，本教學課程和下一步將重點放在建置所需的角色管理工具，在網頁中的，而不是依賴 ASP.NET 網站管理工具。


開啟`ManageRoles.aspx`頁面`Roles`資料夾並將文字方塊和按鈕 Web 控制項加入至頁面。 將文字方塊控制項的`ID`屬性`RoleName`和按鈕的`ID`和`Text`屬性`CreateRoleButton`和建立角色，分別。 此時，網頁的宣告式標記看起來應該如下所示：

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

接下來，按兩下`CreateRoleButton`按鈕控制項在設計工具來建立`Click`事件處理常式，然後加入下列程式碼：

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

上述的程式碼會啟動藉由指派已修剪的角色名稱中輸入`RoleName`文字方塊`newRoleName`變數。 下一步`Roles`類別的`RoleExists`方法呼叫以判斷角色`newRoleName`已經存在系統中。 如果角色不存在，就會建立透過呼叫`CreateRole`方法。 如果`CreateRole`角色名稱已存在於系統中，傳遞給方法`ProviderException`擲回例外狀況。 這就是為什麼程式碼會先檢查以確保角色已不存在於之前呼叫系統`CreateRole`。 `Click`事件處理常式結束時，會清除`RoleName`文字方塊的`Text`屬性。

> [!NOTE]
> 您可能想知道會發生什麼事如果使用者未輸入任何值`RoleName`文字方塊。 如果此值傳遞至`CreateRole`方法`null`或空字串，發生例外狀況，就會引發。 同樣地，如果角色名稱包含逗號就會引發例外狀況。 因此，頁面應包含驗證控制項，以確定使用者輸入的角色，而且它不包含任何逗號。 我將保留做為練習讀取器。


讓我們來建立名為系統管理員的角色。 請瀏覽`ManageRoles.aspx`透過瀏覽器頁面上，在文字方塊中輸入系統管理員 （請參閱圖 3），然後按一下 [建立角色] 按鈕。


[![建立系統管理員角色](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**圖 3**： 建立系統管理員角色 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image9.png))


發生什麼事？ 發生回傳，但沒有視覺提示角色實際上已新增至系統。 我們將會更新以包含視覺化回饋的步驟 5 中的此頁面。 現在，不過，您可以確認角色已建立，請前往`SecurityTutorials.mdf`資料庫和顯示資料的`aspnet_Roles`資料表。 如圖 4 所示，`aspnet_Roles`資料表包含只加入系統管理員角色的記錄。


[![Aspnet_Roles 資料表有一個資料列的管理員](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**圖 4**:`aspnet_Roles`資料表有資料列，系統管理員 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>步驟 5： 顯示在系統中的角色

讓我們來加強`ManageRoles.aspx`頁面，即可包含系統中的目前角色的清單。 若要達成此目的，將 GridView 控制項加入頁面並設定其`ID`屬性`RoleList`。 接下來，將方法加入至名為頁面的程式碼後置類別`DisplayRolesInGrid`使用下列程式碼：

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

`Roles`類別的`GetAllRoles`方法會傳回所有角色系統中的字串陣列。 這個字串陣列，然後繫結至 GridView。 若要將角色的清單繫結至 GridView，第一次載入頁面時，我們要呼叫`DisplayRolesInGrid`方法，從頁面`Page_Load`事件處理常式。 當第一次瀏覽頁面時，但不是能在後續回傳時，下列程式碼會呼叫這個方法。

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

此位置的程式碼，請瀏覽透過瀏覽器頁面。 如圖 5 所示，您應該會看到一個方格，具有單一資料行標示為項目。 方格包含我們加入在步驟 4 中的系統管理員角色的資料列。


[![GridView 會顯示單一資料行中的角色](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**圖 5**: GridView 會顯示單一資料行中的角色 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image15.png))


GridView 會顯示標示為項目，因為唯一資料行的 GridView`AutoGenerateColumns`屬性設定為 True （預設值），這會導致自動建立資料行中每一個屬性 GridView 其`DataSource`。 陣列具有單一屬性，表示項目陣列，因此在 GridView 中的單一資料行。

顯示與 GridView 的資料時，我願意明確定義我的資料行而非讓它們隱含產生的 GridView。 藉由明確定義就能輕鬆將資料格式化的資料行，重新排列資料行，並執行其他一般工作。 因此，我們更新 GridView 的宣告式標記，以明確定義其資料行。

開始，以設定 GridView`AutoGenerateColumns`屬性設定為 False。 接下來，將為 TemplateField 加入至方格中，設定其`HeaderText`屬性至角色，並設定其`ItemTemplate`，使其顯示為陣列的內容。 若要達成此目的，將加入標籤 Web 控制項，名為`RoleNameLabel`至`ItemTemplate`並繫結其`Text`屬性`Container.DataItem`。

這些屬性和`ItemTemplate`的內容可以設定以宣告方式或透過 GridView 的 [欄位] 對話方塊和編輯樣板介面。 連線到 欄位 對話方塊，按一下 編輯資料行中的連結 GridView 的智慧標籤。 接下來，取消核取自動產生欄位核取方塊以設定`AutoGenerateColumns`屬性設定為 False，並為 TemplateField 加入 GridView 中設定其`HeaderText`角色的屬性。 若要定義`ItemTemplate`的內容，從 GridView 的智慧標籤中選擇 [編輯樣板] 選項。 將標籤 Web 控制項拖曳至`ItemTemplate`，將其`ID`屬性`RoleNameLabel`，並設定其資料繫結設定，讓其`Text`屬性繫結至`Container.DataItem`。

不論您使用哪種方法，GridView 的產生的宣告式標記看起來應該類似下面的完畢時。

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> 陣列的內容會顯示使用資料繫結語法`<%# Container.DataItem %>`。 時顯示之陣列的內容繫結至 GridView，為什麼使用此語法的完整描述，已超出本教學課程的範圍。 如需這個問題的詳細資訊，請參閱[繫結至資料的 Web 控制項的純量陣列](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)。


目前， `RoleList` GridView 只能繫結至角色的清單時第一次造訪的網頁。 我們需要加入新的角色時，請重新整理方格。 若要完成這項作業，更新`CreateRoleButton`按鈕的`Click`事件處理常式，因此它會呼叫`DisplayRolesInGrid`方法，如果建立新的角色。

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

現在當使用者將新角色`RoleList`GridView 會顯示只是加入角色在回傳時，提供角色已成功建立的視覺化回應。 為了說明這點，請瀏覽`ManageRoles.aspx`頁面上透過瀏覽器，並新增名為監督員的角色。 在 [建立角色] 按鈕時將發生回傳和方格會更新為包括系統管理員，以及新的角色，監督員。


[![監督員角色都具有已加入](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**圖 6**： 監督員角色具有已加入 ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>步驟 6： 刪除角色

使用者可以建立新的角色與檢視所有現有的角色從此時`ManageRoles.aspx`頁面。 讓我們可讓使用者若要同時刪除角色。 `Roles.DeleteRole`方法有兩個多載：

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/en-us/library/ek4sywc0.aspx)-刪除的角色*roleName*。 如果角色包含一個或多個成員，則會擲回例外狀況。
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/en-us/library/38h6wf59.aspx)-刪除的角色*roleName*。 如果*throwOnPopulateRole*是`true`，則會擲回例外狀況，如果角色包含一個或多個成員。 如果*throwOnPopulateRole*是`false`，然後刪除角色後是否或不包含任何成員。 就內部而言，`DeleteRole(roleName)`方法呼叫`DeleteRole(roleName, true)`。

`DeleteRole`方法也會擲回例外狀況如果*roleName*是`null`或空字串或*roleName*包含逗號。 如果*roleName*不存在於系統`DeleteRole`失敗時以無訊息模式，而不會引發例外狀況。

讓我們來加強在 GridView`ManageRoles.aspx`包含刪除按鈕，按一下時，會刪除選取的角色。 加入 GridView 中的 [刪除] 按鈕，移至 [欄位] 對話方塊，並加入 [刪除] 按鈕，位於 [CommandField] 選項啟動。 請刪除按鈕最左側資料行，並設定其`DeleteText`屬性，以刪除角色。


[![將刪除 按鈕加入至 RoleList GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**圖 7**： 新增至 [刪除] 按鈕`RoleList`GridView ([按一下以檢視完整大小的影像](creating-and-managing-roles-cs/_static/image21.png))


加入 [刪除] 按鈕之後, GridView 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

接下來，建立事件處理常式的 GridView`RowDeleting`事件。 這是按一下刪除角色按鈕時，在回傳時引發的事件。 將下列程式碼加入事件處理常式。

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

程式碼會啟動以程式設計方式參考`RoleNameLabel`Web 中的資料列的刪除角色 button 已按下的控制項。 `Roles.DeleteRole`然後叫用方法，傳入`Text`的`RoleNameLabel`和`false`，藉此刪除該角色，而不論是否有任何與角色相關聯的使用者。 最後， `RoleList` GridView 會重新整理，以便只刪除的角色不會再出現在方格中。

> [!NOTE]
> 刪除角色按鈕不需要任何類型的使用者的確認，再刪除該角色。 確認動作的最簡單方式是透過用戶端確認對話方塊。 如需有關這項技術的詳細資訊，請參閱[新增用戶端確認時刪除](https://asp.net/learn/data-access/tutorial-42-cs.aspx)。


## <a name="summary"></a>總結

許多 web 應用程式具有特定授權規則或頁面層級功能，只使用特定類別的使用者。 例如，可能只有系統管理員可以存取的 web 網頁的一組。 而不是以使用者的使用者為基礎，定義這些授權規則，有時候很多有用來定義角色為基礎的規則。 也就是說，而不是明確地讓使用者 Scott 和 Jisun 存取系統管理的網頁，更容易維護的方法是以允許系統管理員角色的成員存取這些頁面，然後代表屬於使用者 Scott 和 Jisun系統管理員角色。

角色架構可讓您輕鬆地建立及管理角色。 在本教學課程中，我們檢查如何設定要使用的角色 framework `SqlRoleProvider`，使用 Microsoft SQL Server 資料庫為角色存放區。 此外，我們也會建立網頁，列出系統中的現有角色，並允許建立新的角色，以刪除現有的。 在後續的教學課程中，我們會看到如何將使用者指派給角色，以及如何套用以角色為基礎的授權。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [檢查 ASP.NET 2.0 的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [如何： 使用 ASP.NET 2.0 中的 角色管理員](https://msdn.microsoft.com/en-us/library/ms998314.aspx)
- [角色提供者](https://msdn.microsoft.com/en-us/library/aa478950.aspx)
- [復原您的網站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [技術文件`<roleManager>`項目](https://msdn.microsoft.com/en-us/library/ms164660.aspx)
- [使用成員資格和角色管理員 Api](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 前導檢閱者在此教學課程包含 Alicja Maziarz、 Suchi Banerjee 和本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一步](assigning-roles-to-users-cs.md)
