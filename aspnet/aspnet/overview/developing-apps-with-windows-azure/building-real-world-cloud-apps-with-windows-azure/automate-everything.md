---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: "自動化 （建置真實世界雲端應用程式與 Azure） 的所有項目 |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: cf1cb7b07ffe8750724e58e4fb66854c9a033a54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>自動化 （建置真實世界雲端應用程式與 Azure） 的所有項目
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 如需電子書的簡介，請參閱[第一章](introduction.md)。


我們將探討的前三個模式實際上會套用至任何軟體開發專案，但特別雲端專案。 此模式是關於開發工作自動化。 它是很重要的主題，因為手動程序變慢而且容易產生錯誤。為多個可能的可協助快速、 可靠且靈活的工作流程設定為自動執行。 請務必唯一雲端開發因為您可以輕鬆地自動化許多難以或無法在內部部署環境中自動化的工作。 例如，您可以設定整個測試的環境，包括新的 web 伺服器和後端 Vm、 資料庫、 blob 儲存體 （檔案儲存體）、 佇列等等。

## <a name="devops-workflow"></a>DevOps 工作流程

您逐漸聽到詞彙"DevOps。 」 超出辨識，您必須整合開發和作業的工作，若要有效率地開發軟體，開發一詞。 您想要啟用的工作流程的類型是在其中您可以開發應用程式、 將它部署、 了解從它的生產使用方式、 變更以回應您已學會，和快速且可靠地重複的循環。

某些成功的雲端開發團隊部署多個為一天次至即時環境。 用來部署主要 Azure 團隊更新每 2-3 個月，但現在它發行每隔 2-3 天和主要釋放每隔 2-3 週的次要更新。 放入該韻律真正可協助您做出回應客戶的意見反應。

若要這樣做，您必須啟用開發和部署的週期為可重複、 可靠、 可預測，且具有低循環時間。

![DevOps 工作流程](automate-everything/_static/image1.png)

換句話說，當您有一項功能構想和客戶使用它以及提供意見反應時之間的時間期間必須越短越好。 前三個模式中-自動化所有項目，原始檔控制，而且持續整合和傳遞-最佳作法，建議您若要讓該類型的處理程序的所有相關資訊。

## <a name="azure-management-scripts"></a>Azure 管理指令碼

在[此的電子書簡介](introduction.md)，您所看到的網頁型主控台，在 Azure 管理入口網站。 在管理入口網站可讓您監視和管理所有您已部署在 Azure 的資源。 它是可以輕鬆地建立和刪除服務，例如 web 應用程式和 Vm、 設定這些服務、 監控服務作業等等。 這是一個很棒的工具，但使用它是手動程序。 如果您要開發的任何大小的實際執行應用程式，而且特別是在小組環境中，我們建議您瀏覽入口網站以深入了解及探索 Azure，UI，然後再自動執行，也會進行回應的處理程序。

