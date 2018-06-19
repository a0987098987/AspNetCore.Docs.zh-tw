---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: 使用者和角色上的生產網站 (C#) |Microsoft 文件
author: rick-anderson
description: ASP.NET 網站管理工具 (WSAT) 提供網頁型使用者介面，來設定成員資格和角色設定，以及如何建立、 編輯、...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: e3e1165959ae47715e0037db7a3bc6ac58807653
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888288"
---
<a name="users-and-roles-on-the-production-website-c"></a>使用者和角色上的生產網站 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> ASP.NET 網站管理工具 (WSAT) 提供網頁型使用者介面來設定成員資格和角色設定以及建立、 編輯和刪除使用者和角色。 不幸的是，WSAT 只適用於瀏覽從 localhost，這表示您無法透過瀏覽器連線生產網站管理工具。 好消息是因應措施，可讓您能夠管理使用者和實際執行的角色。 本教學課程會查看這些因應措施和其他項目。


## <a name="introduction"></a>簡介

ASP.NET 2.0 導入了一些*應用程式服務*，其為套件，您可以加入您的 web 應用程式的建置組塊服務。 我們加入活頁簿檢閱網站的成員資格和角色服務回[*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-cs.md)。 成員資格服務可協助建立和管理使用者帳戶。角色服務提供應用程式開發介面，使用者分類成群組。 活頁簿檢閱站台具有三個使用者帳戶-Scott、 Jisun，和 Alice 層及的系統管理員，Scott 與 Jisun 系統管理員角色中的角色。

ASP。網路的應用程式服務並不限於特定的實作。 相反地，您可以指示需要使用特定的應用程式服務*提供者*，和該提供者會實作使用特定技術的服務。 我們設定活頁簿檢閱 web 應用程式使用`SqlMembershipProvider`和`SqlRoleProvider`成員資格和角色服務提供者。 這些兩個提供者使用者帳戶和角色的資訊儲存在 SQL Server 資料庫和裝載在虛擬主機公司以網際網路為基礎的 web 應用程式最常使用的提供者。

開發人員使用的成員資格和角色服務的常見挑戰管理使用者和角色在實際執行環境。 您如何從生產網站刪除使用者帳戶、 加入新的角色，或將現有使用者加入至現有的角色？ 本教學課程中，瀏覽不同技術可管理使用者和生產性網站上的角色。

## <a name="using-the-aspnet-web-site-administration-tool"></a>使用 ASP.NET 網頁站台系統管理工具

ASP.NET 包含[網站管理工具](https://msdn.microsoft.com/library/yy40ytx0.aspx)(WSAT)，能讓您輕鬆建立及管理使用者帳戶和角色，並指定使用者和角色為基礎的授權規則。 若要使用 WSAT，按一下 [ASP.NET 組態] 圖示，在 [方案總管] 中，或移至 [網站] 或 [專案] 功能表並選擇 [ASP.NET 組態] 選項。 兩種方法啟動網頁瀏覽器並指向 WSAT 在的地址如下： `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT 分成三個區段：

- **安全性**-管理使用者、 角色和授權規則。
- **ApplicationConfiguration** -管理&lt;appSettings&gt;和從這裡的 SMTP 設定。 您可以也離線應用程式和管理偵錯和追蹤從這裡的設定，以及指定預設的自訂錯誤網頁。
- **ProviderConfiguration** -設定應用程式服務所使用的提供者。

[安全性] 區段 (示**圖 1**) 包含的連結，建立新使用者、 管理使用者、 建立和管理角色，以及建立及管理存取規則。 從這裡您可以將新角色加入至系統、 刪除現有的使用者，或新增或移除特定使用者帳戶的角色。

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**圖 1**: WSAT 安全性 > 一節包含用於管理使用者和角色選項  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-cs/_static/image3.png))

