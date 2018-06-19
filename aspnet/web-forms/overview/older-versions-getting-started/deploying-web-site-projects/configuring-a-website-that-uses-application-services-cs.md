---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
title: 設定網站使用應用程式服務 (C#) |Microsoft 文件
author: rick-anderson
description: ASP.NET 2.0 版導入了一系列的應用程式服務，而這是.NET Framework 的一部分，並做為建置組塊套件服務 yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 1e33d1c6-3f9f-4c26-81e2-2a8f8907bb05
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
msc.type: authoredcontent
ms.openlocfilehash: da4ef328e3461e96fbb0cdca156ce1b9a076748f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890651"
---
<a name="configuring-a-website-that-uses-application-services-c"></a>設定網站使用應用程式服務 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_CS.zip)或[下載 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_cs.pdf)

> ASP.NET 2.0 版導入了一系列的應用程式服務，而這是.NET Framework 的一部分而且做為建置組塊服務可讓您將豐富的功能加入至 web 應用程式套件。 本教學課程會探討如何使用應用程式服務在生產環境中設定網站，以及位址管理使用者帳戶和角色在實際執行環境的一般問題。


## <a name="introduction"></a>簡介

ASP.NET 2.0 版導入了一系列的*應用程式服務*，.NET Framework 的一部分，而且僅做為建置組塊的一組服務，您可以使用將豐富的功能加入至 web 應用程式。 應用程式服務包括：

- **成員資格**-的 API，用於建立及管理使用者帳戶。
- **角色**-的 API，用於使用者分類成群組。
- **設定檔**-的應用程式開發介面來儲存自訂的使用者特定的內容。
- **站台對應**-的 API，用於定義階層，然後透過導覽控制項，例如功能表和階層連結列會顯示表單中的站台 s 邏輯結構。
- **個人化**-的 API，用於維護自訂喜好設定，最常搭配[ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx)。
- **健全狀況監視**-的 API，用於監視效能、 安全性、 錯誤和執行 web 應用程式的其他系統健康情況計量。
  

應用程式服務 Api 並不限於特定的實作。 相反地，您可以指示需要使用特定的應用程式服務*提供者*，和該提供者會實作使用特定技術的服務。 最常使用的提供者，針對以網際網路為基礎的 web 應用程式裝載在虛擬主機公司是使用 SQL Server 資料庫實作這些提供者。 例如，`SqlMembershipProvider`是將使用者帳戶資訊儲存在 Microsoft SQL Server 資料庫中的成員資格 api 提供者。

部署應用程式時使用的應用程式服務和 SQL Server 提供者新增的一些挑戰。 首先，應用程式服務的資料庫物件必須在開發和生產資料庫上正確建立，並適當地初始化。 另外還有必須做的重大組態設定。

