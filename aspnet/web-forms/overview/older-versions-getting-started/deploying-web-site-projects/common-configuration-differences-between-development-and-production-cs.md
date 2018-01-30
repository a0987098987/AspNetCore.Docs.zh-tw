---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: "一般組態差異開發和生產環境 (C#) |Microsoft 文件"
author: rick-anderson
description: "在先前的教學課程中我們會藉由將從開發環境的所有相關檔案複製到生產環境部署我們的網站。 不過，我..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 092362e3811213047820dab08efc16e1a1e75020
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="common-configuration-differences-between-development-and-production-c"></a>一般組態差異開發和生產環境 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> 在先前的教學課程中我們會藉由將從開發環境的所有相關檔案複製到生產環境部署我們的網站。 不過，是不常見的才會設定環境，其需要每個環境已經有唯一的 Web.config 檔案的差異。 本教學課程會檢查在一般組態中的差異，並會查看維護個別的組態資訊的策略。


## <a name="introduction"></a>簡介


最後兩個教學課程逐步進行簡單的 web 應用程式部署作業。 [*部署網站的 FTP 用戶端*](deploying-your-site-using-an-ftp-client-cs.md)教學課程示範了如何使用獨立的 FTP 用戶端的必要檔案複製到生產環境的開發環境。 先前的教學課程中， [*部署您的網站使用 Visual Studio*](deploying-your-site-using-visual-studio-cs.md)、 查閱在部署使用 Visual Studio 複製網站工具和發佈選項。 在這兩個教學課程中實際執行環境中的每個檔案已在開發環境中的檔案的副本。 不過，是不常見的實際執行環境，以不同於開發環境中的組態檔。 Web 應用程式的組態儲存在`Web.config`檔案，且通常包含 外部資源，例如資料庫、 web 和電子郵件伺服器的相關資訊。 它也說明了在某些情況下，例如採取的動作處理的例外狀況發生時要執行應用程式的行為。

部署 web 應用程式時務必正確的設定資訊結束在生產環境中。 在大部分情況下`Web.config`開發環境中的檔案無法複製到實際執行環境，做為是。 相反地，自訂的版本`Web.config`必須上傳到生產環境。 本教學課程簡短檢閱的一些較常見的組態差異;它也摘要列出一些技術來維護環境之間的不同的組態資訊。

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>一般設定開發和生產環境之間的差異

`Web.config`檔案包含 ASP.NET 應用程式的組態資訊的分類。 此組態資訊的某些部分是相同的環境。 例如，驗證設定和 URL 授權規則拼出`Web.config`檔案的`<authentication>`和`<authorization>`項目通常是相同的不論環境。 但是，根據環境的其他組態資訊-例如，外部資源的相關資訊通常有所不同。

