---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: 以角色為基礎的授權 (VB) |Microsoft Docs
author: rick-anderson
description: 本教學課程開始了解如何角色 framework 建立的關聯使用者的角色與他的安全性內容。 然後，它會檢驗如何套用以角色為基礎的 URL...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: b8cf8cc5fa802eb812482b236c8b2825423027e7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390981"
---
<a name="role-based-authorization-vb"></a>以角色為基礎的授權 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> 本教學課程開始了解如何角色 framework 建立的關聯使用者的角色與他的安全性內容。 然後，它會檢驗如何套用以角色為基礎的 URL 授權規則。 之後，我們將探討使用宣告式和程式設計的方式改變顯示的資料和 ASP.NET 網頁所提供的功能。


## <a name="introduction"></a>簡介

在  <a id="_msoanchor_1"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-vb.md)教學課程中我們了解如何使用 URL 授權，以指定哪些使用者可以瀏覽一組特定的頁面。 只要使用一點中的標記與`Web.config`，我們可以指示 ASP.NET 可以只允許已驗證的使用者來瀏覽網頁。 或我們無法決定，只有在 Tito 和 Bob 的使用者才允許，或指出已允許 所有已驗證的使用者，Sam 除外。

除了 URL 授權時，我們也討論過宣告式和程式設計技術，來控制顯示的資料，並根據使用者造訪的頁面所提供的功能。 特別是，我們建立的頁面，列出目前目錄的內容。 任何人都可以瀏覽此頁面上，但只有已驗證的使用者可以檢視檔案的內容，只有 Tito 無法刪除檔案。

套用使用者以使用者為基礎的授權規則，可能會變成簿記夢魘。 更容易維護的方法是使用以角色為基礎的授權。 好消息是套用授權規則供我們運用工具照樣也與角色的使用者帳戶一樣。 URL 授權規則可以指定角色，而不是使用者。 LoginView 控制項，會呈現不同的輸出，已驗證和匿名使用者，可以設定成會顯示登入的使用者角色為基礎的內容不同。 和此角色 API 包含方法來判斷登入的使用者角色。

本教學課程開始了解如何角色 framework 建立的關聯使用者的角色與他的安全性內容。 然後，它會檢驗如何套用以角色為基礎的 URL 授權規則。 之後，我們將探討使用宣告式和程式設計的方式改變顯示的資料和 ASP.NET 網頁所提供的功能。 讓我們開始吧 ！

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>如何了解角色相關聯的使用者安全性內容

每當要求進入 ASP.NET 管線時的安全性內容，其中包含用來識別要求者的資訊與相關聯。 使用表單驗證時，可做為身分識別權杖的驗證票證。 如我們所述<a id="_msoanchor_2"> </a> [*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-vb.md)並<a id="_msoanchor_3"> </a> [*表單驗證組態和進階主題*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)教學課程`FormsAuthenticationModule`負責判斷要求者，它會在身分識別[`AuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

如果找到有效的、 未過期的驗證票證，`FormsAuthenticationModule`解碼以確定要求者的身分識別。 它會建立新`GenericPrincipal`物件，並將它指派`HttpContext.User`物件。 目的為主體時，例如`GenericPrincipal`，是用來識別已驗證的使用者名稱，以及她所屬的角色。 此用途很明顯所有的主體物件都有事實`Identity`屬性和`IsInRole(roleName)`方法。 `FormsAuthenticationModule`，不過，不想記錄角色資訊和`GenericPrincipal`物件，它會建立未指定任何角色。

如果啟用角色架構，則[ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) HTTP 模組中的步驟之後`FormsAuthenticationModule`，並識別已驗證的使用者角色期間[`PostAuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx)，之後，就會引發`AuthenticateRequest`事件。 如果要求是來自已驗證的使用者`RoleManagerModule`會覆寫`GenericPrincipal`所建立的物件`FormsAuthenticationModule`，並將它與[`RolePrincipal`物件](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx)。 `RolePrincipal`類別會使用角色 API 來判斷哪些使用者所屬的角色。

使用表單驗證和角色架構時，圖 1 說明 ASP.NET 管線工作流程。 `FormsAuthenticationModule`會先執行，識別使用者透過其驗證票證，並建立新`GenericPrincipal`物件。 下一步`RoleManagerModule`中的步驟，並覆寫`GenericPrincipal`物件`RolePrincipal`物件。

如果匿名使用者瀏覽網站，都`FormsAuthenticationModule`和`RoleManagerModule`建立主體物件。