> [!NOTE]
> 應用程式服務 Api 所設計使用[*提供者模型*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，允許 API 的實作細節，以在執行階段提供的設計模式。 .NET Framework 隨附許多應用程式服務提供者可用，例如`SqlMembershipProvider`和`SqlRoleProvider`、 哪些是提供者的成員資格和角色 Api 使用 SQL Server 資料庫實作。 您也可以建立和外掛程式的自訂提供者。 事實上，活頁簿檢閱 web 應用程式已經包含站台對應應用程式開發介面的自訂提供者 (`ReviewSiteMapProvider`)，這會建構中的資料從網站地圖`Genres`和`Books`資料庫中的資料表。


本教學課程開頭看我擴充書籍檢閱 web 應用程式使用的成員資格和角色應用程式開發介面的方式。 它接著會逐步部署應用程式服務會使用 SQL Server 資料庫實作和結束時，會定址管理使用者帳戶和角色在實際執行環境的一般問題的 web 應用程式。

## <a name="updates-to-the-book-reviews-application"></a>活頁簿檢閱應用程式的更新

在過去幾書籍檢閱 web 應用程式已更新從靜態網站動態的資料驅動的 web 應用程式的教學課程會使用一組管理頁面來管理內容類型和評論完成。 不過，此系統管理 」 區段目前未受保護-任何使用者都知道 （或猜測） 的管理頁面的 URL 可以 waltz 中和建立、 編輯或刪除在網站上的檢閱。 保護網站的某些部分的常見方式是實作使用者帳戶，然後使用 URL 授權規則限制存取特定使用者或角色。 活頁簿檢閱 web 應用程式可供下載與本教學課程中支援使用者帳戶和角色。 它具有單一角色定義名為系統管理員和只有此角色中的使用者可以存取 [管理] 頁面。

> [!NOTE]
> 我 ve 書籍檢閱 web 應用程式中建立三個使用者帳戶： Scott、 Jisun 和 Alice。 所有的三個使用者擁有相同的密碼：**密碼 ！** Scott 和 Jisun 是系統管理員角色中，Alice 無法。 站台 s 非系統管理頁面就仍然可以存取匿名使用者。 也就是說，您不需要登入網站，除非您想要管理，在此情況下您必須登入系統管理員角色中的使用者身分。


活頁簿檢閱應用程式 s 主版頁面已更新為包含不同的使用者介面，用於驗證和匿名使用者。 如果匿名使用者造訪她看到右上角的登入連結的站台。 已驗證的使用者會看到此訊息中，「 歡迎回來， *username*！ 」 和登出的連結。該處 s 也登入頁面 (`~/Login.aspx`)，其中包含提供使用者介面與邏輯來驗證訪客的登入 Web 控制項。 只有系統管理員可以建立新的帳戶。 (有建立及管理中的使用者帳戶的頁面`~/Admin`資料夾。)

### <a name="configuring-the-membership-and-roles-apis"></a>設定成員資格和角色 Api

若要支援使用者帳戶，並將這些使用者到角色 （也就是 「 管理員 」 角色） 活頁簿檢閱 web 應用程式會使用成員資格和角色應用程式開發介面。 `SqlMembershipProvider`和`SqlRoleProvider`可以使用提供者類別，因為我們想要儲存 SQL Server 資料庫中的帳戶和角色的資訊。

> [!NOTE]
> 本教學課程的目的不是要在設定 web 應用程式支援的成員資格和角色應用程式開發介面詳細的檢查。 徹底看這些 Api，以及您需要設定網站来使用它們所採取的步驟，請閱讀我[*網站安全性教學課程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。


若要使用 SQL Server 資料庫，您必須先新增這些提供者使用的資料庫使用者帳戶所在的資料庫物件和儲存的角色資訊的應用程式服務。 這些必要的資料庫物件包括各種不同的資料表、 檢視和預存程序。 除非另有指定`SqlMembershipProvider`和`SqlRoleProvider`提供者類別會使用名為 SQL Server Express Edition 資料庫`ASPNETDB`位於應用程式的`App_Data`資料夾; 如果這類資料庫不存在，它會自動建立這些提供者在執行階段所需要的資料庫物件。

可以，且通常是理想的做法，來建立應用程式服務在網站 s 應用程式特定資料的儲存位置的相同資料庫中資料庫物件。 .NET Framework 隨附於名為的工具`aspnet_regsql.exe`指定的資料庫上安裝的資料庫物件。 我已事先並使用此工具，這些將物件加入至`Reviews.mdf`資料庫`App_Data`資料夾 （開發資料庫）。 我們會看到如何使用這項工具稍後在本教學課程中，當我們將這些物件加入至實際執行資料庫。

如果您加入應用程式服務資料庫的資料庫物件以外`ASPNETDB`必須將自訂`SqlMembershipProvider`和`SqlRoleProvider`提供者類別的組態，使其使用適當的資料庫。 若要自訂成員資格提供者新增[ *&lt;成員資格&gt;元素*](https://msdn.microsoft.com/library/1b9hw62f.aspx)內`<system.web>`一節中`Web.config`; 使用[ *&lt;roleManager&gt;元素*](https://msdn.microsoft.com/library/ms164660.aspx)設定角色提供者。 下列程式碼片段取自書籍檢閱應用程式的`Web.config`和顯示的成員資格和角色應用程式開發介面的設定。 注意兩者註冊新的提供者-`ReviewMembership`和`ReviewRole`-使用`SqlMembershipProvider`和`SqlRoleProvider`提供者，分別。

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample1.xml)]

`Web.config`檔案的`<authentication>`元素也已經設定為支援表單架構驗證。
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>限制存取的管理頁面

ASP.NET 簡化授與或拒絕存取特定檔案或資料夾的使用者或角色，透過其*URL 授權*功能。 (我們簡要討論中的 URL 授權*核心差異之間 IIS 和 ASP.NET 程式開發伺服器*教學課程和示範如何在 IIS 和 ASP.NET 程式開發伺服器將套用不同的靜態 URL 授權規則與動態內容。）因為我們想要禁止存取`~/Admin`資料夾中的系統管理員角色的使用者，我們需要加入此資料夾的 URL 授權規則。 具體而言，URL 授權規則需要讓系統管理員角色中的使用者，並拒絕所有其他使用者。 這藉由加入`Web.config`檔案`~/Admin`資料夾具有下列內容：

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample3.xml)]

