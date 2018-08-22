---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: 使用者和生產環境網站 (VB) 的角色 |Microsoft Docs
author: rick-anderson
description: ASP.NET 網站管理工具 (WSAT) 提供的 web 架構使用者介面，對於設定成員資格與角色設定，以及建立、 編輯、...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: f63d64532543da681fdf88399d7dd365804674c4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824945"
---
<a name="users-and-roles-on-the-production-website-vb"></a>使用者和角色，生產環境網站 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> ASP.NET 網站管理工具 (WSAT) 提供的 web 架構使用者介面來設定成員資格與角色設定以及建立、 編輯和刪除使用者和角色。 不幸的是，WSAT 只適用於從 localhost，這表示您無法透過瀏覽器到達生產環境網站的系統管理工具。 好消息是，有因應措施，可讓您能夠管理使用者和實際執行的角色。 本教學課程會探討這些因應措施和其他項目。


## <a name="introduction"></a>簡介

ASP.NET 2.0 引進了許多*應用程式服務*，這是一套可以加入您的 web 應用程式的建置組塊服務。 我們已新增的書籍評論網站成員資格和角色服務回到[*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-vb.md)。 成員資格服務可協助建立和管理使用者帳戶;角色服務會提供 API，來將使用者分類成群組。 書籍評論站台有三個使用者帳戶-Scott，Jisun，Alice-和的系統管理員，Scott 與 Jisun Admin 角色中的角色。

ASP。NET 的應用程式服務不會受限於特定實作。 相反地，您可以指示需要使用特定的應用程式服務*提供者*，和該提供者會實作使用特定技術的服務。 我們設定的書籍評論 web 應用程式中使用`SqlMembershipProvider`和`SqlRoleProvider`成員資格和角色服務的提供者。 這些兩個提供者將使用者帳戶和角色的資訊儲存在 SQL Server 資料庫，並針對以網際網路為基礎的 web 應用程式裝載於哪家虛擬主機公司最常使用的提供者。

使用成員資格和角色服務的開發人員的常見挑戰管理使用者和角色，在實際執行環境。 您如何從生產網站中刪除使用者帳戶、 加入新的角色，或將現有的使用者新增至現有的角色？ 本教學課程會探討不同的方法，來管理使用者和生產環境網站的角色。

## <a name="using-the-aspnet-web-site-administration-tool"></a>使用 ASP.NET Web Site Administration Tool

ASP.NET 包含[Web Site Administration Tool](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT)，可輕鬆建立及管理使用者帳戶和角色，並指定使用者和角色型授權規則。 若要使用 WSAT，按一下 [ASP.NET 組態] 圖示，在 [方案總管] 中，或移至 [網站] 或 [專案] 功能表並選擇 [ASP.NET 組態] 選項。 兩種方法會啟動網頁瀏覽器，並指向在的位址，例如 WSAT: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT 分為三個區段：

- **安全性**-管理使用者、 角色和授權規則。
- **ApplicationConfiguration** -管理&lt;appSettings&gt;和 SMTP 設定，從這裡開始。 您可以也離線應用程式及管理偵錯和追蹤從這裡的設定，以及指定預設的自訂錯誤網頁。
- **ProviderConfiguration** -設定應用程式服務所使用的提供者。

安全性 區段 (示**圖 1**) 包含的連結，建立新的使用者、 管理使用者、 建立和管理角色，以及建立和管理存取規則。 從這裡您可以將新角色新增至系統、 刪除現有的使用者，或新增或移除特定使用者帳戶的角色。

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**圖 1**: WSAT 安全性 > 一節包含管理使用者和角色的選項  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-vb/_static/image3.png))

不幸的是，WSAT 就只能存取在本機。 您無法瀏覽遠端的生產環境網站; WSAT如果您瀏覽`www.yoursite.com/asp.netwebadminfiles/default.aspx`您收到 「 404 找不到 」 回應。 提供的 WSAT 用途的程式碼`Membership`和`Roles`類別在.NET Framework，才能建立、 編輯和刪除使用者和角色。 這些類別，請參閱 「 web 應用程式的組態資訊來判斷要使用哪個提供者回到[*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-vb.md)我們設定要使用的書籍評論網站`SqlMembershipProvider`和`SqlRoleProvider`提供者。 這詳述新增`<membership>`並`<roleManager>`區段以`Web.config`。

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

請注意，`<membership>`並`<roleManager>`區段，參考`SqlMembershipProvider`並`SqlRoleProvider`中的提供者其`type`屬性，分別。 這些提供者會將使用者和角色資訊儲存在指定的 SQL Server 資料庫。 這些提供者所使用的資料庫由`connectionStringName`屬性， `ReviewsConnectionString`，其定義於`~/ConfigSections/databaseConnectionStrings.config`檔案。 請記得，`databaseConnectionStrings.config`開發環境中的檔案包含開發資料庫的連接字串，而`databaseConnectionStrings.config`上生產環境檔案包含實際執行資料庫的連接字串。

簡單的說，WSAT 必須透過在本機開發環境中，以及它所使用的使用者和角色資料庫中的資訊中指定`databaseConnectionStrings.config`檔案。 因此，若我們變更中的連接字串資訊`databaseConnectionStrings.config`檔案在開發環境中我們可以使用 WSAT 在本機管理使用者和在生產環境中的角色。

為了說明這項功能，開啟`databaseConnectionStrings.config`檔案在 Visual Studio 中，在開發環境和開發資料庫連接字串取代為實際執行資料庫連接字串。 然後啟動 WSAT，請 安全性 索引標籤中，新增名為密碼"password"！ Sam 使用者 （小於引號）。 **圖 2**顯示 WSAT 螢幕時建立此帳戶。

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**圖 2**： 建立名為 Sam 在生產環境中的新使用者  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-vb/_static/image6.png))