不幸的是，WSAT 才可存取在本機。 您無法在遠端的生產網站; 瀏覽 WSAT如果您瀏覽`www.yoursite.com/asp.netwebadminfiles/default.aspx`取得 404 找不到回應。 可提供簡單的 WSAT 用途的程式碼`Membership`和`Roles`類別在.NET Framework 來建立、 編輯和刪除使用者和角色。 這些類別，請參閱 web 應用程式的組態資訊來判斷何種提供者使用。回到[*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-cs.md)在我們設定的活頁簿檢閱網站使用`SqlMembershipProvider`和`SqlRoleProvider`提供者。 這有權從頭加入`<membership>`和`<roleManager>`區段`Web.config`。

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

請注意，`<membership>`和`<roleManager>`區段參考`SqlMembershipProvider`和`SqlRoleProvider`中的提供者及其`type`分別屬性。 這些提供者的使用者和角色資訊儲存在指定的 SQL Server 資料庫。 這些提供者所使用的資料庫由指定`connectionStringName`屬性`ReviewsConnectionString`，定義在`~/ConfigSections/databaseConnectionStrings.config`檔案。 請記得，`databaseConnectionStrings.config`開發環境中的檔案包含開發資料庫的連接字串，而`databaseConnectionStrings.config`實際執行的檔案包含實際執行資料庫的連接字串。

簡而言之，WSAT 必須透過在本機開發環境中，及使用者和角色資訊中指定的資料庫中將它用於`databaseConnectionStrings.config`檔案。 因此，若我們變更中的連接字串資訊`databaseConnectionStrings.config`檔案在開發環境上我們可以使用 WSAT 本機來管理使用者和實際執行環境中的角色。

為了說明這項功能，開啟`databaseConnectionStrings.config`檔案在 Visual Studio 開發環境和開發資料庫連接字串取代為實際執行資料庫連接字串。 然後啟動 WSAT，請 [安全性] 索引標籤中，加入新的使用者名稱和密碼"password"！ Sam （小於的引號）。 **圖 2**顯示 WSAT 螢幕時建立此帳戶。

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**圖 2**： 建立名為 Sam 實際執行環境中的新使用者  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-cs/_static/image6.png))

因為我們變更中的連接字串`databaseConnectionStrings.config`指向實際資料庫伺服器，Sam 將會加入做為實際執行環境中的使用者。 若要確認這種情況，變更連接字串中的`databaseConnectionStrings.config`傳回至開發資料庫檔案，然後瀏覽`Login.aspx`開發環境中的頁面。 嘗試以 Sam 登入 (請參閱**圖 3**)。

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**圖 3**： 您無法在開發環境中的 Sam 身分登入  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-cs/_static/image9.png))

您無法登入為 Sam 開發環境中因為本機資料庫中不存在的使用者帳戶資訊。 而是會加入至實際執行資料庫。 若要確認這種情況，檢視的內容`aspnet_Users`開發和生產資料庫中的資料表。 在開發環境中應該只有 3 個記錄 Scott、 Jisun 及 Alice 的使用者。 不過，`aspnet_Users`生產資料庫中的資料表有四個記錄： Scott、 Jisun、 Alice 和 Sam。 因此，Sam 可以登入，透過網站在生產環境中，但不是能透過開發環境。

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**圖 4**: Sam 生產網站上，可以登入  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> 別忘了變更連接字串中的`databaseConnectionStrings.config`開發資料庫檔案的連接字串，當您完成 wsat 否則您會使用實際執行資料與測試的開發網站時使用環境。 也請注意，我們剛才所討論的技術可讓我們使用 WSAT 遠端管理使用者和角色，任何其他 WSAT 設定選項 （存取規則、 SMTP 設定、 偵錯和追蹤設定，以及等等） 的變更會修改`Web.config`檔案。 因此，對設定進行任何變更將套用至開發環境，而不實際執行環境。


## <a name="creating-custom-user-and-role-management-web-pages"></a>建立自訂的使用者和角色管理 Web 網頁