ASP.NET 的 URL 授權功能以及如何使用它來拼出授權規則的使用者和角色，如需詳細資訊，應確定有閱讀[*使用者為基礎的授權*](../../older-versions-security/membership/user-based-authorization-cs.md)和[*角色為基礎的授權*](../../older-versions-security/roles/role-based-authorization-cs.md)教學課程，從我[*網站安全性教學課程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。

## <a name="deploying-a-web-application-that-uses-application-services"></a>部署 Web 應用程式使用應用程式服務

在部署應用程式服務和應用程式服務資訊儲存在資料庫中的提供者所使用的網站時，務必，生產資料庫上建立所需的應用程式服務的資料庫物件。 實際執行資料庫一開始並不包含這些物件，因此，當第一次部署應用程式 （或加入應用程式服務後第一次部署時），您必須採取額外步驟，即可取得這些必要的資料庫物件上實際執行資料庫。

部署使用應用程式服務，如果您想要複寫到生產環境的開發環境中建立的使用者帳戶的網站時，可能會發生另一項挑戰。 根據成員資格和角色的組態，則可能即使您已成功複製到實際執行資料庫開發環境中所建立的使用者帳戶，這些使用者無法登入 web 應用程式在生產環境中。 我們會查看此問題的原因，並討論如何避免這種情況。

ASP.NET 隨附 nice [*網站管理工具 (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) ，可以從 Visual Studio 啟動，並讓使用者透過以 web 為基礎的管理帳戶、 角色和授權規則介面。 不幸的是，WSAT 僅適用於本機網站，亦即無法使用遠端管理使用者帳戶、 角色和 web 應用程式在生產環境中的授權規則。 我們會探討不同的方式來實作您的生產網站 WSAT 類似的行為。

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>加入的資料庫物件使用 aspnet\_regsql.exe

*部署資料庫*教學課程示範如何將資料表和資料從開發資料庫複製到生產資料庫，而且這些技術當然可用來複製到應用程式服務資料庫物件實際執行資料庫。 另一個選項是`aspnet_regsql.exe`工具，可加入或從資料庫移除應用程式服務的資料庫物件。

> [!NOTE]
> `aspnet_regsql.exe`工具指定的資料庫上建立資料庫物件。 它不會移轉的那些資料庫物件中的資料開發資料庫從生產資料庫。 如果您想要開發資料庫中的使用者帳戶和角色資訊複製到實際執行資料庫使用所涵蓋的技術*部署資料庫*教學課程。


將 s 看看如何將資料庫物件加入至實際執行資料庫使用`aspnet_regsql.exe`工具。 開啟 Windows 檔案總管並瀏覽至您在電腦上，%WINDIR%\ 的.NET Framework 2.0 版目錄啟動Microsoft.NET\Framework\v2.0.50727。 您應該會那里發現`aspnet_regsql.exe`工具。 這項工具可從命令列，不過它也包含圖形化使用者介面。按兩下`aspnet_regsql.exe`檔案以啟動其圖形化元件。

此工具一開始會顯示啟動顯示畫面以說明其用途。 按一下旁邊前進至 「 選取安裝選項 畫面中，圖 1 所示。 從這裡您可以選擇加入應用程式服務資料庫物件，或從資料庫中移除。 由於我們想要將這些物件加入至實際執行資料庫，選取"設定應用程式服務的 SQL Server"選項，然後按一下 [下一步]。


[![選擇應用程式服務設定 SQL Server](configuring-a-website-that-uses-application-services-cs/_static/image2.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image1.jpg)

**圖 1**： 設定 SQL Server 應用程式服務的選擇 ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-cs/_static/image3.jpg))


在 < 選取伺服器和資料庫 > 畫面提示資訊以連接到資料庫。 輸入資料庫伺服器、 安全性認證和虛擬主機公司提供給您的資料庫名稱，然後按一下 [下一步]。

> [!NOTE]
> 輸入您的資料庫伺服器和認證之後，展開 [資料庫] 下拉式清單時，可能會收到錯誤。 `aspnet_regsql.exe`工具查詢`sysdatabases`系統資料表，以擷取在伺服器上，但某些虛擬主機公司鎖定其資料庫伺服器，因此這項資訊不會公開可用的資料庫清單。 如果您收到這個錯誤您可以直接在下拉式清單中輸入資料庫名稱。


[![提供您的資料庫的連接資訊與工具](configuring-a-website-that-uses-application-services-cs/_static/image5.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image4.jpg)

**圖 2**： 提供的工具與您的資料庫 s 連接資訊 ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-cs/_static/image6.jpg))