[![已驗證使用者時使用表單驗證和角色架構的 ASP.NET 管線事件](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**圖 1**: 已驗證使用者時使用表單驗證和角色架構的 ASP.NET 管線事件 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>在 Cookie 中快取的角色資訊

`RolePrincipal`物件的`IsInRole(roleName)`方法呼叫`Roles`。`GetRolesForUser` 若要取得使用者的角色，以判斷使用者是否隸屬*roleName*。 當使用`SqlRoleProvider`，這會導致查詢角色存放區資料庫。 當使用以角色為基礎的 URL 授權規則`RolePrincipal`的`IsInRole`將受保護的以角色為基礎的 URL 授權規則 頁面的每個要求上呼叫方法。 而不需要查閱每個要求，在資料庫中的角色資訊`Roles`framework 包含快取使用者的角色在 cookie 中的選項。

如果快取使用者的角色在 cookie 中，設定角色架構`RoleManagerModule`ASP.NET 管線期間建立的 cookie [ `EndRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)。 在中的後續要求中會使用此 cookie `PostAuthenticateRequest`，這是當`RolePrincipal`建立物件。 如果 cookie 有效，且尚未過期，cookie 中的資料剖析和用來填入使用者的角色，藉此節省`RolePrincipal`不必呼叫`Roles`類別，以判斷使用者的角色。 圖 2 說明此工作流程。


[![使用者的角色資訊可以儲存在 Cookie 中，以改善效能](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**圖 2**: 使用者的角色資訊可以儲存在 Cookie 中改善效能 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image6.png))


根據預設，會停用角色快取 cookie 機制。 可以透過啟用`<roleManager>`; 中的組態標記`Web.config`。 我們討論了使用[`<roleManager>`項目](https://msdn.microsoft.com/library/ms164660.aspx)來指定角色中的提供者<a id="_msoanchor_4"> </a> [*建立和管理角色*](creating-and-managing-roles-vb.md)教學課程中，因此您應該在您的應用程式中已經有這個項目`Web.config`檔案。 指定角色快取的 cookie 設定的屬性為`<roleManager>`; 項目，並摘要說明 表 1。

> [!NOTE]
> 表 1 中列出的組態設定會指定產生的角色快取 cookie 的屬性。 如需有關 cookie、 其運作方式，以及它們的各種屬性的詳細資訊，請參閱[本教學課程中的 Cookie](http://www.quirksmode.org/js/cookies.html)。


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>描述</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              布林值，指出是否使用 cookie 快取。 預設值為 `false`。                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     角色快取 cookie 的名稱。 預設值是"。ASPXROLES"。                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                角色名稱 cookie 的路徑。 Path 屬性可讓開發人員限制至特定的目錄階層的 cookie 的範圍。 預設值是"/"，而這會通知傳送到任何加入網域所提出的要求的驗證票證 cookie 的瀏覽器。                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               指示何種技術用來保護角色快取 cookie。 允許的值為： `All` （預設值）;`Encryption`;`None`; 和`Validation`。 回頭的步驟 3 <a id="_anchor_5"> </a> [*表單驗證組態和進階主題*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)教學課程，如需有關這些保護層級。                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   布林值，指出是否需要 SSL 連線來傳送驗證 cookie。 預設值是 `false`。                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  布林值，指出是否每次重設 cookie 逾時使用者造訪網站的單一工作階段期間。 預設值是 `false`。 此值時，才相關`createPersistentCookie`設為`true`。                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         指定以分鐘為單位，驗證票證 cookie 到期之前的時間。 預設值是 `30`。 此值時，才相關`createPersistentCookie`設為`true`。                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   上述的標記中的項目指出，「 系統管理員 」 和 「 監督員角色允許; ; 元素會指示所所有使用者被拒絕。 我們將設定我們的應用程式，讓`false`， ，和頁面只會在系統管理員角色中，這些使用者可以存取而頁面仍可存取所有訪客。 若要達成此目的，先新增`true`檔案`cookieTimeout`資料夾。                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 加入至角色目錄的 Web.config 檔案 預設值是空字串，這會導致瀏覽器使用從中它已發行 （例如 www.yourdomain.com) 的網域。 在此情況下，將會在 cookie<strong>不</strong>子網域，例如 admin.yourdomain.com 進行要求時傳送。 圖 3： 新增的檔案目錄 (按一下以檢視完整大小的影像)                                                                                                                                                 |
|    `maxCachedResults`     | 接下來，新增下列組態標記至: 預設值為 25。 `RoleManagerModule`中的項目`maxCachedResults`區段可讓您指出，只有系統管理員角色的使用者，才可以存取 ASP.NET 中資源目錄。 `RolePrincipal`項目定義的 URL 授權規則的另一組`IsInRole`頁面上，可讓所有使用者瀏覽的頁面。 儲存您的變更之後`maxCachedResults`不在系統管理員角色的使用者身分登入，然後再次嘗試瀏覽其中一個受保護的頁面。 會偵測到您沒有權限來瀏覽要求的資源; 因此，會將您重新導向至登入頁面。 登入頁面會再將您重新導向至`maxCachedResults`頁面 （請參閱 圖 4）。 |

**登入頁面，以便從這個最後一個重新導向**因為我們在步驟 2 中的 [登入] 頁面新增的程式碼，就會發生**  使用者為基礎的授權教學課程。

特別是，登入頁面將自動重新導向至任何已驗證的使用者如果查詢字串包含參數，作為此參數會指出，使用者在登入頁面之後進入嘗試檢視的網頁時，他不是檢視權限。 只有系統管理員角色的使用者，才可以檢視受保護的頁面

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

圖 4`cacheRolesInCookie`： 只有系統管理員角色的使用者，才可以檢視受保護的網頁 (`createPersistentCookie`按一下以檢視完整大小的影像`cookieProtection`) 先登出，然後在系統管理員角色的使用者身分登入。 現在您應該能夠檢視三個受保護的頁面。 可以瀏覽 Tito UsersAndRoles.aspx 頁面因為他是在系統管理員角色

這樣就完成了 ！ 圖 5： 可以瀏覽 Tito頁因為他是系統管理員角色中 (按一下以檢視完整大小的影像) 指定的 URL 授權規則 – 角色或使用者時務必記住，規則會分析一次一個，從頂端向下。

> [!NOTE]
> 因此，如果您想要限制存取一或多個使用者帳戶，務必您使用&amp;URL 授權組態中的最後一個元素的項目。 如果未包含您的 URL 授權規則項目中，所有使用者會都獲授與存取權。 URL 授權規則會分析方式的更完整討論，請參閱上一步 」 看會使用授權規則授與或拒絕存取 」 一節   使用者為基礎的授權教學課程。 步驟 2： 限制目前登入使用者的角色為基礎的功能


## <a name="step-1-defining-role-based-url-authorization-rules"></a>允許 URL 授權可讓您輕鬆地指定粗略的授權規則該狀態哪些身分識別，而哪些拒絕檢視特定頁面 （或資料夾及其子資料夾中的所有頁面）。

不過，在某些情況下，我們可能會想要允許所有使用者，請瀏覽] 頁面上，但限制造訪的使用者角色為基礎的網頁的功能。 URL 授權規則會在拼`Web.config`使用[`<authorization>`項目](https://msdn.microsoft.com/library/8d82143t.aspx)具有`<allow>`和`<deny>`子項目。 除了先前的教學課程所討論的使用者相關的授權規則每`<allow>`和`<deny>`也可以包含子項目：

- 特定的角色
- 以逗號分隔的角色清單

例如，URL 授權規則授與存取權的系統管理員 」 和 「 監督員的角色，這些使用者，但拒絕所有其他項目的存取：

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

`<allow>`上述的標記中的項目指出，「 系統管理員 」 和 「 監督員角色允許; `<deny>`; 元素會指示所*所有*使用者被拒絕。

我們將設定我們的應用程式，讓`ManageRoles.aspx`， `UsersAndRoles.aspx`，和`CreateUserWizardWithRoles.aspx`頁面只會在系統管理員角色中，這些使用者可以存取而`RoleBasedAuthorization.aspx`頁面仍可存取所有訪客。

若要達成此目的，先新增`Web.config`檔案`Roles`資料夾。


[![加入至角色目錄的 Web.config 檔案](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**圖 3**： 新增`Web.config`的檔案`Roles`目錄 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image9.png))


接下來，新增下列組態標記至`Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

`<authorization>`中的項目`<system.web>`區段可讓您指出，只有系統管理員角色的使用者，才可以存取 ASP.NET 中資源`Roles`目錄。 `<location>`項目定義的 URL 授權規則的另一組`RoleBasedAuthorization.aspx`頁面上，可讓所有使用者瀏覽的頁面。

儲存您的變更之後`Web.config`不在系統管理員角色的使用者身分登入，然後再次嘗試瀏覽其中一個受保護的頁面。 `UrlAuthorizationModule`會偵測到您沒有權限來瀏覽要求的資源; 因此，`FormsAuthenticationModule`會將您重新導向至登入頁面。 登入頁面會再將您重新導向至`UnauthorizedAccess.aspx`頁面 （請參閱 圖 4）。 登入頁面，以便從這個最後一個重新導向`UnauthorizedAccess.aspx`因為我們在步驟 2 中的 [登入] 頁面新增的程式碼，就會發生<a id="_msoanchor_7"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-vb.md)教學課程。 特別是，登入頁面將自動重新導向至任何已驗證的使用者`UnauthorizedAccess.aspx`如果查詢字串包含`ReturnUrl`參數，作為此參數會指出，使用者在登入頁面之後進入嘗試檢視的網頁時，他不是檢視權限。


[![只有系統管理員角色的使用者，才可以檢視受保護的頁面](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**圖 4**： 只有系統管理員角色的使用者，才可以檢視受保護的網頁 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image12.png))


先登出，然後在系統管理員角色的使用者身分登入。 現在您應該能夠檢視三個受保護的頁面。


[![可以瀏覽 Tito UsersAndRoles.aspx 頁面因為他是在系統管理員角色](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**圖 5**： 可以瀏覽 Tito`UsersAndRoles.aspx`頁因為他是系統管理員角色中 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> 指定的 URL 授權規則 – 角色或使用者時務必記住，規則會分析一次一個，從頂端向下。 只要找到相符項目，使用者會授與或拒絕存取，視在找到相符項`<allow>`或`<deny>`項目。 **如果找到相符項目，則使用者會獲得存取。** 因此，如果您想要限制存取一或多個使用者帳戶，務必您使用`<deny>`URL 授權組態中的最後一個元素的項目。 **如果未包含您的 URL 授權規則**`<deny>`**項目中，所有使用者會都獲授與存取權。** URL 授權規則會分析方式的更完整討論，請參閱上一步 」 看`UrlAuthorizationModule`會使用授權規則授與或拒絕存取 」 一節<a id="_msoanchor_8"> </a> [ *使用者為基礎的授權*](../membership/user-based-authorization-vb.md)教學課程。


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>步驟 2： 限制目前登入使用者的角色為基礎的功能

允許 URL 授權可讓您輕鬆地指定粗略的授權規則該狀態哪些身分識別，而哪些拒絕檢視特定頁面 （或資料夾及其子資料夾中的所有頁面）。 不過，在某些情況下，我們可能會想要允許所有使用者，請瀏覽 頁面上，但限制造訪的使用者角色為基礎的網頁的功能。 這可能需要顯示或隱藏資料，根據使用者的角色，或屬於特定角色的使用者提供額外的功能。

設定第一個的 RoleGroup屬性設為 「 系統管理員 」 並將第二個的 「 監督員 」。 管理 LoginView 的特定角色的範本透過 RoleGroup 集合編輯器 接下來，我們將探討程式設計技術。 圖 8： 管理 LoginView 的特定角色的範本透過 RoleGroup 集合編輯器 (按一下以檢視完整大小的影像)

按一下 確定 關閉 RoleGroup 集合編輯器 中;這會更新 LoginView 的宣告式標記，以包含具有每 RoleGroup 的子項目定義在 RoleGroup 集合編輯器 中。 此外，「 檢視 」 下拉式清單中 LoginView 的智慧標籤-先列出剛才和– 現在包含已新增 kolekci RoleGroups。 編輯 kolekci RoleGroups，讓 「 監督員 」 角色中的使用者會顯示的有關如何編輯使用者帳戶，而系統管理員角色中的使用者會顯示用於編輯和刪除的指示。 進行這些變更之後，您 LoginView 宣告式標記看起來應該如下所示。 進行這些變更之後，儲存頁面，然後瀏覽它透過瀏覽器。

> [!NOTE]
> 第一次為匿名使用者造訪的頁面。 本教學課程中 > 系列聚焦於表單驗證、 授權、 使用者帳戶和角色，因為我不想花太多時間討論 GridView 控制項的內部運作方式。 雖然本教學課程提供特定的逐步指示，設定此頁面，它不會不探討為什麼做特定選擇，或轉譯的輸出上有的效果的特定屬性的詳細資料。 您應該會顯示訊息，「 您未登入系統。


首先開啟`RoleBasedAuthorization.aspx`頁面中`Roles`資料夾。 因此您無法編輯或刪除任何使用者資訊。 」 然後已驗證的使用者，但不在監督員或系統管理員角色的其中一個身分登入。`GetAllUsers` 此時您應該會看到訊息: 「 您不是監督員或系統管理員角色的成員。 接下來，使用者是 「 主管 」 角色的成員身分登入。

此時，您應該會看到各有特定角色的訊息 （請參閱 圖 9）。 如果您的使用者角色，您應該會看到系統管理員角色特定訊息 （請參閱 圖 10） 的系統管理員身分登入。 Bruce 顯示各有特定角色的訊息 圖 9`ShowDeleteButton`: Bruce 顯示各有特定角色的訊息 (按一下以檢視完整大小的影像) Tito 會顯示系統管理員角色特定訊息 圖 10`LastLoginDate`: Tito 會顯示系統管理員角色特定訊息 (`Email`按一下以檢視完整大小的影像`Comment`)

身為螢幕擷取畫面，在 圖 9 和 10 的節目，LoginView 只會呈現一個範本，即使套用多個範本。 Bruce 和 Tito 都會記錄在 [使用者]，但 LoginView 呈現只比對的 RoleGroup 而非`ReadOnly`。 此外，Tito 屬於系統管理員 」 和 「 監督員的角色，但 LoginView 控制項呈現系統管理員角色特定範本，而不是監督員內其中一個。 [圖 11] 說明 LoginView 控制項用來判斷哪些範本用來呈現工作流程。 請注意，是否有多個指定的其中一個 RoleGroup，LoginView 範本就會呈現`HtmlEncode`第一個`DataFormatString`RoleGroup 符合。 換句話說，如果我們有放置為第一個 RoleGroup 監督員 RoleGroup 和第二個為系統管理員，然後當 Tito 瀏覽此頁面他會看到監督員內訊息。

LoginView 控制項的工作流程，來判斷要呈現的範本


[![[圖 11![： 決定哪些範本要轉譯的 LoginView 控制項的工作流程 (](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)按一下以檢視完整大小的影像)](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**LoginView 控制項則會顯示不同的瀏覽頁面的使用者角色為基礎的指示，[編輯] 和 [取消] 按鈕會保持所有可見的。


我們需要以程式設計方式隱藏匿名訪客和收錄監督員或系統管理員角色中使用者的 編輯和刪除按鈕。 我們需要隱藏每一位使用者不是系統管理員的 [刪除] 按鈕。

為達成此目的，我們會撰寫的程式碼，以程式設計方式參考 CommandField 的編輯和刪除 Linkbutton 而設定其`Email`屬性，以`EditItemTemplate`，如有必要。 若要以程式設計方式參考 CommandField 中的控制項的最簡單方式是先將它轉換成範本。 若要這麼做，按一下 編輯欄位 連結，從 GridView 的智慧標籤，從目前的欄位清單中選取 CommandField 然後按一下 「 將這個欄位轉換為 TemplateField 」 連結。 這會變成使用 TemplateField CommandField`Columns`和`Rows`。

在之後設定這些 TemplateFields，其宣告式標記看起來應該如下所示：

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

編輯或刪除使用者帳戶，我們需要知道該使用者時`UserName`屬性值。 設定 GridView`DataKeyNames`為"UserName"的屬性，讓這項資訊可透過 GridView 的`DataKeys`集合。

最後，ValidationSummary 控制項加入頁面，並設定其`ShowMessageBox`屬性設為 True 並將其`ShowSummary`屬性設定為 False。 使用這些設定，ValidationSummary 會顯示用戶端警示，如果使用者嘗試編輯具有遺失或無效的電子郵件地址的使用者帳戶。

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

我們現在已完成此頁面的宣告式標記。 我們的下一個工作是將一組使用者帳戶繫結至 GridView。 新增名為`BindUserGrid`要`RoleBasedAuthorization.aspx`頁面的程式碼後置類別繫結`MembershipUserCollection`所傳回`Membership.GetAllUsers`到`UserGrid`GridView。 呼叫這個方法從`Page_Load`上第一個頁面瀏覽事件處理常式。

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

使用此程式碼就緒之後，請瀏覽透過瀏覽器頁面。 如 [圖 7] 所示，您應該會看到 GridView，列出系統中的每個使用者帳戶的相關資訊。


[![UserGrid GridView 列出系統中的每個使用者的相關資訊](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**圖 7**: `UserGrid` GridView 列出資訊有關每個使用者在系統中 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> `UserGrid` GridView 會列出所有的非分頁的介面中的使用者。 這個簡單的方格介面不是適用於案例有數個數十個或多個使用者。 其中一個選項是設定 GridView，以啟用分頁功能。 `Membership.GetAllUsers`方法有兩個多載︰ 一個可接受任何輸入的參數且傳回的所有使用者，另一個則會採用頁面索引和頁面大小的整數值，並都傳回使用者的指定的子集。 第二個多載可以用來更有效率地透過使用者的頁面，因為它會傳回只是使用者帳戶的精確子集而非*所有*其中。 如果您有數千個使用者帳戶，您可能要考慮的篩選為基礎的介面，其中只會顯示其使用者名稱以選取字元開頭，例如那些使用者。 [ `Membership.FindUsersByName`方法](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx)適合用來建立篩選器為基礎的使用者介面。 我們將探討在未來的教學課程中建置這類介面。


GridView 控制項提供內建的編輯和刪除時的控制項繫結至已正確設定的資料來源控制項，例如 SqlDataSource 或 ObjectDataSource 的支援。 `UserGrid` GridView，不過，具有以程式設計方式繫結其資料; 因此，我們必須撰寫程式碼來執行這兩項工作。 特別是，我們必須建立事件處理常式，如 GridView `RowEditing`， `RowCancelingEdit`， `RowUpdating`，和`RowDeleting`訪客按一下 GridView 的時所引發的事件編輯 [取消]，更新或刪除按鈕。

建立事件處理常式，如 GridView 的著手`RowEditing`， `RowCancelingEdit`，和`RowUpdating`事件然後加入下列程式碼：

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

`RowEditing`並`RowCancelingEdit`事件處理常式只會設定 GridView 的`EditIndex`屬性，並接著重新繫結，清單中的使用者帳戶方格。 中發生的有趣的東西`RowUpdating`事件處理常式。 這個事件處理常式一開始會確保資料有效，且接著抓取`UserName`值中的已編輯的使用者帳戶`DataKeys`集合。 `Email`並`Comment`文字方塊中兩個的 TemplateFields `EditItemTemplate` s 接著會以程式設計方式參考。 其`Text`屬性包含的已編輯的電子郵件地址和註解。

若要更新使用者帳戶的 Membership api，我們需要先取得使用者的資訊，我們完成這件事，透過對呼叫`Membership.GetUser(userName)`。 傳回`MembershipUser`物件的`Email`和`Comment`屬性接著會更新編輯介面中輸入兩個文字方塊的值。 最後，這些修改會儲存藉由呼叫[ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)。 `RowUpdating`事件處理常式完成藉由還原成其預先編輯介面 GridView。

接下來，建立`RowDeleting`RowDeleting 事件處理常式，然後加入下列程式碼：

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

上述的事件處理常式會先擷取`UserName`GridView 的值`DataKeys`集合; 這`UserName`值，然後傳入 Membership 類別[`DeleteUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)。 `DeleteUser`方法刪除的使用者帳戶從系統中，包括相關的成員資格資料 （例如哪些角色這位使用者屬於）。 刪除使用者，方格之後`EditIndex`（如果使用者按下 Delete，而另一個資料列處於編輯模式） 設為-1 和`BindUserGrid`呼叫方法。

> [!NOTE]
> [刪除] 按鈕不需要使用者確認，然後再刪除使用者帳戶的任何排序。 建議您將加入某種形式的使用者確認，為了降低不小心刪除的帳戶。 其中一個最簡單的方式，可確認動作是透過用戶端確認對話方塊。 如需有關這項技術的詳細資訊，請參閱 <<c0> [ 正在刪除時新增用戶端確認](https://asp.net/learn/data-access/tutorial-42-vb.aspx)。


請確認此頁面函式，如預期般運作。 您應該能夠編輯任何使用者的電子郵件地址和註解，以及刪除任何使用者帳戶。 因為`RoleBasedAuthorization.aspx`頁面可供所有使用者存取，任何使用者 – 甚至是匿名的使用者 – 瀏覽此頁面，編輯和刪除使用者帳戶 ！ 因此，只有 「 監督員 」 和 「 系統管理員角色中的使用者可以編輯使用者的電子郵件地址和註解，只有系統管理員可以刪除使用者帳戶，讓我們更新此頁面。

「 使用 LoginView 控制項 」 一節探討使用 LoginView 控制項以顯示指示特定的使用者角色。 如果系統管理員角色中的人員造訪此頁面，我們將說明如何編輯和刪除使用者的指示。 如果監督員角色中的使用者達到此頁面，我們將說明在編輯使用者的指示。 而如果造訪者為匿名，或不是監督員 」 或 「 系統管理員角色中，我們會顯示訊息，說明他們無法編輯或刪除使用者帳戶資訊。 「 以程式設計的方式限制的功能 > 一節中，我們會撰寫程式碼，以程式設計的方式顯示或隱藏使用者的角色為基礎的編輯和刪除按鈕。

### <a name="using-the-loginview-control"></a>使用 LoginView 控制項

因為我們在過去的教學課程中所見，LoginView 控制項可用於顯示不同的介面，針對已驗證和匿名使用者，但 LoginView 控制項也可用來顯示不同的標記，根據使用者的角色。 讓我們使用 LoginView 控制項來顯示不同根據造訪的使用者角色的指示。

開始新增上述 LoginView `UserGrid` GridView。 如同稍早所討論，LoginView 控制項有兩個內建的範本：`AnonymousTemplate`和`LoggedInTemplate`。 在下列兩個範本，以通知使用者，他們無法編輯或刪除任何使用者資訊，請輸入簡短訊息。

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

除了`AnonymousTemplate`並`LoggedInTemplate`，LoginView 控制項可以包含*kolekci RoleGroups*，這是特定角色的範本。 每個 RoleGroup 包含單一屬性， `Roles`，以指定 RoleGroup 適用於哪些角色。 `Roles`給單一角色 （像是"Administrators") 或逗號分隔清單的角色 （例如 「 系統管理員，監督員 」），就可以設定屬性。

若要管理 kolekci RoleGroups，按一下要顯示 RoleGroup 集合編輯器控制項的智慧標籤的 「 編輯 kolekci RoleGroups 」 連結。 新增兩個新 kolekci RoleGroups。 設定第一個的 RoleGroup`Roles`屬性設為 「 系統管理員 」 並將第二個的 「 監督員 」。


[![管理 LoginView 的特定角色的範本透過 RoleGroup 集合編輯器](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**圖 8**： 管理 LoginView 的特定角色的範本透過 RoleGroup 集合編輯器 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image24.png))


按一下 確定 關閉 RoleGroup 集合編輯器 中;這會更新 LoginView 的宣告式標記，以包含`<RoleGroups>`具有`<asp:RoleGroup>`每 RoleGroup 的子項目定義在 RoleGroup 集合編輯器 中。 此外，「 檢視 」 下拉式清單中 LoginView 的智慧標籤-先列出剛才`AnonymousTemplate`和`LoggedInTemplate`– 現在包含已新增 kolekci RoleGroups。

編輯 kolekci RoleGroups，讓 「 監督員 」 角色中的使用者會顯示的有關如何編輯使用者帳戶，而系統管理員角色中的使用者會顯示用於編輯和刪除的指示。 進行這些變更之後，您 LoginView 宣告式標記看起來應該如下所示。

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

進行這些變更之後，儲存頁面，然後瀏覽它透過瀏覽器。 第一次為匿名使用者造訪的頁面。 您應該會顯示訊息，「 您未登入系統。 因此您無法編輯或刪除任何使用者資訊。 」 然後已驗證的使用者，但不在監督員或系統管理員角色的其中一個身分登入。 此時您應該會看到訊息: 「 您不是監督員或系統管理員角色的成員。 因此您無法編輯或刪除任何使用者資訊。 」

接下來，使用者是 「 主管 」 角色的成員身分登入。 此時，您應該會看到各有特定角色的訊息 （請參閱 圖 9）。 如果您的使用者角色，您應該會看到系統管理員角色特定訊息 （請參閱 圖 10） 的系統管理員身分登入。


[![Bruce 顯示各有特定角色的訊息](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**圖 9**: Bruce 顯示各有特定角色的訊息 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image27.png))


[![Tito 會顯示系統管理員角色特定訊息](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**圖 10**: Tito 會顯示系統管理員角色特定訊息 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image30.png))


身為螢幕擷取畫面，在 圖 9 和 10 的節目，LoginView 只會呈現一個範本，即使套用多個範本。 Bruce 和 Tito 都會記錄在 [使用者]，但 LoginView 呈現只比對的 RoleGroup 而非`LoggedInTemplate`。 此外，Tito 屬於系統管理員 」 和 「 監督員的角色，但 LoginView 控制項呈現系統管理員角色特定範本，而不是監督員內其中一個。

[圖 11] 說明 LoginView 控制項用來判斷哪些範本用來呈現工作流程。 請注意，是否有多個指定的其中一個 RoleGroup，LoginView 範本就會呈現*第一個*RoleGroup 符合。 換句話說，如果我們有放置為第一個 RoleGroup 監督員 RoleGroup 和第二個為系統管理員，然後當 Tito 瀏覽此頁面他會看到監督員內訊息。


[![LoginView 控制項的工作流程，來判斷要呈現的範本](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**圖 11**： 決定哪些範本要轉譯的 LoginView 控制項的工作流程 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>以程式設計的方式限制功能

LoginView 控制項則會顯示不同的瀏覽頁面的使用者角色為基礎的指示，[編輯] 和 [取消] 按鈕會保持所有可見的。 我們需要以程式設計方式隱藏匿名訪客和收錄監督員或系統管理員角色中使用者的 編輯和刪除按鈕。 我們需要隱藏每一位使用者不是系統管理員的 [刪除] 按鈕。 為達成此目的，我們會撰寫的程式碼，以程式設計方式參考 CommandField 的編輯和刪除 Linkbutton 而設定其`Visible`屬性，以`False`，如有必要。

若要以程式設計方式參考 CommandField 中的控制項的最簡單方式是先將它轉換成範本。 若要這麼做，按一下 編輯欄位 連結，從 GridView 的智慧標籤，從目前的欄位清單中選取 CommandField 然後按一下 「 將這個欄位轉換為 TemplateField 」 連結。 這會變成使用 TemplateField CommandField`ItemTemplate`和`EditItemTemplate`。 `ItemTemplate`包含編輯和刪除時的 Linkbutton`EditItemTemplate`裝載的更新和取消的 Linkbutton。


[![CommandField 轉換為 TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**圖 12**： 轉換 CommandField 到 TemplateField ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image36.png))


更新的編輯和刪除中的 Linkbutton `ItemTemplate`，將其`ID`屬性的值來`EditButton`和`DeleteButton`分別。

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

GridView 資料繫結至 GridView，每當列舉中的記錄及其`DataSource`屬性，並產生對應`GridViewRow`物件。 因為每個`GridViewRow`建立物件時，`RowCreated`引發事件。 若要隱藏未經授權的使用者編輯和刪除按鈕，我們要建立此事件的事件處理常式，並以程式設計方式參考的編輯和刪除的 Linkbutton，設定其`Visible`屬性據此。

建立事件處理常式`RowCreated`事件然後加入下列程式碼：

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

請記住`RowCreated`事件的引發*所有*的 GridView 資料列，包括標頭、 頁尾、 頁面巡覽區介面等等。 我們只想要以程式設計方式參考的編輯和刪除 Linkbutton，如果我們正在處理不是處於編輯模式的資料列 （因為在編輯模式中的資料列已更新 和 取消 按鈕，而不是編輯和刪除）。 這項檢查由`If`陳述式。

如果我們正在處理不是處於編輯模式的資料列，所參考的編輯和刪除 Linkbutton 及其`Visible`屬性會根據設定所傳回的布林值`User`物件的`IsInRole(roleName)`方法。 `User`物件參考所建立的主體`RoleManagerModule`; 因此，`IsInRole(roleName)`方法會使用角色 API 來判斷目前的造訪者是否已屬於*roleName*。

> [!NOTE]
> 我們也可以使用角色類別的直接取代呼叫`User.IsInRole(roleName)`藉由呼叫[`Roles.IsUserInRole(roleName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)。 我決定使用主體的物件`IsInRole(roleName)`方法在此範例中因為它是比直接使用角色 API 更有效率。 稍早在本教學課程中，我們會設定快取使用者的角色在 cookie 中的角色管理員。 此快取的 cookie 資料只會利用當主體的`IsInRole(roleName)`呼叫方法時，對角色 API 的直接呼叫一定會涉及到角色存放區的某趟車程支付。 即使角色不會在 cookie 中快取，呼叫的主體物件`IsInRole(roleName)`方法是通常更有效率，因為它呼叫時針對要求的第一次快取結果。 角色 API，相反地，不會執行任何快取。 因為`RowCreated`事件引發一次，在 gridview 裡，每個資料列使用`User.IsInRole(roleName)`牽涉到一個來回存取角色存放區，而`Roles.IsUserInRole(roleName)`需要*N*車程，其中*N*是使用者帳戶方格中顯示的數字。


[編輯] 按鈕`Visible`屬性設定為`True`如果使用者瀏覽此頁面是在系統管理員或監督員的角色中，否則它會設定為`False`。 [刪除] 按鈕`Visible`屬性設定為`True`只有使用者是系統管理員角色。

測試此頁面，透過瀏覽器。 如果您瀏覽頁面匿名訪客或不是監督員或系統管理員使用者身分，CommandField 是空的。它仍存在，但為精簡型的銀級，而不需要編輯或刪除按鈕。

> [!NOTE]
> 您可隱藏 CommandField 完全當非監督員和非系統管理員就瀏覽的頁面。 我不要更動此練習的讀取器。


[![編輯和刪除按鈕會隱藏非監督員和非系統管理員](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**圖 13**: 編輯和刪除按鈕會隱藏非監督員和非系統管理員 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image39.png))


如果使用者屬於 「 主管 」 角色 （但不是屬於系統管理員角色） 瀏覽，他會看到只有 [編輯] 按鈕。


[![適用於各有 [編輯] 按鈕時，會隱藏 [刪除] 按鈕](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**圖 14**： 雖然 [編輯] 按鈕是適用於監督員內，會隱藏 [刪除] 按鈕 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image42.png))


如果系統管理員身分造訪時，她就能存取來編輯和刪除按鈕。


[![編輯和刪除按鈕才可以使用系統管理員](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**圖 15**: 編輯和刪除按鈕才可以使用系統管理員 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>步驟 3： 將以角色為基礎的授權規則套用至類別和方法

在步驟 2 中我們有限編輯功能，可在 「 監督員 」 和 「 系統管理員角色的使用者，並刪除只有系統管理員功能。 這是藉由隱藏未經授權的使用者，透過程式設計技術的相關聯的使用者介面項目來達成。 這類量值不保證，未經授權的使用者將無法執行特殊權限的動作。 可能會稍後新增的使用者介面項目，或我們忘了可隱藏未經授權的使用者。 或者，駭客可能會發現一些其他方式可取得 ASP.NET 網頁的執行所需的方法。

簡單的方法，以確保未經授權的使用者無法存取特定的一種功能是該類別或方法的裝飾[`PrincipalPermission`屬性](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)。 當.NET 執行階段會使用的類別，或執行其中一個方法時，它會檢查以確保目前的安全性內容有權限。 `PrincipalPermission`屬性提供一個機制，讓我們可以定義這些規則。

我們探討了使用`PrincipalPermission`屬性上上一步<a id="_msoanchor_9"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-vb.md)教學課程。 具體來說，我們了解如何裝飾 GridView`SelectedIndexChanged`和`RowDeleting`事件處理常式，這樣就可以只會執行已驗證的使用者和 Tito，分別。 `PrincipalPermission`屬性與角色也一樣好。

讓我們將示範如何使用`PrincipalPermission`屬性上的 GridView`RowUpdating`和`RowDeleting`禁止未經授權的使用者執行的事件處理常式。 我們要做的只是新增在每個函式定義適當的屬性：

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

屬性`RowUpdating`事件處理常式會指定只有系統管理員或監督員的角色中的使用者，可以執行的事件處理常式中，在做為屬性`RowDeleting`事件處理常式會限制執行中的系統管理員的使用者角色。


> [!NOTE]
> `PrincipalPermission`屬性會表示為中的類別`System.Security.Permissions`命名空間。 請務必新增`Imports System.Security.Permissions`頂端的程式碼後置類別檔匯入此命名空間的陳述式。


如果以某種方式，非系統管理員嘗試執行`RowDeleting`事件處理常式或如果非監督員 」 或 「 非系統管理員執行的嘗試`RowUpdating`.NET 執行階段將會引發的事件處理常式， `SecurityException`。


[![如果安全性內容未獲授權執行方法，會擲回安全性例外狀況](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**圖 16**： 如果安全性內容無權執行此方法中，`SecurityException`就會擲回 ([按一下以檢視完整大小的影像](role-based-authorization-vb/_static/image48.png))


除了 ASP.NET 頁面中，許多應用程式還包含各種層級，例如商務邏輯和資料存取層的架構。 這些層級通常會實作為類別庫，並提供類別和方法來執行商務邏輯與資料相關的功能。 `PrincipalPermission`屬性可用於將授權規則套用至這些層級。

如需有關使用`PrincipalPermission`屬性來定義類別和方法的授權規則，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格項目[的商務和資料層使用新增授權規則`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>總結

在本教學課程中我們探討了如何指定粗略，微調授權規則會根據使用者的角色。 ASP。NET 的 URL 授權 功能可讓網頁開發人員指定哪些身分識別允許或拒絕存取的網頁。 如我們所見年代<a id="_msoanchor_10"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-vb.md)教學課程中，規則可以套用使用者以使用者為基礎的 URL 授權。 它們也可以套用以角色的角色為基礎，如我們在本教學課程的步驟 1 中所見。

以宣告方式或以程式設計方式，可能會套用微調授權規則。 在步驟 2 中我們探討使用 LoginView 控制項 kolekci RoleGroups 功能來轉譯不同造訪的使用者角色為基礎的輸出。 我們也會探討如何以程式設計方式判斷使用者所屬的特定角色，以及如何據以調整頁面的功能。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [將授權規則新增至商務和資料層使用 `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [檢查 ASP.NET 2.0 的成員資格、 角色和設定檔： 使用角色](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [ASP.NET 2.0 的安全性問題清單](https://msdn.microsoft.com/library/ms998375.aspx)
- [技術文件`<roleManager>`項目](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝...

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者包括 Suchi Banerjee 和 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](assigning-roles-to-users-vb.md)
