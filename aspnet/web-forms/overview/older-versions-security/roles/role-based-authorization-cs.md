---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: "以角色為基礎的授權 (C#) |Microsoft 文件"
author: rick-anderson
description: "本教學課程開頭看看如何角色架構會將使用者的角色與他的安全性內容。 然後，它會檢查如何套用以角色為基礎的 URL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 749404a571a777c862a9d2652b3c460c75e870c4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="role-based-authorization-c"></a>以角色為基礎的授權 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> 本教學課程開頭看看如何角色架構會將使用者的角色與他的安全性內容。 然後，它會檢查套用以角色為基礎的 URL 授權規則的方式。 之後，我們將探討使用宣告式和以程式設計的方式改變顯示的資料和 ASP.NET 網頁所提供的功能。


## <a name="introduction"></a>簡介

在<a id="_msoanchor_1"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-cs.md)教學課程中我們可了解如何使用 URL 授權來指定哪些使用者可以瀏覽一組特定的頁面。 只要些許中標記`Web.config`，我們也可以指示，只允許已驗證的使用者造訪網頁的 ASP.NET。 或者我們可以聽寫，只有在 Tito 和 Bob 的使用者才允許，或指出所允許的 Sam 除外的所有已驗證的使用者。

除了 URL 授權，我們也討論了宣告式和程式設計技巧來控制顯示的資料和頁面，根據使用者瀏覽所提供的功能。 特別是，我們建立一個頁面，列出目前目錄的內容。 任何人都可以造訪此頁，但已驗證的使用者可以檢視檔案的內容並僅 Tito 無法刪除的檔案。

套用使用者以使用者為基礎的授權規則可能會變成簿記惡夢。 更容易維護的方法是使用以角色為基礎的授權。 好消息是的工具，在我們處置時套用授權規則的平均工作以及與角色的使用者帳戶一樣。 URL 授權規則可指定角色，而不是使用者。 LoginView 控制項，其會呈現不同的輸出為已驗證和匿名使用者，可以設定為顯示不同登入的使用者角色為基礎的內容。 與角色 API 包含判斷登入的使用者角色的方法。

本教學課程開頭看看如何角色架構會將使用者的角色與他的安全性內容。 然後，它會檢查套用以角色為基礎的 URL 授權規則的方式。 之後，我們將探討使用宣告式和以程式設計的方式改變顯示的資料和 ASP.NET 網頁所提供的功能。 讓我們開始吧 ！

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>了解如何角色相關聯的使用者安全性內容

當要求進入 ASP.NET 管線相關聯，所以與安全性內容，其中包含用來識別要求者的資訊。 使用表單驗證時，驗證票券會用做為身分識別權杖。 如我們所述<a id="_msoanchor_2"> </a> [*的表單驗證概觀*](../introduction/an-overview-of-forms-authentication-cs.md)和<a id="_msoanchor_3"> </a> [*表單驗證設定和進階主題*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)教學課程，`FormsAuthenticationModule`負責判斷要求者，它會在識別[ `AuthenticateRequest` 事件](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx).

如果找到有效的、 未過期的驗證票證，`FormsAuthenticationModule`解碼來確定要求者的身分識別。 它會建立新`GenericPrincipal`物件，並指派此`HttpContext.User`物件。 要為主體時，目的`GenericPrincipal`，是用來識別已驗證的使用者名稱，以及她屬於的角色。 明顯的所有主要物件都具有此目的，是`Identity`屬性和`IsInRole(roleName)`方法。 `FormsAuthenticationModule`，不過，不想要錄製角色資訊和`GenericPrincipal`物件，它會建立未指定任何角色。