後續畫面摘要說明的動作即將進行，也就是應用程式服務的資料庫物件所要加入至指定的資料庫。 按一下旁邊的 完成此動作。 在幾分鐘之後, 會顯示最後的畫面，您會看到有 （請參閱圖 3） 已加入的資料庫物件。


[![成功 ！應用程式服務的資料庫物件已加入至實際執行資料庫](configuring-a-website-that-uses-application-services-cs/_static/image8.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image7.jpg)

**圖 3**： 成功 ！ 應用程式服務的資料庫物件已加入實際執行資料庫 ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-cs/_static/image9.jpg))


若要確認應用程式服務的資料庫物件已成功新增到實際執行資料庫，開啟 SQL Server Management Studio 並連接到您的生產資料庫。 如圖 4 所示，您現在應該會看到應用程式服務資料庫資料表在資料庫中， `aspnet_Applications`， `aspnet_Membership`， `aspnet_Users`，依此類推。


[![確認資料庫物件已新增到實際執行資料庫](configuring-a-website-that-uses-application-services-cs/_static/image11.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image10.jpg)

**圖 4**： 確認的資料庫物件已加入到實際執行資料庫 ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-cs/_static/image12.jpg))


您只需要使用`aspnet_regsql.exe`工具時部署 web 應用程式第一次，或已使用應用程式服務啟動後第一次。 一旦這些資料庫物件是在生產資料庫上它們贏得 t 不必重新加入或修改。

### <a name="copying-user-accounts-from-development-to-production"></a>複製使用者帳戶從開發到生產環境

當使用`SqlMembershipProvider`和`SqlRoleProvider`提供者類別，將應用程式服務資訊儲存在 SQL Server 資料庫中，使用者帳戶和角色的資訊會儲存在資料庫資料表，包括各種`aspnet_Users`， `aspnet_Membership`，`aspnet_Roles`，和`aspnet_UsersInRoles`，和其他項目。 如果您可以在開發期間建立使用者帳戶來開發環境中您可以藉由複製適用的資料庫資料表中對應記錄複寫在生產環境中的這些使用者帳戶。 如果您使用資料庫發行精靈 」 來部署您可能也要複製的記錄，也是在生產環境的開發工作建立的使用者帳戶可能會導致選取的應用程式服務資料庫物件。 但是，根據您的組態設定，您可能會發現這些開發中建立並複製到生產環境之帳戶的使用者會無法從生產網站登入。 呢？

`SqlMembershipProvider`和`SqlRoleProvider`提供者類別皆設計單一資料庫可作為使用者存放區的多個應用程式，其中每個應用程式可以在理論上，有重疊的使用者名稱與使用者和角色具有相同的名稱。 若要允許這種彈性，資料庫會保留一份應用程式中`aspnet_Applications`資料表，以及每個使用者都與上述任一應用程式。 具體來說，`aspnet_Users`資料表有`ApplicationId`繫結合作對每位使用者中之記錄的資料行`aspnet_Applications`資料表。

除了`ApplicationId`資料行，`aspnet_Applications`資料表也包含`ApplicationName`資料行，提供應用程式的人類看得更易記名稱。 網站嘗試使用 [使用者帳戶，例如先驗證使用者 s 來自的認證登入] 頁面上，它必須告知`SqlMembershipProvider`類別要使用之應用程式。 它通常會提供應用程式名稱，這和此值的來源中的提供者的組態`Web.config`-特別透過`applicationName`屬性。

但要是`applicationName`中未指定屬性`Web.config`嗎？ 在此情況下的成員資格系統會使用應用程式根路徑，做為`applicationName`值。 如果`applicationName`中未明確設定屬性`Web.config`，接著，則可能會開發環境和生產環境使用不同的應用程式根目錄，因此會有不同的應用程式相關聯在 應用程式服務的名稱。 如果發生這類不符的情形，則在開發環境中建立那些使用者就能`ApplicationId`不相符的值`ApplicationId`實際執行環境的值。 最後結果就是贏了 t 那些使用者無法登入。

> [!NOTE]
> 如果您發現自己在此情況下-複製到生產環境與不相符的使用者帳戶與`ApplicationId`值-您可以撰寫查詢以更新這些錯誤`ApplicationId`值`ApplicationId`用於生產環境。 一旦更新，在開發環境中建立之帳戶的使用者現在就可以登入實際執行的 web 應用程式。


