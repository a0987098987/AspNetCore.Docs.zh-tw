---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: ASP.NET 健康監視 (VB) 與記錄錯誤的詳細資料 |Microsoft 文件
author: rick-anderson
description: Microsoft 的健全狀況監視系統會提供簡單可自訂的方式記錄各種 web 事件，包括未處理的例外狀況。 本教學課程會逐步引導 thr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a457d4b8773f0f4ed343f5005c76f48a5cc178f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888938"
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>記錄錯誤的詳細資訊與 ASP.NET 健全狀況監視 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip)或[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Microsoft 的健全狀況監視系統會提供簡單可自訂的方式記錄各種 web 事件，包括未處理的例外狀況。 這個教學課程引導記錄至資料庫的未處理例外狀況，並通知使用者透過電子郵件訊息的開發人員設定監視系統健全狀況。


## <a name="introduction"></a>簡介

記錄是很有用的工具來監視部署的應用程式的健全狀況及診斷可能會發生任何問題。 它是特別重要的記錄，以便可以進行補救，在部署的應用程式中發生的錯誤。 `Error` ASP.NET 應用程式; 未處理的例外狀況時，就會引發事件[前述教學課程](processing-unhandled-exceptions-vb.md)示範了如何通知開發人員的錯誤並記錄其詳細資料，藉由建立的事件處理常式`Error`事件。 不過，建立`Error`記錄錯誤的詳細資料，並通知開發人員的事件處理常式是不必要的與 ASP 的可執行這項工作。網路的*健全狀況監視系統*。

監視系統健全狀況在 ASP.NET 2.0 引進，而且設計來監視部署的 ASP.NET 應用程式的健全狀況，應用程式或要求的存留期期間發生的事件記錄。 監視系統健全狀況所記錄的事件指*健康監視事件*或*Web 事件*，並且包含：

- 應用程式生命週期事件，例如當應用程式啟動或停止
- 安全性事件，包括登入嘗試失敗，失敗的 URL 授權要求
- 應用程式錯誤，包括未處理例外狀況，剖析例外狀況、 要求驗證例外狀況，以及在錯誤的其他類型之間的編譯錯誤的檢視狀態。

當健全狀況監視的事件引發時可以記錄至任何數量的指定*記錄來源*。 健全狀況監視系統隨附 Web 事件記錄到 Windows 事件記錄，或透過電子郵件和其他項目的 Microsoft SQL Server 資料庫的記錄檔來源。 您也可以建立您自己的記錄檔來源。

事件監視系統健全狀況記錄檔，以及使用，記錄檔來源中定義`Web.config`。 設定標記的幾行中，您可以使用健全狀況監視記錄至資料庫的所有未處理的例外狀況，並通知您透過電子郵件的例外狀況。

## <a name="exploring-the-health-monitoring-systems-configuration"></a>探索健全狀況監視系統的設定

健全狀況監視系統行為由其組態資訊時，位於所定義[`<healthMonitoring>`元素](https://msdn.microsoft.com/library/2fwh2ss9.aspx)中`Web.config`。 這個組態區段會定義，在其他方面，下列三個重要部分的資訊：

1. 健全狀況監視的事件，當引發，應記錄
2. 記錄檔來源，並
3. 每個健全狀況監視 (1) 中定義的事件如何對應至記錄檔來源 (2) 中定義。

這項資訊透過三個子系的組態項目指定： [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx)， [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx)，和[ `<rules>`](https://msdn.microsoft.com/library/fe5wyxa0.aspx)分別。

預設健全狀況監視的系統設定資訊位於`Web.config`檔案`%WINDIR%\Microsoft.NET\Framework\version\CONFIG`資料夾。 此預設設定資訊中，為求簡潔，移除某些標記如下所示：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

監視感興趣的事件中所定義的健全狀況`<eventMappings>`元素，其健全狀況監視的事件類別會提供人類看得好記的名稱。 上述項目，在標記中`<eventMappings>`項目指派人們易記名稱 」 的所有錯誤 」 的健全狀況監視類型的事件`WebBaseErrorEvent`名稱 」 失敗稽核 「 健全狀況監視類型的事件和`WebFailureAuditEvent`。

`<providers>`項目會定義記錄檔的來源，讓人類的易記名稱，並指定任何記錄檔來源特定的組態資訊。 第一個`<add>`項目定義了"EventLogProvider"提供者，它會記錄指定的健全狀況監視事件使用`EventLogWebEventProvider`類別。 `EventLogWebEventProvider`類別會記錄事件至 Windows 事件記錄檔。 第二個`<add>`項目會定義事件記錄到透過 Microsoft SQL Server 資料庫的 「 SqlWebEventProvider"提供者`SqlWebEventProvider`類別。 「 SqlWebEventProvider 」 組態中指定資料庫的連接字串 (`connectionStringName`) 其他組態選項。

`<rules>`元素會對應中指定的事件`<eventMappings>`來源登入的項目`<providers>`項目。 根據預設，ASP.NET web 應用程式會記錄所有未處理的例外狀況和稽核失敗 Windows 事件記錄檔。

## <a name="logging-events-to-a-database"></a>事件記錄到資料庫

健全狀況監視系統的預設組態可以新增的自訂 web 應用程式的 web 應用程式為基礎`<healthMonitoring>`應用程式的區段`Web.config`檔案。 您可以包含其他項目中的`<eventMappings>`， `<providers>`，和`<rules>`區段使用`<add>`項目。 若要移除預設組態使用的設定`<remove>`項目或使用`<clear />`移除其中一個區段的所有預設值。 讓我們設定活頁簿檢閱 web 應用程式使用 Microsoft SQL Server 資料庫記錄所有未處理的例外狀況`SqlWebEventProvider`類別。

`SqlWebEventProvider`類別屬於監視系統健全狀況和健全狀況監視指定的 SQL Server 資料庫的事件記錄檔。 `SqlWebEventProvider`類別必須是指定的資料庫包含名為預存程序`aspnet_WebEvent_LogEvent`。 這個預存程序會傳遞事件的詳細資料，並負責儲存事件的詳細資料。 好消息是您不需要建立此預存程序和資料表來儲存事件的詳細資料。 您可以將這些物件加入至您的資料庫使用`aspnet_regsql.exe`工具。

> [!NOTE]
> `aspnet_regsql.exe`工具已於回[*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-vb.md)我們加入時的 ASP 支援。網路的應用程式服務。 因此，活頁簿檢閱網站的資料庫已包含`aspnet_WebEvent_LogEvent`預存程序，會儲存事件資訊儲存至資料表，名為`aspnet_WebEvent_Events`。


一旦您擁有必要的預存程序和資料表加入至您的資料庫，剩下的就是要指示健全狀況監視，以記錄至資料庫的所有未處理的例外狀況。 完成這項作業將下列標記加入至您的網站`Web.config`檔案：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

健全狀況監視會使用上述組態標記`<clear />`抹除預先定義的健全狀況監視的設定資訊的項目`<eventMappings>`， `<providers>`，和`<rules>`區段。 然後將單一項目加入至每一個區段。

- `<eventMappings>`項目會定義單一健康監視名為 「 所有錯誤，」 每次處理的例外狀況發生時引發的感興趣的事件。
- `<providers>`項目會定義名為"SqlWebEventProvider 」 使用單一記錄檔來源`SqlWebEventProvider`類別。 `connectionStringName`屬性已設定為"ReviewsConnectionString"，這是我們連線的名稱中定義的字串`<connectionStrings>`> 一節。
- 最後，&lt;規則&gt;項目表示，當 「 所有錯誤 」 事件瓿，則應該記錄使用"SqlWebEventProvider 「 提供者。

此組態資訊會指示監視系統登書籍檢閱資料庫的所有未處理的例外狀況的健全狀況。

> [!NOTE]
> `WebBaseErrorEvent`才會引發事件的伺服器錯誤，則不會引發 HTTP 錯誤，例如找不到 ASP.NET 資源的要求。 這與不同的行為`HttpApplication`類別的`Error`伺服器和 HTTP 錯誤，就會引發的事件。


若要查看監視動作中的系統健全狀況，瀏覽的網站和產生執行階段錯誤造訪`Genre.aspx?ID=foo`。 您應該會看到適當的錯誤頁面-例外狀況詳細資料黃色螢幕的死 （當本機瀏覽） 或自訂錯誤網頁 （瀏覽網站時在生產環境中）。 在幕後，健全狀況監視系統會記錄到資料庫資訊時發生錯誤。 應該有一筆記錄`aspnet_WebEvent_Events`資料表 (請參閱**圖 1**); 這個記錄包含剛剛發生執行階段錯誤的相關資訊。

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**圖 1**： 錯誤的詳細資料登入到`aspnet_WebEvent_Events`資料表  
([按一下以檢視完整大小的影像](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>在網頁中顯示的錯誤記錄檔

網站的目前組態，健全狀況監視系統會記錄到資料庫所有未處理的例外狀況。 但是，健全狀況監視不提供任何機制可檢視錯誤記錄檔，透過網頁。 不過，您可以建立 ASP.NET 網頁，會顯示資料庫的這項資訊。 （我們會暫時發現，您可以選擇將電子郵件中傳送給您的錯誤詳細資料。）

如果您建立這樣的頁面，請確定您採取步驟，只允許已授權的使用者以檢視錯誤詳細資料。 如果您的網站已經使用使用者帳戶，然後您可以使用 URL 授權規則來限制存取權給特定使用者或角色頁面。 如需有關如何授與或限制存取已登入使用者為基礎的 web 網頁的詳細資訊，請參閱我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。

> [!NOTE]
> 後續的教學課程會探索名為 ELMAH 替代錯誤記錄和通知系統。 ELMAH 包含內建的機制，以檢視錯誤記錄檔從網頁上以及 RSS 摘要。


## <a name="logging-events-to-email"></a>事件記錄到電子郵件

監視系統健全狀況包含 「 記錄 」 事件，以電子郵件訊息的記錄來源提供者。 記錄檔來源包含相同的資訊記錄至電子郵件訊息內文中的資料庫。 您可以使用此記錄檔來源到特定健全狀況監視事件發生時通知開發人員。

讓我們來更新活頁簿檢閱網站的設定，讓我們收到的電子郵件時例外狀況，就會發生。 若要這麼做，我們需要執行三個工作：

1. 設定 ASP.NET web 應用程式傳送電子郵件。 這是藉由指定 電子郵件訊息會透過傳送`<system.net>`組態項目。 如需有關傳送電子郵件訊息中的 ASP.NET 應用程式是指[ASP.NET 中的 傳送電子郵件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)和[System.Net.Mail 常見問題集](http://systemnetmail.com/)。
2. 登錄中的電子郵件記錄來源提供者`<providers>`項目，並
3. 新增項目以`<rules>`將 「 所有錯誤 」 事件對應至在步驟 (2) 中新增的記錄檔來源提供者的項目。

監視系統健全狀況包含兩個電子郵件記錄來源提供者類別：`SimpleMailWebEventProvider`和`TemplatedMailWebEventProvider`。 [ `SimpleMailWebEventProvider`類別](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx)傳送純文字電子郵件訊息，其中包含事件詳細資料，並提供一些自訂的電子郵件內文。 與[`TemplatedMailWebEventProvider`類別](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx)指定 ASP.NET 網頁，其呈現的標記用於做為主體電子郵件訊息。 [ `TemplatedMailWebEventProvider`類別](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx)讓您的內容和格式的電子郵件訊息，更進一步控制，但需要更多的前方工作因為您需要建立 ASP.NET 網頁，會產生電子郵件訊息的本文。 本教學課程著重於使用`SimpleMailWebEventProvider`類別。

更新健全狀況監視系統的`<providers>`中的項目`Web.config`要包含的記錄檔來源檔`SimpleMailWebEventProvider`類別：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

使用上述的標記`SimpleMailWebEventProvider`類別做為記錄檔來源提供者，並將其指派的易記名稱 」 EmailWebEventProvider"。 此外，`<add>`屬性包含其他組態選項，例如 收件者，以及來自的電子郵件地址。

與電子郵件記錄來源定義，剩下的就是要指示健全狀況監視系統来用於此來源 「 記錄 」 未處理例外狀況。 這藉由加入新的規則在`<rules>`> 一節：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

`<rules>`區段現在包含兩個規則。 第一個，名為 [所有錯誤以電子郵件]，會將所有未處理的例外狀況傳送到 「 EmailWebEventProvider 」 記錄檔來源。 此規則的效果網站上的錯誤的相關詳細資料傳送至指定的位址。 「 所有錯誤到資料庫 」 規則會記錄至站台的資料庫的錯誤詳細資料。 因此，每次處理的例外狀況發生時的站台上其詳細資料一併記錄到資料庫及傳送至指定的電子郵件地址。

**圖 2**示範所產生的電子郵件`SimpleMailWebEventProvider`類別造訪時`Genre.aspx?ID=foo`。

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**圖 2**: 電子郵件訊息中傳送的錯誤詳細資料  
([按一下以檢視完整大小的影像](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>總結

ASP.NET 健康監視系統的設計可讓系統管理員可以監視已部署的 web 應用程式的健全狀況。 展開某些動作，例如當應用程式停止時，當使用者成功登入站台，或發生未處理的例外狀況時，會引發健全狀況監視的事件。 這些事件可以記錄至任何數量的記錄檔來源。 本教學課程示範了如何記錄至資料庫，以及透過電子郵件訊息的未處理例外狀況的詳細資料。

本教學課程著重於使用記錄未處理的例外狀況，但請注意，健全狀況監視設計來測量的已部署的 ASP.NET 應用程式的整體健全狀況，而且包含豐富的健全狀況監視的事件和記錄來源不監視的健全狀況此探索。 什麼是多個項目，您可以建立您自己的健全狀況監視的事件和記錄檔來源，需要發生。 如果您有興趣深入了解健康狀態監控，理想的第一個步驟是閱讀[Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)的[健全狀況監視常見問題集](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)。 接下來，請參閱[How To： 使用 ASP.NET 2.0 中的健全狀況監視](https://msdn.microsoft.com/library/ms998306.aspx)。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 健康監視概觀](https://msdn.microsoft.com/library/bb398933.aspx)
- [設定和自訂監視的 ASP.NET 系統的健全狀況](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [常見問題集-健全狀況監視在 ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [如何： 傳送健全狀況監視通知的電子郵件](https://msdn.microsoft.com/library/ms227553.aspx)
- [如何： 使用 ASP.NET 中的健全狀況監視](https://msdn.microsoft.com/library/ms998306.aspx)
- [在 ASP.NET 監視的健全狀況](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [上一頁](processing-unhandled-exceptions-vb.md)
> [下一頁](logging-error-details-with-elmah-vb.md)
