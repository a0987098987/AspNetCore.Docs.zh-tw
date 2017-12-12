---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: "建立使用者帳戶 (VB) |Microsoft 文件"
author: rick-anderson
description: "在本教學課程中，我們將探討使用 （透過 SqlMembershipProvider) 的成員資格 framework 來建立新的使用者帳戶。 我們會了解如何建立新我們..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 43e8423cd69a9cc345f2d8ef7d0a022252235462
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-user-accounts-vb"></a>建立使用者帳戶 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip)或[下載 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> 在本教學課程中，我們將探討使用 （透過 SqlMembershipProvider) 的成員資格 framework 來建立新的使用者帳戶。 我們會了解如何以程式設計方式和 ASP 透過建立新的使用者。網路的內建適用於 CreateUserWizard 控制項。


## <a name="introduction"></a>簡介

在<a id="_msoanchor_1"> </a>[前述教學課程](creating-the-membership-schema-in-sql-server-vb.md)我們在資料庫中，會加入資料表、 檢視和預存程序所需的安裝應用程式服務結構描述`SqlMembershipProvider`和`SqlRoleProvider`。 這會建立基礎結構，我們需要此數列的教學課程的其餘部分。 在本教學課程中我們將探討使用成員資格架構 (透過`SqlMembershipProvider`) 來建立新的使用者帳戶。 我們會了解如何以程式設計方式和 ASP 透過建立新的使用者。網路的內建適用於 CreateUserWizard 控制項。