如果已啟用角色架構， [ `RoleManagerModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.rolemanagermodule.aspx) HTTP 模組中的步驟之後`FormsAuthenticationModule`並找出期間已驗證的使用者角色[`PostAuthenticateRequest`事件](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.postauthenticaterequest.aspx)，之後，都會引發`AuthenticateRequest`事件。 如果要求是來自已驗證的使用者，`RoleManagerModule`會覆寫`GenericPrincipal`所建立的物件`FormsAuthenticationModule`取代，它與[`RolePrincipal`物件](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.aspx)。 `RolePrincipal`類別使用角色 API 來判斷哪些使用者所屬的角色。

圖 1 描繪 ASP.NET 管線工作流程時使用表單驗證和角色架構。 `FormsAuthenticationModule`會最先執行，識別使用者可透過其驗證票證，並建立新`GenericPrincipal`物件。 下一步`RoleManagerModule`中的步驟，並覆寫`GenericPrincipal`物件`RolePrincipal`物件。

如果匿名使用者造訪的網站，兩者都不`FormsAuthenticationModule`和`RoleManagerModule`建立主體物件。


[![已驗證的使用者使用表單驗證和角色架構時的 ASP.NET 管線事件](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**圖 1**: 驗證使用者時使用表單驗證和角色架構的 ASP.NET 管線事件 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>在 Cookie 中快取的角色資訊

`RolePrincipal`物件的`IsInRole(roleName)`方法呼叫`Roles.GetRolesForUser`取得以判斷使用者是否為成員的使用者角色*roleName*。 當使用`SqlRoleProvider`，這會導致查詢至角色存放區資料庫。 當使用以角色為基礎的 URL 授權規則`RolePrincipal`的`IsInRole`頁面所保護的以角色為基礎的 URL 授權規則的每個要求會呼叫方法。 而不需要查閱角色資訊在資料庫中的每個要求，則角色 framework 會包含快取在 cookie 中的使用者角色的選項。

如果角色 framework 設定為快取在 cookie 中，使用者的角色`RoleManagerModule`ASP.NET 管線期間建立的 cookie [ `EndRequest`事件](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.endrequest.aspx)。 在中的後續要求中會使用此 cookie `PostAuthenticateRequest`，這是當`RolePrincipal`建立物件。 如果 cookie 有效，且尚未過期，剖析和用來填入使用者的角色，進而儲存在 cookie 中的資料`RolePrincipal`不必呼叫`Roles`類別以決定使用者的角色。 圖 2 說明此工作流程。


[![使用者的角色資訊可以儲存在 Cookie 來改善效能](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**圖 2**: 使用者角色資訊可以儲存在 Cookie，以改善效能 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image6.png))


根據預設，會停用角色快取 cookie 機制。 可透過啟用`<roleManager>`中的組態標記`Web.config`。 我們將討論使用[`<roleManager>`元素](https://msdn.microsoft.com/en-us/library/ms164660.aspx)指定角色提供者中的<a id="_msoanchor_4"> </a> [*建立和管理角色*](creating-and-managing-roles-cs.md)教學課程中，所以應該在應用程式中已有此項目`Web.config`檔案。 指定角色快取的 cookie 設定的屬性為`<roleManager>`表 1 中的項目，並摘要。

> [!NOTE]
> 表 1 中所列的組態設定會指定產生的角色快取 cookie 的屬性。 如需有關 cookie、 它們如何運作，以及其各種屬性的詳細資訊，請閱讀[本教學課程中的 Cookie](http://www.quirksmode.org/js/cookies.html)。


| **Property** | **說明** |
| --- | --- |
| `cacheRolesInCookie` | 布林值，指出是否使用 cookie 快取。 預設值為 `false`。 |
| `cookieName` | 角色快取 cookie 的名稱。 預設值是"。ASPXROLES"。 |
| `cookiePath` | 角色名稱 cookie 的路徑。 Path 屬性可讓開發人員限制到特定目錄階層 cookie 的範圍。 預設值是"/"，這會通知要對網域執行任何要求傳送驗證票證 cookie 的瀏覽器。 |
| `cookieProtection` | 指出哪些技術可用來保護角色快取 cookie。 允許值為： `All` （預設值）。`Encryption`;`None`; 和`Validation`。 步驟 3 會回頭<a id="_anchor_5"> </a> [*表單驗證設定和進階主題*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)教學課程，如需有關這些保護層級。 |
| `cookieRequireSSL` | 布林值，指出是否需要 SSL 連線來傳送驗證 cookie。 預設值是 `false`。 |
| `cookieSlidingExpiration` | 布林值，指出是否每次重設的 cookie 逾時使用者造訪網站的單一工作階段期間。 預設值是 `false`。 這個值時，才相關`createPersistentCookie`設`true`。 |
| `cookieTimeout` | 指定以分鐘為單位的驗證票證 cookie 過期前的時間。 預設值是 `30`。 這個值時，才相關`createPersistentCookie`設`true`。 |
| `createPersistentCookie` | 布林值，指定角色快取 cookie 工作階段 cookie 或永續性 cookie。 如果`false`（預設值），會使用工作階段 cookie、 瀏覽器關閉時，這會刪除。 如果`true`，就會使用永續性 cookie，才會到期`cookieTimeout`數幾分鐘後已建立或之後的值而定的上一個造訪`cookieSlidingExpiration`。 |
| `domain` | 指定 cookie 的網域值。 預設值為空字串，這會導致瀏覽器使用從中發行 （例如 www.yourdomain.com) 的網域。 在此情況下，cookie 將**不**子網域，例如 admin.yourdomain.com 進行要求時傳送。如果您想要傳遞至所有子網域的 cookie 需要自訂`domain`屬性，將它設定為"yourdomain.com"。 |
| `maxCachedResults` | 在 cookie 中指定快取的角色名稱的數目上限。 預設值為 25。 `RoleManagerModule`不屬於使用者的 cookie 會建立多個`maxCachedResults`角色。 因此，`RolePrincipal`物件的`IsInRole`方法會使用`Roles`類別以決定使用者的角色。 原因`maxCachedResults`存在是因為許多使用者代理程式不允許 cookie 大於 4096 個位元組。 因此這個端點是用來降低超過此大小限制的可能性。 如果您有很長的角色名稱，您可能要考慮指定更小`maxCachedResults`值; contrariwise，如果您有極短的角色名稱，您可以可能增加這個值。 |

**表 1:**角色快取 Cookie 組態選項

讓我們來設定應用程式使用非持續性的角色快取 cookie。 若要完成這項作業，更新`<roleManager>`中的項目`Web.config`要包含的 cookie 相關的下列屬性：

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

我更新`<roleManager>`藉由加入三個屬性的項目： `cacheRolesInCookie`， `createPersistentCookie`，和`cookieProtection`。 藉由設定`cacheRolesInCookie`至`true`、`RoleManagerModule`會立即自動快取在 cookie 中的使用者角色，而不是需要查閱每個要求的使用者角色資訊。 明確地設定`createPersistentCookie`和`cookieProtection`屬性至`false`和`All`分別。 技術上來說，我不需要指定這些屬性，因為我剛才為其預設值，指派，但是我將它們放在此將明確值清除未使用永續性 cookie 和 cookie 會同時進行加密及驗證。

這就是這麼簡單 ！ 從此以後，角色架構會快取在 cookie 中的使用者的角色。 如果使用者的瀏覽器不支援 cookie，或如果其 cookie 已被刪除，或遺失，以某種方式，它是任何非同小可 –`RolePrincipal`物件時便會使用`Roles`沒有 cookie （或無效或已過期一） 是可用的案例中的類別。

> [!NOTE]
> Microsoft 的 Patterns&amp;作法群組阻止使用持續性的角色快取 cookie。 如果駭客能夠以某種方式取得存取權的有效使用者的 cookie，擁有的角色快取的 cookie 就足以證明角色成員資格，因為他可以模擬該使用者。 發生此情形的可能性會增加保存 cookie 在使用者的瀏覽器上。 如需安全性建議的方式，以及其他安全性考量的詳細資訊，請參閱[ASP.NET 2.0 的安全性問題清單](https://msdn.microsoft.com/en-us/library/ms998375.aspx)。


## <a name="step-1-defining-role-based-url-authorization-rules"></a>步驟 1： 定義以角色為基礎的 URL 授權規則

中所述<a id="_msoanchor_6"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-cs.md)教學課程中，URL 授權提供一組使用者的使用者或角色的角色上的頁面來限制存取基準。 URL 授權規則拼中`Web.config`使用[`<authorization>`元素](https://msdn.microsoft.com/en-us/library/8d82143t.aspx)與`<allow>`和`<deny>`子項目。 除了先前的教學課程所討論的使用者相關的授權規則每個`<allow>`和`<deny>`也可以包含子元素：

- 特定的角色
- 以逗號分隔清單的角色

比方說，URL 授權規則授與存取權的系統管理員和監督員的角色中的使用者，但拒絕存取其他所有項目：

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

`<allow>`上述標記中的項目狀態，則允許系統管理員和監督員角色;`<deny>`項目指示的*所有*使用者被拒絕。

讓我們設定我們的應用程式，讓`ManageRoles.aspx`， `UsersAndRoles.aspx`，和`CreateUserWizardWithRoles.aspx`頁面只會在系統管理員角色中，這些使用者可以存取時`RoleBasedAuthorization.aspx`頁面仍可存取所有造訪者。

若要完成這項作業，一開始新增`Web.config`檔案`Roles`資料夾。


[![加入至角色目錄的 Web.config 檔案](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**圖 3**： 新增`Web.config`檔案`Roles`目錄 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image9.png))


接下來，加入下列組態標記以`Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

`<authorization>`中的項目`<system.web>`區段指出，只有系統管理員角色中的使用者可以存取 ASP.NET 中資源`Roles`目錄。 `<location>`項目定義的 URL 授權規則替代集`RoleBasedAuthorization.aspx`頁面上，讓所有使用者瀏覽的頁面。

在儲存變更之後`Web.config`、 不是系統管理員角色的使用者身分登入，然後再嘗試瀏覽的其中一個受保護的頁面。 `UrlAuthorizationModule`會偵測到您沒有權限，以瀏覽要求的資源; 因此，`FormsAuthenticationModule`會將您導向至登入頁面。 登入頁面會再將您導向至`UnauthorizedAccess.aspx`頁面 （請參閱圖 4）。 這個最後的重新導向登入頁面，即可從`UnauthorizedAccess.aspx`因為我們加入登入頁面的步驟 2 中的程式碼，就會發生<a id="_msoanchor_7"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-cs.md)教學課程。 特別是，登入頁面將自動重新導向至任何已驗證的使用者`UnauthorizedAccess.aspx`如果查詢字串包含`ReturnUrl`參數，做為此參數會指出使用者到達在登入頁面後嘗試檢視的網頁時，他不是檢視權限。