幾乎所有的作業，您可以手動在管理入口網站中，或從 Visual Studio 也可以藉由呼叫 REST 管理 API 完成。 您可以撰寫指令碼使用[Windows PowerShell](https://msdn.microsoft.com/en-us/library/windowsazure/jj156055.aspx)，或您可以使用一種開放原始碼架構，例如[Chef](http://www.opscode.com/chef/)或[Puppet](http://puppetlabs.com/puppet/what-is-puppet)。 您也可以在 Mac 或 Linux 的環境中使用 Bash 命令列工具。 Azure 有針對所有這些不同環境中，指令碼應用程式開發介面，而且有[.NET 管理 API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)萬一您想要撰寫程式碼，而不是指令碼。

我們已修正它應用程式建立自動化的程序，建立測試環境，並將專案部署到該環境中，某些 Windows PowerShell 指令碼，我們會檢閱一些這些指令碼的內容。

## <a name="environment-creation-script"></a>環境建立指令碼

名為第一個指令碼，我們會探討*新增 AzureWebsiteEnv.ps1*。 它會建立 Azure 環境，您可以部署它修正應用程式進行測試。 此指令碼執行的主要工作如下所示：

- 建立 web 應用程式。
- 建立儲存體帳戶。 （需具備適用於 blob 和佇列，您會發現在後續章節）。
- 建立 SQL Database 伺服器和兩個資料庫： 應用程式資料庫和成員資格資料庫。
- 將設定儲存在 Azure 應用程式將用來存取儲存體帳戶和資料庫。
- 建立設定檔，將用來自動化部署。

### <a name="run-the-script"></a>執行指令碼


> [!NOTE]
> 本章的此部分顯示指令碼和您輸入才能執行這些命令的範例。 此示範，而且沒有提供您需要知道才能執行指令碼的所有項目。 How-要執行的 it 逐步解說，請參閱[附錄： 修正它範例應用程式](the-fix-it-sample-application.md#deploybase)。


若要執行 PowerShell 指令碼管理 Azure 服務，您必須安裝 Azure PowerShell 主控台，並將它設定為使用您的 Azure 訂用帳戶。 一旦您設定好，您可以使用下面類似的命令來執行修正它的環境建立指令碼：

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name`參數會指定要建立資料庫和儲存體帳戶時使用的名稱和`SqlDatabasePassword`參數會指定將建立 SQL 資料庫系統管理員帳戶的密碼。 沒有您可以使用，我們會探討稍後其他參數。

![PowerShell 視窗](automate-everything/_static/image2.png)

在指令碼完成之後您可以查看在管理入口網站中建立的內容。 您會發現兩個資料庫：

![資料庫](automate-everything/_static/image3.png)

儲存體帳戶：

![儲存體帳戶](automate-everything/_static/image4.png)

和 web 應用程式：

![網站](automate-everything/_static/image5.png)

在**設定** 索引標籤上的 web 應用程式中，您可以看到它受到儲存體帳戶設定，以及 SQL database 的連接字串設定為修正它的應用程式。

![appSettings 和 connectionStrings](automate-everything/_static/image6.png)

*自動化*資料夾現在還包含 *&lt;websitename&gt;.pubxml*檔案。 這個檔案會儲存 MSBuild 將會使用部署至 Azure 環境剛建立應用程式的設定。 例如: 

[!code-xml[Main](automate-everything/samples/sample1.xml)]

如您所見，指令碼建立完整的測試環境中，且在 90 秒內完成整個程序。

如果您的小組的其他人想要建立測試環境，就可以執行指令碼。 不只是快速，但是也可以確信他們使用的相同的您所使用的環境。 您無法將未經確信，如果每個人都已進行設定以手動方式使用管理入口網站 UI。

### <a name="a-look-at-the-scripts"></a>查看指令碼

沒有實際三個的指令碼，執行這項工作。 您呼叫其中一個從命令列，它會自動使用其他兩個執行的一些工作：

- *新 AzureWebSiteEnv.ps1*是主要的指令碼。

    - *新 AzureStorage.ps1*建立儲存體帳戶。
    - *新 AzureSql.ps1*會建立資料庫。

### <a name="parameters-in-the-main-script"></a>主要的指令碼中的參數

主要的指令碼，*新增 AzureWebSiteEnv.ps1*，定義數個參數：

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

兩個參數是必要的：

- 指令碼會建立 web 應用程式的名稱。 (這也適用於 URL: `<name>.azurewebsites.net`。)
- 指令碼建立的資料庫伺服器的新系統管理員密碼。

選擇性參數可讓您指定的資料中心位置 （預設為 「 美國西部 」）、 資料庫伺服器管理員的名稱 （預設為"dbuser"），以及資料庫伺服器的防火牆規則。

### <a name="create-the-web-app"></a>建立 web 應用程式

指令碼會執行第一件事是建立 web 應用程式藉由呼叫`New-AzureWebsite`cmdlet，傳遞給它的 web 應用程式名稱和位置參數值：

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>建立儲存體帳戶

然後執行主要指令碼*新增 AzureStorage.ps1*指令碼，請指定"*&lt;websitename&gt;*儲存體 」 的儲存體帳戶名稱，和相同資料中心的位置做為web 應用程式。

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*新 AzureStorage.ps1*呼叫`New-AzureStorageAccount`cmdlet 來建立儲存體帳戶，並傳回所用帳戶名稱和存取金鑰值。 若要存取的 blob 和佇列儲存體帳戶中的，應用程式會需要這些值。

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

您可能不一定要建立新的儲存體帳戶。您所加入的參數 （選擇性） 會將其導向使用現有的儲存體帳戶，加強指令碼。

### <a name="create-the-databases"></a>建立資料庫

然後執行主要指令碼的資料庫建立指令碼，*新增 AzureSql.ps1*、 after 預設資料庫設定和防火牆規則名稱：

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

資料庫建立指令碼擷取開發人員電腦的 IP 位址，並設定防火牆規則，才能連接到開發電腦，以及管理伺服器。 然後經歷數個步驟來設定資料庫的資料庫建立指令碼：

- 使用建立伺服器`New-AzureSqlDatabaseServer`cmdlet。

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- 建立防火牆規則才能啟用管理伺服器，並啟用連接到該 web 應用程式的開發電腦。 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- 建立包含伺服器名稱和認證，可藉由使用的資料庫內容`New-AzureSqlDatabaseServerContext`cmdlet。

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText`是函式呼叫的指令碼中`ConvertTo-SecureString`指令程式以加密的密碼與傳回`PSCredential`物件相同的型別， `Get-Credential` cmdlet 會傳回。
- 使用建立應用程式資料庫和成員資格資料庫`New-AzureSqlDatabase`cmdlet。

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- 每個資料庫呼叫本機定義的函式 tocreates 連接字串。 應用程式會使用這些連接字串來存取資料庫。 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get SQLAzureDatabaseConnectionString 是建立連接字串提供給它的參數值的指令碼中定義的函式。

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- 傳回與資料庫伺服器名稱和連接字串的雜湊資料表。

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

修正它應用程式使用個別的成員資格和應用程式資料庫。 它也可將單一資料庫中的成員資格和應用程式資料。

### <a name="store-app-settings-and-connection-strings"></a>市集應用程式設定和連接字串

Azure 有此功能可讓您將設定和自動覆寫當它嘗試讀取時傳回至應用程式的連接字串儲存`appSettings`或`connectionStrings`Web.config 檔案中的集合。 這是要套用的替代[Web.config 轉換](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)當您部署。 如需詳細資訊，請參閱[敏感性資料儲存在 Azure 中](source-control.md#appsettings)稍後在本電子書。

環境的建立指令碼儲存在 Azure 中的所有`appSettings`和`connectionStrings`應用程式必須存取儲存體帳戶和資料庫，在 Azure 中執行時的值。

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/)是遙測架構，我們將示範在[監控與遙測](monitoring-and-telemetry.md)章節。 環境的建立指令碼也會重新啟動 web 應用程式，並確定它收取 New Relic 設定。

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>準備部署

在此程序結束時，環境建立指令碼呼叫，將使用部署指令碼檔案建立兩個函式。

其中一個這些函式建立的發行設定檔*(&lt;websitename&gt;.pubxml*檔案)。 程式碼會呼叫 Azure REST API，以取得發行設定，並將儲存中的資訊*.publishsettings*檔案。 然後會從該檔案和範本檔案一起使用的資訊 (*pubxml.template*) 來建立*.pubxml*所屬的發行設定檔的檔案。 這兩個步驟程序會模擬您在 Visual Studio 中執行的動作： 下載*.publishsettings*檔案和匯入，若要建立的發行設定檔。

另一個函式會使用另一個範本檔 (網站 environment.template) 來建立*網站 environment.xml*檔案，其中包含的設定部署指令碼將使用連同*.pubxml*檔案。

### <a name="troubleshooting-and-error-handling"></a>疑難排解和錯誤處理

指令碼就像是程式： 它們可能會失敗，而且這樣做時您想要知道儘可能瞭解錯誤與造成它的原因。 基於這個理由，環境建立指令碼的值變更`VerbosePreference`變數從`SilentlyContinue`至`Continue`以便顯示所有詳細資訊訊息。 它也會變更的值`ErrorActionPreference`變數從`Continue`至`Stop`，如此一來，即使當它遇到非終止錯誤的指令碼停止：

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

它沒有任何工作之前，指令碼，讓它可以計算經過的時間，完成時，就會儲存的開始時間：

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

此作業完成其工作之後，指令碼會顯示已耗用時間：

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

並針對每個索引鍵的作業指令碼寫入詳細資訊訊息，例如：

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>部署指令碼

什麼*新增 AzureWebsiteEnv.ps1*指令碼會執行環境建立*發行 AzureWebsite.ps1*指令碼會執行應用程式部署。

部署指令碼取得從 web 應用程式的名稱*網站 environment.xml*環境建立指令碼所建立的檔案。

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

它會取得部署使用者密碼從*.publishsettings*檔案：

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

它會執行[MSBuild](http://msbuildbook.com/)建置並部署專案的命令：

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

如果您指定`Launch`參數在命令列上的，它會呼叫`Show-AzureWebsite`指令程式可開啟預設的瀏覽器的網站 url。

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

您可以使用下面類似的命令來執行部署指令碼：

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

完成時，瀏覽器會開啟與在雲端中執行的站台和`<websitename>.azurewebsites.net`URL。

![修正應用程式部署至 Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>總結

這些指令碼與您可以放心的相同步驟一定會執行使用相同選項的順序相同。 這有助於確保每個小組的開發人員不遺漏的項目或項目弄亂或部署自訂自己實際上不會在另一個小組成員的環境或生產環境中運作方式相同的電腦上的項目。

類似的方式，您可以自動化大部分 Azure 的管理功能，您可以在管理入口網站利用 REST API、 Windows PowerShell 指令碼、.NET 語言應用程式開發介面或您可以在 Linux 或 mac 執行 Bash 公用程式

在[下一章](source-control.md)我們會查看原始程式碼，並說明它為何如此重要您原始程式碼儲存機制中包含您的指令碼。

## <a name="resources"></a>資源

- [安裝和設定 Windows PowerShell 的 Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)。 說明如何安裝 Azure PowerShell cmdlet，以及如何安裝憑證，您需要在您的電腦，以管理您的 Azure 帳戶。 這是因為它也可用來學習本身 PowerShell 資源的連結，開始著手的絕佳地方。
- [Azure 指令碼中心](https://docs.microsoft.com/azure/automation/automation-runbook-gallery)。 WindowsAzure.com 入口網站，以用於開發管理 Azure 服務，連結來取得教學課程、 cmdlet 參考文件集和來源的程式碼和範例指令碼的指令碼資源
- [週末 Scripter： 開始使用 Azure PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx)。 部落格專用的 Windows PowerShell，在這篇文章會提供使用 PowerShell 管理 Azure 功能的絕佳簡介。
- [安裝及設定 Azure 跨平台命令列介面](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。 快速入門教學課程適用於 Mac 和 Linux，以及 Windows 系統的 Azure 指令碼架構。
- [命令列工具區段下載 Azure Sdk 和工具的主題](https://azure.microsoft.com/downloads/)。 入口網站的文件集和相關的 Azure 命令列工具的下載頁面。
- [自動化的所有項目使用 Azure 管理程式庫和.NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)。 Scott Hanselman 介紹 Azure.NET 管理 API。
- [若要發行至開發和測試環境中使用 Windows PowerShell 指令碼](https://msdn.microsoft.com/library/azure/dn642480.aspx)。 說明如何使用的 MSDN 文件發行 Visual Studio 會自動產生 web 專案的指令碼。
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597)。 在 Visual Studio 中新增的 Windows PowerShell 語言支援的 visual Studio 擴充。

>[!div class="step-by-step"]
[上一頁](introduction.md)
[下一頁](source-control.md)
