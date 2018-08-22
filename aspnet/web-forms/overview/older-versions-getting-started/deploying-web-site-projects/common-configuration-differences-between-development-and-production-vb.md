---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: 開發與生產環境 (VB) 之間的常見組態差異 |Microsoft Docs
author: rick-anderson
description: 在先前的教學課程中，我們會部署我們的網站所複製的所有相關的檔案從開發環境到生產環境。 不過，我...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: ec1d575fac4527db8f5bcab5f0d84e1f17eac46b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825303"
---
<a name="common-configuration-differences-between-development-and-production-vb"></a>開發與生產環境 (VB) 之間的常見組態差異
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> 在先前的教學課程中，我們會部署我們的網站所複製的所有相關的檔案從開發環境到生產環境。 不過，不可能有組態差異會使得每個環境都有唯一的 Web.config 檔案的環境發生的狀況。 本教學課程會檢查常見組態差異，並探討維護個別的組態資訊的策略。


## <a name="introduction"></a>簡介


最後兩個教學課程會逐步部署簡單的 web 應用程式。 [*部署您的網站使用 FTP 用戶端*](deploying-your-site-using-an-ftp-client-vb.md)教學課程示範了如何使用獨立 FTP 用戶端從開發環境，一直到生產階段複製必要的檔案。 先前的教學課程中， [*部署您的網站使用的 Visual Studio*](deploying-your-site-using-visual-studio-vb.md)、 探討使用 Visual Studio 複製網站工具和發佈選項的部署。 在這兩個教學課程中的生產環境中的每個檔案是一份檔案，以在開發環境。 不過，就經常以不同於在開發環境與生產環境中的組態檔。 Web 應用程式的組態儲存在`Web.config`檔案，並通常包括外部資源，例如資料庫、 web 和電子郵件伺服器的相關資訊。 它也詳細說明，請在某些情況下，例如採取的未處理的例外狀況發生時要採取的動作中的應用程式的行為。

部署 web 應用程式時很重要，正確的設定資訊最後在生產環境中。 在大部分情況下`Web.config`無法在開發環境中的檔案複製到與生產環境-是。 相反地，自訂的版本的`Web.config`需要上傳到生產環境。 本教學課程簡短回顧一些較常見的組態差異;本文件也摘要了維護不同的組態資訊在環境之間的一些技術。

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>開發和生產環境的常見組態差異

`Web.config`檔案包含各種 ASP.NET 應用程式的組態資訊。 部分組態資訊，無論是一樣的環境。 比方說，驗證設定和 URL 授權規則拼出`Web.config`檔案的`<authentication>`和`<authorization>`項目通常是相同的環境無關。 但其他組態資訊-例如有關外部資源，通常與不同的環境而定。