[![只有系統管理員角色中的使用者可以檢視受保護的頁面](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**圖 4**： 只有系統管理員角色中的使用者可以檢視受保護的網頁 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image12.png))


先登出，然後在系統管理員角色的使用者身分登入。 現在您應該能夠檢視受保護的三個頁面。


[![可以瀏覽 Tito UsersAndRoles.aspx 頁面因為他是系統管理員角色](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**圖 5**: Tito 可以瀏覽`UsersAndRoles.aspx`頁面因為他是系統管理員角色 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image15.png))


> [!NOTE]
> 指定的 URL 授權規則-適用於角色或使用者 – 時務必牢記的規則會分析一次一個，從頂端向下。 找到相符項目，因為使用者被授與或拒絕存取，取決於如果比對中找不到`<allow>`或`<deny>`項目。 **如果找到相符項目，授與使用者存取。** 因此，如果您想要限制存取一或多個使用者帳戶，務必您使用`<deny>`URL 授權設定中的最後一個元素的項目。 **如果您的 URL 授權規則不包括**`<deny>`**元素中，所有使用者會被授都與存取。** 如的 URL 授權規則的分析方式的更完整討論，請參閱上一步 」 如何查看`UrlAuthorizationModule`使用授權規則授與或拒絕存取 」 一節<a id="_msoanchor_8"> </a> [ *使用者為基礎的授權*](../membership/user-based-authorization-cs.md)教學課程。


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>步驟 2： 限制目前登入的使用者角色為基礎的功能

