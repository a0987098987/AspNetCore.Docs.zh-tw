---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: 安全性基本概念和 ASP.NET 支援 (VB) |Microsoft Docs
author: rick-anderson
description: 這是一系列教學課程會探索技術驗證訪客透過 web 表單，授與參與存取權的第一個教學課程...
ms.author: aspnetcontent
ms.date: 01/13/2008
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: ebd4e52720fc36bfcf86b7ef4205afcca7e2bc4a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820872"
---
<a name="security-basics-and-aspnet-support-vb"></a>安全性基本概念和 ASP.NET 支援 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> 這是一系列將探索驗證透過 web 表單的訪客、 授權存取特定頁面和功能，以及管理 ASP.NET 應用程式中的使用者帳戶的技術的教學課程的第一個教學課程。


## <a name="introduction"></a>簡介

什麼是一件事論壇、 電子商務網站、 線上電子郵件網站、 入口網站和社交網路網站都有共通？ 它們都提供*使用者帳戶*。 提供使用者帳戶的網站必須提供許多服務。 至少必須要能夠建立帳戶的新訪客人數，並傳回訪客必須要能夠登入。 這類的 web 應用程式進行登入的使用者為基礎的決策： 某些頁面或動作可能會限制為只有登入使用者，或使用者子集其他頁面可能會顯示資訊特定登入的使用者，或者可能會增加或減少顯示的詳細資訊，取決於哪些使用者在檢視頁面。

這是一系列將探索驗證透過 web 表單的訪客、 授權存取特定頁面和功能，以及管理 ASP.NET 應用程式中的使用者帳戶的技術的教學課程的第一個教學課程。 這些教學課程期間，我們會檢驗如何：

- 找出並讓使用者登入的網站
- 使用 ASP。NET 的成員資格架構來管理使用者帳戶
- 建立、 更新和刪除使用者帳戶
- 限制存取網頁、 目錄或特定登入的使用者為基礎的功能
- 使用 ASP。NET 的角色架構，來與角色產生關聯的使用者帳戶
- 管理使用者角色
- 限制存取網頁、 目錄或特定登入的使用者角色為基礎的功能
- 自訂並擴充 ASP。NET 的安全 Web 控制項

這些教學課程都被為了很簡潔，並提供足夠的螢幕擷取畫面的逐步指示，可逐步引導您完成此程序以視覺化方式而定。 每個教學課程適用於 C# 和 Visual Basic 版本，而且包含的下載時，使用完整的程式碼。 （此第一個教學課程著重於安全性概念，從高階觀點來看的而且因此不包含任何相關聯的程式碼）。

在本教學課程中，我們將討論重要的安全性概念，以及哪些功能可在 ASP.NET 中以協助實作表單驗證、 授權、 使用者帳戶和角色。 讓我們開始吧 ！

> [!NOTE]
> 安全性是很重要的層面跨越實體、 技術、 任何應用程式和原則決策，而且需要較高程度的規劃和網域知識。 本教學課程系列不是作為指南來開發安全的 web 應用程式。 相反地，它特別著重於表單驗證、 授權、 使用者帳戶和角色。 在這一系列討論上方解決這些問題的一些安全性概念，而其他項目會保留未探索。


## <a name="authentication-authorization-user-accounts-and-roles"></a>驗證、 授權、 使用者帳戶和角色

驗證、 授權、 使用者帳戶和角色是四個將用於經常在此教學課程系列，因此我想要快速花定義這些詞彙的 web 安全性內容中的條款。 在用戶端-伺服器模式下，例如網際網路，有許多案例中的伺服器需要識別提出要求的用戶端。 *驗證*是查明用戶端的身分識別的程序。 已成功識別出的用戶端即所謂*驗證*。 無法辨識的用戶端即所謂*未經驗證*或是*匿名*。