因為我們變更中的連接字串`databaseConnectionStrings.config`指向生產環境資料庫伺服器，Sam 已新增為生產環境中的使用者。 若要確認這點，將變更中的連接字串`databaseConnectionStrings.config`傳回至開發資料庫檔案，然後瀏覽`Login.aspx`開發環境中的頁面。 嘗試將 Sam 身分登入 (請參閱**圖 3**)。

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**圖 3**： 您無法在開發環境中的 Sam 身分登入  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-vb/_static/image9.png))

您無法登入為 Sam 開發環境中因為不存在的使用者帳戶資訊，這是在本機資料庫。 而是加入至生產環境資料庫。 若要確認，檢視的內容`aspnet_Users`開發和生產資料庫中的資料表。 在開發環境中應該只有三個記錄 Scott、 Jisun 及 Alice 的使用者。 不過，`aspnet_Users`實際執行資料庫中的資料表有四個記錄： Scott、 Jisun、 Alice 和 Sam。 因此，Sam 可以登入，透過網站的生產環境，而不是透過開發環境。

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**圖 4**: Sam 生產網站上，可以登入  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> 別忘了變更中的連接字串`databaseConnectionStrings.config`回到開發資料庫檔案的連接字串，當您完成時 wsat 否則您會使用實際執行資料測試的網站，在開發時使用環境。 也請注意，我們剛剛所討論的技術可讓我們使用 WSAT 遠端管理使用者和角色，變更任何其他 WSAT 組態選項 （存取規則、 SMTP 設定、 偵錯和追蹤設定，以及等等） 會修改`Web.config`檔案。 因此，對設定進行任何變更適用於開發環境，不適用於生產環境。


## <a name="creating-custom-user-and-role-management-web-pages"></a>建立自訂的使用者和角色管理網頁

WSAT 提供的方塊中的系統管理使用者和角色，但只能在本機啟動，而且需要對連接字串資訊中的變更，若要管理的使用者和實際執行的角色。 支援使用者帳戶的大部分網站也包含許多使用者和角色管理網頁可讓系統管理員管理使用者和角色從站台內的頁面。 這類 web 為基礎的管理頁面使其更容易管理使用者和角色，並不可或缺的網站可能是許多系統管理員或沒有存取權，或者使用 Visual Studio 啟動 WSAT 的技術背景的系統管理員。

ASP.NET 會包含一些內建的登入相關的 Web 控制項使實作許多這些系統管理網頁鬆拖放一樣簡單。 例如，您可以建立 CreateUserWizard 控制項拖曳到頁面，並設定一些屬性，以建立新的使用者帳戶的系統管理員的頁面。 事實上，在示 WSAT 中建立使用者的頁面**圖 2**使用相同的 CreateUserWizard 控制項，您可以新增至您的頁面。 此外，成員資格和角色服務的功能都是以程式設計方式透過`Membership`和`Roles`.NET Framework 中的類別。 有了這些類別中，您可以撰寫程式碼來建立、 編輯和刪除使用者和角色，以及新增或移除使用者角色，並判斷哪些使用者有哪些角色，以及執行其他使用者和角色相關的工作。

在  [*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-vb.md)我加入頁面，即可`Admin`資料夾名為`CreateAccount.aspx`。 此頁面可讓系統管理員將新的使用者帳戶新增至站台，以及指定新建立的使用者是否為系統管理員角色 (請參閱**圖 5**)。

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**圖 5**： 系統管理員可以建立新的使用者帳戶  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-vb/_static/image15.png))

建立使用者和角色管理頁面，以及使用的逐步指示，更詳細查看`Membership`並`Roles`類別和登入相關的 ASP.NET Web 控制項中，務必先閱讀我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。 那里您可以找到相關指引如何建置網頁，建立新的帳戶、 建立和管理角色，將使用者指派給角色，以及其他常見的管理工作。

生產網站上實作 WSAT 類似的功能，您可以一律建置您自己的系列的實作 WSAT 功能的網頁。 為了開始使用，請查看 WSAT 原始碼，位於資料夾`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`。 另一個選項是使用 Dan Clem WSAT 另一種分享他的文章中，[輪流您自己 Web Site Administration Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)。 Dan 將逐步引導讀者了解建置自訂的 WSAT 類似工具的程序、 包含他的應用程式的原始程式碼下載 （在 C# 中)，並提供其自訂 WSAT 加入託管網站的逐步指示。

## <a name="summary"></a>總結

ASP.NET 網站管理工具 (WSAT) 可以用於成員資格和角色的應用程式服務的結合，來管理您的網站的使用者和角色資訊。 不幸的是，WSAT 才可存取在本機，而且不能從您的生產網站中瀏覽。 不過，變更連接字串，在開發環境，以指到實際執行資料庫可以使用 WSAT 管理使用者和生產環境網站的角色。

雖然 WSAT 方法提供快速且輕鬆的方式來管理使用者和角色，就需要啟動 WSAT 從 Visual Studio，以及因應暫時變更連接字串資訊。 WSAT 提供快速的方式來管理使用者和角色，在生產環境中，但是很麻煩，而且不適用於網站使用多個系統管理員或系統管理員不需要或不熟悉 Visual Studio 和 WSAT。 基於這些理由，大部分的網站，以支援使用者帳戶會包含一組的系統管理網頁。 將這類網頁就不需要 WSAT 和供從任何電腦的各種系統管理使用者。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [正在檢查 ASP。NET 的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [復原您自己的 Web Site Administration Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [網站系統管理工具概觀](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [上一步](precompiling-your-website-vb.md)
