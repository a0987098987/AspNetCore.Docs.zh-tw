---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: 建立使用者帳戶 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將探討使用 （透過 SqlMembershipProvider) 的成員資格架構來建立新的使用者帳戶。 我們將了解如何建立新我們...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: fe5e55df3fa9f65a94199c2064a785255f231537
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815339"
---
<a name="creating-user-accounts-vb"></a>建立使用者帳戶 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip)或[下載 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> 在本教學課程中，我們將探討使用 （透過 SqlMembershipProvider) 的成員資格架構來建立新的使用者帳戶。 我們將了解如何以程式設計方式及透過 ASP，建立新的使用者。NET 的內建的 CreateUserWizard 控制項。


## <a name="introduction"></a>簡介

在  <a id="_msoanchor_1"> </a>[前述教學課程](creating-the-membership-schema-in-sql-server-vb.md)我們在資料庫中，而且在加入資料表、 檢視、 預存程序所需的安裝應用程式服務結構描述`SqlMembershipProvider`和`SqlRoleProvider`。 這會建立我們需要在這一系列的教學課程的其餘部分的基礎結構。 在本教學課程中我們將探討使用成員資格架構 (透過`SqlMembershipProvider`) 來建立新的使用者帳戶。 我們將了解如何以程式設計方式及透過 ASP，建立新的使用者。NET 的內建的 CreateUserWizard 控制項。

