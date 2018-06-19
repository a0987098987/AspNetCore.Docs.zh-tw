---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: 安全性基本概念和 ASP.NET 支援 (VB) |Microsoft 文件
author: rick-anderson
description: 這是一系列將探索驗證訪客透過 web 表單的存取權 partic 授權技術的教學課程中的第一個教學課程...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: e62bb865e211a279b60f3120162ffc3c49cbdcc5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890004"
---
<a name="security-basics-and-aspnet-support-vb"></a>安全性基本概念和 ASP.NET 支援 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> 這是一系列將探索驗證透過 web 表單的訪客、 授權存取特定的頁面和功能，以及管理 ASP.NET 應用程式中的使用者帳戶的技術的教學課程中的第一個教學課程。


## <a name="introduction"></a>簡介

什麼是一件事論壇、 電子商務網站、 線上電子郵件網站、 入口網站和所有有共同的社交網路網站？ 所有提供了*使用者帳戶*。 提供使用者帳戶的網站必須提供服務的數目。 至少新增訪客需要能夠建立帳戶，並傳回訪客必須要能夠登入。 這類 web 應用程式便可做出決策是根據登入的使用者： 某些頁面或動作可能會限制為僅登入的使用者，或使用者子集其他頁面可能會對顯示資訊特定登入的使用者，或可能會增加或減少顯示的詳細資訊，根據使用者檢視頁面。

這是一系列將探索驗證透過 web 表單的訪客、 授權存取特定的頁面和功能，以及管理 ASP.NET 應用程式中的使用者帳戶的技術的教學課程中的第一個教學課程。 透過這些教學課程的課程將介紹如何：

- 找出並讓使用者登入的網站
- 使用 ASP。網路的成員資格架構來管理使用者帳戶
- 建立、 更新和刪除使用者帳戶
- 限制 web 網頁、 目錄或特定功能登入的使用者為基礎的存取
- 使用 ASP。網路的角色架構，以與角色產生關聯的使用者帳戶
- 管理使用者角色
- 限制 web 網頁、 目錄或特定功能登入的使用者角色為基礎的存取
- 自訂及擴充 ASP。網路的安全 Web 控制項

這些教學課程被專很簡潔，並提供具有足夠的螢幕擷取畫面來引導您完成程序以視覺化方式的逐步指示。 每一個教學課程使用在 C# 和 Visual Basic 版本，包括使用的完整程式碼的下載。 （這個第一個教學課程著重在從高階觀點來看的安全性概念和因此不包含任何相關聯的程式碼）。

在本教學課程中，我們將討論重要的安全性概念和 ASP.NET，協助您實作表單驗證、 授權、 使用者帳戶和角色中的哪些功能可用。 讓我們開始吧 ！

> [!NOTE]
> 安全性是很重要的層面跨越實體、 技術，任何應用程式和原則決策，且需要較高程度的規劃和定義域的知識。 此教學課程系列不適合做為指南開發安全的 web 應用程式。 相反地，它著重特別是在表單驗證、 授權、 使用者帳戶和角色。 在這一系列討論上方解決這些問題的一些安全性概念，而會保留其他尚未探測。


## <a name="authentication-authorization-user-accounts-and-roles"></a>驗證、 授權、 使用者帳戶和角色

驗證、 授權、 使用者帳戶和角色是安全性的四個會用經常在此教學課程系列，因此我想要快速詳讀定義 web 內容中的這些授權條款的條款。 在用戶端與伺服器模型中，例如網際網路有許多案例中的伺服器需要識別提出要求的用戶端。 *驗證*是演練用戶端身分識別的程序。 已成功地識別的用戶端即為*驗證*。 無法辨識的用戶端即為*未經驗證*或*匿名*。

安全的驗證系統牽涉到下列三個 facet 中至少一個：[您知道、 必須是方法，或您](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html)。 大部分 web 應用程式依賴用戶端知道，例如密碼或 PIN 的項目。 用來識別-其使用者名稱和密碼，例如-使用者資訊統稱為*認證*。 此教學課程系列著重於*表單驗證*，這是在使用者登入站台提供他們網頁表單中的認證驗證模型。 我們所有發生這種類型的驗證之前。 移至任何電子商務網站。 當您準備好要簽出將會要求在文字方塊輸入您的使用者名稱和密碼，在網頁上登入。