安全的驗證系統涉及下列三個 facet 中至少一個：[您知道的東西、 有項目方法，或您](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html)。 大部分的 web 應用程式需要知道用戶端，例如密碼或 PIN 的項目。 用來識別使用者-她的使用者名稱和密碼，例如-的資訊指*認證*。 本教學課程系列著重*表單驗證*，這是其中的使用者登入網站藉由提供他們網頁表單中的認證驗證模型。 所有遇到這種驗證之前。 移至任何電子商務網站。 當您準備好要簽出時系統會要求您登入，在文字方塊輸入您的使用者名稱和密碼，在網頁上。

除了找出用戶端，伺服器可能需要限制哪些資源或功能會根據提出要求的用戶端可存取。 *授權*是決定特定的使用者是否有權存取特定資源或功能的程序。

A*使用者帳戶*是特定使用者的相關資訊保存存放區。 使用者帳戶至少必須包含唯一識別使用者，例如使用者的登入名稱和密碼的資訊。 此基本資訊，以及使用者帳戶可能包含像是： 使用者的電子郵件地址;日期和時間建立帳戶;使用者上次登入; 的時間與日期第一個和最後一個名稱;電話號碼;和郵寄地址。 使用表單驗證時，使用者帳戶資訊通常會儲存在 Microsoft SQL Server 等關聯式資料庫。

支援使用者帳戶的 web 應用程式可能會選擇性地分組到的使用者*角色*。 角色是只會套用至使用者，並提供定義授權規則和頁面層級功能的抽象概念的標籤。 例如，網站可能包含禁止存取一組特定網頁的系統管理員以外的任何人的授權規則與系統管理員角色。 此外，各種不同的 （包括非系統管理員） 的所有使用者都都可以存取的頁面可能會顯示其他資料，或提供額外的功能，當系統管理員角色中的使用者瀏覽。 使用角色時，我們可以在角色的角色為基礎，而不是在使用者的使用者定義這些授權規則。

## <a name="authenticating-users-in-an-aspnet-application"></a>驗證的 ASP.NET 應用程式中的使用者