資料庫連接字串是不同的組態資訊的基本範例會根據環境。 當 web 應用程式資料庫與伺服器通訊，它必須先建立連線，並可透過[連接字串](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)。 雖然您可以用硬式編碼直接在網頁或連接至資料庫的程式碼中的資料庫連接字串，所以最好先將它放`Web.config`的[`<connectionStrings>`項目](https://msdn.microsoft.com/library/bf7sd233.aspx)以便在連接字串資訊是在單一的集中式位置。 通常不同的資料庫是在開發期間使用非用於生產環境。因此，連接字串資訊必須是唯一的每個環境。

> [!NOTE]
> 未來的教學課程會探索部署資料導向應用程式，此時我們將探討如何將資料庫連接字串儲存在組態檔中的細節。


開發和生產環境的預期的行為與本質上不同。 在開發環境中的 web 應用程式正在建立、 測試及偵錯的一小群開發人員。 在生產環境中由許多不同的同步使用者所造訪該相同的應用程式。 ASP.NET 包含數項功能，可協助開發人員測試和偵錯應用程式，但這些功能應停用效能和安全性理由，在生產環境中。 讓我們看看一些這類組態設定。

### <a name="configuration-settings-that-impact-performance"></a>會影響效能的組態設定

當瀏覽 ASP.NET 網頁時的第一次 （或第一次變更後），其宣告式標記必須先轉換成類別，這個類別必須編譯。 如果 web 應用程式使用自動編譯網頁的程式碼後置類別就必須重新編譯，太。 您可以設定編譯選項，透過各種`Web.config`檔案的[`<compilation>`項目](https://msdn.microsoft.com/library/s10awwz0.aspx)。

偵錯屬性是其中一個最重要的屬性，在`<compilation>`項目。 如果`debug`屬性設為"true"則表示已編譯的組件包含偵錯 Visual Studio 中的應用程式時所需的偵錯符號。 但是，偵錯符號增加組件的大小，並執行程式碼時，會造成額外的記憶體需求。 此外，當`debug`屬性設定為"true"所傳回的任何內容`WebResource.axd`未快取，這表示，每次使用者瀏覽他們必須重新下載靜態內容所傳回的頁面`WebResource.axd`。

> [!NOTE]
> `WebResource.axd` 是內建的 HTTP 處理常式在 ASP.NET 2.0 引進伺服器控制項用來擷取內嵌的資源，例如指令碼檔案、 影像、 CSS 檔案和其他內容。 如需有關如何`WebResource.axd`運作，以及如何使用它來存取內嵌的資源，從您的自訂伺服器控制項，請參閱[存取內嵌的資源透過 URL 使用`WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx)。


`<compilation>`項目的`debug`屬性通常設定為"true"在開發環境中。 事實上，此屬性必須設為"true"，才能在 web 應用程式進行偵錯如果您嘗試偵錯 ASP.NET 應用程式從 Visual Studio 和`debug`屬性設定為"false"時，Visual Studio 會顯示訊息，說明無法進行應用程式偵錯，直到`debug`屬性設為"true"，並將若要進行這項變更為您的供應項目。

您應該**從未**有`debug`屬性設定為"true"，在生產環境中，因為它對效能的影響。 如需本主題的深入討論，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格文章[不執行生產環境 ASP.NET 應用程式與`debug="true"`Enabled](https://weblogs.asp.net/scottgu/442448)。

### <a name="custom-errors-and-tracing"></a>自訂的錯誤和追蹤

在 ASP.NET 應用程式中處理的例外狀況時它會顯示最多三個事項之一發生在哪個時間點的執行階段：

- 會顯示一般的執行階段錯誤訊息。 此頁面會通知那里的使用者執行階段錯誤，但不提供任何錯誤的詳細資訊。
- 會顯示例外狀況詳細資料訊息，其中包含剛擲回的例外狀況的資訊。
- 顯示自訂錯誤頁面，這是 ASP.NET 網頁，您建立可顯示您想要的任何訊息。

發生未處理的例外狀況的情況取決於`Web.config`檔案的[`<customErrors>`一節](https://msdn.microsoft.com/library/h0hfz6fc.aspx)。

開發和測試應用程式時最好先查看瀏覽器中的任何例外狀況的詳細資料。 不過，在生產環境應用程式中顯示例外狀況詳細資料是潛在的安全性風險。 此外，它是 unflattering，並可讓您的網站看起來不夠專業。 在理想情況下，發生未處理的例外狀況時在開發環境中的 web 應用程式會顯示例外狀況的詳細資料而在生產環境中相同的應用程式將會顯示自訂錯誤頁面。

> [!NOTE]
> 預設值`<customErrors>`例外狀況詳細資料訊息的頁面所造訪透過 localhost，且否則顯示一般的執行階段錯誤頁面時，才會顯示區段設定。 這並不理想，但它會確保知道的預設行為並不會顯示非本機訪客的例外狀況詳細資料。 未來的教學課程會檢查`<customErrors>`一節中更多詳細資料，並說明如何將自訂錯誤網頁顯示在生產環境中發生的錯誤。


在開發期間很有用的另一個 ASP.NET 功能追蹤。 追蹤，如果啟用，記錄每個傳入要求的相關資訊，並提供特殊的網頁， `Trace.axd`，來檢視最近的要求詳細資料。 您可以開啟和設定追蹤透過[`<trace>`項目](https://msdn.microsoft.com/library/6915t83k.aspx)在`Web.config`。

如果您啟用追蹤，請務必確定它已停用在生產環境中。 追蹤資訊包括 cookie、 工作階段資料，以及其他潛在的敏感資訊，因此務必停用在生產環境中的追蹤。 好消息是，根據預設，已停用追蹤和`Trace.axd`檔案就只能透過 localhost 存取。 如果您變更這些預設設定，在開發過程中確定，它們會關閉上一步在生產環境中。

## <a name="techniques-for-maintaining-separate-configuration-information"></a>維護個別的組態資訊的技術

開發和生產環境中有不同的組態設定變得非常複雜的部署程序。 先前的兩個教學課程中的部署程序牽涉到複製所有必要的檔案從開發到生產環境，但此方法僅適用於組態資訊為兩個環境中相同。 有各種不同的部署具有不同的組態資訊的應用程式的技術。 讓我們目錄其中一些託管的 web 應用程式的選項。

### <a name="manually-deploying-the-production-environment-configuration-file"></a>手動部署實際執行環境組態檔

最簡單的方法是維護兩個版本`Web.config`檔案： 一個用於開發環境，一個用於生產環境。 部署至生產環境的站台需要的所有檔案複製到實際執行伺服器，在開發環境*除了*如`Web.config`檔案。 相反地，實際執行環境特定`Web.config`檔案將會複製到實際執行環境。

此方法不是非常複雜，但很容易實作，因為不常變更的組態資訊。 它適合用於應用程式與小型開發團隊，單一的 web 伺服器上裝載，而且不常變更其組態資訊。 手動部署使用獨立 FTP 用戶端應用程式檔案時，它是最 tenable。 使用 Visual Studio 複製網站工具或 [發佈] 選項時您必須先抽換部署特定`Web.config`以在部署之前，生產環境特定的檔案，然後再將其交換部署完成之後。

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>變更組建或部署程序期間設定

到目前為止的討論被假設特定的建置和部署程序。 許多較大型的軟體專案擁有更正式的程序，讓使用開放原始碼，自產或第三方工具。 這類專案您可能可以自訂推桿推到生產環境之前適當地修改的組態資訊的組建或部署程序。 如果您要建置您的 web 應用程式使用[MSBuild](http://en.wikipedia.org/wiki/MSBuild)， [NAnt](http://nant.sourceforge.net/)，或其他建置工具，您可以有可能加入建置步驟以修改`Web.config`檔案以包含生產環境特定設定。 或您的部署工作流程無法以程式設計方式連接到原始檔控制伺服器並擷取適當`Web.config`檔案。

取得適當的組態資訊到生產環境的實際方法會依據您的工具和工作流程而有所不同。 因此，我們不會探討本主題會進一步。 如果您使用熱門的建置工具 MSBuild 或 NAnt 您可以找到部署文件和教學課程特有這些工具，透過 web 搜尋。

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>管理組態差異，透過 Web 部署專案增益集

2006 Microsoft 發行的 Web 開發專案增益集適用於 Visual Studio 2005。 適用於 Visual Studio 2008 增益集是在 2008年發行。 此增益集可讓 ASP.NET 開發人員建立個別的 Web 部署專案和其 web 應用程式專案，建置時、 明確編譯 web 應用程式並將檔案複製到部署到本機的輸出目錄。 Web 應用程式專案會在幕後使用 MSBuild。

根據預設，在開發環境的`Web.config`檔案複製到輸出目錄中，但您可以設定自訂的 Web 部署專案

取得透過下列方式複製到這個目錄的組態資訊：

- 透過`Web.config`檔案區段取代，您可以在其中指定要取代的區段和 XML 檔案，其中包含取代文字。
- 藉由提供外部組態來源檔的路徑。 選取此選項，Web 部署專案複製特定`Web.config`到輸出目錄的檔案 (而非`Web.config`開發環境中使用的檔案)。
- 加入 Web 部署專案所使用的 MSBuild 檔案中的自訂規則。

若要部署 web 應用程式建置 Web 部署專案，然後將檔案從專案的輸出資料夾複製到生產環境。

若要了解使用 Web 部署專案的詳細資訊請參閱[Web 部署專案本文](https://msdn.microsoft.com/magazine/cc163448.aspx)2007 年 4 月號的[MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx)，或請參閱 「 深入閱讀 」 一節中的連結本教學課程的結尾。

> [!NOTE]
> 您無法使用 Web 部署專案使用 Visual Web Developer，因為 Web 部署專案做為 Visual Studio 增益集實作，而且 Visual Studio Express 各版本 （包括 Visual Web Developer） 不支援增益集。


## <a name="summary"></a>總結

外部資源和 web 應用程式開發中的行為是通常不同於相同的應用程式處於生產環境。 例如，資料庫連接字串、 編譯選項，以及處理的例外狀況通常發生時的行為不同環境之間。 部署程序必須配合這些差異。 如我們在本教學課程所討論的最簡單的方法是手動複製到生產環境的 其他設定檔案。 可能會有更精緻的解決方案或使用更正式的組建或部署程序可容納這類自訂項目時使用 Web 部署專案增益集。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [連接字串說明](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [資料庫連接字串 @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [不會執行實際執行 ASP.NET 應用程式與`debug="true"`啟用](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [依正常程序回應未處理的例外狀況-顯示使用者易記的錯誤頁面](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [如何： 使用 Visual Studio 2008 Web 部署專案嗎？](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [部署資料庫時的重要組態設定](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 Web 部署專案下載](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 Web 部署專案下載](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [VS 2008 Web 部署專案](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [VS 2008 Web 部署專案支援發行](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Web 部署專案](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [上一頁](deploying-your-site-using-visual-studio-vb.md)
> [下一頁](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
