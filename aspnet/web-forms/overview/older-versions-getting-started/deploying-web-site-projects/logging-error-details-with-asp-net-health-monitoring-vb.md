---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: 使用 ASP.NET 狀況監控 (VB) 中記錄錯誤的詳細資料 |Microsoft Docs
author: rick-anderson
description: Microsoft 的健全狀況監視系統會提供簡單且可自訂的方式來記錄各種 web 事件，包括未處理的例外狀況。 本教學課程將逐步引導儲存...
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: a19c1dc6ad5b3b45501befded4d8f14f7549b019
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809919"
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>記錄錯誤的詳細資料，使用 ASP.NET 狀況監控 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip)或[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Microsoft 的健全狀況監視系統會提供簡單且可自訂的方式來記錄各種 web 事件，包括未處理的例外狀況。 本教學課程會逐步設定狀況監控系統，以記錄至資料庫的未處理例外狀況，並通知開發人員透過電子郵件訊息。


## <a name="introduction"></a>簡介

記錄是很有用的工具來監視部署的應用程式的健全狀況及診斷任何可能發生的問題。 它是特別重要的記錄，以便可以進行補救，已部署的應用程式中發生的錯誤。 `Error`處理的例外狀況發生在 ASP.NET 應用程式; 時，就會引發事件[前述教學課程](processing-unhandled-exceptions-vb.md)示範了如何通知錯誤的開發人員，並藉由建立事件處理常式，如記錄其詳細資料`Error`事件。 不過，建立`Error`記錄錯誤的詳細資料，並告知開發人員的事件處理常式是不必要的與 ASP 的可執行這項工作。NET 的*狀況監控系統*。

狀況監控系統 ASP.NET 2.0 中引進，設計來監視部署的 ASP.NET 應用程式的健全狀況，在應用程式或要求的存留期期間發生的事件記錄。 狀況監控系統所記錄的事件指*健康監視事件*或是*Web 事件*，並包含：

- 應用程式存留期事件，例如當應用程式啟動或停止
- 安全性事件，包括登入嘗試失敗，以及失敗的 URL 授權要求
- 應用程式錯誤，包括未處理例外狀況，剖析例外狀況、 要求驗證例外狀況和其他類型的錯誤之間的編譯錯誤的檢視狀態。

當健全狀況監視的事件引發時可以記錄至任何數目的指定*記錄來源*。 狀況監控系統隨附於 Web 事件記錄到至 Windows 事件記錄檔中，或透過電子郵件訊息，以及其他的 Microsoft SQL Server 資料庫的記錄檔來源。 您也可以建立您自己的記錄檔來源。

中所定義的事件監視系統健全狀況記錄檔，以及使用，記錄檔來源`Web.config`。 只要幾行的組態標記中，您可以使用健全狀況監視記錄加入資料庫的所有未處理的例外狀況，並通知您透過電子郵件的例外狀況。

## <a name="exploring-the-health-monitoring-systems-configuration"></a>探索健全狀況監視系統的組態

狀況監控系統的行為由其組態資訊定義，位於[`<healthMonitoring>`項目](https://msdn.microsoft.com/library/2fwh2ss9.aspx)在`Web.config`。 這個組態區段會定義，以及其他項目中，下列三個重要部分的資訊：

1. 健康監視事件，當引發時，應該記錄
2. 記錄檔來源，以及
3. 每個健全狀況監視 (1) 中定義的事件對應至記錄檔來源的方式 (2) 中所定義。

這項資訊透過三個子系的組態項目指定： [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx)， [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx)，以及[ `<rules>`](https://msdn.microsoft.com/library/fe5wyxa0.aspx)分別。

預設健全狀況監視的系統設定資訊位於`Web.config`檔案中`%WINDIR%\Microsoft.NET\Framework\version\CONFIG`資料夾。 此預設設定資訊中，為求簡潔，移除某個標記與如下所示：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

監視感興趣的事件中所定義的健全狀況`<eventMappings>`元素，其健康監視事件的類別會提供人類看得好記的名稱。 在上述的標記`<eventMappings>`項目會人類的易記名稱 」 的所有錯誤 「 指定的健全狀況監視類型的事件`WebBaseErrorEvent`名稱 [失敗稽核] 健全狀況監視類型的事件和`WebFailureAuditEvent`。

`<providers>`項目會定義記錄檔的來源，讓他們人類的易記名稱，並指定任何記錄檔來源的特定組態資訊。 第一個`<add>`項目會定義 「 EventLogProvider"提供者，它會記錄指定的健全狀況監視使用的事件`EventLogWebEventProvider`類別。 `EventLogWebEventProvider`類別的事件記錄至 Windows 事件記錄檔。 第二個`<add>`項目會定義 「 SqlWebEventProvider"提供者，將事件記錄到 Microsoft SQL Server 資料庫透過`SqlWebEventProvider`類別。 「 SqlWebEventProvider 」 組態中指定資料庫的連接字串 (`connectionStringName`) 等其他組態選項。

`<rules>`項目對應中指定的事件`<eventMappings>`登入來源的項目`<providers>`項目。 根據預設，ASP.NET web 應用程式會記錄所有未處理的例外狀況，而稽核失敗 Windows 事件記錄檔。

## <a name="logging-events-to-a-database"></a>事件記錄到資料庫

狀況監控系統的預設組態可以新增的自訂 web 應用程式的 web 應用程式為基礎`<healthMonitoring>`一節，以應用程式的`Web.config`檔案。 您可以包含在其他項目`<eventMappings>`， `<providers>`，並`<rules>`區段，方法是使用`<add>`項目。 若要儦蘤飶從預設組態，請使用`<remove>`項目或使用`<clear />`移除其中一個區段的所有預設值。 讓我們設定的書籍評論 web 應用程式使用 Microsoft SQL Server 資料庫記錄所有未處理的例外狀況`SqlWebEventProvider`類別。

`SqlWebEventProvider`類別是狀況監控系統的一部分，而記錄健康監視事件，以指定的 SQL Server 資料庫。 `SqlWebEventProvider`類別必須是指定的資料庫包含名為預存程序`aspnet_WebEvent_LogEvent`。 這個預存程序會傳遞事件的詳細資料，並負責儲存事件的詳細資料。 好消息是您不需要建立此預存程序和資料表來儲存事件的詳細資料。 您可以將這些物件新增至您的資料庫使用`aspnet_regsql.exe`工具。

> [!NOTE]
> `aspnet_regsql.exe`討論工具回到[*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-vb.md)當我們新增了 asp 的支援。NET 的應用程式服務。 因此，書籍評論網站的資料庫已經包含`aspnet_WebEvent_LogEvent`預存程序，將名為事件資訊儲存在`aspnet_WebEvent_Events`。


一旦您擁有所需的預存程序和資料表加入至您的資料庫，全都是指示記錄到資料庫的所有未處理的例外狀況監視的健全狀況。 將下列標記新增至您的網站完成這項作業`Web.config`檔案：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

健全狀況監視會使用上述的組態標記`<clear />`抹除預先定義的健康情況監視來自組態資訊的項目`<eventMappings>`， `<providers>`，和`<rules>`區段。 接著，它會加入一個項目至每個區段。

- `<eventMappings>`項目會定義單一的健康狀態監視名為 「 所有錯誤 」，會引發未處理的例外狀況發生時的感興趣的事件。
- `<providers>`項目會定義名為"SqlWebEventProvider 」 使用的單一記錄檔來源`SqlWebEventProvider`類別。 `connectionStringName`屬性已設定為"ReviewsConnectionString 」，也就是我們連接的名稱中所定義的字串`<connectionStrings>`一節。
- 最後，&lt;規則&gt;項目表示，當 「 所有錯誤 」 事件瓿，它應該會記錄使用"SqlWebEventProvider 「 提供者。

此組態資訊會指示監視系統，以記錄所有未處理的例外狀況的書籍評論資料庫的健全狀況。

> [!NOTE]
> `WebBaseErrorEvent`才會引發事件的伺服器錯誤，它就不會引發 HTTP 錯誤，例如找不到 ASP.NET 資源的要求。 這與不同的行為`HttpApplication`類別的`Error`伺服器和 HTTP 錯誤，就會引發的事件。


若要查看狀況監控系統作用中的，瀏覽網站和產生執行階段錯誤，請造訪`Genre.aspx?ID=foo`。 您應該會看到適當的錯誤頁面-例外狀況詳細資料黃色死亡畫面 （當在本機瀏覽） 或自訂錯誤頁面 （瀏覽網站時在生產環境中）。 在幕後，狀況監控系統會記錄到資料庫資訊時發生錯誤。 應該有一筆記錄`aspnet_WebEvent_Events`資料表 (請參閱 < **圖 1**); 這個記錄包含剛剛才發生執行階段錯誤的相關資訊。

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**圖 1**： 錯誤詳細資料登入到`aspnet_WebEvent_Events`資料表  
([按一下以檢視完整大小的影像](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>在網頁上顯示的錯誤記錄檔

使用網站的目前設定時，監視系統健全狀況會記錄所有未處理的例外狀況至資料庫。 不過，健全狀況監視不提供任何機制可檢視錯誤記錄檔，透過網頁。 不過，您可以建置的 ASP.NET 網頁，會顯示資料庫的這項資訊。 （我們暫時會發現，您可以選擇以電子郵件傳送給您的錯誤詳細資料。）

如果您建立這類網頁時，請確定您採取行動，只允許已獲授權的使用者若要檢視錯誤詳細資料。 如果您的網站已採用使用者帳戶，則您可以使用 URL 授權規則來限制存取權給特定使用者或角色 頁面。 如需有關如何授與或限制存取登入的使用者為基礎的 web 網頁的詳細資訊，請參閱我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。

> [!NOTE]
> 後續的教學課程會探索名為 ELMAH 替代錯誤記錄和通知系統。 ELMAH 包含內建的機制，以檢視錯誤記錄檔從這兩個網頁，並透過 rss 摘要。


## <a name="logging-events-to-email"></a>事件記錄到電子郵件

狀況監控系統包含電子郵件訊息 [記錄] 事件記錄檔來源提供者。 記錄檔來源包含相同的資訊記錄至電子郵件訊息內文中的資料庫。 若要在特定的健全狀況監視事件發生時，通知開發人員，您可以使用此記錄檔來源。

讓我們更新網站的設定，讓我們收到一封電子郵件時例外狀況發生的書籍評論。 若要這麼做，我們需要執行三項工作：

1. 設定 ASP.NET web 應用程式傳送電子郵件。 這可以藉由指定透過電子郵件訊息的傳送方式`<system.net>`組態項目。 如需有關傳送電子郵件訊息中的 ASP.NET 應用程式來參考[在 ASP.NET 中的 傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)並[System.Net.Mail 常見問題集](http://systemnetmail.com/)。
2. 註冊電子郵件記錄來源提供者中的`<providers>`項目，以及
3. 新增項目以`<rules>`項目會對應至在步驟 (2) 中新增的記錄檔來源提供者的 「 所有錯誤 」 事件。

狀況監控系統包含兩個電子郵件記錄來源提供者類別：`SimpleMailWebEventProvider`和`TemplatedMailWebEventProvider`。 [ `SimpleMailWebEventProvider`類別](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx)傳送純文字電子郵件訊息，其中包含事件詳細資料，並提供一些自訂的電子郵件內文。 具有[`TemplatedMailWebEventProvider`類別](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx)指定 ASP.NET 網頁的呈現的標記會作為主體電子郵件訊息。 [ `TemplatedMailWebEventProvider`類別](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx)可讓您的內容和格式的電子郵件訊息中，更進一步控制，但需要更多的前置工作，您必須建立 ASP.NET 網頁所產生的電子郵件訊息本文。 本教學課程著重於使用`SimpleMailWebEventProvider`類別。

更新監視系統的健康情況`<providers>`中的項目`Web.config`檔案，以包含的記錄檔來源`SimpleMailWebEventProvider`類別：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

上述標記會使用`SimpleMailWebEventProvider`類別作為記錄檔來源提供者，並將其指派的易記名稱 「 EmailWebEventProvider"。 此外，`<add>`屬性包含其他組態選項，例如 收件者，以及來自電子郵件地址。

與電子郵件記錄來源定義，全都是指示健全狀況監視系統，才能使用此來源以 「 記錄 」 未處理例外狀況。 這藉由加入新的規則，在`<rules>`區段：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

`<rules>`區段現在包含兩個規則。 第一個名為 「 所有錯誤以電子郵件 」，會將所有未處理的例外狀況傳送至 「 EmailWebEventProvider 」 記錄檔來源。 此規則的效果網站上的錯誤相關的詳細資料傳送至指定的位址。 「 所有錯誤到資料庫 」 規則會記錄至站台資料庫的錯誤詳細資料。 因此，每次發生站台上其詳細資料的未處理的例外狀況時一併記錄到資料庫及傳送至指定的電子郵件地址。

**圖 2**顯示所產生的電子郵件`SimpleMailWebEventProvider`類別，在瀏覽`Genre.aspx?ID=foo`。

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**圖 2**: 錯誤詳細資料會傳送電子郵件訊息  
([按一下以檢視完整大小的影像](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>總結

ASP.NET 健康監視系統可讓系統管理員可以監視已部署的 web 應用程式的健全狀況。 展開特定的動作，例如當應用程式停止時，當使用者成功登入網站，或未處理的例外狀況發生時，會引發健康情況監視的事件。 這些事件可以記錄至任何數目的記錄檔來源。 本教學課程會示範如何將記錄的未處理例外狀況詳細資料的資料庫，以及透過電子郵件訊息。

本教學課程著重於使用狀況監控記錄未處理的例外狀況，但請記住，健康狀態監控設計來測量所部署的 ASP.NET 應用程式的整體健全狀況，以及包括豐富的健全狀況監視的事件並不記錄來源此探索。 什麼是多個項目，您可以建立您自己的健全狀況監視的事件和記錄檔來源，需要發生。 如果您有興趣深入了解健康狀態監視，良好的第一個步驟是閱讀[Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)的[健全狀況監視常見問題集](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)。 接下來，請參閱[How To： 使用 ASP.NET 2.0 中的健全狀況監視](https://msdn.microsoft.com/library/ms998306.aspx)。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 健康監視概觀](https://msdn.microsoft.com/library/bb398933.aspx)
- [設定和自訂健全狀況監視的 ASP.NET 的系統](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [常見問題集-在 ASP.NET 2.0 中監視的健全狀況](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [如何： 傳送電子郵件健康情況監視通知](https://msdn.microsoft.com/library/ms227553.aspx)
- [如何： 使用 ASP.NET 的健康狀態監視](https://msdn.microsoft.com/library/ms998306.aspx)
- [在 ASP.NET 中監視的健全狀況](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [上一頁](processing-unhandled-exceptions-vb.md)
> [下一頁](logging-error-details-with-elmah-vb.md)