允許 URL 授權可輕鬆地指定粗略的授權規則該狀態何種身分識別，而被拒絕檢視特定頁面 （或資料夾以及其子資料夾中的所有頁面）。 不過，在某些情況下，我們可能想要允許所有使用者 頁面上，請瀏覽，但正在瀏覽的使用者角色為基礎的網頁的功能限制。 這可能需要根據使用者的角色，或屬於特定角色的使用者提供額外的功能顯示或隱藏資料。

可能是以宣告方式或透過程式設計方式 （或兩者的某種組合），可以實作這類細部角色為基礎的授權規則。 下一節中，我們會看到如何實作透過 LoginView 控制項的宣告式微調授權。 接下來，我們將探討程式設計技巧。 我們來看看套用微調授權規則之前，不過，我們先建立的網頁，其功能取決於使用者造訪網站的角色。

讓我們來建立會列出所有使用者帳戶在 GridView 中系統中的頁面。 在 GridView 會包含每個使用者的使用者名稱、 電子郵件地址、 上次登入的日期和相關使用者註解。 除了顯示每個使用者的資訊，GridView 會包含編輯和刪除功能。 我們會一開始建立此頁面編輯和刪除功能，可用於所有使用者。 中的 「 使用 LoginView 控制項 」 和 「 以程式設計的方式限制功能 」 區段中，我們會看到如何啟用或停用這些功能，根據正在瀏覽的使用者角色。

> [!NOTE]
> 我們即將建置的 ASP.NET 網頁使用 GridView 控制項顯示的使用者帳戶。 因為這個教學課程中數列著重於表單驗證、 授權、 使用者帳戶和角色，我不想花費太多討論 GridView 控制項的內部運作的時間。 雖然本教學課程提供特定的此頁面所設定的逐步指示，它不會不深入為什麼進行特定選擇，或轉譯的輸出上有的效果的特定屬性的詳細資料。 如詳盡 GridView 控制項，請參閱我*[在 ASP.NET 2.0 中使用資料](../../data-access/index.md)*教學課程系列。


先開啟`RoleBasedAuthorization.aspx`頁面`Roles`資料夾。 拖曳到設計工具，並設定頁面的 GridView 其`ID`至`UserGrid`。 在不久後，我們會撰寫程式碼呼叫`Membership.GetAllUsers`方法，並繫結所產生的`MembershipUserCollection`GridView 的物件。 `MembershipUserCollection`包含`MembershipUser`物件中每個使用者帳戶系統。`MembershipUser`物件具有屬性，例如`UserName`， `Email`， `LastLoginDate`，依此類推。

我們會撰寫將使用者帳戶繫結至方格中的程式碼之前，讓我們先定義 GridView 的欄位。 從 GridView 的智慧標籤，按一下 [編輯欄位] 連結以啟動 [欄位] 對話方塊 （請參閱圖 6）。 從這裡，取消核取左下角中的 「 自動產生欄位 」 核取方塊。 因為我們想要編輯和刪除功能，包括新增 CommandField 並設定此 GridView 其`ShowEditButton`和`ShowDeleteButton`屬性為 True。 接下來，加入四個欄位來顯示`UserName`， `Email`， `LastLoginDate`，和`Comment`屬性。 使用 BoundField 為兩個唯讀屬性 (`UserName`和`LastLoginDate`) 和兩個可編輯欄位的 TemplateFields (`Email`和`Comment`)。

第一個 BoundField 顯示`UserName`屬性; 設定其`HeaderText`和`DataField`屬性為"UserName"。 此欄位無法編輯，因此，設定其`ReadOnly`屬性設定為 True。 設定`LastLoginDate`BoundField 藉由設定其`HeaderText`至 「 最後一個登入 」 及其`DataField`至 「 LastLoginDate"。 讓我們來格式化此 BoundField 的輸出，使只將日期顯示 （而不是日期和時間）。 若要達成此目的，將設定這個 BoundField`HtmlEncode`屬性設定為 False 且其`DataFormatString`"{0: d}"的屬性。 也設定`ReadOnly`屬性設定為 True。

設定`HeaderText`的兩個 TemplateFields"Email"和"Comme"屬性。


[![在 GridView 的欄位可以透過 [欄位] 對話方塊來設定](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**圖 6**: GridView 欄位可以是設定透過欄位 對話方塊 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image18.png))


我們現在需要定義`ItemTemplate`和`EditItemTemplate`"Email"和"Comme"TemplateFields。 將標籤 Web 控制項加入至每個`ItemTemplate`s 和繫結其`Text`屬性`Email`和`Comment`屬性，分別。

"Email"TemplateField 中，加入名為文字方塊`Email`至其`EditItemTemplate`並繫結其`Text`屬性`Email`使用雙向資料繫結的屬性。 加入 RequiredFieldValidator 和至 RegularExpressionValidator`EditItemTemplate`確保編輯的電子郵件屬性訪客已輸入有效的電子郵件地址。 「 註解 」 TemplateField 中，加入名為多行文字方塊`Comment`至其`EditItemTemplate`。 設定文字方塊的`Columns`和`Rows`屬性，以 40 和 4，分別，然後再繫結其`Text`屬性`Comment`使用雙向資料繫結的屬性。