好消息是簡單的步驟可確保兩個環境都使用相同`ApplicationId`-明確設定`applicationName`屬性`Web.config`所有應用程式的服務提供者。 明確地設定`applicationName`屬性中的"BookReviews"`<membership>`和`<roleManager>`項目從這個程式碼片段為`Web.config`顯示。

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample4.xml)]

如需設定的詳細討論`applicationName`屬性和原理，是指[ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s 部落格文章： [*永遠設定應用程式名稱屬性設定 ASP.NET 成員資格和其他提供者時*](https://weblogs.asp.net/scottgu/443634)。

### <a name="managing-user-accounts-in-the-production-environment"></a>在生產環境中管理使用者帳戶

ASP.NET 網站管理工具 (WSAT) 可讓您輕鬆地建立及管理使用者帳戶、 定義及套用角色和拼出使用者和角色為基礎的授權規則。 您可以啟動 Visual Studio 從 WSAT 移至 [方案總管] 並按一下 [ASP.NET 組態] 圖示，或移至 [網站] 或 [專案] 功能表，然後選取 [ASP.NET 組態] 功能表項目。 不幸的是，WSAT 只能搭配本機網站。 因此，您無法從工作站使用 WSAT 來管理生產環境中的網站。

好消息是功能的 WSAT 所提供所有公開都是功能的以程式設計方式透過成員資格和角色應用程式開發介面。此外，許多 WSAT 螢幕使用標準 ASP.NET 登入相關的控制項。 簡單地說，您可以將 ASP.NET 網頁加入您的網站，提供必要的管理功能。

前文提過早的教學課程，更新活頁簿檢閱 web 應用程式，以包含`~/Admin`資料夾，然後此資料夾已設定，只允許系統管理員角色中的使用者。 我將頁面加入到該資料夾中名為`CreateAccount.aspx`從系統管理員可以建立新的使用者帳戶。 此頁面會使用適用於 CreateUserWizard 控制項來顯示使用者介面和後端邏輯來建立新的使用者帳戶。 更多、 哪些 s 我自訂以包含提示是否有新的使用者也應該加入至管理員角色的核取方塊控制項 （請參閱圖 5）。 使用一些工作，您可以建立一組自訂的網頁可實作使用者和角色管理相關的工作，否則會 WSAT 所提供。

> [!NOTE]
> 登入相關的 ASP.NET Web 控制項，以及使用的成員資格和角色應用程式開發介面的詳細資訊，應確定有閱讀我[*網站安全性教學課程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。 如需自訂適用於 CreateUserWizard 控制項的詳細資訊，請參閱[*建立使用者帳戶*](../../older-versions-security/membership/creating-user-accounts-cs.md)和[*儲存額外的使用者資訊*](../../older-versions-security/membership/storing-additional-user-information-cs.md)教學課程中或簽出[*何 Peterson* ](http://www.erichpeterson.com/) s 文章[*自訂適用於 CreateUserWizard 控制項*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![系統管理員可以建立新的使用者帳戶](configuring-a-website-that-uses-application-services-cs/_static/image14.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image13.jpg)

**圖 5**： 系統管理員可以建立新的使用者帳戶 ([按一下以檢視完整大小的影像](configuring-a-website-that-uses-application-services-cs/_static/image15.jpg))


如果您需要的 WSAT 簽出的完整功能[*輪流您自己的網站管理工具*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)中的作者 Dan Clem 逐步解說建立自訂的 WSAT 類似工具的程序。 Dan 共用他的應用程式的來源的程式碼 （C# 中），並提供逐步指示，將它加入至您裝載的網站。

## <a name="summary"></a>總結

部署 web 應用程式使用應用程式服務資料庫實作時必須先確定生產資料庫的必要資料庫物件。 這些物件可以使用中討論的技術加入*部署資料庫*教學課程; 或者，您可以使用`aspnet_regsql.exe`工具，我們在本教學課程，了解。 其他的挑戰我們觸及中心周圍同步處理中 （這是很重要，如果您想要在生產環境中有效的使用者和開發環境中建立的角色） 的開發和生產環境和技術使用的應用程式名稱管理使用者和實際執行環境中的角色。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [*ASP.NET SQL Server 註冊工具 (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*建立適用於 SQL Server 的應用程式服務資料庫*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*在 SQL Server 中建立成員資格結構描述*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)
- [*檢查 ASP.NET s 成員資格、 角色和設定檔*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*復原您的網站管理工具*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*網站安全性教學課程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*網站管理工具概觀*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [上一頁](configuring-the-production-web-application-to-use-the-production-database-cs.md)
> [下一頁](strategies-for-database-development-and-deployment-cs.md)
