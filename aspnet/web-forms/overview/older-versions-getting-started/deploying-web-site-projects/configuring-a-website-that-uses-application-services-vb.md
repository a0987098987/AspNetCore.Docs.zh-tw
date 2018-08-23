---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: 設定使用應用程式服務 (VB) 的網站 |Microsoft Docs
author: rick-anderson
description: ASP.NET 2.0 版引進了一系列的應用程式服務，這是.NET framework 的一部分，並且做為建置組塊一套服務 yo...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: d5fe8dc8486cf08e0aaf0e107069972eee7fbada
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833859"
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>設定網站使用應用程式服務 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip)或[下載 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> ASP.NET 2.0 版引進了一系列的應用程式服務，這是.NET framework 的一部分，並且做為一套可供您將豐富的功能新增至您的 web 應用程式的建置組塊服務。 本教學課程會探討如何將應用程式服務在生產環境中設定網站，並解決常見的問題，管理使用者帳戶和實際執行環境的角色。


## <a name="introduction"></a>簡介

ASP.NET 2.0 版引進了一系列*應用程式服務*，這是.NET framework 的一部分，並且做為建置組塊一套服務，您可以使用豐富的功能加入您的 web 應用程式。 應用程式服務包括：

- **成員資格**-此 API 可用於建立及管理使用者帳戶。
- **角色**-此 API 可用於將使用者分類成群組。
- **設定檔**-此 API 可用於儲存自訂的使用者特定的內容。
- **網站導覽**-此 API 可用於定義的階層，然後透過導覽控制項，例如功能表和階層連結可以顯示的表單中的站台 s 邏輯結構。
- **個人化**-此 API 可讓您維護自訂喜好設定，最常搭配[ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx)。
- **健全狀況監視**-此 API 可用於監視效能、 安全性、 錯誤及其他執行中 web 應用程式的系統健康情況計量。
  

應用程式服務 Api 不會受限於特定實作。 相反地，您可以指示需要使用特定的應用程式服務*提供者*，和該提供者會實作使用特定技術的服務。 針對以網際網路為基礎的 web 應用程式裝載於哪家虛擬主機公司最常使用的提供者會使用 SQL Server 資料庫實作那些提供者。 比方說，`SqlMembershipProvider`是儲存在 Microsoft SQL Server 資料庫中的使用者帳戶資訊的成員資格 api 提供者。

部署應用程式時使用的應用程式服務和 SQL Server 提供者加入一些挑戰。 首先，應用程式服務資料庫物件必須正確地建立開發和生產資料庫上，並適當地初始化。 也有一些必須進行的重要組態設定。

> [!NOTE]
> 使用所設計的 Api 應用程式服務[*提供者模型*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，可讓 api s 實作細節，以在執行階段提供的設計模式。 .NET Framework 隨附於應用程式服務提供者，可以使用，例如許多`SqlMembershipProvider`和`SqlRoleProvider`、 哪些是提供者的成員資格和角色 Api，使用 SQL Server 資料庫實作。 您也可以建立和外掛程式自訂提供者。 事實上，書籍評論 web 應用程式已經包含站台地圖 API 的自訂提供者 (`ReviewSiteMapProvider`)，這會建構中的資料從網站導覽`Genres`和`Books`資料庫中的資料表。


本教學課程開始，看看我擴充的書籍評論 web 應用程式使用的成員資格和角色 Api 的方式。 它接著逐步部署使用 SQL Server 資料庫實作時，使用應用程式服務，並結束時，會定址常見的問題，管理使用者帳戶和角色，在實際執行環境的 web 應用程式。

## <a name="updates-to-the-book-reviews-application"></a>書籍評論應用程式的更新

近幾活頁簿會檢閱 web 應用程式時，已更新靜態網站上，動態、 資料驅動 web 應用程式的教學課程會使用一組來管理內容類型和評論的系統管理 頁面完成。 不過，此管理 區段中目前未受保護-任何使用者都知道 （或猜測） 的系統管理 頁面的 URL 可以 waltz 中與建立、 編輯或刪除檢閱我們的網站上。 保護網站的特定部分的常見方式是實作的使用者帳戶，然後使用 URL 授權規則限制到特定使用者或角色的存取。 適用於本教學課程中下載的書籍評論 」 web 應用程式。 支援的使用者帳戶和角色。 它有一個角色定義名為系統管理員，而且只有此角色的使用者，才可以存取管理頁面。

> [!NOTE]
> 我已建立的書籍評論的 web 應用程式中的三個使用者帳戶： Scott、 Jisun 和 Alice。 所有的三個使用者都有相同的密碼：**密碼 ！** Scott 和 Jisun Admin 角色中，不是 Alice。 網站 s 非系統管理 頁面是匿名的使用者仍可存取。 也就是說，您不需要登入瀏覽網站，除非您想要管理，在此情況下您必須登入為系統管理員角色中的使用者。


書籍評論應用程式 s 主版頁面已更新為包含已驗證和匿名使用者不同的使用者介面。 如果匿名使用者瀏覽網站會出現在右上角的登入連結。 已驗證的使用者會看到此訊息中，「 歡迎回來， *username*！ 」 和登出的連結。該處 s 也登入頁面 (`~/Login.aspx`)，其中包含的登入 Web 控制項，提供的使用者介面與邏輯驗證訪客。 只有系統管理員可以建立新的帳戶。 (有用於建立和管理使用者帳戶中的頁面`~/Admin`資料夾。)

### <a name="configuring-the-membership-and-roles-apis"></a>設定成員資格和角色 Api

若要支援使用者帳戶，並將這些使用者到角色 （也就是系統管理員角色） 的書籍評論的 web 應用程式會使用成員資格和角色 Api。 `SqlMembershipProvider`和`SqlRoleProvider`會使用提供者類別，因為我們想要將帳戶和角色的資訊儲存在 SQL Server 資料庫。

> [!NOTE]
> 本教學課程的目的不是要在設定的 web 應用程式，以支援的成員資格和角色 Api 詳細的檢查。 徹底了解這些 Api，您需要設定網站來使用它們所採取的步驟，請閱讀我[*網站安全性教學課程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。


若要使用 SQL Server 資料庫，您必須先新增這些提供者使用的資料庫所在的使用者帳戶的資料庫物件和儲存的角色資訊的應用程式服務。 這些必要的資料庫物件包括各種不同的資料表、 檢視和預存程序。 除非另有指定，否則為`SqlMembershipProvider`並`SqlRoleProvider`提供者類別會使用名為 SQL Server Express Edition 資料庫`ASPNETDB`位於 應用程式的`App_Data`資料夾; 如果這類資料庫不存在，它會自動建立使用這些提供者在執行階段所需的資料庫物件。

它是可行的及通常是理想的做法，來建立應用程式服務網站 s 應用程式專屬資料儲存所在的相同資料庫中的資料庫物件。 .NET Framework 隨附工具，叫做`aspnet_regsql.exe`，指定資料庫上安裝資料庫物件。 我已事先，並使用此工具將這些物件`Reviews.mdf`資料庫中`App_Data`資料夾 （開發資料庫）。 我們將了解如何使用此工具在本教學課程稍後，當我們將這些物件加入至實際執行資料庫。

如果您新增的應用程式服務資料庫的資料庫物件以外`ASPNETDB`您必須自訂`SqlMembershipProvider`和`SqlRoleProvider`提供者類別的組態，使其使用適當的資料庫。 若要自訂的成員資格提供者新增[ *&lt;的成員資格&gt;項目*](https://msdn.microsoft.com/library/1b9hw62f.aspx)內`<system.web>`一節中`Web.config`; 使用[ *&lt;roleManager&gt;項目*](https://msdn.microsoft.com/library/ms164660.aspx)設定角色提供者。 下列程式碼片段取自的書籍評論應用程式 s `Web.config` ，並顯示為成員資格和角色 Api 的組態設定。 同時註冊新的提供者-的附註`ReviewMembership`並`ReviewRole`-使用`SqlMembershipProvider`和`SqlRoleProvider`提供者，分別。

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

`Web.config`檔案的`<authentication>`元素也已經設定為支援表單型驗證。
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>限制存取的管理頁面

ASP.NET 可讓您輕鬆地授與或拒絕存取特定檔案或資料夾的使用者或透過 azure 角色及其*URL 授權*功能。 (我們簡短討論中的 URL 授權*差異的核心之間 IIS 和 ASP.NET Development Server*教學課程和示範如何在 IIS 和 ASP.NET Development Server 適用於 URL 授權規則，以不同的方式為靜態與動態內容。）因為我們想要禁止存取`~/Admin`資料夾中的系統管理員角色的使用者，我們需要將 URL 授權規則加入至這個資料夾。 具體而言，URL 授權規則，就需要允許系統管理員角色的使用者，以及拒絕其他所有使用者。 這藉由加入`Web.config`檔案`~/Admin`資料夾中的，使用下列內容：

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

如需有關 ASP.NET 的 URL 授權 功能，以及如何用它來拼出授權規則的使用者和角色，務必先閱讀[*使用者為基礎的授權*](../../older-versions-security/membership/user-based-authorization-vb.md)和[*角色為基礎的授權*](../../older-versions-security/roles/role-based-authorization-vb.md)教學課程，從我[*網站安全性教學課程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。

## <a name="deploying-a-web-application-that-uses-application-services"></a>部署 Web 應用程式使用應用程式服務

在部署應用程式服務和應用程式服務會將資訊儲存在資料庫中的提供者所使用的網站時，務必，生產資料庫上建立所需的應用程式服務的資料庫物件。 一開始生產環境資料庫中不包含這些物件，因此，當第一次部署應用程式 （或第一次之後已加入應用程式服務部署時），您必須採取額外步驟以取得這些必要的資料庫物件生產環境資料庫。

部署使用應用程式服務，如果您想要複寫到生產環境的開發環境中建立的使用者帳戶的網站時，可能會發生另一項挑戰。 根據成員資格和角色的設定，則可能即使您已成功複製到實際執行資料庫開發環境中所建立的使用者帳戶，這些使用者無法登入在生產環境中的 web 應用程式。 我們將看看此問題的原因，並討論如何避免發生。

ASP.NET 隨附有多好[*網站管理工具 (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) ，可以從 Visual Studio 中啟動，並允許使用者帳戶、 角色和授權規則，以透過以 web 為基礎進行管理介面。 不幸的是，WSAT 僅適用於本機網站，這表示它不能用來從遠端管理使用者帳戶、 角色和 web 應用程式在生產環境中的授權規則。 我們將探討不同的方式來實作 WSAT 類似的行為，從您的生產網站。

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>新增的資料庫物件使用 aspnet\_regsql.exe

*將資料庫部署*教學課程示範了如何從開發資料庫的資料表和資料複製到生產資料庫，以及這些技巧的確可以用來複製到的應用程式服務資料庫物件生產環境資料庫。 另一個選項是`aspnet_regsql.exe`工具，可新增或移除資料庫中的應用程式服務資料庫物件。

> [!NOTE]
> `aspnet_regsql.exe`工具指定的資料庫上建立資料庫物件。 它不會移轉的那些資料庫物件中的資料從開發資料庫的生產環境資料庫。 如果您想要開發資料庫中的使用者帳戶和角色資訊複製至生產環境資料庫使用所述的技巧*將資料庫部署*教學課程。


將探討如何將資料庫物件新增至生產環境資料庫使用的 s`aspnet_regsql.exe`工具。 開啟 Windows 檔案總管並瀏覽至您的電腦，%WINDIR%\ 的.NET Framework 2.0 版目錄開始Microsoft.NET\Framework\v2.0.50727。 您應該在這裡找到`aspnet_regsql.exe`工具。 這項工具可從命令列中，但它也包含圖形化使用者介面;按兩下`aspnet_regsql.exe`檔案以啟動其圖形化元件。

此工具一開始會顯示啟動顯示畫面，說明其用途。 按一下旁邊前進至 「 選取安裝選項 」 畫面中，圖 1 所示。 從這裡您可以選擇將應用程式服務資料庫物件，或從資料庫中移除。 因為我們想要將這些物件加入至實際執行資料庫，選取 「 設定 SQL Server 的應用程式服務 」 選項，然後按一下 [下一步]。


[![選擇 設定 SQL Server 的應用程式服務](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**圖 1**： 選擇 應用程式服務的 設定 SQL Server ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


在 [選取伺服器和資料庫] 畫面會提示輸入資訊以連接到資料庫。 輸入資料庫伺服器、 安全性認證和資料庫名稱提供給您的網站主機代管公司，然後按一下 [下一步]。

> [!NOTE]
> 輸入您的資料庫伺服器和認證之後，展開 [資料庫] 下拉式清單時，可能會收到錯誤。 `aspnet_regsql.exe`工具查詢`sysdatabases`系統資料表，以擷取一份在伺服器上，但某些 web 裝載其資料庫伺服器的公司鎖定，使這項資訊不是公開可用的資料庫。 如果您收到這個錯誤您可以直接在下拉式清單中輸入資料庫名稱。


[![提供資料庫的連線資訊與工具](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**圖 2**： 提供的工具與您的資料庫 s 連接資訊 ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


後續畫面摘要說明要進行，也就是動作，將應用程式服務資料庫物件加入至指定的資料庫。 按一下 [下一步] 完成此動作。 幾分鐘後，會顯示最後一個畫面，您會看到已加入 （請參閱 圖 3） 的資料庫物件。


[![成功 ！應用程式服務的資料庫物件已加入至生產環境資料庫](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**圖 3**： 成功 ！ 應用程式服務資料庫物件已加入至生產環境資料庫 ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


若要確認應用程式服務資料庫物件已成功新增至生產環境資料庫中，開啟 SQL Server Management Studio 並連接到您的生產資料庫。 如 [圖 4] 所示，您現在應該會看到應用程式服務資料庫資料表在資料庫中， `aspnet_Applications`， `aspnet_Membership`， `aspnet_Users`，依此類推。


[![確認您的資料庫物件已加入至生產環境資料庫](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**圖 4**： 確認的資料庫物件已加入至生產環境資料庫 ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


您只需要使用`aspnet_regsql.exe`工具時部署 web 應用程式第一次，或在第一次之後您開始使用應用程式服務。 一旦這些資料庫物件會在生產資料庫上他們贏得 t 需要重新新增或修改。

### <a name="copying-user-accounts-from-development-to-production"></a>複製使用者帳戶從開發到生產環境

使用時`SqlMembershipProvider`並`SqlRoleProvider`提供者類別，將應用程式的服務資訊儲存在 SQL Server 資料庫中，使用者帳戶和角色資訊會儲存在資料庫資料表，包括各種`aspnet_Users`， `aspnet_Membership`，`aspnet_Roles`，和`aspnet_UsersInRoles`，其他項目。 如果您可以在開發期間建立的使用者帳戶來開發環境中您可以複寫在生產環境中的使用者帳戶從適用的資料庫資料表複製對應的記錄。 如果您可以使用資料庫發行精靈來部署您可能有也選擇要複製的記錄，可能會導致開發也是在生產環境中建立的使用者帳戶的應用程式服務資料庫物件。 但是，根據組態設定，您可能會發現這些使用者的帳戶已在開發過程中建立及複製到生產環境很難從生產網站登入。 什麼會這樣呢？

`SqlMembershipProvider`和`SqlRoleProvider`提供者類別所設計，可讓多個應用程式，其中每個應用程式可以在理論上，有重疊的使用者名稱的使用者和具有相同名稱的角色，將可做為使用者存放區的單一資料庫。 若要允許這種彈性，資料庫會保留一份應用程式中`aspnet_Applications`資料表，以及每個使用者會與其中一個應用程式相關聯。 具體而言，`aspnet_Users`資料表有`ApplicationId`繫結中的記錄每個使用者的資料行`aspnet_Applications`資料表。

除了`ApplicationId`資料行`aspnet_Applications`表格也包含`ApplicationName`資料行，提供應用程式更方便人類的名稱。 當網站嘗試使用使用者帳戶，例如驗證使用者的認證從登入 頁面中，它必須告訴`SqlMembershipProvider`類別要使用之應用程式。 它通常會藉由提供應用程式名稱，這與這個值來自中的提供者的組態`Web.config`-特別是透過`applicationName`屬性。

但要是`applicationName`屬性中未指定`Web.config`嗎？ 在此情況下的成員資格系統會使用應用程式根路徑，做為`applicationName`值。 如果`applicationName`中未明確設定屬性`Web.config`，，就可能的開發環境和生產環境使用不同的應用程式根目錄，因此會與不同的應用程式相關聯在 應用程式服務的名稱。 如果在開發環境中建立這些使用者就需要，就會發生這種不相符`ApplicationId`的不相符的值`ApplicationId`用於生產環境的值。 最後結果就是贏得 t 那些使用者能夠登入。

> [!NOTE]
> 如果您發現自己處於這種情況下-使用使用者帳戶複製到不符合生產環境`ApplicationId`值-您可以撰寫查詢來更新這些不正確`ApplicationId`值來`ApplicationId`用於生產環境。 更新之後，在開發環境中建立其帳戶的使用者現在能夠登入生產環境的 web 應用程式。


好消息是，沒有簡單的步驟，以確保兩個環境都使用相同，您可以採取`ApplicationId`-明確設定`applicationName`屬性中`Web.config`所有您的應用程式服務提供者。 明確設定`applicationName`"BookReviews"屬性中`<membership>`並`<roleManager>`項目與此程式碼片段從`Web.config`顯示。

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

如需設定的詳細討論`applicationName`屬性和理由，請參閱[ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s 部落格文章： [*永遠設定應用程式名稱設定 ASP.NET 成員資格和其他提供者時的屬性*](https://weblogs.asp.net/scottgu/443634)。

### <a name="managing-user-accounts-in-the-production-environment"></a>管理生產環境中的使用者帳戶

ASP.NET 網站管理工具 (WSAT) 可讓您輕鬆地建立和管理使用者帳戶、 定義和套用角色，以及拼出使用者和角色型授權規則。 您可以啟動從 Visual Studio WSAT，移至 方案總管 並按一下 ASP.NET 組態 圖示，或移至 網站 或 專案功能表，然後選取 ASP.NET 組態 功能表項目。 不幸的是，WSAT 只能使用本機網站。 因此，您無法從您的工作站使用 WSAT，管理在生產環境中的網站。

好消息是功能的由 WSAT 所有公開是功能的以程式設計方式透過成員資格和角色 Api;此外，許多 WSAT 畫面使用標準的 ASP.NET 登入相關控制項。 簡單地說，您可以將 ASP.NET 網頁加入您的網站，提供必要的管理功能。

還記得先前的教學課程已更新的書籍評論 web 應用程式，使`~/Admin`資料夾，然後此資料夾已設定為只允許系統管理員角色中的使用者。 將頁面新增至名為該資料夾`CreateAccount.aspx`從系統管理員可以建立新的使用者帳戶。 此頁面會使用 CreateUserWizard 控制項來顯示使用者介面和後端邏輯來建立新的使用者帳戶。 更多、 哪些 s 我自訂控制項，以包含提示是否有新的使用者也應該新增至系統管理員角色的核取方塊 （請參閱 [圖 5]）。 利用最少的工作中，您可以建立一組自訂的頁面實作的使用者和角色管理相關工作，否則會 WSAT 所提供。

> [!NOTE]
> 針對更多有關使用成員資格和角色 Api，以及登入相關的 ASP.NET Web 控制項中，務必先閱讀我[*網站安全性教學課程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。 如需自訂 CreateUserWizard 控制項的詳細資訊，請參閱[*建立使用者帳戶*](../../older-versions-security/membership/creating-user-accounts-vb.md)並[*儲存額外的使用者資訊*](../../older-versions-security/membership/storing-additional-user-information-vb.md)教學課程中或查看[ *Erich Peterson* ](http://www.erichpeterson.com/) s 文章[*自訂 CreateUserWizard 控制項*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![系統管理員可以建立新的使用者帳戶](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**圖 5**： 系統管理員可以建立新的使用者帳戶 ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


如果您需要 WSAT 簽出的完整功能[*輪流您自己 Web Site Administration Tool*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)，作者 Dan Clem 逐步建置自訂的 WSAT 類似工具的程序。 Dan 分享他的應用程式的來源的程式碼 （在 C# 中)，並提供逐步指示，以將它新增至您裝載的網站。

## <a name="summary"></a>總結

部署使用的應用程式服務資料庫實作的 web 應用程式時您必須先確定生產環境資料庫的必要資料庫物件。 這些物件可以使用所述的技巧來新增*將資料庫部署*教學課程; 或者，您可以使用`aspnet_regsql.exe`工具，如我們在本教學課程中所見。 我們觸及圍繞在同步處理應用程式名稱，用於開發和生產環境 （這是很重要，如果您想要在生產環境中有效的使用者和開發環境中建立的角色） 和技術上的其他挑戰管理使用者和在生產環境中的角色。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [*ASP.NET SQL Server 註冊工具 (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*建立 SQL Server 應用程式服務資料庫*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*在 SQL Server 中建立成員資格結構描述*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*檢查 ASP.NET s 成員資格、 角色和設定檔*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*復原您自己的 Web Site Administration Tool*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*網站安全性教學課程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*網站系統管理工具概觀*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [上一頁](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [下一頁](strategies-for-database-development-and-deployment-vb.md)