設定之後這些 TemplateFields，其宣告式標記看起來應該如下所示：

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

當編輯或刪除使用者帳戶，我們需要知道該使用者的`UserName`屬性值。 設定 GridView `DataKeyNames` "UserName"的屬性，讓這項資訊可透過 GridView`DataKeys`集合。

最後，ValidationSummary 控制項加入網頁，並設定其`ShowMessageBox`屬性設定為 True 且其`ShowSummary`屬性設定為 False。 這些設定，如果使用者嘗試編輯具有遺失或無效的電子郵件地址的使用者帳戶 Aggregate 時，會顯示用戶端警示。

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

我們現在已完成這個頁面的宣告式標記。 下一步是將一組使用者帳戶繫結至 GridView。 新增名為`BindUserGrid`至`RoleBasedAuthorization.aspx`網頁的繫結的程式碼後置類別`MembershipUserCollection`傳回`Membership.GetAllUsers`至`UserGrid`GridView。 呼叫這個方法從`Page_Load`在第一個頁面造訪的事件處理常式。

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

此位置的程式碼，請瀏覽透過瀏覽器頁面。 如圖 7 所示，您應該會看到 GridView，列出系統中的每個使用者帳戶的相關資訊。


[![UserGrid GridView 列出系統中的每個使用者的相關資訊](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**圖 7**: `UserGrid` GridView 列出資訊有關每個使用者在系統中 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image21.png))