除了了解如何建立新的使用者帳戶，我們也需要更新示範網站我們第一次建立中 *<a id="_msoanchor_2"> </a>[的表單驗證概觀](../introduction/an-overview-of-forms-authentication-vb.md)*教學課程和增強的功能 *<a id="_msoanchor_3"> </a>[表單驗證設定和進階主題](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*教學課程。 我們的示範 web 應用程式具有會驗證使用者的認證，硬式編碼使用者名稱/密碼組的登入頁面。 此外，`Global.asax`包含可建立自訂程式碼`IPrincipal`和`IIdentity`已驗證使用者的物件。 我們將會更新登入頁面，來驗證使用者的認證，針對成員資格 framework 和移除主體和身分識別的自訂邏輯。

讓我們開始吧 ！

## <a name="the-forms-authentication-and-membership-checklist"></a>表單驗證和成員資格檢查清單

我們開始使用的成員資格 framework 之前，請讓我們花一點時間檢閱重要的步驟，我們已達到這個點。 當使用成員資格架構與`SqlMembershipProvider`必須在 web 應用程式中實作的成員資格功能之前會先執行下列步驟在表單型驗證案例中，：

1. **啟用表單型驗證。** 如我們所述 *<a id="_msoanchor_4"> </a>[的表單驗證概觀](../introduction/an-overview-of-forms-authentication-vb.md)*，藉由編輯啟用表單驗證`Web.config`和設定`<authentication>`項目的`mode`屬性`Forms`。 使用表單驗證已啟用，每個傳入要求會檢查*表單驗證票證*，其中，如果出現找出要求者。
2. **將應用程式服務結構描述加入至適當的資料庫。** 當使用`SqlMembershipProvider`我們需要安裝應用程式服務結構描述至資料庫。 通常這個結構描述會加入至相同的資料庫所在的應用程式資料模型。  *<a id="_msoanchor_5"> </a>[在 SQL Server 中建立成員資格結構描述](creating-the-membership-schema-in-sql-server-vb.md)*教學課程看使用`aspnet_regsql.exe`工具來完成這項作業。
3. **自訂 Web 應用程式的設定步驟 2 中參考的資料庫。** *在 SQL Server 中建立成員資格結構描述*教學課程示範了兩種方式可設定 web 應用程式，讓`SqlMembershipProvider`會使用在步驟 2 中選取的資料庫： 藉由修改`LocalSqlServer`連接字串名稱。或者，透過將新的註冊提供者加入至成員資格 framework 提供者的清單，以及自訂該新的提供者使用的資料庫從步驟 2。

當建置 web 應用程式使用`SqlMembershipProvider`和表單型驗證，您必須使用之前執行這三個步驟`Membership`類別或 ASP.NET 登入 Web 控制項。 因為我們已經執行下列步驟，在先前的教學課程中，我們已準備好開始使用的成員資格 framework ！

## <a name="step-1-adding-new-aspnet-pages"></a>步驟 1： 加入新的 ASP.NET 網頁

在本教學課程中，下一步的三個我們將會檢查各種成員資格相關的函數和功能。 我們需要一系列的 ASP.NET 頁面實作檢查這些教學課程中的主題。 讓我們來建立這些頁面，然後在站台對應檔`(Web.sitemap)`。

藉由建立新的資料夾中名為專案啟動`Membership`。 接下來，加入至五個新 ASP.NET 網頁`Membership`資料夾中，連結與每個頁面`Site.master`主版頁面。 命名頁面：

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

此時您專案的方案總管 看起來應該類似螢幕擷取畫面中圖 1 所示。


[![五個新的頁面已加入至 [成員資格] 資料夾](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**圖 1**： 五個新頁面已加入至`Membership`資料夾 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image3.png))


每一頁，此時，應有兩個內容控制項，各供一個主版頁面的 ContentPlaceHolders:`MainContent`和`LoginContent`。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

請記得， `LoginContent` ContentPlaceHolder 的預設標記會顯示登入或登出的站台，根據是否已驗證使用者的連結。 與否`Content2`內容控制項，不過，會覆寫主版頁面的預設標記。 如我們所述 *<a id="_msoanchor_6"> </a>[的表單驗證概觀](../introduction/an-overview-of-forms-authentication-vb.md)*教學課程中，這非常有用，我們不想在左側的資料行中顯示登入相關選項的頁面中。

對這些五頁，不過，我們想要顯示在主版頁面的預設標記`LoginContent`ContentPlaceHolder。 因此，移除的宣告式標記`Content2`內容控制項。 之後，請每五個網頁的標記應該包含單一內容控制項。

## <a name="step-2-creating-the-site-map"></a>步驟 2： 建立站台地圖

幾乎連最簡單的網站必須實作某種形式的巡覽使用者介面。 巡覽使用者介面，可能是簡單的站台的不同區段的連結清單。 此外，這些連結可能會分成功能表或樹狀檢視。 網頁開發人員建立巡覽使用者介面是劇本的一半。 我們也會需要某些方法來定義站台的邏輯結構，可維護性和可更新的方式。 當新的頁面會加入或移除現有的頁面時，我們想要能夠更新單一來源站台對應的而且這些修改反映在站台的巡覽使用者介面。

定義站台對應和實作巡覽使用者介面，根據站台對應為這兩項工作可輕鬆地完成這點受惠網站地圖架構，並瀏覽 Web 控制中加入的 ASP.NET 2.0 版。 站台地圖架構可讓開發人員定義站台對應，然後存取透過程式設計的 API ( [ `SiteMap`類別](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx))。 瀏覽 Web 控制項的內建包括[功能表控制項](https://msdn.microsoft.com/en-us/library/bz09dy46.aspx)、 [TreeView 控制項](https://msdn.microsoft.com/en-us/library/3eafky27.aspx)，而[SiteMapPath 控制項](https://msdn.microsoft.com/en-us/library/3eafky27.aspx)。

成員資格和角色的架構，例如網站地圖 framework 建置在之上[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)。 站台地圖提供者類別的工作是產生所使用的記憶體中結構`SiteMap`從永續性資料存放區，例如 XML 檔案或資料庫資料表的類別。 .NET Framework 隨附於從 XML 檔案讀取網站導覽資料的預設站台地圖提供者 ([`XmlSiteMapProvider`](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx))，而且這是我們將在本教學課程中使用的提供者。 針對某些其他的站台地圖提供者實作，請參閱進一步讀數區段在本教學課程結尾處。

預設站台地圖提供者必須要有格式正確 XML 檔案，名為`Web.sitemap`存在的根目錄。 因為我們使用此預設提供者，我們需要加入這類檔案，並以適當的 XML 格式定義站台對應的結構。 若要加入檔案，以滑鼠右鍵按一下方案總管] 中的專案名稱上，選擇 [加入新項目。 從對話方塊中，選擇加入的檔案的類型名為站台地圖`Web.sitemap`。


[![加入名為 Web.sitemap 至專案的根目錄的檔案](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**圖 2**： 將檔案命名為`Web.sitemap`至專案的根目錄 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image6.png))


XML 的站台對應檔會定義為階層的網站結構。 此階層式關聯性中的 XML 檔案的上階透過模型化`<siteMapNode>`項目。 `Web.sitemap`開頭必須是`<siteMap>`有精確的父節點`<siteMapNode>`子系。 這個最上層`<siteMapNode>`元素代表階層的根，且可能有任意數目的下階節點。 每個`<siteMapNode>`元素必須包含`title`屬性，並可選擇性地包含`url`和`description`屬性，和其他項目; 每個非空白`url`屬性必須是唯一。

輸入下列 XML`Web.sitemap`檔案：

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

上述的站台地圖標記定義階層圖 3 所示。


[![站台地圖代表階層式導覽結構](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**圖 3**： 網站導覽代表階層式導覽結構 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>步驟 3： 更新主版頁面加入巡覽使用者介面

ASP.NET 包含梒葯弮 Web 控制項的設計使用者介面的數字。 其中包括功能表、 樹狀檢視，以及 SiteMapPath 控制項。 功能表和 TreeView 控制項分別轉譯網站導覽結構或樹狀目錄中，設定功能表中的，而 sitemappath 可顯示階層連結列會顯示目前的節點以及其祖系造訪。 網站導覽資料可以繫結至其他 Web 控制項使用的 Treeview 的資料，並可透過程式設計方式存取`SiteMap`類別。

由於網站地圖架構和導覽控制項的完整討論超出此教學課程系列的範圍，而不是比花時間讓我們來製作自己的巡覽使用者介面改為借用中使用我 *[在 ASP.NET 2.0 中使用資料](../../data-access/index.md)*教學課程系列，如圖 4 所示，使用中繼器控制項來顯示兩個局部深度分項清單的導覽連結。

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>在左側的資料行中加入兩個層級的連結清單

若要建立此介面，加入下列宣告式標記`Site.master`主版頁面左邊的資料行位置的文字 TODO： 功能表將會移至這裡...目前常駐。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

上述的標記將繫結中繼器控制項，名為`menu`Treeview，來傳回中所定義的對應站台階層`Web.sitemap`。 Treeview 控制項的自[`ShowStartingNode`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)設為的 False，同時也會傳回開頭為首頁節點的下階的站台對應的階層。 中繼器的每個這些節點 （目前只成員資格） 中顯示`<li>`項目。 另一個、 內部中繼器然後會在巢狀的未排序清單中顯示目前節點的子系。

圖 4 顯示上述的標記呈現的輸出與步驟 2 中建立站台對應結構。 中繼器呈現香草未排序的清單的標記。階層式樣式表規則中定義`Styles.css`負責悅耳版面配置。 如上述的標記如何運作的更詳細的描述，請參閱[主版頁面和站台瀏覽](https://asp.net/learn/data-access/tutorial-03-vb.aspx)教學課程。


[![巡覽使用者介面會呈現使用巢狀未排序清單](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**圖 4**: 巡覽使用者介面會呈現使用巢狀未排序清單 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>加入階層連結巡覽

除了左側資料行中的連結清單中，我們也有每個頁面顯示[階層連結](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29)。 階層連結列會快速顯示使用者站台階層中的目前位置中的巡覽使用者介面項目。 SiteMapPath 控制項使用網站地圖 framework 判斷站台對應中的目前頁面的位置，並顯示階層連結列根據這項資訊。

具體來說，新增`<span>`主版頁面的標頭項目`<div>`項目，並設定新`<span>`項目的`class`屬性階層連結。 (`Styles.css`類別包含的階層連結類別的規則。)接下來，加入至這個新的 SiteMapPath`<span>`項目。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

圖 5 顯示 SiteMapPath 輸出造訪時`~/Membership/CreatingUserAccounts.aspx`。


[![階層連結列會顯示目前網頁和網站中的其祖系對應](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**圖 5**： 階層圖會顯示目前頁面和其祖系站台 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>步驟 4： 移除的自訂主體和身分識別邏輯

在 *<a id="_msoanchor_7"> </a>[表單驗證設定和進階主題](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*教學課程中我們可了解如何將已驗證使用者自訂的主體和身分識別物件產生關聯。 我們來建立事件處理常式中的完成這`Global.asax`應用程式的`PostAuthenticateRequest`之後引發的事件`FormsAuthenticationModule`已經驗證使用者。 這個事件處理常式取代`GenericPrincipal`和`FormsIdentity`所加入的物件`FormsAuthenticationModule`與`CustomPrincipal`和`CustomIdentity`我們在該教學課程建立的物件。

在某些情況下，在大部分情況下很有用的主體和身分識別的自訂物件時`GenericPrincipal`和`FormsIdentity`物件就已足夠。 因此，我認為很值得傳回的預設行為。 進行這項變更移除或註解`PostAuthenticateRequest`事件處理常式或藉由刪除`Global.asax`整個檔案。

> [!NOTE]
> 一旦您已標記為註解或移除中的程式碼`Global.asax`，必須將標記為註解中的程式碼`Default.aspx's`將轉換的程式碼後置類別`User.Identity`屬性`CustomIdentity`執行個體。


## <a name="step-5-programmatically-creating-a-new-user"></a>步驟 5： 以程式設計方式建立新的使用者

若要建立新的使用者帳戶，透過使用成員資格 framework`Membership`類別的[`CreateUser`方法](https://msdn.microsoft.com/En-US/library/system.web.security.membership.createuser.aspx)。 這個方法具有輸入參數的使用者名稱、 密碼及其他使用者相關的欄位。 在引動過程，它會委派給已設定的成員資格提供者建立新的使用者帳戶，然後傳回[`MembershipUser`物件](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx)代表剛建立的使用者帳戶。

`CreateUser`方法具有四個多載，各接受不同數目的輸入參數：

- [`CreateUser(username, password)`](https://msdn.microsoft.com/En-US/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/En-US/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/En-US/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/En-US/library/ms152012.aspx)

這些四個多載的差別收集的資訊數量。 第一個多載，例如，需要使用者名稱和密碼為新的使用者帳戶，而第二個也需要使用者的電子郵件地址。

這些多載存在，因為若要建立新的使用者帳戶所需的資訊取決於成員資格提供者的組態設定。 在 *<a id="_msoanchor_8"> </a>[在 SQL Server 中建立成員資格結構描述](creating-the-membership-schema-in-sql-server-vb.md)*教學課程中我們會檢查指定的成員資格提供者組態設定`Web.config`。 表 2 中包含的組態設定的完整清單。

一個這類成員資格提供者組態設定會影響哪些`CreateUser`可能使用多載是`requiresQuestionAndAnswer`設定。 如果`requiresQuestionAndAnswer`設`true`（預設值），然後建立新的使用者帳戶時必須指定安全性問題與解答。 如果使用者需要重設或變更其密碼，之後會使用這項資訊。 特別是，它們會在該時間顯示的安全性問題，而他們必須輸入正確解答才能重設或變更其密碼。 因此，如果`requiresQuestionAndAnswer`設`true`然後呼叫其中前兩個`CreateUser`多載會產生例外狀況，因為遺失的安全性問題和解答。 由於我們的應用程式目前設定為需要的安全性問題和答案，我們必須以程式設計方式建立使用者時，請使用其中一種後者的兩個多載。

為了說明使用`CreateUser`方法，讓我們來建立使用者介面提示輸入使用者輸入其名稱、 密碼、 電子郵件，以及預先定義的安全性問題的回答。 開啟`CreatingUserAccounts.aspx`頁面`Membership`資料夾並將下列 Web 控制項加入至內容控制項：

- 名為文字方塊`Username`
- 名為文字方塊`Password`、 其`TextMode`屬性設定為`Password`
- 名為文字方塊`Email`
- 名稱為的標籤`SecurityQuestion`具有其`Text`清除屬性
- 名為文字方塊`SecurityAnswer`
- 名為按鈕`CreateAccountButton`其`Text`屬性會設定為建立使用者帳戶
- 標籤控制項，名為`CreateAccountResults`具有其`Text`清除屬性

此時您的畫面看起來應該類似螢幕擷取畫面圖 6 所示。


[![將不同的 Web 控制項加入至 CreatingUserAccounts.aspx 頁面](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**圖 6**： 將各種 Web 控制項加入`CreatingUserAccounts.aspx Page`([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image18.png))


`SecurityQuestion`標籤和`SecurityAnswer`文字方塊要顯示的預先定義的安全性問題並收集使用者的回應。 請注意，安全性問題和答案會儲存使用者以使用者為基礎，因此可以允許每個使用者定義自己的安全性問題。 不過，對於 我已涇決定使用通用的安全性問題，也就是這個範例： 什麼是程式偏愛顏色？

若要實作此預先定義的安全性問題，請將常數加入至名為頁面的程式碼後置類別`passwordQuestion`，將其指派的安全性問題。 然後，在`Page_Load`事件處理常式，指派此常數`SecurityQuestion`標籤的`Text`屬性：

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

接下來，建立事件處理常式`CreateAccountButton'`s`Click`事件並加入下列程式碼：

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

`Click`事件處理常式開始定義變數，名為`createStatus`型別的[ `MembershipCreateStatus` ](https://msdn.microsoft.com/En-US/library/system.web.security.membershipcreatestatus.aspx)。 `MembershipCreateStatus`是一種列舉，指出狀態`CreateUser`作業。 例如，如果使用者帳戶建立成功，產生`MembershipCreateStatus`執行個體將會設定為值`Success;`相反地，如果作業失敗，因為已經存在具有相同的使用者名稱的使用者，它就會設定為值`DuplicateUserName`. 在`CreateUser`我們使用多載，我們需要將`MembershipCreateStatus`至方法的執行個體。 此參數設定為適當的值內`CreateUser`方法，所以我們可以檢查它的值來判斷是否已成功建立使用者帳戶在方法呼叫之後。

在呼叫`CreateUser`，並傳入`createStatus`、`Select Case`陳述式用來輸出指派給的值而定有適當錯誤訊息`createStatus`。 圖 7 顯示輸出時已成功建立新的使用者。 未建立使用者帳戶時，數字 8 和 9 顯示輸出。 圖 8 中造訪者輸入五個字母密碼不符合成員資格提供者的組態設定中的密碼強度需求。 圖 9 中造訪者嘗試建立使用者帳戶使用現有的使用者名稱 （圖 7 中建立的一個）。


[![新的使用者帳戶已建立成功](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**圖 7**: 新的使用者帳戶已成功建立 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image21.png))


[![因為提供的密碼太弱，無法建立使用者帳戶](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**圖 8**： 因為提供的密碼太弱，不會建立使用者帳戶 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image24.png))


[![使用者帳戶是不建立是因為使用者名稱是使用中](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**圖 9**: 使用者帳戶是不建立是因為使用者名稱已在使用 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> 您可能想知道如何判斷成功或失敗時使用的前兩個的其中一種`CreateUser`方法多載，兩者都不具有類型參數的`MembershipCreateStatus`。 這些前兩個多載會擲回[`MembershipCreateUserException`例外狀況](https://msdn.microsoft.com/en-us/library/system.web.security.membershipcreateuserexception.aspx)時失敗，其中包括[`StatusCode`屬性](https://msdn.microsoft.com/en-us/library/system.web.security.membershipcreateuserexception.statuscode.aspx)型別的`MembershipCreateStatus`。


建立幾個使用者帳戶之後，請確認帳戶都由列出的內容`aspnet_Users`和`aspnet_Membership`中的資料表`SecurityTutorials.mdf`資料庫。 如圖 10 所示，我已經加入兩位使用者透過`CreatingUserAccounts.aspx`頁面： Tito 和 Bruce。


[![成員資格使用者存放區中有兩位使用者： Tito 和 Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**圖 10**： 成員資格使用者存放區中有兩位使用者： Tito 和 Bruce ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image30.png))


雖然成員資格使用者存放區現在包括 Bruce 和 Tito 的帳戶資訊，我們還沒有實作允許 Bruce 或 Tito 登入站台的功能。 目前，`Login.aspx`會驗證使用者的認證會根據使用者名稱/密碼組的硬式編碼組*不*驗證針對成員資格 framework 提供的認證。 現在查看中的新使用者帳戶`aspnet_Users`和`aspnet_Membership`資料表必須已足夠。 在下一個教學課程中，  *<a id="_msoanchor_9"> </a>[驗證使用者認證對成員資格使用者儲存](validating-user-credentials-against-the-membership-user-store-vb.md)*，我們將會更新成員資格儲存區對其進行驗證的登入頁面。

> [!NOTE]
> 如果看不到任何使用者在您`SecurityTutorials.mdf`資料庫，則可能是因為 web 應用程式使用預設的成員資格提供者， `AspNetSqlMembershipProvider`，它會使用`ASPNETDB.mdf`做為其使用者存放區資料庫。 若要判斷是否為問題，按一下 [方案總管] 的 [重新整理] 按鈕。 如果資料庫名為`ASPNETDB.mdf`已新增至`App_Data`資料夾中，這是此問題。 返回步驟 4  *<a id="_msoanchor_10"> </a>[在 SQL Server 中建立成員資格結構描述](creating-the-membership-schema-in-sql-server-vb.md)*教學課程，如需如何適當地設定成員資格提供者的指示。


在大部分建立使用者帳戶案例，請造訪者會看到的一些介面輸入其使用者名稱、 密碼、 電子郵件和其他重要資訊，此時建立新的帳戶。 在此步驟中我們探討了以手動方式建立這種介面，然後可了解如何使用`Membership.CreateUser`方法來以程式設計方式加入新的使用者帳戶會根據使用者的輸入。 不過，我們的程式碼，只建立新的使用者帳戶。 它並未執行任何待處理的動作，如登入使用者剛建立的使用者帳戶，站台，或確認電子郵件傳送給使用者。 這些額外的步驟會需要額外的程式碼中的按鈕`Click`事件處理常式。

ASP.NET 隨附適用於 CreateUserWizard 控制項，其設計為處理使用者帳戶建立程序，從轉譯的使用者介面建立新的使用者帳戶，來建立成員資格 framework 中的帳戶和執行後的帳戶建立工作，例如傳送確認電子郵件及剛建立的使用者登入網站。 使用適用於 CreateUserWizard 控制項很簡單，只適用於 CreateUserWizard 控制項從 [工具箱] 拖曳到頁面中，拖然後設定一些屬性。 在大部分情況下，您不需要撰寫一行程式碼。 我們將探討這個各式各樣控制項詳細步驟 6 中。

如果只建立新的使用者帳戶會透過一般建立帳戶網頁，也不太可能，您將需要撰寫程式碼使用`CreateUser`方法，為適用於 CreateUserWizard 控制項可能符合您的需求。 不過，`CreateUser`方法很方便的案例中，您需要高度自訂的 建立帳戶使用者經驗的位置，或當您需要以程式設計方式建立新的使用者帳戶，透過替代的介面。 比方說，您可能會有允許使用者上載 XML 檔案，其中包含從其他應用程式的使用者資訊頁面。 頁面可能會的剖析內容的上傳的 XML 檔案，並在 XML 中建立新的帳戶，表示每個使用者，藉由呼叫`CreateUser`方法。

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>步驟 6： 使用適用於 CreateUserWizard 控制項建立新的使用者

ASP.NET 隨附之登入 Web 控制項的數字。 這些控制項可協助多的常見使用者帳戶和登入相關案例。 [適用於 CreateUserWizard 控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)是設計用來呈現使用者介面，將新的使用者帳戶加入的成員資格 framework 一個這類控制項。

如同許多其他的登入相關的 Web 控制項，適用於 CreateUserWizard 可用而不需要撰寫一行程式碼。 它直覺的方式提供使用者介面，根據成員資格提供者的組態設定和在內部呼叫`Membership`類別的`CreateUser`之後使用者輸入所需的資訊，並按一下 建立使用者按鈕的方法。 適用於 CreateUserWizard 控制項是極可自訂的。 有主機的帳戶建立程序的不同階段引發的事件。 我們可以建立事件處理常式，如有需要將自訂邏輯插入帳戶建立工作流程。 此外，適用於 CreateUserWizard 的外觀會非常大的彈性。 有許多屬性，定義預設介面; 的外觀必要時，控制項可以轉換成範本，或可能加入其他使用者註冊步驟。

讓我們開始使用適用於 CreateUserWizard 控制項的預設介面和行為會查看。 然後，我們將探討如何自訂透過控制項的屬性和事件的外觀。

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>檢查適用於 CreateUserWizard 的預設介面和行為

返回`CreatingUserAccounts.aspx`頁面`Membership`資料夾中，切換至 設計 或 分割模式中，，然後再將加入頁面頂端的 適用於 CreateUserWizard 控制項。 適用於 CreateUserWizard 控制項是必填欄位，在 [工具箱] 的登入控制項 」 一節。 加入控制項之後, 設定其`ID`屬性`RegisterUser`。 圖 11 顯示中的螢幕擷取畫面，因為適用於 CreateUserWizard 會轉譯文字方塊的新使用者的使用者名稱、 密碼、 電子郵件地址和安全性問題與解答的介面。


[![適用於 CreateUserWizard 控制項呈現泛型建立使用者介面](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**圖 11**： 適用於 CreateUserWizard 控制項呈現泛型建立使用者介面 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image33.png))


讓我們花一點時間來比較適用於 CreateUserWizard 控制項與步驟 5 中建立的介面產生的預設使用者介面。 首先，適用於 CreateUserWizard 控制項允許的造訪者指定安全性問題和答案，而我們手動建立使用介面的預先定義的安全性問題。 適用於 CreateUserWizard 控制項的介面也會包含驗證控制項，而我們尚未在我們的介面表單欄位上實作驗證。 適用於 CreateUserWizard 控制項介面包括 [確認密碼] 文字方塊 （以及以確保輸入密碼的文字，並比較密碼文字方塊相等 CompareValidator)。

有趣的是適用於 CreateUserWizard 控制項在呈現其使用者介面時，會參照成員資格提供者的組態設定。 例如，安全性問題和解答文字方塊才顯示`requiresQuestionAndAnswer`設為 True。 同樣地，適用於 CreateUserWizard 會自動加入 RegularExpressionValidator 控制項，以確保密碼強度需求均符合，並設定其`ErrorMessage`和`ValidationExpression`屬性根據`minRequiredPasswordLength`， `minRequiredNonalphanumericCharacters`，和`passwordStrengthRegularExpression`組態設定。

適用於 CreateUserWizard 控制項，正如其名，衍生自[精靈控制項](https://msdn.microsoft.com/en-us/library/s2etd1ek.aspx)。 精靈控制項的設計來提供介面來完成多步驟工作。 精靈控制項可能包含任意數量的`WizardSteps`、 每一個都是定義 HTML 的範本與 Web 控制項的該步驟。 精靈控制項最初顯示的第一個`WizardStep`，以及瀏覽控制項，可讓使用者從一個步驟繼續執行下一步，或若要返回上一個步驟。

圖 11 中的宣告式標記所示，適用於 CreateUserWizard 控制項的預設介面包含兩個`WizardStep`s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? 呈現的介面來收集資訊，以建立新的使用者帳戶。 這是顯示在圖 11 中的步驟。
- [`CompleteWizardStep`](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.completewizardstep.aspx) ? 轉譯訊息，指出已成功建立帳戶。

可以修改適用於 CreateUserWizard 的外觀和行為，透過將其中一個步驟轉換成範本，或是加入您自己`WizardStep`s。 我們將探討加入`WizardStep`中登錄介面*儲存額外的使用者資訊*教學課程。

我們來看看適用於 CreateUserWizard 控制項中的動作。 請瀏覽`CreatingUserAccounts.aspx`透過瀏覽器的頁面。 適用於 CreateUserWizard 的介面中輸入一些無效的值來啟動。 請嘗試輸入密碼不符合密碼強度需求，或將 [使用者名稱] 文字方塊保留空白。 適用於 CreateUserWizard 會顯示適當的錯誤訊息。 當您嘗試使用不足以強式密碼建立使用者時，圖 12 顯示輸出。


[![適用於 CreateUserWizard 自動插入驗證控制項](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**圖 12**: 適用於 CreateUserWizard 自動插入驗證控制項 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image36.png))


接下來，在適用於 CreateUserWizard 輸入適當的值，然後按一下 [建立使用者] 按鈕。 假設已輸入必要的欄位，而且密碼的強度足夠，適用於 CreateUserWizard 會建立新的使用者帳戶，透過 「 成員資格 」 架構，並且顯示`CompleteWizardStep`（請參閱圖 13） 的介面。 在幕後呼叫適用於 CreateUserWizard`Membership.CreateUser`方法，如同我們在步驟 5。


[![新的使用者帳戶已成功建立](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**圖 13**: 新的使用者帳戶已成功建立 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> 如圖 13 所示，`CompleteWizardStep`的介面包括 [繼續] 按鈕。 不過，此時按一下只會執行回傳時，在相同頁面上離開訪客。 在自訂適用於 CreateUserWizard 的外觀和其內容到行為區段我們將探討如何使用此按鈕，將傳送的造訪者`Default.aspx`（或某些其他頁面）。


建立新的使用者帳戶後，返回 Visual Studio，並檢查`aspnet_Users`和`aspnet_Membership`資料表像我們在圖 10 中，確認已成功建立帳戶。

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>自訂適用於 CreateUserWizard 的行為和外觀，透過它的屬性

中有許多種，透過屬性，可自訂適用於 CreateUserWizard `WizardStep` s 和事件處理常式。 這一節我們將探討如何自訂控制項的外觀，透過它的屬性。擴充控制項的行為，透過事件處理常式會查看下一節。

可以透過其內容的眾多自訂幾乎所有適用於 CreateUserWizard 控制項的預設使用者介面中顯示的文字。 例如，文字方塊的左邊顯示的使用者名稱、 密碼、 確認密碼、 電子郵件、 安全性問題和安全性解答標籤可以透過自訂[ `UserNameLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx)， [ `PasswordLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx)， [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx)， [ `EmailLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx)， [ `QuestionLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)，和[ `AnswerLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)屬性，分別。 同樣地，有指定中的 Create User 及繼續按鈕的文字屬性`CreateUserWizardStep`和`CompleteWizardStep`，也如同這些按鈕會呈現為按鈕、 LinkButtons 或 ImageButtons。

可透過主機的樣式屬性來設定色彩、 框線、 字型和其他視覺化項目。 適用於 CreateUserWizard 控制項本身有共通的 Web 控制項樣式屬性- `BackColor`， `BorderStyle`， `CssClass`，`Font`等等-而且有很多的樣式屬性定義的特定區段的外觀適用於 CreateUserWizard 的介面。 [ `TextBoxStyle`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx)，比方說，定義中文字方塊的樣式`CreateUserWizardStep`，雖然[`TitleTextStyle`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx)定義 （登入您新標題的樣式帳戶）。

除了外觀相關的屬性，還有一些會影響適用於 CreateUserWizard 控制項的行為的屬性。 [ `DisplayCancelButton`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx)，如果設為 True，顯示 （預設值為 False） 的 [建立使用者] 按鈕旁邊的 [取消] 按鈕。 如果顯示 [取消] 按鈕時，請務必也設定[`CancelDestinationPageUrl`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)，以指定使用者按一下 [取消] 5d; 之後，會傳送至頁面。 如前一節中的 [繼續] 按鈕中所述`CompleteWizardStep`的介面，導致回傳，但在相同頁面上離開訪客。 若要傳送至其他網頁訪客按一下 [繼續] 按鈕之後，您只需要指定中的 URL [ `ContinueDestinationPageUrl`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)。

讓我們更新`RegisterUser`適用於 CreateUserWizard 控制項顯示取消按鈕，並傳送至訪客`Default.aspx`當按下 取消 5d; 或繼續 按鈕。 若要達成此目的，設定`DisplayCancelButton`屬性設為 True，同時兩者皆`CancelDestinationPageUrl`和`ContinueDestinationPageUrl`屬性 ~ / Default.aspx。 圖 14 顯示透過瀏覽器檢視時更新適用於 CreateUserWizard。


[![CreateUserWizardStep 包含 [取消] 5d; 按鈕](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**圖 14**:`CreateUserWizardStep`包含 [取消] 按鈕 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image42.png))


當訪客輸入使用者名稱、 密碼、 電子郵件地址和安全性問題和答案，並按一下 建立使用者時，會建立新的使用者帳戶和訪客，會建立新使用者的身分登入。 假設瀏覽頁面的人員正在自行建立新的帳戶，這可能是所要的行為。 不過，您可能想要允許系統管理員新增使用者帳戶。 在此情況下，會建立使用者帳戶，但系統管理員會仍保持登入系統管理員身分 （而不是新建立的帳戶）。 此行為可以修改透過布林[`LoginCreatedUser`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)。

成員資格 framework 中的使用者帳戶包含已核准的旗標。未核准的使用者將無法登入網站。 根據預設，新建立的帳戶已標示為核准，讓使用者立即登入網站。 不過，有標示為 未核准的新使用者帳戶會盡可能。 也許您想要手動核准新的使用者，才可以登入; 系統管理員或者，您想要檢查在註冊期間取得輸入的電子郵件地址有效，才允許使用者登入。 任何大小寫，可能是您可以藉由設定適用於 CreateUserWizard 控制項的標記為 未核准的新建立的使用者帳戶[`DisableCreatedUser`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)為 True （預設值為 False）。

附註的其他行為相關的屬性包含`AutoGeneratePassword`和`MailDefinition`。 如果[`AutoGeneratePassword`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx)設為 True，`CreateUserWizardStep`不會顯示密碼和確認密碼文字方塊中; 相反地，新建立之使用者的密碼自動產生使用`Membership`類別的[`GeneratePassword`方法](https://msdn.microsoft.com/en-us/library/system.web.security.membership.generatepassword.aspx)。 `GeneratePassword`方法會建構具有足夠數目的非英數字元，以滿足設定的密碼強度需求及指定長度的密碼。

[ `MailDefinition`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)如果您想要在帳戶建立程序期間指定的電子郵件地址傳送電子郵件就很有用。 `MailDefinition`屬性包含一系列的子屬性，定義建構的電子郵件訊息的相關資訊。 這些子屬性包括選項，例如`Subject`， `Priority`， `IsBodyHtml`， `From`， `CC`，和`BodyFileName`。 [ `BodyFileName`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)指向的文字或 HTML 檔案，其中包含電子郵件訊息的主體。 本文支援兩個預先定義的預留位置：`<%UserName%>`和`<%Password%>`。 如果出現在這些預留位置`BodyFileName`檔案中，將會取代剛建立的使用者名稱和密碼。

> [!NOTE]
> `CreateUserWizard`控制項的`MailDefinition`屬性只會指定在建立新的帳戶時，會傳送電子郵件訊息詳細資料。 它不包含任何實際如何傳送電子郵件訊息上的詳細資料 （也就是是否使用 SMTP 伺服器或郵件放置目錄，任何驗證資訊，以及等等）。 這些低層級的詳細資料，必須定義在`<system.net>`一節中`Web.config`。 這些組態設定和在一般情況下傳送電子郵件從 ASP.NET 2.0 的詳細資訊，請參閱[常見問題集在 SystemNetMail.com](http://www.systemnetmail.com/)和我的文件： [ASP.NET 2.0 中的 傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)。


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>擴充適用於 CreateUserWizard 的行為使用事件處理常式

適用於 CreateUserWizard 控制項在其工作流程期間引發的事件數目。 例如，訪客輸入其使用者名稱、 密碼和其他相關資訊，並按下 [建立使用者] 按鈕後，適用於 CreateUserWizard 控制項引發其[`CreatingUser`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)。 如果在建立過程中，問題[`CreateUserError`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)引發; 不過，如果已成功建立使用者，然後在[`CreatedUser`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)，就會引發。 取得引發的其他適用於 CreateUserWizard 控制項事件，但這些是三個最密切關聯。

在某些情況下，我們可能要挖掘適用於 CreateUserWizard 工作流程，我們可以透過建立適當的事件的事件處理常式中執行。 為了說明這點，讓我們來增強`RegisterUser`適用於 CreateUserWizard 控制項，以包含某些自訂驗證使用者名稱和密碼。 特別是，讓我們來增強我們適用於 CreateUserWizard 以便使用者名稱不能包含開頭或尾端空格及使用者名稱不能出現在任何位置的密碼。 簡單地說，我們想要防止別人建立 「 Scott"，例如使用者名稱或使用者名稱/密碼組合 Scott 和 Scott.1234 等。

若要完成此我們將建立的事件處理常式`CreatingUser`執行我們的額外的驗證檢查的事件。 如果所提供的資料無效。 我們需要取消建立處理程序。 我們也要將標籤 Web 控制項加入頁面以顯示訊息，說明使用者名稱或密碼不正確。 將標籤控制項適用於 CreateUserWizard 控制項下方，設定來開始其`ID`屬性`InvalidUserNameOrPasswordMessage`及其`ForeColor`屬性`Red`。 清除其`Text`屬性並設定其`EnableViewState`和`Visible`屬性為 False。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

接下來，建立適用於 CreateUserWizard 控制項的事件處理常式`CreatingUser`事件。 若要建立事件處理常式，設計工具中選取的控制項，然後移至 [屬性] 視窗。 從該處閃電圖示上按一下，然後按兩下適當的事件建立事件處理常式。

將下列程式碼加入至 `CreatingUser` 事件處理常式：

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

請注意，使用者名稱和密碼輸入適用於 CreateUserWizard 控制項都可透過其[ `UserName` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.username.aspx)和[`Password`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.password.aspx)分別。 上述的事件處理常式中使用這些屬性時，我們判斷提供的使用者名稱是否包含開頭或尾端空格，且是否密碼中找到使用者名稱。 如果符合任一條件，則錯誤訊息，都會在`InvalidUserNameOrPasswordMessage`標籤和事件處理常式的`e.Cancel`屬性設定為`True`。 如果`e.Cancel`設`True`，適用於 CreateUserWizard short-circuits 其工作流程中，有效地取消 使用者帳戶建立程序。

圖 15 所示的螢幕擷取畫面`CreatingUserAccounts.aspx`當使用者輸入使用者名稱的開頭空白。


[![不允許有前置或尾端空格的使用者名稱](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**圖 15**： 不允許有前置或尾端空格的使用者名稱 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> 我們會看到使用適用於 CreateUserWizard 控制項的範例`CreatedUser`中的事件 *<a id="_msoanchor_11"> </a>[儲存額外的使用者資訊](storing-additional-user-information-vb.md)*教學課程。


## <a name="summary"></a>總結

`Membership`類別的`CreateUser`方法會建立新的使用者帳戶的成員資格 framework 中。 它會委派設定的成員資格提供者的呼叫。 如果是`SqlMembershipProvider`、`CreateUser`方法加入至資料錄`aspnet_Users`和`aspnet_Membership`資料庫資料表。

雖然可以建立新的使用者帳戶，以程式設計方式 （如我們所見在步驟 5 中），以更快且更容易的方法是使用適用於 CreateUserWizard 控制項。 這個控制項轉譯來收集使用者資訊，並建立新的使用者成員資格 framework 中的多步驟的使用者介面。 基本上，此控制項會使用相同`Membership.CreateUser`方法，如步驟 5，但該控制項中檢查建立使用者介面，驗證控制項和使用者帳戶建立錯誤回應，而不需要撰寫的程式碼。

現在我們來建立新的使用者帳戶具有的功能。 不過，登入頁面仍然會比對驗證我們指定第二個教學課程中，回這些硬式編碼的認證。 在<a id="_msoanchor_12"> </a>[下一個教學課程](validating-user-credentials-against-the-membership-user-store-vb.md)我們將會更新`Login.aspx`來驗證使用者的所提供的認證針對成員資格 framework。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [`CreateUser`技術文件](https://msdn.microsoft.com/En-US/library/system.web.security.membershipprovider.createuser.aspx)
- [適用於 CreateUserWizard 控制項概觀](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [建立檔案系統為基礎的站台對應提供者](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [建立與 ASP.NET 2.0 精靈控制項的逐步使用者介面](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [檢查 ASP.NET 2.0 的站台瀏覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [主版頁面和站台瀏覽](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [您已經等待的 SQL 站台地圖提供者](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com)。

>[!div class="step-by-step"]
[上一頁](creating-the-membership-schema-in-sql-server-vb.md)
[下一頁](validating-user-credentials-against-the-membership-user-store-vb.md)