除了了解如何建立新的使用者帳戶，我們也必須更新我們第一次建立中的示範網站 *<a id="_msoanchor_2"> </a>[的表單驗證概觀](../introduction/an-overview-of-forms-authentication-vb.md)* 教學課程，然後增強 *<a id="_msoanchor_3"> </a>[表單驗證組態和進階主題](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* 教學課程。 我們的示範 web 應用程式已驗證使用者的認證，對硬式編碼的使用者名稱/密碼組的登入頁面。 此外，`Global.asax`包含建立自訂的程式碼`IPrincipal`和`IIdentity`已驗證使用者的物件。 我們將會更新登入頁面，來驗證使用者的認證，對成員資格架構和移除主體和身分識別的自訂邏輯。

讓我們開始吧 ！

## <a name="the-forms-authentication-and-membership-checklist"></a>表單驗證和成員資格檢查清單

我們開始使用成員資格架構之前，讓我們花一點時間檢閱重要的步驟，我們已到達此點。 使用成員資格架構時`SqlMembershipProvider`在表單型驗證案例中，必須在 web 應用程式中實作成員資格功能之前要執行下列步驟：

1. **啟用表單型驗證。** 如我們所述 *<a id="_msoanchor_4"> </a>[的表單驗證概觀](../introduction/an-overview-of-forms-authentication-vb.md)*，藉由編輯啟用表單驗證`Web.config`和設定`<authentication>`項目的`mode`屬性設定為`Forms`。 使用啟用表單驗證，每個傳入要求會檢查*表單驗證票證*，其中，如果有的話，識別要求者。
2. **將應用程式服務結構描述新增至適當的資料庫。** 當使用`SqlMembershipProvider`我們需要安裝應用程式服務結構描述的資料庫。 通常這個結構描述加入至相同保存應用程式的資料模型的資料庫。 *<a id="_msoanchor_5"> </a> [SQL Server 中建立成員資格結構描述](creating-the-membership-schema-in-sql-server-vb.md)* 教學課程會探討使用`aspnet_regsql.exe`工具來完成這項作業。
3. **自訂 Web 應用程式的設定，以從步驟 2 中參考的資料庫。** *在 SQL Server 中建立成員資格結構描述*教學課程示範了兩種方式可以設定 web 應用程式，以便`SqlMembershipProvider`會使用在步驟 2 中所選取的資料庫： 藉由修改`LocalSqlServer`連接字串名稱;或將新註冊的提供者新增至成員資格架構提供者的清單，並自訂該新的提供者，以使用資料庫。 請從步驟 2。

當建置 web 應用程式，使用`SqlMembershipProvider`表單型驗證，您必須使用之前執行這三個步驟`Membership`類別或 ASP.NET 登入 Web 控制項。 因為我們已經執行下列步驟，在先前的教學課程中，我們已準備好開始使用成員資格架構 ！

## <a name="step-1-adding-new-aspnet-pages"></a>步驟 1： 加入新的 ASP.NET 網頁

在本教學課程和下列三個我們將檢查各種成員資格相關的函式和功能。 我們需要一系列的 ASP.NET 頁面，來實作檢查在這些教學課程的主題。 讓我們來建立這些頁面，然後網站地圖檔案`(Web.sitemap)`。

藉由建立新的資料夾中名為專案啟動`Membership`。 接下來，新增五個以新的 ASP.NET 網頁`Membership`資料夾中，連結每個頁面`Site.master`主版頁面。 名稱的頁面：

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

此時您專案的方案總管] 看起來應該類似螢幕擷取畫面的 [圖 1 所示。


[![五個新的頁面已新增至 [成員資格] 資料夾](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**圖 1**： 五個新頁面已加入至`Membership`資料夾 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image3.png))


每個頁面，到目前為止，有兩個內容控制項，一個用於每個主版頁面的 ContentPlaceHolders:`MainContent`和`LoginContent`。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

請記得， `LoginContent` ContentPlaceHolder 的預設標記會顯示登入或登出的站台，根據使用者是否已驗證的連結。 是否存在`Content2`內容控制項，不過，會覆寫的主版頁面的預設標記。 如我們所述 *<a id="_msoanchor_6"> </a>[的表單驗證概觀](../introduction/an-overview-of-forms-authentication-vb.md)* 教學課程中，使用此方法中，我們不想顯示在左側的資料行中的登入相關選項的頁面。

下列五個的頁面，不過，我們想要顯示的主版頁面的預設標記`LoginContent`ContentPlaceHolder。 因此，移除的宣告式標記`Content2`內容控制項。 完成後，每五個的頁面標記應包含一個內容控制項。

## <a name="step-2-creating-the-site-map"></a>步驟 2： 建立站台對應

所有但最簡單的網站必須實作某種形式的巡覽使用者介面。 瀏覽使用者介面可能是簡單的站台的不同區段的連結清單。 或者，這些連結可能會排列在功能表或樹狀檢視中。 身為頁面開發人員，建立巡覽使用者介面是故事的一半。 我們也需要一些方法可維護且可更新的方式定義站台的邏輯結構。 當新的頁面會加入或移除現有的頁面，我們想要能夠更新單一來源站台對應-並有這些修改會反映在站台的巡覽使用者介面。

-定義站台對應，以及實作巡覽使用者介面，根據站台對應-這兩項工作皆可輕鬆地完成感謝網站地圖架構，並瀏覽 Web 控制已新增至的 ASP.NET 2.0 版。 站台對應架構可讓開發人員定義站台對應，然後透過程式設計的 API 存取它 ( [ `SiteMap`類別](https://msdn.microsoft.com/library/system.web.sitemap.aspx))。 瀏覽 Web 控制項的內建包括[Menu 控制項](https://msdn.microsoft.com/library/bz09dy46.aspx)，則[TreeView 控制項](https://msdn.microsoft.com/library/3eafky27.aspx)，而[SiteMapPath 控制項](https://msdn.microsoft.com/library/3eafky27.aspx)。

成員資格與角色架構，例如站台對應 framework 建置之上[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)。 網站導覽提供者類別的工作是產生所使用的記憶體中結構`SiteMap`從永續性資料存放區，例如 XML 檔案或資料庫資料表的類別。 .NET Framework 隨附於從 XML 檔案讀取網站導覽資料的預設網站導覽提供者 ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx))，這是我們將在本教學課程中使用的提供者。 針對某些替代的網站導覽提供者實作，請參閱進一步閱讀資料區段在本教學課程結尾處。

預設的網站導覽提供者必須要有格式正確之 XML 檔案，名為`Web.sitemap`存在的根目錄。 因為我們要使用此預設提供者，我們需要加入這類檔案，並在適當的 XML 格式來定義站台對應的結構。 若要新增的檔案，以滑鼠右鍵按一下方案總管] 中的專案名稱，並選擇 [加入新項目。 從對話方塊中，選擇 新增類型名為的網站地圖檔案`Web.sitemap`。


[![新增名為專案的根目錄的 Web.sitemap 檔案](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**圖 2**： 將檔案命名為`Web.sitemap`專案的根目錄 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image6.png))


XML 網站地圖檔案會定義網站的結構為階層。 此階層式關聯性模型的 XML 檔案的上階透過`<siteMapNode>`項目。 `Web.sitemap`必須以開頭`<siteMap>`有精確的父節點`<siteMapNode>`子系。 此最上層`<siteMapNode>`代表階層的根項目，而且可能會有任意數目的子系節點。 每個`<siteMapNode>`元素必須包含`title`屬性，並可選擇性地包含`url`並`description`屬性，其他項目; 每個非空白`url`屬性必須是唯一。

輸入下列 XML 插入`Web.sitemap`檔案：

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

上述的站台地圖標記會定義 [圖 3] 所示的階層。


[![站台對應都代表階層式導覽結構](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**圖 3**: 站台對應都代表階層式導覽結構 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>步驟 3： 更新主版頁面加入巡覽使用者介面

ASP.NET 包含多個瀏覽相關 Web 控制項來設計使用者介面。 這些包括功能表、 樹狀檢視中和 SiteMapPath 控制項。 功能表與 TreeView 控制項分別呈現功能表或樹狀目錄中中的網站導覽結構，而 SiteMapPath 顯示階層連結列會顯示目前的節點以及其祖系所造訪。 網站導覽資料可以與其他使用 Treeview Web 控制項的資料繫結，可透過以程式設計方式存取`SiteMap`類別。

因為站台對應架構，並瀏覽控制項的完整討論已超出本教學課程系列的範圍，而不必花時間製作自己的巡覽使用者介面，讓我們改為借用中使用我 *[使用 ASP.NET 2.0 中的資料](../../data-access/index.md)* 教學課程系列，它會使用重複項控制項顯示深的兩個項目符號清單的導覽連結，如 圖 4 所示。

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>左側的資料行中加入兩個層級清單的連結

若要建立此介面，加入下列宣告式標記`Site.master`主版頁面的左側資料行其中文字 TODO： 功能表放在這裡...目前所在。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

上述標記會繫結的重複項控制項，名為`menu`至 SiteMapDataSource，它會傳回中所定義的對應站台階層`Web.sitemap`。 因為 SiteMapDataSource 控制項[`ShowStartingNode`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)設為 False，就會開始傳回網站地圖的階層從 [首頁] 節點的下階。 Repeater 會顯示這些節點 （目前只成員資格） 的每個在`<li>`項目。 另一個，內部的重複項則會顯示在巢狀的排序清單中的目前節點的子系。

圖 4 顯示上述的標記與我們在步驟 2 中建立的網站導覽結構轉譯的輸出。 中繼器呈現 vanilla 的未排序的清單的標記;中所定義的階層式樣式表規則`Styles.css`負責悅耳且具專業水準舒適的版面配置。 上述標記的運作方式的更詳細的描述，請參閱[主版頁面與網站導覽](https://asp.net/learn/data-access/tutorial-03-vb.aspx)教學課程。


[![瀏覽的使用者介面會轉譯使用巢狀未排序清單](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**圖 4**: 瀏覽的使用者介面會轉譯使用巢狀未排序清單 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>加入階層連結巡覽

除了左側資料行中的連結清單中，我們還提供每個頁面顯示[軌跡](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29)。 階層連結列會快速地顯示使用者站台階層中的目前位置中的巡覽使用者介面項目。 SiteMapPath 控制項來判斷目前網頁的網站導覽中的位置使用的站台對應架構，並顯示這項資訊為基礎的階層連結列。

具體來說，新增`<span>`項目至主版頁面的標頭`<div>`項目，並設定新`<span>`項目的`class`屬性階層連結。 (`Styles.css`類別包含的階層連結類別的規則。)接下來，新增到這個新的 SiteMapPath`<span>`項目。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

圖 5 瀏覽時顯示輸出的 SiteMapPath `~/Membership/CreatingUserAccounts.aspx`。


[![階層連結會顯示目前的頁面，並在站台及其上的階對應](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**圖 5**： 階層連結可顯示目前頁面和其祖系網站導覽中 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>步驟 4： 移除的自訂主體和身分識別邏輯

在   *<a id="_msoanchor_7"> </a>[表單驗證組態和進階主題](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* 教學課程中我們了解如何將通過驗證的使用者自訂的 principal 和 identity 物件產生關聯。 我們來建立事件處理常式中的完成這`Global.asax`的應用程式`PostAuthenticateRequest`之後所引發的事件`FormsAuthenticationModule`已驗證使用者。 這個事件處理常式取代`GenericPrincipal`並`FormsIdentity`所加入的物件`FormsAuthenticationModule`具有`CustomPrincipal`和`CustomIdentity`我們在本教學課程中建立的物件。

雖然自訂的 principal 和 identity 物件會在某些情況下，在大部分情況下有用`GenericPrincipal`和`FormsIdentity`物件就已足夠。 因此，我認為值得討論要傳回的預設行為。 進行這項變更，藉由移除，或標記為註解`PostAuthenticateRequest`事件處理常式或藉由刪除`Global.asax`整個檔案。

> [!NOTE]
> 一旦您已標記為註解或移除中的程式碼`Global.asax`，您必須標記為註解中的程式碼`Default.aspx's`轉換的程式碼後置類別`User.Identity`屬性設`CustomIdentity`執行個體。


## <a name="step-5-programmatically-creating-a-new-user"></a>步驟 5： 以程式設計方式建立新的使用者

若要建立新的使用者帳戶，透過使用成員資格 framework`Membership`類別的[`CreateUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)。 這個方法具有輸入參數的使用者名稱、 密碼和其他使用者相關的欄位。 一旦叫用，它會將新的使用者帳戶建立委派給已設定的成員資格提供者，然後傳回[`MembershipUser`物件](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)代表剛建立的使用者帳戶。

`CreateUser`方法有四個多載，每個接受不同數目的輸入參數：

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

這些四個多載的差別收集的資訊數量。 第一個多載，例如，需要使用者名稱和密碼為新的使用者帳戶，而第二個也需要使用者的電子郵件地址。

這些多載存在，因為建立新的使用者帳戶所需的資訊取決於成員資格提供者的組態設定。 在   *<a id="_msoanchor_8"> </a> [SQL Server 中建立成員資格結構描述](creating-the-membership-schema-in-sql-server-vb.md)* 教學課程中，我們檢查中指定的成員資格提供者組態設定`Web.config`。 表 2 包含的組態設定的完整清單。

一個這類成員資格提供者組態設定會影響哪些`CreateUser`可能會使用多載是`requiresQuestionAndAnswer`設定。 如果`requiresQuestionAndAnswer`設為`true`（預設值），然後建立新的使用者帳戶時我們必須指定一組安全性問題和解答。 如果使用者需要重設或變更其密碼，稍後會使用這項資訊。 具體來說，在該時間中，它們會顯示安全性問題，他們必須輸入正確解答才能重設或變更其密碼。 因此，如果`requiresQuestionAndAnswer`設定為`true`則呼叫的前兩個`CreateUser`多載會產生例外狀況，因為遺失的安全性問題和解答。 因為我們的應用程式目前設定為需要的安全性問題和答案，我們必須以程式設計方式建立使用者時，請使用其中一種後者的兩個多載。

若要說明如何使用`CreateUser`方法，讓我們建立我們用來提示使用者輸入其名稱、 密碼、 電子郵件，以及預先定義的安全性問題的解答的使用者介面。 開啟`CreatingUserAccounts.aspx`頁面中`Membership`資料夾並將下列的 Web 控制項新增至內容控制項：

- 名為 TextBox `Username`
- 名為 TextBox `Password`，其`TextMode`屬性設定為 `Password`
- 名為 TextBox `Email`
- 名稱為標籤`SecurityQuestion`具有其`Text`屬性被清除
- 名為 TextBox `SecurityAnswer`
- 名為按鈕`CreateAccountButton`其`Text`屬性會設定為建立使用者帳戶
- Label 控制項，名為`CreateAccountResults`具有其`Text`屬性被清除

此時您的畫面應該看起來像螢幕擷取畫面的 圖 6 所示。


[![將各種不同的 Web 控制項新增至 CreatingUserAccounts.aspx 頁面](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**圖 6**： 將各種不同的 Web 控制項，加入`CreatingUserAccounts.aspx Page`([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image18.png))


`SecurityQuestion`標籤和`SecurityAnswer`文字方塊所顯示的預先定義的安全性問題並收集使用者的回應。 請注意，安全性問題和答案會儲存在使用者的使用者為基礎，因此可允許每個使用者定義自己的安全性問題。 不過，對於此範例中也就是使用通用的安全性問題，我決定： 什麼是您最愛的色彩？

若要實作此預先定義的安全性問題，請將常數新增至名為頁面的程式碼後置類別`passwordQuestion`，將它指派的安全性問題。 然後，在`Page_Load`事件處理常式中，此常數指派`SecurityQuestion`標籤的`Text`屬性：

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

接下來，建立的事件處理常式`CreateAccountButton'`s`Click`事件，並新增下列程式碼：

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

`Click`事件處理常式一開始會定義名為的變數`createStatus`型別的[ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx)。 `MembershipCreateStatus` 是一種列舉，指出狀態`CreateUser`作業。 例如，如果使用者帳戶建立成功，產生`MembershipCreateStatus`會設為值的執行個體`Success;`相反地，如果作業失敗，因為已經存在具有相同的使用者名稱的使用者，它會設定為的值`DuplicateUserName`. 在 `CreateUser`我們會使用多載，我們需要傳遞`MembershipCreateStatus`至方法的執行個體。 此參數設定為適當的值內`CreateUser`方法，而我們可以在方法呼叫，以判斷是否已成功建立使用者帳戶之後，檢查它的值。

之後呼叫`CreateUser`，並傳入`createStatus`，則`Select Case`陳述式用來輸出指派給的值根據適當的訊息`createStatus`。 圖 7 顯示輸出時已成功建立新的使用者。 圖 8 和 9 顯示輸出時不會建立使用者帳戶。 在 圖 8 中，造訪者輸入五個字母的密碼，不符合成員資格提供者的組態設定中的密碼強度需求。 在 圖 9 中，造訪者嘗試使用現有的使用者名稱 （圖 7 中建立的那一個） 中建立的使用者帳戶。


[![新的使用者帳戶已成功建立](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**圖 7**: 新的使用者帳戶已成功建立 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image21.png))


[![因為提供的密碼太弱，不會建立使用者帳戶](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**圖 8**： 因為提供的密碼太弱，不會建立使用者帳戶 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image24.png))


[![使用者帳戶是未建立因為使用者名稱已在使用](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**圖 9**: 使用者帳戶是未建立因為使用者名稱已在使用中的 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> 您可能想知道如何判斷成功或失敗時使用的前兩個的其中一種`CreateUser`方法多載，也都有一個型別的參數`MembershipCreateStatus`。 這些前兩個多載會擲回[`MembershipCreateUserException`例外狀況](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx)面對失敗時，其中包括[`StatusCode`屬性](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx)型別的`MembershipCreateStatus`。


建立幾個使用者帳戶之後，請確認已列出的內容建立的帳戶`aspnet_Users`並`aspnet_Membership`中的資料表`SecurityTutorials.mdf`資料庫。 如 [圖 10] 所示，我已新增兩個使用者透過`CreatingUserAccounts.aspx`頁面： Tito 和 Bruce。


[![成員資格使用者存放區中有兩位使用者： Tito 和 Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**圖 10**： 在成員資格使用者存放區中有兩位使用者： Tito 和 Bruce ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image30.png))


雖然成員資格使用者存放區現在包含 Bruce 和 Tito 的帳戶資訊，我們尚未實作功能，可讓 Bruce 或 Tito 登入網站。 目前，`Login.aspx`驗證使用者的認證會針對硬式編碼的一組使用者名稱/密碼組-*不*驗證成員資格架構提供的認證。 現在查看新的使用者帳戶，在`aspnet_Users`和`aspnet_Membership`資料表必須妥協。 在下一個教學課程中，  *<a id="_msoanchor_9"> </a>[驗證使用者的認證對成員資格使用者存放區](validating-user-credentials-against-the-membership-user-store-vb.md)*，我們將會更新成員資格儲存區對其進行驗證的登入頁面。

> [!NOTE]
> 如果看不到任何使用者在您`SecurityTutorials.mdf`資料庫中，可能是因為 web 應用程式所使用的預設成員資格提供者中， `AspNetSqlMembershipProvider`，它會使用`ASPNETDB.mdf`做為其使用者存放區的資料庫。 若要判斷這是否問題，請按一下方案總管 中的重新整理 按鈕。 如果名為的資料庫`ASPNETDB.mdf`已新增至`App_Data`資料夾中，這是問題。 返回步驟 4  *<a id="_msoanchor_10"> </a> [SQL Server 中建立成員資格結構描述](creating-the-membership-schema-in-sql-server-vb.md)* 教學課程，如需有關如何適當地設定成員資格提供者。


在大部分建立使用者帳戶的案例，請造訪者會看到一些介面，以輸入他們的使用者名稱、 密碼、 電子郵件，以及其他重要的資訊，此時建立新的帳戶。 在此步驟中我們探討了以手動方式建置這類介面，並接著將看到如何使用`Membership.CreateUser`方法來以程式設計方式加入新的使用者帳戶會根據使用者的輸入。 不過，我們的程式碼，只被建立新的使用者帳戶。 它並未執行任何後續動作，例如登入使用者剛建立的使用者帳戶的站台，或確認電子郵件傳送給使用者。 這些額外的步驟會需要額外的程式碼中的按鈕`Click`事件處理常式。

ASP.NET 隨附 CreateUserWizard 控制項，其設計可處理使用者帳戶建立程序，從轉譯使用者介面來建立新的使用者帳戶，在 成員資格架構中建立帳戶並執行後的帳戶建立工作，例如傳送確認電子郵件和剛建立的使用者登入網站。 使用 CreateUserWizard 控制項是簡單，只要將 CreateUserWizard 控制項從 [工具箱] 拖曳至頁面上，然後設定一些屬性。 在大部分情況下，您不需要撰寫一行程式碼。 我們將探討這個小巧的控制項，在步驟 6 中的詳細資料。

如果新的使用者帳戶，僅會透過一般的建立帳戶網頁，也不太可能，您將需要撰寫程式碼，會使用`CreateUser`方法，為 CreateUserWizard 控制項可能符合您的需求。 不過，`CreateUser`方法是方便在案例中，您需要提供高度自訂的建立帳戶的使用者體驗，或當您需要以程式設計方式建立新的使用者帳戶，透過替代的介面。 例如，您可能必須允許使用者上傳 XML 檔案，其中包含從其他應用程式的使用者資訊的頁面。 頁面可能會的剖析內容的上傳 XML 檔案，並在 XML 中建立新的帳戶，表示每個使用者，藉由呼叫`CreateUser`方法。

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>步驟 6： 建立新的使用者與 CreateUserWizard 控制項

ASP.NET 隨附於多個登入 Web 控制項。 這些控制項可協助許多的一般使用者帳戶和登入相關案例。 [CreateUserWizard 控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)是一個這類控制項是設計用來呈現成員資格架構中加入新的使用者帳戶的使用者介面。

如同許多其他的登入相關的 Web 控制項，CreateUserWizard 可用而不需要撰寫一行程式碼。 它直接易懂的方式提供成員資格提供者的組態設定，並在內部呼叫為基礎的使用者介面`Membership`類別的`CreateUser`方法之後的使用者輸入所需的資訊並按一下 [建立使用者] 按鈕。 CreateUserWizard 控制項是極可自訂。 有許多不同帳戶建立程序階段所引發的事件。 我們可以建立事件處理常式，如有需要將帳戶建立工作流程中插入自訂邏輯。 此外，CreateUserWizard 的外觀會非常有彈性。 有許多定義預設的介面; 外觀的屬性如有必要，控制項可以轉換成範本，或可能加入其他使用者註冊步驟。

讓我們開始了解使用 CreateUserWizard 控制項的預設介面和行為。 然後，我們將探討如何自訂透過控制項的屬性和事件的外觀。

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>檢查 CreateUserWizard 的預設介面和行為

返回`CreatingUserAccounts.aspx`頁面中`Membership`資料夾，切換至 設計 或 分割模式中，然後再將 CreateUserWizard 控制項加入頁面頂端。 CreateUserWizard 控制項歸檔 [工具箱] 中的登入控制項區段。 加入控制項之後, 設定其`ID`屬性設`RegisterUser`。 如 圖 11 所示的螢幕擷取畫面，CreateUserWizard 會轉譯文字方塊的新使用者的使用者名稱、 密碼、 電子郵件地址和安全性問題與解答的介面。


[![CreateUserWizard 控制項轉譯為泛型建立使用者介面](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**圖 11**: CreateUserWizard 控制項呈現一般的建立使用者介面 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image33.png))


讓我們花一點時間比較 CreateUserWizard 控制項與我們在步驟 5 中建立的介面所產生的預設使用者介面。 首先，CreateUserWizard 控制項可讓訪客能夠指定安全性問題和解答，而我們以手動方式建立的介面使用預先定義的安全性問題。 CreateUserWizard 控制項的介面也會包含驗證控制項，而我們尚未實作驗證我們的介面表單欄位。 及 CreateUserWizard 控制項介面包括 [確認密碼] 文字方塊 （以及以確保輸入密碼的文字，並比較密碼文字方塊相等 CompareValidator)。

有趣的是 CreateUserWizard 控制項在呈現其使用者介面時，會參照成員資格提供者的組態設定。 例如，安全性問題和解答文字方塊才顯示`requiresQuestionAndAnswer`設為 True。 CreateUserWizard 同樣地，會自動新增 RegularExpressionValidator 控制項，以確保密碼強度需求均符合，並設定其`ErrorMessage`並`ValidationExpression`屬性根據`minRequiredPasswordLength`， `minRequiredNonalphanumericCharacters`，和`passwordStrengthRegularExpression`組態設定。

CreateUserWizard 控制項，正如其名，衍生自[Wizard 控制項](https://msdn.microsoft.com/library/s2etd1ek.aspx)。 精靈的控制項被設計來提供介面來完成多步驟工作。 精靈控制項可能會有任意數目的`WizardSteps`、 每個都是定義 HTML 的範本和 Web 控制項的該步驟。 精靈控制項一開始會顯示第一個`WizardStep`，以及瀏覽控制項，可允許使用者以一個步驟中前往下一步，或若要返回上一個步驟。

如圖 11 中的宣告式標記所示，CreateUserWizard 控制項的預設介面包含兩個`WizardStep`s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? 呈現的介面來收集資訊，以建立新的使用者帳戶。 這是 [圖 11] 所示的步驟。
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? 呈現一則訊息指出已成功建立帳戶。

可以修改 CreateUserWizard 的外觀和行為，將其中一個步驟轉換成範本，或藉由新增您自己`WizardStep`s。 我們將探討如何新增`WizardStep`中註冊介面*儲存額外的使用者資訊*教學課程。

我們來看看 CreateUserWizard 控制項作用中。 請瀏覽`CreatingUserAccounts.aspx`透過瀏覽器的頁面。 開始在 CreateUserWizard 介面中輸入一些無效的值。 請嘗試輸入密碼不符合密碼強度需求，或將 [使用者名稱] 文字方塊保留空白。 CreateUserWizard 會顯示適當的錯誤訊息。 當您嘗試使用不足的強式密碼建立使用者時，圖 12 顯示的輸出。


[![CreateUserWizard 自動插入驗證控制項](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**圖 12**: CreateUserWizard 自動會將驗證控制項 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image36.png))


接下來，CreateUserWizard 中輸入適當的值，然後按一下 [建立使用者] 按鈕。 假設輸入必要的欄位和密碼的強度就已足夠，CreateUserWizard 會建立新的使用者帳戶，透過成員資格架構，並且顯示`CompleteWizardStep`（請參閱 圖 13） 的介面。 在幕後呼叫 CreateUserWizard`Membership.CreateUser`方法，就像我們在步驟 5。


[![新的使用者帳戶已成功建立](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**圖 13**: 新的使用者帳戶已成功建立 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> 如 [圖 13] 所示，`CompleteWizardStep`的介面包括 [繼續] 按鈕。 不過，此時按一下只會執行回傳時，在相同頁面上離開訪客。 在 「 自訂 CreateUserWizard 的外觀和其屬性透過行為區段我們將探討如何有此按鈕，傳送訪客`Default.aspx`（或某些其他分頁）。


建立新的使用者帳戶後，返回 Visual Studio，並檢查`aspnet_Users`和`aspnet_Membership`像我們在圖 10，以確認帳戶已成功建立資料表。

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>自訂 CreateUserWizard 的行為和外觀，透過它的屬性

可以以各種方式，透過屬性，以自訂 CreateUserWizard`WizardStep`和事件處理常式。 這一節中，我們將看看如何自訂控制項的外觀，透過它的屬性;下一節探討擴充控制項的行為，透過事件處理常式。

幾乎所有 CreateUserWizard 控制項的預設使用者介面中顯示的文字可以透過其眾多屬性自訂。 例如，文字方塊的左邊顯示的使用者名稱、 密碼、 確認密碼、 電子郵件、 安全性問題，以及安全性解答標籤可以來自訂[ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx)， [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx)， [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx)， [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx)， [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)，與[ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)屬性，分別。 同樣地，有屬性來指定中的 [建立使用者後繼續] 按鈕的文字`CreateUserWizardStep`和`CompleteWizardStep`、 也如同這些按鈕會呈現為選項按鈕、 Linkbutton 或 ImageButtons。

色彩、 框線、 字型和其他視覺項目皆可設定透過主控件的樣式屬性。 CreateUserWizard 控制項本身有常見的 Web 控制項樣式屬性- `BackColor`， `BorderStyle`， `CssClass`，`Font`等等-而且有定義的特定區段的外觀的樣式屬性的數目CreateUserWizard 的介面。 [ `TextBoxStyle`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx)，比方說，定義中文字方塊的樣式`CreateUserWizardStep`，而[`TitleTextStyle`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx)定義 （登入您的新標題的樣式帳戶）。

除了與外觀相關的屬性，還有一些會影響 CreateUserWizard 控制項的行為的屬性。 [ `DisplayCancelButton`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx)，如果設為 True，會顯示（預設值為 False） 的 [建立使用者] 按鈕旁邊的 [取消] 按鈕。 如果顯示 [取消] 按鈕時，請務必 ssprop_param_table_default [ `CancelDestinationPageUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)，指定使用者按一下 [取消] 之後，會傳送至的頁面。 上一節中的 [繼續] 按鈕所述`CompleteWizardStep`的介面造成回傳，但在相同頁面上離開訪客。 若要傳送至其他網頁的訪客，按一下 [繼續] 按鈕後，只要指定中的 URL [ `ContinueDestinationPageUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)。

讓我們更新`RegisterUser`CreateUserWizard 控制項以顯示 [取消] 按鈕，並傳送訪客`Default.aspx`時按一下 [取消] 或 [繼續] 按鈕。 若要達成此目的，將`DisplayCancelButton`屬性設為 True，同時兩者皆`CancelDestinationPageUrl`和`ContinueDestinationPageUrl`屬性 ~ / Default.aspx。 [圖 14] 顯示更新的 CreateUserWizard 透過瀏覽器檢視時。


[![CreateUserWizardStep 含有 [取消] 按鈕](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**圖 14**:`CreateUserWizardStep`含有 [取消] 按鈕 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image42.png))


訪客會輸入使用者名稱、 密碼、 電子郵件地址和安全性問題和答案，然後按一下 建立使用者，會建立新的使用者帳戶和訪客，會以該新建立的使用者身分登入。 假設瀏覽頁面的人正在為自己建立新的帳戶，這可能是所要的行為。 不過，您可能要允許系統管理員新增新的使用者帳戶。 在此情況下，會建立使用者帳戶，但系統管理員仍然登入為系統管理員 （而不是新建立的帳戶）。 透過此布林值，可以修改此行為[`LoginCreatedUser`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)。

在 成員資格架構中的使用者帳戶包含已核准的旗標;未核准的使用者將無法登入網站。 根據預設，新建立的帳戶已標示為核准，可讓使用者立即登入網站。 不過，有標示為 未核准的新使用者帳戶是可行的。 或許您想要手動核准新使用者，他們可以登入; 之前的系統管理員或者，您想要確認註冊時所輸入的電子郵件地址有效，再允許使用者登入。 任何情況下可能是，您可以藉由設定 CreateUserWizard 控制項標示為 未核准的新建立的使用者帳戶[`DisableCreatedUser`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)設為 True （預設值為 False）。

值得注意的其他行為相關的屬性包含`AutoGeneratePassword`和`MailDefinition`。 如果[`AutoGeneratePassword`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx)設定為 True，`CreateUserWizardStep`不會顯示密碼和確認密碼文字方塊中; 相反地，新建立之使用者的密碼會自動產生使用`Membership`類別的[`GeneratePassword`方法](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)。 `GeneratePassword`方法會建構具有足夠數目的非英數字元，以滿足設定的密碼強度需求及指定長度的密碼。

[ `MailDefinition`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)如果您想要在帳戶建立程序期間指定的電子郵件地址傳送電子郵件會很有用。 `MailDefinition`屬性包含一系列的子屬性來定義建構的電子郵件訊息的相關資訊。 這些子屬性包括選項，例如`Subject`， `Priority`， `IsBodyHtml`， `From`， `CC`，和`BodyFileName`。 [ `BodyFileName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)指向的文字或 HTML 檔案，其中包含電子郵件訊息的本文。 本文會支援預先定義的兩個預留位置：`<%UserName%>`和`<%Password%>`。 若存在於這些預留位置`BodyFileName`檔案中，將會取代為剛建立的使用者名稱和密碼。

> [!NOTE]
> `CreateUserWizard`控制項的`MailDefinition`屬性只會指定建立新的帳戶時，會傳送電子郵件訊息的詳細資料。 它不包含任何有關如何實際傳送電子郵件訊息 （也就是是否使用 SMTP 伺服器或郵件放置目錄、 任何驗證資訊等等）。 這些低階詳細資料，就必須定義在`<system.net>`一節中`Web.config`。 如需有關這些組態設定和在一般情況下傳送電子郵件從 ASP.NET 2.0 的詳細資訊，請參閱[常見問題集，網址 SystemNetMail.com](http://www.systemnetmail.com/)和 我的文章[ASP.NET 2.0 中的 傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)。


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>擴充 CreateUserWizard 的行為使用事件處理常式

CreateUserWizard 控制項在其工作流程期間引發的事件數目。 比方說，訪客輸入其使用者名稱、 密碼和其他相關資訊，並按一下 [建立使用者] 按鈕之後，CreateUserWizard 控制項引發其[`CreatingUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)。 如果在建立過程中，問題[`CreateUserError`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)引發; 不過，如果已成功建立使用者，則[`CreatedUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)，就會引發。 取得引發的額外 CreateUserWizard 控制項事件，但這些是三個最密切關聯的項目。

在某些情況下我們可能會想要善用 CreateUserWizard 工作流程，我們可以藉由建立適當的事件的事件處理常式。 為了說明這點，讓我們來增強`RegisterUser`CreateUserWizard 控制項，以包含某些自訂驗證使用者名稱和密碼。 特別是，讓我們來增強我們 CreateUserWizard，讓使用者名稱不能包含開頭或尾端空格和使用者名稱不能出現在任何位置的密碼。 簡單地說，我們想要防止其他人建立的使用者名稱，例如"Scott"，或需要使用者名稱/密碼組合 Scott.1234 Scott 等。

若要完成此我們將建立的事件處理常式`CreatingUser`執行我們的額外的驗證檢查的事件。 如果提供的資料無效。 我們需要取消建立程序。 我們也需要將標籤 Web 控制項新增至頁面以顯示訊息，說明使用者名稱或密碼不正確。 開始新增標籤控制項 CreateUserWizard 控制項下方，設定其`ID`屬性，以`InvalidUserNameOrPasswordMessage`並將其`ForeColor`屬性設`Red`。 清除其`Text`屬性並設定其`EnableViewState`和`Visible`屬性為 False。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

接下來，建立事件處理常式 CreateUserWizard 控制項`CreatingUser`事件。 若要建立事件處理常式，在設計工具中選取的控制項，然後移至 [屬性] 視窗。 從該處按一下閃電圖示，然後按兩下適當的事件建立事件處理常式。

將下列程式碼加入至 `CreatingUser` 事件處理常式：

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

請注意，使用者名稱和密碼輸入 CreateUserWizard 控制項都可透過其[ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx)並[`Password`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)分別。 我們在上述的事件處理常式中使用這些屬性，來判斷提供的使用者名稱是否包含前置或尾端空格，以及是否找到密碼中的使用者名稱。 如果符合任一條件，則在顯示錯誤訊息`InvalidUserNameOrPasswordMessage`標籤和事件處理常式`e.Cancel`屬性設定為`True`。 如果`e.Cancel`設為`True`，CreateUserWizard 縮短其工作流程，有效地取消使用者帳戶建立程序。

[圖 15] 顯示的螢幕擷取畫面`CreatingUserAccounts.aspx`當使用者輸入使用者名稱加上前置空格。


[![不允許有前置或尾端空格的使用者名稱](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**圖 15**： 不允許有前置或尾端空格的使用者名稱 ([按一下以檢視完整大小的影像](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> 我們會看到範例使用 CreateUserWizard 控制項`CreatedUser`中的事件 *<a id="_msoanchor_11"> </a>[儲存額外的使用者資訊](storing-additional-user-information-vb.md)* 教學課程。


## <a name="summary"></a>總結

`Membership`類別的`CreateUser`方法會在成員資格架構來建立新的使用者帳戶。 它會委派給已設定的成員資格提供者的呼叫。 若是`SqlMembershipProvider`，則`CreateUser`方法新增至記錄`aspnet_Users`和`aspnet_Membership`資料庫資料表。

雖然可以建立新的使用者帳戶，以程式設計方式 （如我們在步驟 5 中所見的） 中，以更快速且輕鬆的方法是使用 CreateUserWizard 控制項。 這個控制項呈現多步驟的使用者介面來收集使用者資訊，並在 成員資格架構中建立新的使用者。 基本上，此控制項使用相同`Membership.CreateUser`檢查步驟 5 中，但該控制項中做的方法會建立使用者介面，驗證控制項，並會回應使用者帳戶建立錯誤，而不需要撰寫可靠的程式碼。

現在我們已備妥来建立新的使用者帳戶的功能。 不過，登入頁面仍然會比對驗證我們指定第二個教學課程中，回到這些硬式編碼的認證。 在  <a id="_msoanchor_12"> </a>[下一個教學課程](validating-user-credentials-against-the-membership-user-store-vb.md)我們將會更新`Login.aspx`來驗證使用者的所提供的認證對成員資格架構。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [`CreateUser` 技術文件](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [CreateUserWizard 控制項概觀](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [建立檔案系統為基礎的網站導覽提供者](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [建立與 ASP.NET 2.0 的精靈控制項的逐步使用者介面](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [檢查 ASP.NET 2.0 的網站導覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [主版頁面與網站導覽](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [SQL 網站導覽提供者您期待已久](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com)。

> [!div class="step-by-step"]
> [上一頁](creating-the-membership-schema-in-sql-server-vb.md)
> [下一頁](validating-user-credentials-against-the-membership-user-store-vb.md)