除了識別用戶端，伺服器可能需要限制哪些資源或功能會根據提出要求的用戶端可存取。 *授權*會決定特定的使用者是否有權存取特定資源或功能的程序。

A*使用者帳戶*會保存特定使用者的相關資訊的存放區。 使用者帳戶至少必須包含可唯一識別使用者，例如使用者的登入名稱和密碼資訊。 此基本資訊，以及使用者帳戶可能像是： 使用者的電子郵件地址。日期和時間建立帳戶。日期和時間使用者上次登入。第一個和最後一個名稱。電話號碼;和郵寄地址。 使用表單驗證時，使用者帳戶資訊通常會儲存在類似 Microsoft SQL Server 關聯式資料庫。

支援使用者帳戶的 web 應用程式可能會選擇性地使用者分組到*角色*。 角色是只套用到使用者，並提供定義授權規則和頁面層級功能的抽象概念的標籤。 例如，網站可能包含禁止存取一組特定的 web 網頁系統管理員以外的任何人的授權規則與系統管理員角色。 此外，各種不同的 （包括非系統管理員） 的所有使用者都都可以存取的頁面可能會顯示其他資料，或提供額外的功能，當系統管理員角色中的使用者瀏覽。 使用角色時，我們可以在角色的角色為基礎，而不是使用者的使用者定義這些授權規則。

## <a name="authenticating-users-in-an-aspnet-application"></a>驗證 ASP.NET 應用程式中的使用者