> [!NOTE]
> `UserGrid` GridView 會列出所有未分頁的介面中的使用者。 這個簡單的方格介面並不適合案例有數個數十個或多個使用者。 其中一個選項是設定在 GridView 啟用分頁。 `Membership.GetAllUsers`方法有兩個多載： 其中一個可接受任何輸入的參數和傳回的所有使用者，另一個則會在頁面索引和頁面大小的整數值，並都傳回只有指定的使用者子集。 第二個多載可以用來更有效率地透過使用者的頁面，因為它會傳回只精確子集的使用者帳戶而非*所有*它們。 如果您有數千個使用者帳戶，您可能要考慮的篩選器為基礎的介面，其中只會顯示其使用者名稱開頭為選取的字元，例如那些使用者。 [ `Membership.FindUsersByName method` ](https://msdn.microsoft.com/en-us/library/system.web.security.membership.findusersbyname.aspx)適合用來建立篩選器為基礎的使用者介面。 我們將探討未來教學課程中建立這類介面。


GridView 控制項提供內建的編輯和刪除支援此控制項繫結至已正確設定的資料來源控制項，例如 SqlDataSource 或 ObjectDataSource 時。 `UserGrid` GridView，不過，以程式設計的方式繫結其資料; 因此，我們必須撰寫程式碼來執行這兩項工作。 特別是，我們必須建立事件處理常式的 GridView `RowEditing`， `RowCancelingEdit`， `RowUpdating`，和`RowDeleting`訪客按一下 GridView 時所引發的事件編輯 [取消]，更新或刪除按鈕。

從建立 GridView 的事件處理常式開始`RowEditing`， `RowCancelingEdit`，和`RowUpdating`事件，然後加入下列程式碼：

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

`RowEditing`和`RowCancelingEdit`事件處理常式直接將 GridView`EditIndex`屬性，然後重新繫結清單的使用者帳戶新增至方格。 有趣的東西會發生在`RowUpdating`事件處理常式。 這個事件處理常式一開始會確保資料有效，然後抓取`UserName`值的已編輯的使用者帳戶的`DataKeys`集合。 `Email`和`Comment`文字方塊中兩個 TemplateFields' `EditItemTemplate` s 然後以程式設計方式參考。 其`Text`屬性包含已編輯的電子郵件地址和註解。

若要更新使用者帳戶的成員資格應用程式開發介面透過，我們需要先取得使用者的資訊，透過對呼叫進行`Membership.GetUser(userName)`。 傳回`MembershipUser`物件的`Email`和`Comment`屬性接著會更新與兩個文字方塊中輸入從編輯介面的值。 最後，這些修改會儲存呼叫[ `Membership.UpdateUser` ](https://msdn.microsoft.com/en-us/library/system.web.security.membership.updateuser.aspx)。 `RowUpdating`藉由還原至其預先編輯介面 GridView 完成事件處理常式。

接下來，建立`RowDeleting`事件處理常式，然後加入下列程式碼：

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

上述的事件處理常式一開始會抓取`UserName`值從 GridView`DataKeys`集合; 此`UserName`值接著會傳遞到成員資格類別[`DeleteUser`方法](https://msdn.microsoft.com/en-us/library/system.web.security.membership.deleteuser.aspx)。 `DeleteUser`方法會刪除使用者帳戶從系統中，包括相關的成員資格資料 （例如角色此使用者所屬）。 刪除使用者，方格之後`EditIndex`（如果使用者已按下 Delete，而另一個資料列處於編輯模式） 設為-1 和`BindUserGrid`方法呼叫。

> [!NOTE]
> [刪除] 按鈕並不需要使用者確認再刪除使用者帳戶的任何排序。 建議您將加入某種形式的使用者確認，為了降低意外刪除的帳戶。 確認動作的最簡單方式是透過用戶端確認對話方塊。 如需有關這項技術的詳細資訊，請參閱[新增用戶端確認時刪除](https://asp.net/learn/data-access/tutorial-42-cs.aspx)。


請確認此頁面會在如預期運作。 您應該能夠編輯任何使用者的電子郵件地址和註解，以及刪除任何使用者帳戶。 因為`RoleBasedAuthorization.aspx`頁面是供所有使用者存取，任何使用者 – 即使匿名訪客 – 可以造訪此頁和編輯和刪除使用者帳戶 ！ 讓我們來更新此頁面，而只有監督員與系統管理員角色中的使用者可以編輯使用者的電子郵件地址和註解，只有系統管理員可以刪除使用者帳戶。

「 使用 LoginView 控制項 」 一節探討使用 LoginView 控制項以顯示指示特定的使用者角色。 如果系統管理員角色中的人員造訪此頁，我們將示範如何編輯和刪除使用者的指示。 如果監督員角色的使用者到達此頁面，我們會顯示指示編輯使用者。 如果訪客是匿名的或不是監督員或系統管理員角色，我們將會顯示訊息，說明他們無法編輯或刪除使用者帳戶資訊。 在 「 以程式設計的方式限制功能 」 一節中，我們會撰寫程式碼，以程式設計方式顯示或隱藏使用者的角色為基礎的編輯和刪除按鈕。

### <a name="using-the-loginview-control"></a>使用 LoginView 控制項

如同我們在過去的教學課程中，LoginView 控制項適用於顯示不同的介面，針對已驗證和匿名使用者，但 LoginView 控制項也可用來顯示不同的標記，根據使用者的角色。 讓我們使用 LoginView 控制項來顯示不同於正在瀏覽的使用者角色的指示。

藉由新增上述 LoginView 開始`UserGrid`GridView。 如同我們先前所討論，LoginView 控制項有兩個內建的範本：`AnonymousTemplate`和`LoggedInTemplate`。 兩個通知使用者他們無法編輯或刪除任何使用者資訊這些範本中輸入簡短訊息。

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

除了`AnonymousTemplate`和`LoggedInTemplate`，LoginView 控制項可以包含*RoleGroups*，這是特定角色的範本。 每個 RoleGroup 包含單一屬性， `Roles`，以指定 RoleGroup 適用於哪些角色。 `Roles`給單一角色 （例如 「 系統管理員 」），或以逗號分隔的角色 （例如 「 系統管理員，監督員 」） 清單，可以設定屬性。

若要管理 RoleGroups，按一下控制項的智慧標籤上顯示 RoleGroup 集合編輯器的 「 編輯 RoleGroups 」 連結。 加入兩個新 RoleGroups。 設定第一個的 RoleGroup`Roles`屬性設為 「 系統管理員 」，並將第二個的"監督員 」。


[![管理 LoginView 角色特定範本 RoleGroup 集合編輯器](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**圖 8**： 管理 LoginView 角色特定範本透過 RoleGroup 集合編輯器 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image24.png))


按一下 確定 關閉 RoleGroup 集合編輯器 中;這會更新 LoginView 的宣告式標記来包含`<RoleGroups>`區段中具有`<asp:RoleGroup>`子元素的每個 RoleGroup 定義 RoleGroup 集合編輯器 中。 此外，「 檢視 」 下拉式清單中 LoginView 的智慧標籤-先列出只`AnonymousTemplate`和`LoggedInTemplate`– 現在包含也加入的 RoleGroups。

編輯 RoleGroups，以便監督員角色中的使用者是有關如何編輯使用者帳戶，而系統管理員角色中的使用者顯示的編輯和刪除指示的顯示的指示。 進行這些變更之後，您 LoginView 宣告式標記看起來應該如下所示。

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

進行這些變更之後，儲存資料頁，然後瀏覽它透過瀏覽器。 為匿名使用者第一次瀏覽的頁面。 您應該會顯示訊息，「 不登入系統。 因此您無法編輯或刪除任何使用者資訊。 」 然後登入為已驗證的使用者，但不在監督員或系統管理員角色的其中一個。 此時您應該會看到訊息: 「 您不是監督員或系統管理員角色的成員。 因此您無法編輯或刪除任何使用者資訊。 」

接下來，監督員角色成員的使用者身分登入。 此時，您應該會看到監督員角色特定訊息 （請參閱圖 9）。 如果您的系統管理員角色，您應該會看到系統管理員角色特定訊息 （請參閱圖 10） 中的使用者身分登入。


[![Bruce 顯示監督員角色特有的訊息](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**圖 9**: Bruce 顯示監督員角色特有的訊息 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image27.png))


[![Tito 顯示系統管理員角色特有的訊息](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**圖 10**: Tito 顯示系統管理員角色特有的訊息 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image30.png))


做為螢幕擷取畫面中數字 9，10 顯示 LoginView 只會呈現一個範本，即使套用多個範本。 Bruce 和 Tito 都會記錄在 [使用者]，但 LoginView 呈現只比對的 RoleGroup 而非`LoggedInTemplate`。 此外，Tito 屬於系統管理員和監督員角色，但 LoginView 控制項呈現系統管理員角色特定範本，而不是監督員其中一個。

圖 11 說明用來判斷哪一個範本來呈現 LoginView 控制項的工作流程。 請注意，是否有多個指定的其中一個 RoleGroup，LoginView 範本就會呈現*第一個*符合 RoleGroup。 換句話說，如果我們必須置於做為第一個 RoleGroup 監督員 RoleGroup，做為第二個系統管理員，然後當 Tito 瀏覽此網頁他會看到 「 監督員。


[![LoginView 控制項的工作流程，以判斷哪一個範本來轉換](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**圖 11**： 判斷項目範本，以呈現 LoginView 控制項的工作流程 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>以程式設計的方式限制功能

LoginView 控制項則會顯示不同的瀏覽頁面的使用者角色為基礎的指示，[編輯] 和 [取消] 5d; 按鈕會保持所有可見的。 我們需要以程式設計方式隱藏匿名訪客和中收錄監督員或系統管理員角色之使用者的 編輯和刪除按鈕。 我們需要隱藏每一位使用者不是系統管理員的 [刪除] 按鈕。 為達成此目的，我們會撰寫以程式設計方式參考 CommandField 編輯和刪除 LinkButtons 和集合的程式碼的位元其`Visible`屬性`false`，必要時。

以程式設計方式參考 CommandField 中的控制項的最簡單方式是先將它轉換為範本。 若要完成此動作，按一下 GridView 的智慧標籤的 「 編輯資料行 」 連結，從目前的欄位清單中選取 CommandField 再按一下 」 將這個欄位轉換為 TemplateField 」 的連結。 這會變成具有為 TemplateField CommandField`ItemTemplate`和`EditItemTemplate`。 `ItemTemplate`包含編輯和刪除 LinkButtons 時`EditItemTemplate`裝載的更新和取消 LinkButtons。


[![CommandField 轉換為 TemplateField](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**圖 12**： 轉換 CommandField 到 TemplateField ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image36.png))


更新的編輯和刪除 LinkButtons 中`ItemTemplate`，設定其`ID`屬性的值`EditButton`和`DeleteButton`分別。

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

資料繫結至 GridView，每當 GridView 列舉中的記錄其`DataSource`屬性，並產生對應`GridViewRow`物件。 為每個`GridViewRow`建立物件後，`RowCreated`引發事件。 若要隱藏編輯和刪除按鈕的未經授權的使用者，我們要建立此事件的事件處理常式，並以程式設計方式參考的編輯和刪除 LinkButtons，設定其`Visible`屬性據此。

建立事件處理常式`RowCreated`事件，然後加入下列程式碼：

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

請注意，`RowCreated`事件引發的*所有*的 GridView 資料列，包括標頭、 頁尾、 頁面巡覽區介面等等。 我們只想要以程式設計方式參照編輯和刪除 LinkButtons，如果我們正在處理不是處於編輯模式的資料列 （因為編輯模式中的資料列已更新 和 取消 5d; 按鈕，而不是編輯和刪除）。 這項檢查由`if`陳述式。

如果我們處理資料列處於編輯模式時，所參考的編輯和刪除 LinkButtons 及其`Visible`屬性會根據設定所傳回的布林值`User`物件的`IsInRole(roleName)`方法。 使用者物件參考所建立的主體`RoleManagerModule`; 因此，`IsInRole(roleName)`方法使用角色 API 來判斷目前的訪客是否已屬於*roleName*。

> [!NOTE]
> 我們原也可以使用角色類別直接取代呼叫`User.IsInRole(roleName)`呼叫[`Roles.IsUserInRole(roleName)`方法](https://msdn.microsoft.com/en-us/library/system.web.security.roles.isuserinrole.aspx)。 我決定要使用的主體物件`IsInRole(roleName)`方法在此範例中，所以比直接使用角色 API 更有效率。 稍早在本教學課程中，我們會設定要快取在 cookie 中的使用者角色的角色管理員。 此快取的 cookie 資料僅當利用主體`IsInRole(roleName)`方法呼叫，則角色 API 的直接呼叫一定會涉及到角色存放區。 即使不會在 cookie 中快取的角色，呼叫的主體物件`IsInRole(roleName)`方法是通常更有效率，因為當呼叫它的第一次要求期間快取結果。 角色 API，相反地，不會執行任何快取。 因為`RowCreated`事件針對在 GridView 中每個資料列引發一次使用`User.IsInRole(roleName)`牽涉到單一往返角色存放區，而`Roles.IsUserInRole(roleName)`需要*N*往返，其中*N*是顯示在方格中的使用者帳戶數目。


[編輯] 按鈕`Visible`屬性設定為`true`如果使用者瀏覽此頁面是 「 系統管理員或監督員角色中; 否則它會設為`false`。 [刪除] 按鈕`Visible`屬性設定為`true`只有使用者是系統管理員角色。

測試此頁面，透過瀏覽器。 如果您瀏覽的頁面，為匿名的造訪者或使用者不主管人員和系統管理員，CommandField 是空的。它仍存在，但為精簡的銀級，而不需要編輯或刪除按鈕。

> [!NOTE]
> 您可隱藏 CommandField 完全時的非監督員和非系統管理員瀏覽頁面。 我將保留此為一項工作讀取器。


[![編輯及刪除按鈕是隱藏的非監督員和非系統管理員](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**圖 13**: 編輯及刪除按鈕隱藏的非監督員和非系統管理員 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image39.png))


如果造訪所屬的監督員角色 （但不是屬於系統管理員角色） 的使用者，他可以看到只有 [編輯] 按鈕。


[![可供監督員 [編輯] 按鈕時，會隱藏 [刪除] 按鈕](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**圖 14**： 時編輯按鈕可供監督員，隱藏 [刪除] 按鈕 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image42.png))


而且如果系統管理員身分造訪時，她會具有存取權編輯和刪除的按鈕。


[![編輯和刪除 按鈕後，才可以使用系統管理員](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**圖 15**: 編輯和刪除按鈕只可用為系統管理員 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>步驟 3： 將以角色為基礎的授權規則套用至類別和方法

在步驟 2 中我們有限的編輯功能，以監督員與系統管理員角色中的使用者，並刪除只有系統管理員功能。 這是隱藏的未經授權的使用者，透過程式設計技術的相關聯的使用者介面項目以完成。 這類量值不保證，未經授權的使用者將無法執行特殊權限的動作。 可能會稍後加入的使用者介面項目或我們忘記可隱藏未經授權的使用者。 或駭客可能會發現某些其他方法可以讓 ASP.NET 網頁的執行所需的方法。

以確保未經授權的使用者無法存取一項特定功能的簡單方法是該類別或方法具有裝飾[`PrincipalPermission`屬性](https://msdn.microsoft.com/en-us/library/system.security.permissions.principalpermissionattribute.aspx)。 當.NET 執行階段會使用類別或執行其中一個方法時，它會檢查以確認目前的安全性內容有權限。 `PrincipalPermission`屬性提供一個機制，我們可以透過定義這些規則。

我們探討了使用`PrincipalPermission`屬性回<a id="_msoanchor_9"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-cs.md)教學課程。 具體來說，我們可了解如何裝飾 GridView`SelectedIndexChanged`和`RowDeleting`事件處理常式，讓它們無法只能執行經過驗證的使用者和 Tito，分別。 `PrincipalPermission`屬性一樣能夠運作的角色。

讓我們將示範如何使用`PrincipalPermission`屬性在 GridView 的`RowUpdating`和`RowDeleting`來禁止未經授權的使用者執行的事件處理常式。 我們要做的就是將加入每個函式定義在適當的屬性：

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

屬性`RowUpdating`事件處理常式規定只有系統管理員或監督員角色中的使用者，可以執行事件處理常式上的屬性為`RowDeleting`事件處理常式會限制系統管理員中的使用者執行角色。


> [!NOTE]
> `PrincipalPermission`屬性中的類別會以表示`System.Security.Permissions`命名空間。 請務必新增`using System.Security.Permissions`頂端的程式碼後置類別檔匯入此命名空間的陳述式。


如果由於某種原因，而非系統管理員嘗試執行`RowDeleting`事件處理常式或如果非監督者或非系統管理員嘗試執行`RowUpdating`.NET 執行階段將會引發的事件處理常式， `SecurityException`。


[![如果安全性內容未獲授權執行方法，會擲回安全性例外狀況](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**圖 16**： 若未經授權的安全性內容執行方法，`SecurityException`就會擲回 ([按一下以檢視完整大小的影像](role-based-authorization-cs/_static/image48.png))


除了 ASP.NET 網頁中，許多應用程式也會有架構，其中包括各種層，例如商務邏輯和資料存取層級。 這些圖層通常實作為類別庫，並提供類別和方法來執行商務邏輯與資料相關的功能。 `PrincipalPermission`屬性可用於將授權規則套用至這些層級。

如需有關使用`PrincipalPermission`屬性來定義類別和方法上的授權規則，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格項目[企業和資料層使用新增授權規則`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>總結

本教學課程中我們探討如何指定粗略並微調授權規則根據使用者的角色。 ASP。網路的 URL 授權功能可讓網頁開發人員指定允許或拒絕存取網頁何種身分識別。 如我們所見回<a id="_msoanchor_10"> </a> [*使用者為基礎的授權*](../membership/user-based-authorization-cs.md)教學課程，規則可以套用使用者以使用者為基礎的 URL 授權。 它們也可以套用以角色的角色為基礎，如我們在本教學課程的步驟 1 中所見。

以宣告方式或以程式設計方式，可能會套用微調授權規則。 在步驟 2 中我們會探討使用 LoginView 控制項 RoleGroups 功能來呈現不同的輸出，根據正在瀏覽的使用者角色。 我們也討論了如何以程式設計方式判斷使用者所屬的特定角色，以及如何據以調整頁面的功能。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [加入商務和資料層級使用的授權規則`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [檢查 ASP.NET 2.0 的成員資格、 角色和設定檔： 使用角色](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [ASP.NET 2.0 的安全性問題清單](https://msdn.microsoft.com/en-us/library/ms998375.aspx)
- [技術文件`<roleManager>`項目](https://msdn.microsoft.com/en-us/library/ms164660.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝...

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者包括 Suchi Banerjee 和本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](assigning-roles-to-users-cs.md)
[下一頁](creating-and-managing-roles-vb.md)