資料庫連接字串是根據環境的基本和不同的組態資訊的使用範例。 當 web 應用程式資料庫與伺服器通訊，必須先建立連接，而且，只透過[連接字串](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)。 雖然也可以用硬式編碼直接在網頁或連接至資料庫的程式碼中的資料庫連接字串，但是建議您最好將它放入`Web.config`的[`<connectionStrings>`元素](https://msdn.microsoft.com/library/bf7sd233.aspx)使連接字串資訊是在單一且集中式位置。 有時候不同的資料庫用在開發期間比用在生產環境。因此，連接字串資訊必須是唯一的每個環境。

> [!NOTE]
> 未來的教學課程中，瀏覽部署資料導向應用程式，此時我們將探討如何將資料庫連接字串儲存在組態檔中的細節。


本質上不同的開發和生產環境的行為。 在開發環境中的 web 應用程式正在建立、 測試及偵錯的開發人員一小群。 在生產環境中許多不同的同時使用者造訪該相同的應用程式。 ASP.NET 包含數個功能，可協助開發人員在測試和偵錯應用程式，但這些功能應該停用的效能和安全性理由，在實際執行環境中。 讓我們看看一些此類組態設定。

### <a name="configuration-settings-that-impact-performance"></a>會影響效能的組態設定

當瀏覽 ASP.NET 網頁時第一次 （或第一次變更後），其宣告式標記必須先轉換成類別，這個類別必須進行編譯。 如果 web 應用程式會使用自動編譯網頁的程式碼後置類別需要進行編譯，太。 您可以設定各種編譯選項，透過`Web.config`檔案的[`<compilation>`元素](https://msdn.microsoft.com/library/s10awwz0.aspx)。

偵錯屬性是最重要的屬性中的其中一個`<compilation>`項目。 如果`debug`屬性設定為"true"，則在已編譯的組件包含偵錯 Visual Studio 中的應用程式時，所需要的偵錯符號。 但偵錯符號增加組件的大小，並執行程式碼時造成額外的記憶體需求。 此外，當`debug`屬性設定為"true"傳回的任何內容`WebResource.axd`未快取，這表示，每次使用者存取他們需要重新下載所傳回的靜態內容網頁`WebResource.axd`。

> [!NOTE]
> `WebResource.axd`是內建的 HTTP 處理常式在 ASP.NET 2.0 引進伺服器控制項用來擷取內嵌的資源，例如指令碼檔案、 影像、 CSS 檔案和其他內容。 如需有關如何`WebResource.axd`運作以及如何使用它來存取內嵌的資源的自訂伺服器控制項，請參閱[存取內嵌資源透過 URL 使用`WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx)。


`<compilation>`項目的`debug`屬性通常設定為"true"開發環境中。 事實上，這個屬性必須設定為"true"，才能偵錯 web 應用程式。如果您嘗試偵錯 ASP.NET 應用程式從 Visual Studio 和`debug`屬性設定為"false"時，Visual Studio 會顯示訊息，說明應用程式，無法偵錯直到`debug`屬性設為"true"，並將若要進行這項變更為您的方案。

您應該**從未**有`debug`屬性設定為"true"，在生產環境中，因為它對效能的影響。 本主題的更完整討論，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格文章：[不執行實際執行 ASP.NET 應用程式與`debug="true"`啟用](https://weblogs.asp.net/scottgu/442448)。

### <a name="custom-errors-and-tracing"></a>自訂的錯誤和追蹤

ASP.NET 應用程式中發生未處理的例外狀況時它會顯示最多三個項目的其中一個時間點發生執行階段：

- 會顯示一般的執行階段錯誤訊息。 此頁面會通知使用者，但執行階段錯誤，但不提供任何錯誤的詳細資訊。
- 會顯示例外狀況詳細資料訊息，其中包含只擲回的例外狀況的資訊。
- 會顯示自訂錯誤網頁，這是 ASP.NET 網頁，您建立顯示您想要的任何訊息。

遇到未處理的例外狀況時的事取決於`Web.config`檔案的[`<customErrors>`區段](https://msdn.microsoft.com/library/h0hfz6fc.aspx)。

在開發和測試應用程式時這樣做有助於查看瀏覽器中的任何例外狀況的詳細資料。 不過，實際執行應用程式中顯示例外狀況詳細資料是潛在的安全性風險。 此外，它是 unflattering，而且可讓您看起來不夠專業的網站。 在理想情況下，期間發生未處理的例外狀況在開發環境中的 web 應用程式會顯示例外狀況的詳細資料時相同的應用程式在生產環境中會顯示自訂錯誤網頁。

> [!NOTE]
> 預設值`<customErrors>`區段設定例外狀況詳細資料訊息頁面所造訪透過 localhost，且否則示範一般執行階段錯誤頁面時，才會顯示。 這不是理想的做法，但它以確保知道的預設行為並不會顯示為非本機訪客的例外狀況詳細資料。 未來的教學課程會檢查`<customErrors>`> 一節中更多詳細資料，並說明如何將自訂錯誤網頁顯示在生產環境中發生的錯誤。


追蹤另一項 ASP.NET 功能，在開發期間很有用。 追蹤，如果啟用，記錄每個連入要求的相關資訊，並提供特殊的網頁上， `Trace.axd`，來檢視最近的要求詳細資料。 您可以開啟和設定追蹤透過[`<trace>`元素](https://msdn.microsoft.com/library/6915t83k.aspx)中`Web.config`。

如果您啟用追蹤，請務必確定它已停用在生產環境中。 追蹤資訊包括 cookie、 工作階段資料，以及其他潛在的敏感資訊，因此務必停用在生產環境中的追蹤。 好消息是，根據預設，已停用追蹤和`Trace.axd`檔案僅可透過 localhost 存取。 如果您變更這些預設設定，在開發中確定，它們會關閉回生產環境中。

## <a name="techniques-for-maintaining-separate-configuration-information"></a>維護個別的組態資訊的技術

在開發和生產環境中具有不同的組態設定變得非常複雜的部署程序。 在先前的兩個教學課程部署程序涉及複製所有必要的檔案從開發到生產環境中，但該方法僅適用於組態資訊是否兩個環境中相同。 有各種不同的部署具有不同的組態資訊的應用程式的技術。 讓我們的目錄部分託管的 web 應用程式的這些選項。

### <a name="manually-deploying-the-production-environment-configuration-file"></a>手動部署生產環境的組態檔

最簡單的方式會維護兩個版本的`Web.config`檔案： 一個用於開發環境，一個用於實際執行環境。 將所有檔案複製到實際執行伺服器，在開發環境中部署網站至生產環境都需要*除了*如`Web.config`檔案。 相反地，實際執行環境專屬`Web.config`會將檔案複製到生產環境。

此方法不是非常複雜，但很容易實作，因為不常變更的組態資訊。 它適合用於應用程式與開發小組的單一 web 伺服器上主控的和不常變更其組態資訊。 手動部署使用獨立的 FTP 用戶端應用程式檔案時，它是最 tenable。 使用 Visual Studio 複製網站工具或 [發佈] 選項時您必須先部署特定交換`Web.config`部署之前，請以實際執行特定檔案，然後再將其交換部署完成之後。

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>變更組建或部署程序期間的設定

討論到目前為止已假設特定組建和部署處理。 許多較大的軟體專案擁有更正式的處理程序會使用開放原始碼，首頁成長或協力廠商工具。 這類專案，您可能可以自訂推入到生產環境之前適當地修改的組態資訊的組建或部署程序。 如果您要建置您 web 應用程式使用[MSBuild](http://en.wikipedia.org/wiki/MSBuild)， [NAnt](http://nant.sourceforge.net/)，或某些其他建置工具，您可以有可能加入建置步驟修改`Web.config`檔案以加入實際執行環境專屬的設定。 或您的部署工作流程無法以程式設計方式連接至原始檔控制伺服器並擷取適當`Web.config`檔案。

如需取得適當的組態資訊到生產環境的實際方法廣泛依據您的工具和工作流程而異。 因此，我們不會深入本主題將進一步。 如果您使用 MSBuild 或 NAnt 受歡迎的建置工具您可以找到部署文件和教學課程特有網頁搜尋透過這些工具。

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>管理組態差異透過 Web 部署專案增益集

在 2006 Microsoft Web 開發專案中的發行適用於 Visual Studio 2005。 Visual Studio 2008 增益集已於 2008年發行。 此增益集可讓 ASP.NET 開發人員建立個別的 Web 部署專案連同其 web 應用程式專案，建置時、 明確編譯 web 應用程式並複製檔案將部署到本機的輸出目錄。 Web 應用程式專案會在幕後使用 MSBuild。

根據預設，在開發環境的`Web.config`檔案複製到輸出目錄中，但您可以設定自訂 Web 部署專案

取得以下列方式複製到這個目錄的設定資訊：

- 透過`Web.config`檔案 > 一節更換時，您可以在其中指定要取代的區段和 XML 檔案，其中包含取代文字。
- 所提供的外部組態原始程式檔的路徑。 選取此選項，Web 部署專案複製特定`Web.config`到輸出目錄的檔案 (而非`Web.config`開發環境中使用的檔案)。
- 將自訂的規則加入至 Web 部署專案所使用的 MSBuild 檔案。

若要部署的 web 應用程式的組建 Web 部署專案，然後從專案的輸出資料夾複製檔案到生產環境。

若要深入了解使用 Web 部署專案的詳細資訊請參閱[Web 部署專案本文](https://msdn.microsoft.com/magazine/cc163448.aspx)從 2007 年 4 月發行的[MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx)，或洽詢進一步閱讀一節中的連結本教學課程的結尾。

> [!NOTE]
> 因為 Web 部署專案會實作為 Visual Studio 增益集，而且 Visual Studio Express Edition （包括 Visual Web Developer） 不支援增益集，您無法使用隨 Visual Web Developer Web 部署專案。


## <a name="summary"></a>總結

外部資源，在開發中的 web 應用程式的行為是通常不同於相同的應用程式處於生產環境。 例如，資料庫連接字串、 編譯選項和處理的例外狀況通常發生時的行為不同環境之間。 部署程序必須配合這些差異。 如同我們在本教學課程所討論，最簡單的方法是手動複製到生產環境的其他設定檔案。 可能會有更有質感的解決方案或搭配更正式的組建或部署程序可容納這類自訂項目時使用的 Web 部署專案增益集。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [連接字串說明](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [資料庫連接字串 @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [不執行實際執行 ASP.NET 應用程式與`debug="true"`啟用](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [依正常程序回應未處理的例外狀況-顯示使用者易記的錯誤網頁](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [如何： 使用 Visual Studio 2008 Web 部署專案嗎？](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [部署資料庫時，索引鍵的組態設定](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 Web 部署專案下載](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 Web 部署專案下載項目](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [VS 2008 Web 部署專案](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [VS 2008 Web 部署專案支援發行](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Web 部署專案](https://msdn.microsoft.com/magazine/cc163448.aspx)

>[!div class="step-by-step"]
[上一頁](deploying-your-site-using-visual-studio-cs.md)
[下一頁](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