WSAT 提供的系統管理使用者和角色，但只能在本機上啟動，而且需要對連接字串資訊中的變更，以便管理使用者和實際執行的角色。 大多數的網站，以支援使用者帳戶也會包含使用者和角色管理網頁可讓系統管理員管理使用者和角色，從站台內的頁面數目。 更輕鬆地管理使用者和角色進行這類 web 為基礎的管理頁面，並是不可或缺的站台其中可能有許多系統管理員或不具有存取權或使用 Visual Studio 啟動 WSAT 技術背景的系統管理員。

ASP.NET 包含一些內建實作許多這些系統管理網頁簡單，拖曳和卸除的登入相關的 Web 控制項。 例如，您可以建立適用於 CreateUserWizard 控制項拖曳到頁面，並設定一些屬性，以建立新的使用者帳戶的系統管理員的頁面。 事實上，在所示的 WSAT 中建立使用者頁面**圖 2**使用相同的適用於 CreateUserWizard 控制項，您可以加入至您的頁面。 此外，成員資格和角色服務的功能都是以程式設計方式透過`Membership`和`Roles`.NET Framework 中的類別。 使用這些類別中，您可以撰寫程式碼來建立、 編輯和刪除使用者和角色，以及新增或移除使用者角色，以判斷使用者是在何種角色，以及執行其他使用者和角色相關的工作。

在[*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-cs.md)我加入頁面的`Admin`資料夾名為`CreateAccount.aspx`。 此頁面可讓系統管理員將新的使用者帳戶新增至站台，以及指定新建立的使用者是否為系統管理員角色中 (請參閱**圖 5**)。

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**圖 5**： 系統管理員可以建立新的使用者帳戶  
([按一下以檢視完整大小的影像](users-and-roles-on-the-production-website-cs/_static/image15.png))

建立使用者和角色 頁面，以及使用的逐步指示以詳細查看`Membership`和`Roles`類別和登入相關的 ASP.NET Web 控制項中，務必閱讀我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。 那里您可以找到相關指引如何建置網頁，來建立新的帳戶、 建立及管理角色，將使用者指派給角色，以及其他常見的管理工作。

在生產網站上實作 WSAT 類似的功能，您可以一律建置自己的實作 WSAT 功能的網頁的序列。 為了開始使用，請查看位於資料夾 WSAT 原始碼`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`。 另一個選項是使用 Dan Clem WSAT 代替他共用在他的文章，[輪流您自己的網站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)。 Dan 讀取器，透過建立自訂的 WSAT 類似工具的程序會逐步引導、 包含 （在 C# 中)，下載他的應用程式原始程式碼，並提供他自訂 WSAT 加入託管網站的逐步指示。

## <a name="summary"></a>總結

ASP.NET 網站管理工具 (WSAT) 可用的成員資格和角色的應用程式服務結合來管理使用者和角色的資訊，為您的網站。 不幸的是，WSAT 才可存取在本機，而無法從生產網站中瀏覽。 不過，藉由變更連接字串，在開發環境，以點為實際執行資料庫可以使用 WSAT 管理使用者和生產性網站上的角色。

雖然的 WSAT 方法提供快速簡便的方式來管理使用者和角色，它必須啟動 WSAT 從 Visual Studio，以及暫時變更連接字串資訊。 WSAT 提供快速的方式來管理使用者和角色在生產環境中，但是很麻煩，而且不適用於網站使用多個系統管理員或系統管理員不需要或不熟悉 Visual Studio 和 WSAT。 基於這些理由，大多數支援使用者帳戶的網站包含一組系統管理的網頁。 這類組的網頁就不需要 WSAT，並使用不同的系統管理使用者從任何電腦。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [正在檢查 ASP。網路的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [復原您的網站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [網站管理工具概觀](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [上一頁](precompiling-your-website-cs.md)
> [下一頁](asp-net-hosting-options-vb.md)