當使用者將 URL 輸入瀏覽器的位址視窗或按下的連結時，可讓瀏覽器[都會使用超文字傳輸通訊協定 (HTTP)](http://en.wikipedia.org/wiki/HTTP)要求到 web 伺服器指定的內容，不論是 ASP.NET 頁面上，映像時，JavaScript檔案或任何其他類型的內容。 Web 伺服器被負責傳回要求的內容。 在此情況下，它必須決定要求，包括進行要求的人員，以及識別是否獲得授權來擷取要求的內容相關的項目的數目。

根據預設，瀏覽器會傳送 HTTP 要求沒有任何類型的識別資訊。 但是，如果瀏覽器並包含驗證資訊的網頁伺服器啟動驗證工作流程，會嘗試識別提出要求的用戶端。 驗證工作流程的步驟取決於 web 應用程式正在使用的驗證類型。 ASP.NET 支援三種驗證類型： Windows、 Passport 和表單。 本教學課程系列的重點在於表單驗證，但讓我們花點時間比較和對照 Windows 驗證的使用者存放區和工作流程。

### <a name="authentication-via-windows-authentication"></a>透過 Windows 驗證進行驗證

Windows 驗證工作流程會使用下列的驗證技術之一：

- 基本驗證
- 摘要式驗證
- Windows 整合式的驗證

這三項技術運作方式大致相同： 當未經授權，匿名要求抵達時，web 伺服器會傳回 HTTP 回應，指出該授權，才能繼續。 瀏覽器，然後顯示強制回應對話方塊，提示使用者輸入其使用者名稱和密碼 （請參閱 圖 1）。 這項資訊接著會傳送至 web 伺服器透過 HTTP 標頭。


![強制回應對話方塊會提示使用者輸入其認證](security-basics-and-asp-net-support-vb/_static/image1.png)

**圖 1**： 強制回應對話方塊會提示使用者輸入其認證


提供的認證會驗證對 web 伺服器的 Windows 使用者存放區。 這表示 web 應用程式中每個已驗證的使用者必須有您組織中的 Windows 帳戶。 這是在內部網路案例中的老生常談。 事實上，當使用 Windows 整合式驗證，在內部網路設定中，瀏覽器會自動提供 web 伺服器與用來登入網路，藉此隱藏 [圖 1] 所示的對話方塊中的認證。 Windows 驗證適合內部網路應用程式時，它是通常不可行的網際網路應用程式因為您不會想要建立註冊您的站台的每個使用者的 Windows 帳戶。

### <a name="authentication-via-forms-authentication"></a>透過表單驗證的驗證

表單驗證，相反地，適合用於網際網路的 web 應用程式。 提醒您，表單驗證會識別使用者，提示他們輸入其認證，透過 web 表單。 因此，當使用者嘗試存取未經授權的資源，它們會自動重新導向至可輸入其認證登入頁面。 提交的認證經過驗證對自訂使用者存放區-通常是資料庫。

確認已提交的認證之後,*表單驗證票證*建立使用者。 此票證會指示使用者已經驗證，且包含身分識別資訊，例如使用者名稱。 表單驗證票證 （通常） 會儲存為用戶端電腦上的 cookie 中。 因此，後續造訪網站會在 HTTP 要求中，藉此讓 web 應用程式，來識別使用者，他們必須登入之後，包含表單驗證票證。

[圖 2] 說明高階的有利點的表單驗證工作流程。 請注意如何在 ASP.NET 中的驗證和授權項目作為兩個不同的實體。 表單驗證系統來識別的使用者 （或報表是匿名）。 授權系統可決定使用者是否具有要求之資源的存取。 如果使用者是未經授權 （因為它們是圖 2 中，當您嘗試以匿名方式瀏覽 ProtectedPage.aspx 時），授權系統會報告遭到拒絕的使用者，使表單驗證系統會自動將使用者重新導向至登入頁面。

使用者已成功登入之後，後續的 HTTP 要求包含表單驗證票證。 表單驗證系統只會識別使用者-它會決定使用者是否可以存取所要求的資源的授權系統。


![表單驗證工作流程](security-basics-and-asp-net-support-vb/_static/image2.png)

**圖 2**： 表單驗證工作流程


我們將會更詳細的表單驗證，在下面兩個教學課程中，深入探究[的表單驗證概觀](an-overview-of-forms-authentication-vb.md)並[表單驗證組態和進階主題](forms-authentication-configuration-and-advanced-topics-vb.md)。 如需有關 ASP。NET 的驗證選項，請參閱[ASP.NET 驗證](https://msdn.microsoft.com/library/eeyk640h.aspx)。

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>限制存取的網頁、 目錄和頁面功能

ASP.NET 包含兩種方式可判斷特定使用者是否具有特定檔案或目錄的存取權限：

- **檔案授權**-由於 ASP.NET 網頁和 web 服務會實作為透過存取控制清單 (Acl) 來指定位於 web 伺服器的檔案系統，這些檔案的存取權的檔案。 檔案授權通常會用於使用 Windows 驗證，因為設定的 Acl 會套用到 Windows 帳戶的權限。 使用表單驗證時，所有的作業系統和檔案系統層級要求會執行相同的 Windows 帳戶，無論使用者瀏覽的網站。
- **URL 授權**-頁面開發人員使用 URL 授權時，在 Web.config 中指定授權規則。這些授權規則會指定使用者或角色允許存取還是遭到拒絕存取特定頁面或應用程式中的目錄。

檔案授權和 URL 授權定義授權規則存取特定的 ASP.NET 頁面，或適用於所有的 ASP.NET 網頁特定目錄中。 我們可以使用這些技術來指示 ASP.NET 要求特定的頁面，以特定的使用者，拒絕或允許存取一組使用者並拒絕其他所有人的存取。 其中的所有使用者可以存取頁面上，但頁面的功能取決於使用者的情況呢？ 比方說，許多支援使用者帳戶的站台會有這些頁面會顯示不同的內容或已驗證的使用者與匿名使用者的資料。 匿名使用者可能會看到登入網站的連結，已驗證的使用者會改為看到一則訊息像，歡迎回來，而*Username*以及登出的連結。另一個範例： 檢視項目在拍賣網站時您會看到不同的資訊，取決於您是否效勞或拍賣項目。

以宣告方式或以程式設計方式，就可以完成這類頁面層級調整。 若要顯示不同的內容，針對已驗證的使用者，只要拖曳比匿名[LoginView 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)拖曳到頁面然後其 AnonymousTemplate 和 LoggedInTemplate 範本中輸入適當的內容。 或者，您可以以程式設計方式判斷是否已驗證目前的要求，身為使用者，以及哪些角色所屬 （如果有的話）。 您可以使用這項資訊，然後顯示或隱藏方格或面板頁面上的資料行。

這一系列包含三個教學課程著重於授權。 ***使用者為基礎的授權***檢驗如何限制存取特定的使用者帳戶; 頁面或在目錄中的頁面***角色型授權***會提供授權規則，在角色層級; 最後，查看***顯示目前已登入使用者的內容以***教學課程會探討修改特定頁面的內容和使用者瀏覽頁面為基礎的功能。 如需有關 ASP。NET 的授權選項，請參閱[ASP.NET 授權](https://msdn.microsoft.com/library/wce3kxhd.aspx)。

## <a name="user-accounts-and-roles"></a>使用者帳戶和角色

ASP。NET 的表單驗證提供的基礎結構，以便讓使用者登入網站，並將記憶跨網頁瀏覽其已驗證的狀態。 和 URL 授權提供用來限制存取特定檔案或資料夾中的 ASP.NET 應用程式的架構。 兩項功能，不過，提供一種方式儲存的使用者帳戶資訊或管理角色。

ASP.NET 2.0 中，開發人員之前，負責建立自己的使用者和角色存放區。 它們也是在設計使用者介面，並撰寫程式碼不可或缺的使用者帳戶相關的網頁，例如登入頁面和頁面，即可建立新的帳戶，以及其他的勾點。 沒有任何內建使用者帳戶架構在 ASP.NET 中，實作的使用者帳戶已到達在自己的設計決策的問題，例如，每位開發人員如何儲存密碼或其他機密資訊？並且何種方針應該我加上密碼長度和強度？

現在，在 ASP.NET 應用程式中實作的使用者帳戶十分簡便*成員資格架構*和內建登入 Web 控制項。 成員資格架構是少數幾個類別中[System.Web.Security 命名空間](https://msdn.microsoft.com/library/system.web.security.aspx)，提供功能來執行基本的使用者帳戶相關的工作。 在 成員資格架構中的索引鍵類別是[Membership 類別](https://msdn.microsoft.com/library/system.web.security.membership.aspx)，其具有類似的方法：

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

成員資格架構會使用[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，其中清楚分隔成員資格架構的 API 從它的實作。 這可讓開發人員使用常見的 API，但可讓使用者使用實作符合其應用程式的自訂需求。 簡單地說，Membership 類別定義 （方法、 屬性和事件），framework 的基本功能，但實際上不會提供任何實作詳細資料。 相反地，成員資格類別的方法叫用已設定的提供者，它會執行實際工作。 例如，叫用成員資格類別的 CreateUser 方法時，成員資格類別並不知道使用者存放區的詳細資料。 它並不知道是否資料庫或 XML 檔案或其他存放區，使用者會被保留。 Membership 類別會檢查以判斷哪些提供者委派，呼叫的 web 應用程式的組態，該提供者類別會負責實際建立適當的使用者存放區中的 新的使用者帳戶。 圖 3 說明這種互動。

Microsoft.NET Framework 中提供兩個成員資格提供者類別：

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -實作成員資格 API，在 Active Directory 和 Active Directory Application Mode (ADAM) 的伺服器。
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -SQL Server 資料庫中實作成員資格 API。

本教學課程系列專門著重於 SqlMembershipProvider。


[![提供者模型可讓不同的實作是順暢地插入到的架構](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**圖 03**: 提供者模型可讓不同的實作是順暢地插入到的架構 ([按一下以檢視完整大小的影像](security-basics-and-asp-net-support-vb/_static/image5.png))


提供者模型的優點是可以由 Microsoft、 協力廠商或個別的開發人員開發替代的實作，並順暢地插入成員資格架構。 例如，Microsoft 已發行[的成員資格提供者，針對 Microsoft Access 資料庫](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi)。 如需有關成員資格提供者的詳細資訊，請參閱[提供者工具組](https://msdn.microsoft.com/asp.net/aa336558.aspx)，其中包含的成員資格提供者範例自訂提供者、 超過 100 個頁面上提供者模型中，文件的逐步解說，完成原始碼的內建成員資格提供者 （也就是 ActiveDirectoryMembershipProvider 和 SqlMembershipProvider）。

ASP.NET 2.0 也引進了角色架構。 成員資格架構，像是角色 framework 是建置在提供者模型之上。 其 API 可透過公開[Roles 類別](https://msdn.microsoft.com/library/system.web.security.roles.aspx)和.NET Framework 隨附三個提供者類別：

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -管理授權管理員原則存放區，例如 Active Directory 或 ADAM 中的角色資訊。
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -實作 SQL Server 資料庫中的角色。
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -將訪客的 Windows 群組為基礎的角色資訊。 這個方法通常是使用 Windows 驗證。

本教學課程系列專門著重於 SqlRoleProvider。

因為提供者模型會包含單一的前方 API （成員資格與角色類別），就可以建置該 API 功能，而不必擔心的實作詳細資料-這些都由頁面所選取的提供者開發人員。 此統一的 API 可讓 Microsoft 和協力廠商提供，該介面的成員資格與角色架構建置 Web 控制項。 ASP.NET 隨附許多[登入 Web 控制項](https://msdn.microsoft.com/library/ms178329.aspx)實作常見的使用者帳戶的使用者介面。 例如，[登入控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)提示使用者輸入其認證，驗證它們，並再記錄中透過表單驗證。 [LoginView 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)提供範本，來顯示不同的標記，以匿名使用者與已驗證的使用者或不同的標記，根據使用者的角色。 而[CreateUserWizard 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx)提供逐步的使用者介面來建立新的使用者帳戶。

基本上與成員資格與角色架構的各種登入控制項進行互動。 可以實作大部分的登入控制項，而不需要撰寫一行程式碼。 我們將檢視這些控制項，更詳細地在未來的教學課程，包括延伸與自訂其功能的技術。

## <a name="summary"></a>總結

支援使用者帳戶的所有 web 應用程式都需要類似的功能，例如： 可讓使用者登入，然後在跨網頁瀏覽; 記憶的狀態有其記錄檔若要建立帳戶; 新增訪客 web 網頁以及若要指定哪些使用者或角色可以使用哪些資源、 資料和功能的網頁開發人員的能力。 進行驗證和授權使用者，以及管理使用者帳戶和角色的工作是由於表單驗證、 URL 授權和成員資格與角色架構的 ASP.NET 應用程式中完成要容易得多。

下一步 的數個教學課程的課程中，我們將檢視這些層面藉由以逐步方式建置全新的工作 web 應用程式。 在下面兩個教學課程中，我們將探討在詳細資料中的表單驗證。 我們會看到-表單驗證工作流程，在動作中，會仔細剖析表單驗證票證、 討論的安全性考量，並了解如何設定表單驗證系統同時建置 web 應用程式，可以讓登入和登出的訪客。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 2.0 成員資格、 角色、 表單驗證和安全性資源](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2.0 安全性指導方針](https://msdn.microsoft.com/library/ms998258.aspx)
- [ASP.NET 驗證](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [ASP.NET 授權](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [ASP.NET 登入控制項概觀](https://msdn.microsoft.com/library/ms178329.aspx)
- [檢查 ASP.NET 2.0 的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [I： 如何保護我的網站使用成員資格和角色？](https://asp.net/learn/videos/video-45.aspx) （影片）
- [成員資格簡介](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2.0 安全性、 成員資格和角色管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [提供者工具組](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 潛在客戶的檢閱者，在本教學課程是系列由許多實用的檢閱者檢閱本教學課程。 本教學課程中的潛在客戶檢閱者包括 Alicja Maziarz、 John Suru 和 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](forms-authentication-configuration-and-advanced-topics-cs.md)
> [下一頁](an-overview-of-forms-authentication-vb.md)