當使用者將 URL 輸入瀏覽器的位址視窗或按下的連結時，可讓瀏覽器[超文字傳輸通訊協定 (HTTP)](http://en.wikipedia.org/wiki/HTTP)指定內容的網頁伺服器的要求，可能是 ASP.NET 頁面上，映像時，JavaScript檔案或任何其他類型的內容。 Web 伺服器被負責傳回要求的內容。 在此情況下，它必須判斷要求，包括進行要求的人員，以及是否使用身分識別授權擷取要求的內容相關的事項數目。

根據預設，瀏覽器會傳送 HTTP 要求缺少任何類型的識別資訊。 但是如果瀏覽器並包含驗證資訊的 web 伺服器開始驗證工作流程，會嘗試識別提出要求的用戶端。 驗證工作流程的步驟取決於 web 應用程式正在使用的驗證類型。 ASP.NET 支援三種驗證類型： Windows、 Passport 和表單。 此教學課程系列著重於表單驗證，但是讓我們花點時間比較與對照 Windows 驗證的使用者存放區和工作流程。

### <a name="authentication-via-windows-authentication"></a>透過 Windows 驗證進行驗證

Windows 驗證工作流程會使用以下驗證方法的其中一個：

- 基本驗證
- 摘要式驗證
- Windows 整合式的驗證

所有的三種技術運作方式大致相同： 當未經授權的匿名要求抵達時，web 伺服器會傳回 HTTP 回應，指出該授權，才能繼續。 瀏覽器，然後顯示強制回應對話方塊，提示使用者輸入使用者名稱和密碼 （請參閱圖 1）。 然後，此資訊會傳送至 web 伺服器，透過 HTTP 標頭。


![強制回應對話方塊會提示使用者輸入其認證](security-basics-and-asp-net-support-vb/_static/image1.png)

**圖 1**： 強制回應對話方塊會提示使用者輸入其認證


提供的認證會驗證對 web 伺服器的 Windows 使用者存放區。 這表示 web 應用程式中每個已驗證的使用者必須擁有您組織中的 Windows 帳戶。 這是在內部網路案例相差不多。 事實上，當使用 Windows 整合式驗證內部網路設定中，瀏覽器會自動提供網頁伺服器與用來登入網路，藉此隱藏圖 1 所示的對話方塊中的認證。 Windows 驗證最適合用於內部網路應用程式時，它是網際網路應用程式通常並不可行因為您不想為每個註冊位於您網站的使用者建立 Windows 帳戶。

### <a name="authentication-via-forms-authentication"></a>透過表單驗證的驗證

相反地，表單驗證是適用於網際網路的 web 應用程式。 提醒您，表單驗證識別的使用者提示他們輸入其認證，透過 web 表單。 因此，當使用者嘗試存取未經授權的資源，它們會自動重新導向可以在其中輸入他們的認證登入頁面。 針對自訂使用者存放區-通常是資料庫提交的認證經過驗證。

確認提交的認證，*表單驗證票證*建立使用者。 此票證表示使用者已經驗證，且包括識別資訊，例如使用者名稱。 表單驗證票證 （通常） 會儲存成用戶端電腦上的 cookie。 因此，後續造訪的網站會包含在 HTTP 要求中，藉此讓 web 應用程式，來識別使用者，一旦使用者登入表單驗證票證。

圖 2 說明整體的優勢點表單驗證工作流程。 請注意如何在 ASP.NET 中的驗證和授權部分做為兩個不同的實體。 表單驗證系統識別使用者 （或報表是匿名）。 授權系統，以及決定使用者是否具有存取權要求的資源。 如果使用者未獲得授權，（因為它們是在圖 2 中，當您嘗試以匿名方式瀏覽 ProtectedPage.aspx 時），授權系統會報告已拒絕使用者，造成表單驗證系統會自動將使用者重新導向至登入頁面。

一旦使用者已成功登入，後續 HTTP 要求將包含表單驗證票證。 表單驗證系統只會識別使用者-決定使用者是否可以存取要求之資源的授權系統。


![表單驗證工作流程](security-basics-and-asp-net-support-vb/_static/image2.png)

**圖 2**： 表單驗證工作流程


我們將會深入探究在下面兩個教學課程中，更詳細的表單驗證[的表單驗證概觀](an-overview-of-forms-authentication-vb.md)和[表單驗證設定和進階主題](forms-authentication-configuration-and-advanced-topics-vb.md)。 如需有關 ASP。網路的驗證選項，請參閱[ASP.NET 驗證](https://msdn.microsoft.com/library/eeyk640h.aspx)。

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>限制存取的網頁、 目錄和頁面功能

ASP.NET 包含兩種方式可判斷特定的使用者是否具有特定檔案或目錄的存取權限：

- **檔案授權**-因為 ASP.NET 網頁和 web 服務都會實作為可透過存取控制清單 (Acl) 指定位於 web 伺服器的檔案系統，存取這些檔案的檔案。 因為 Acl 是套用到 Windows 帳戶的權限，檔案授權是最常用來使用 Windows 驗證。 使用表單驗證時，所有的作業系統和檔案系統層級要求是由相同的 Windows 帳戶，無論使用者瀏覽的網站中執行。
- **URL 授權**-網頁開發人員使用 URL 授權，在 Web.config 中指定授權規則。這些授權規則指定使用者或角色可以存取或拒絕存取特定頁面或應用程式中的目錄。

檔案授權和 URL 授權定義授權規則，以存取特定的 ASP.NET 頁面或適用於所有 ASP.NET 網頁特定目錄中。 我們可以使用這些技術來指示 ASP.NET 特定的頁面上，針對特定的使用者的要求被拒絕或允許存取的一組使用者而拒絕存取其他人。 當所有的使用者可以存取頁面，但在頁面的功能取決於使用者案例呢？ 比方說，許多支援使用者帳戶的站台會有不同的內容或與匿名使用者已驗證使用者的資料顯示的頁面。 匿名使用者可能會看到登入網站的連結，而驗證的使用者會改看到一則訊息，歡迎回來， *Username*以及登出的連結。另一個範例： 檢視項目在拍賣站台時，您會看到不同的資訊，根據您所效勞或 auctioning 項目。

以宣告方式或以程式設計方式，就可以完成這類頁面層級調整。 若要可顯示不同內容的匿名驗證的使用者，只要拖放到比[LoginView 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)拖曳到頁面然後其 AnonymousTemplate 和 LoggedInTemplate 範本中輸入適當的內容。 或者，您可以以程式設計方式判斷是否已驗證目前的要求、 身為使用者，以及哪些角色所屬 （如果有的話）。 然後顯示或隱藏資料行在方格或面板頁面上的，您可以使用這項資訊。

這一系列包含三個教學課程將焦點放在授權。 ***使用者為基礎的授權***檢查如何限制存取特定的使用者帳戶; 頁面或在目錄中的網頁***角色式授權***提供授權規則的角色層級，則最後，會查看***顯示目前登入使用者的內容基礎***修改特定教學課程-探索網頁的內容和使用者瀏覽頁面為基礎的功能。 如需有關 ASP。網路的授權選項，請參閱[ASP.NET 授權](https://msdn.microsoft.com/library/wce3kxhd.aspx)。

## <a name="user-accounts-and-roles"></a>使用者帳戶和角色

ASP。網路的表單驗證提供一種基礎結構以便讓使用者登入網站，並有跨網頁瀏覽記住其已驗證的狀態。 和 URL 授權提供用來限制存取特定檔案或資料夾中的 ASP.NET 應用程式的架構。 兩項功能，不過，提供適用於儲存使用者帳戶資訊或管理角色的方法。

在 ASP.NET 2.0 之前開發人員所負責建立自己的使用者和角色存放區。 它們也是在設計使用者介面，並撰寫程式的必要的使用者帳戶相關的網頁登入頁面和頁面，即可建立新的帳戶，和其他項目等的勾點。 沒有任何內建的使用者帳戶架構，在 ASP.NET 中，實作的使用者帳戶已到達在自己的設計決策問題，例如，每位開發人員如何我儲存密碼或其他機密資訊？以及何種方針應該我強制密碼長度和強度？

現在，在 ASP.NET 應用程式中實作使用者帳戶是以更簡單感謝您*成員資格 framework*以及內建登入 Web 控制項。 成員資格 framework 是少數幾個類別中[System.Web.Security 命名空間](https://msdn.microsoft.com/library/system.web.security.aspx)執行必要的使用者帳戶相關的工作提供功能。 索引鍵的類別中的成員資格 framework 是[成員資格類別](https://msdn.microsoft.com/library/system.web.security.membership.aspx)，其具有類似的方法：

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

成員資格架構會使用[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，其中清楚分隔成員資格架構的應用程式開發介面的實作。 這可讓開發人員使用常見的 API，但是讓他們使用的實作，符合其應用程式的自訂需求。 簡單地說，成員資格類別定義的基本功能的架構 （方法、 屬性和事件），但實際上不會提供任何實作詳細資料。 相反地，成員資格類別的方法叫用設定的提供者，會執行實際工作。 例如，叫用的成員資格類別的 CreateUser 方法時，成員資格類別並不知道使用者存放區的詳細資料。 它並不知道是否使用者必須維持的資料庫或 XML 檔案或其他存放區。 成員資格類別會檢查以判斷哪個委派，呼叫的提供者的 web 應用程式的組態，而且該提供者類別會負責實際適當的使用者存放區中建立新的使用者帳戶。 這種互動如圖 3 所示。

Microsoft.NET Framework 中隨附兩個成員資格提供者類別：

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -實作成員資格 API，在 Active Directory 和 Active Directory 應用程式模式 (ADAM) 的伺服器。
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -SQL Server 資料庫中實作的成員資格 API。

此教學課程系列專門著重於 SqlMembershipProvider。


[![提供者模型可讓不同的實作能夠順暢地連接到架構](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**圖 03**: 提供者模型可讓不同的實作能夠順暢地連接到的 Framework ([按一下以檢視完整大小的影像](security-basics-and-asp-net-support-vb/_static/image5.png))


替代執行方式可以由 Microsoft、 協力廠商或開發人員開發而順暢地連接到成員資格 framework 提供者模型的優點。 例如，Microsoft 已發行[Microsoft Access 資料庫的成員資格提供者](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi)。 如需成員資格提供者的詳細資訊，請參閱[提供者 Toolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx)，其中包含的成員資格提供者、 範例自訂提供者、 超過 100 個頁面上的提供者模型，文件的逐步解說和內建成員資格提供者 （也就是 ActiveDirectoryMembershipProvider 和 SqlMembershipProvider） 的完整原始程式碼。

ASP.NET 2.0 也導入了角色架構。 成員資格架構，例如角色 framework 是建置在提供者模型之上。 它的 API 會公開透過[角色類別](https://msdn.microsoft.com/library/system.web.security.roles.aspx)和.NET Framework 隨附於提供者的三個類別：

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -管理授權管理員原則存放區，例如 Active Directory 或 ADAM 中的角色資訊。
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -SQL Server 資料庫中實作角色。
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -將相關聯的訪客的 Windows 群組為基礎的角色資訊。 這個方法通常是使用 Windows 驗證。

此教學課程系列專門著重於 SqlRoleProvider。

因為提供者模型包含單一的正向 API （成員資格和角色類別），可建置該 API 周圍的功能，而不必擔心實作詳細資料-這些由頁面所選取的提供者開發人員。 這個統一的 API 可讓 Microsoft 和協力廠商所提供介面的成員資格和角色架構建立 Web 控制項。 ASP.NET 隨附許多[登入 Web 控制項](https://msdn.microsoft.com/library/ms178329.aspx)實作一般使用者帳戶的使用者介面。 例如，[登入控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)會提示使用者輸入其認證，驗證，並再記錄中透過表單驗證。 [LoginView 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)提供範本，來顯示不同的標記，匿名使用者與已驗證的使用者或不同的標記，根據使用者的角色。 和[適用於 CreateUserWizard 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx)提供逐步的使用者介面建立新的使用者帳戶。

基本上與成員資格和角色架構的各種登入控制項進行互動。 大部分的登入控制項可以實作不需要撰寫一行程式碼。 我們將在未來的教學課程，包括擴充及自訂其功能的技術來檢查更詳細的這些控制項。

## <a name="summary"></a>總結

支援使用者帳戶的所有 web 應用程式都需要類似的功能，例如： 使用者登入，並在整個網頁瀏覽; 記住的狀態中有其記錄檔的能力若要建立的帳戶; 新訪客的網頁若要指定哪些使用者或角色可以使用哪些資源、 資料和功能的網頁開發人員的能力。 工作和管理使用者帳戶和角色的使用者驗證和授權是非常容易達成這點受惠 form 驗證、 URL 授權及成員資格和角色架構的 ASP.NET 應用程式中。

下一步的幾個教學課程的過程中，我們將檢查這些層面以逐步方式建置 web 應用程式運作從頭。 在下面兩個教學課程中，我們將探討詳細表單驗證。 我們會看到表單驗證工作流程，在動作中，仔細分析表單驗證票證、 討論安全性考量，並了解如何設定表單驗證系統-所有建置 web 應用程式時可讓登入和登出的訪客。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 2.0 的成員資格、 角色、 表單驗證和安全性資源](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2.0 的安全性指導方針](https://msdn.microsoft.com/library/ms998258.aspx)
- [ASP.NET 驗證](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [ASP.NET 授權](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [ASP.NET 登入控制項概觀](https://msdn.microsoft.com/library/ms178329.aspx)
- [檢查 ASP.NET 2.0 的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [I： 如何保護我的站台使用成員資格和角色？](https://asp.net/learn/videos/video-45.aspx) （影片）
- [成員資格的簡介](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [專業 ASP.NET 2.0 安全性、 成員資格和角色管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [提供者的工具組](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已數列已經過許多有用的檢閱者檢閱本教學課程。 前導檢閱者在此教學課程包含 Alicja Maziarz、 John Suru 和本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](forms-authentication-configuration-and-advanced-topics-cs.md)
> [下一頁](an-overview-of-forms-authentication-vb.md)
