---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: 自動化所有項目 （建置使用 Azure 的真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 4acf7361bed7e135c73cd46c99d15e12757e4679
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832237"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>自動化 （建置使用 Azure 的真實世界的雲端應用程式） 的所有項目
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 如需簡介電子書，請參閱[第 1 章](introduction.md)。


我們將探討的前三個模式實際上會套用至任何軟體開發專案中，但特別是雲端專案。 此模式是關於開發工作自動化。 它是很重要的主題，因為手動程序是既緩慢又容易發生錯誤;多少個為可能有助於設定快速、 可靠且敏捷式軟體開發工作流程自動化。 它是唯一重要的雲端開發的因為您可以輕鬆自動化許多難以或無法在內部部署環境中自動化的工作。 例如，您可以設定整個測試的環境，包括新的 web 伺服器和後端 Vm、 資料庫、 blob 儲存體 （檔案）、 佇列等。

## <a name="devops-workflow"></a>DevOps 工作流程

您將會越來越聽到詞彙 「 DevOps 」。 從辨識，您必須將開發和作業的工作整合以有效率地開發軟體，開發一詞。 您想要啟用的工作流程的類型是指在其中您可以開發應用程式、 將它部署、 從生產環境的使用方式的了解、 變更以回應您的學習成果，和重複循環，快速且可靠地。

某些成功的雲端開發團隊會部署一天多次至即時環境。 用來部署主要 Azure 團隊更新每隔 2-3 月，但目前版本的次要更新每個 2-3 天和主要版本每隔 2 到 3 週。 放入該韻律非常有幫助您客戶的意見反應做出回應。

若要這樣做，您必須啟用開發和部署的週期可重複、 可靠、 可預測的且具有低的週期時間。

![DevOps 工作流程](automate-everything/_static/image1.png)

換句話說，如果您已了解功能和當客戶使用它，並提供意見反應之間的時間期間必須越短越好。 前三個模式-自動化原始檔控制的一切，而且持續整合與傳遞，我們建議您若要讓這種程序的最佳作法的所有相關資訊。

## <a name="azure-management-scripts"></a>Azure 管理指令碼

在 [簡介本電子書](introduction.md)，您看到網頁型主控台，在 Azure 管理入口網站。 在管理入口網站可讓您監視和管理所有您已在 Azure 部署的資源。 它是簡單的方法，來建立和刪除服務，例如 web 應用程式和 Vm，設定這些服務、 監視服務作業，以及進行其他操作。 很棒的工具，但使用它是手動程序。 如果您要開發生產應用程式的任何大小，而且特別是在小組環境中，我們建議您瀏覽入口網站以了解並探索 Azure 中的 UI，然後再自動執行您要重複執行的處理程序。

幾乎所有項目，您可以手動在管理入口網站或從 Visual Studio 也可以藉由呼叫 REST 管理 API。 您可以撰寫使用指令碼[Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)，或您可以使用開放原始碼架構，例如[Chef](http://www.opscode.com/chef/)或是[Puppet](http://puppetlabs.com/puppet/what-is-puppet)。 您也可以在 Mac 或 Linux 環境中使用 Bash 命令列工具。 Azure 可讓您有指令碼 Api 針對所有這些不同的環境，而且它有[.NET 管理 API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)萬一您想要撰寫程式碼，而不是指令碼。

修正其應用程式中，我們已建立自動化的處理程序建立測試環境，並將專案部署到該環境中，一些 Windows PowerShell 指令碼，我們會檢閱一些這些指令碼的內容。

## <a name="environment-creation-script"></a>環境建立指令碼

我們將探討第一個指令碼名為*新增 AzureWebsiteEnv.ps1*。 它會建立在 Azure 環境，您可以部署此修正其應用程式進行測試。 此指令碼執行的主要工作如下所示：

- 建立 web 應用程式。
- 建立儲存體帳戶。 （需具備適用於 blob 和佇列，如您所見在後續章節）。
- 建立 SQL Database 伺服器和兩個資料庫： 應用程式資料庫，以及成員資格資料庫。
- 設定儲存在 Azure 應用程式將用來存取儲存體帳戶和資料庫。
- 建立將用來自動化部署的設定檔。

### <a name="run-the-script"></a>執行指令碼


> [!NOTE]
> 本章的這個部分中顯示的指令碼和您輸入才能執行這些命令的範例。 此示範並不會提供您需要知道，才能執行指令碼的所有項目。 逐步作法-要-執行-it 指示，請參閱[附錄： 修正它的範例應用程式](the-fix-it-sample-application.md#deploybase)。


若要執行 PowerShell 指令碼管理 Azure 服務，您必須安裝 Azure PowerShell 主控台，然後將它設定為使用您 Azure 訂用帳戶。 一旦設定好，您可以使用與下列類似的命令來執行修正它的環境建立指令碼：

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name`參數會指定要建立資料庫和儲存體帳戶時使用的名稱和`SqlDatabasePassword`參數會指定將建立 SQL Database 的系統管理員帳戶的密碼。 還有，我們將探討稍後，您可以使用其他參數。

![PowerShell 視窗](automate-everything/_static/image2.png)

指令碼完成之後您可以看到管理入口網站中建立的項目。 您會發現兩個資料庫：

![資料庫](automate-everything/_static/image3.png)

儲存體帳戶：

![儲存體帳戶](automate-everything/_static/image4.png)

和 web 應用程式：

![網站](automate-everything/_static/image5.png)

在 [**設定**] 索引標籤上的 web 應用程式中，您可以看到它有沒有儲存體帳戶設定，SQL 資料庫連接字串設定此修正其應用程式。

![appSettings 和 connectionStrings](automate-everything/_static/image6.png)

*自動化*資料夾現在也包含 *&lt;websitename&gt;.pubxml*檔案。 這個檔案會儲存 MSBuild 用來部署應用程式以剛建立的 Azure 環境的設定。 例如: 

[!code-xml[Main](automate-everything/samples/sample1.xml)]

如您所見，指令碼已建立完整的測試環境，並在 90 秒內完成整個程序。

如果您的小組的其他人想要建立測試環境，可以只執行指令碼。 不只是速度快，但是也可自信，它們都使用相同於所使用的環境。 您無法為信心，如果每個人都已進行設定以手動方式使用管理入口網站 UI 相當。

### <a name="a-look-at-the-scripts"></a>了解指令碼

有實際三個的指令碼，進行這項工作。 您呼叫其中一個從命令列，並自動使用其他兩個來執行部分工作：

- *新 AzureWebSiteEnv.ps1*是主要的指令碼。

    - *新 AzureStorage.ps1*建立儲存體帳戶。
    - *新 AzureSql.ps1*會建立資料庫。

### <a name="parameters-in-the-main-script"></a>主要的指令碼中的參數

主要的指令碼中，*新增 AzureWebSiteEnv.ps1*，定義數個參數：

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

兩個參數都是必要項目：

- 指令碼會建立 web 應用程式的名稱。 (這也會用於 URL: `<name>.azurewebsites.net`。)
- 指令碼會建立資料庫伺服器的新系統管理員密碼。

選擇性參數可讓您指定的資料中心位置 （預設為"West US"）、 資料庫伺服器管理員名稱 （預設為"dbuser 」），以及資料庫伺服器的防火牆規則。

### <a name="create-the-web-app"></a>建立 web 應用程式

指令碼會執行第一件事是建立 web 應用程式藉由呼叫`New-AzureWebsite`cmdlet，將傳遞給它的 web 應用程式名稱和位置的參數值：

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>建立儲存體帳戶

然後會執行主要指令碼<em>新增 AzureStorage.ps1</em>指令碼，請指定"<em>&lt;websitename&gt;</em>儲存體 」 儲存體帳戶名稱，和相同的資料中心位置web 應用程式。

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*新 AzureStorage.ps1*呼叫`New-AzureStorageAccount`cmdlet 來建立儲存體帳戶中，它會傳回帳戶名稱] 和 [存取金鑰的值。 若要存取的 blob 和佇列儲存體帳戶中的，應用程式將會需要這些值。

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

您可能不一定要建立新的儲存體帳戶;您可以加入的參數，選擇性指示以使用現有的儲存體帳戶，以加強指令碼。

### <a name="create-the-databases"></a>建立資料庫

主要的指令碼再執行資料庫建立指令碼中，*新增 AzureSql.ps1*、 之後設定預設資料庫和防火牆規則名稱：

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

資料庫建立指令碼擷取開發電腦的 IP 位址，並設定防火牆規則，因此可以連線到開發電腦，並將其管理伺服器。 資料庫建立指令碼再經歷數個步驟來設定資料庫：

- 使用建立伺服器`New-AzureSqlDatabaseServer`cmdlet。

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- 建立防火牆規則，以讓開發人員機器來管理伺服器，以及啟用 web 應用程式連接到它。 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- 建立資料庫內容，其中包含伺服器名稱和認證，使用`New-AzureSqlDatabaseServerContext`cmdlet。

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` 是在指令碼中呼叫的函式`ConvertTo-SecureString`cmdlet 來加密密碼並傳回`PSCredential`物件時，相同的類型， `Get-Credential` cmdlet 會傳回。
- 藉由建立應用程式資料庫和成員資格資料庫`New-AzureSqlDatabase`cmdlet。

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- 呼叫每個資料庫的本機定義的函式 tocreates 連接字串。 應用程式會使用這些連接字串來存取資料庫。 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    取得 SQLAzureDatabaseConnectionString 是從提供給它的參數值中建立的連接字串的指令碼中定義的函式。

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- 傳回與資料庫伺服器名稱和連接字串的雜湊資料表。

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

修正其應用程式使用個別的成員資格和應用程式資料庫。 它也可將成員資格和應用程式資料放入單一資料庫。

### <a name="store-app-settings-and-connection-strings"></a>市集應用程式設定和連接字串

Azure 有一項功能，可讓您儲存設定和自動覆寫它嘗試讀取時傳回給應用程式的連接字串`appSettings`或`connectionStrings`Web.config 檔案中的集合。 這是要套用的替代[Web.config 轉換](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)當您部署。 如需詳細資訊，請參閱 <<c0> [ 在 Azure 中儲存敏感性資料](source-control.md#appsettings)稍後本電子書。

環境建立指令碼會將儲存在 Azure 中的所有`appSettings`和`connectionStrings`應用程式需要存取儲存體帳戶和資料庫，在 Azure 中執行時的值。

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/)是我們示範在遙測架構[監視和遙測](monitoring-and-telemetry.md)一章。 環境建立指令碼也會重新啟動 web 應用程式，以確定其挑選的 New Relic 設定。

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>準備部署

在此程序結束時，環境的建立指令碼會呼叫兩個函式來建立將用於部署指令碼的檔案。

其中一個函式會建立發行設定檔 *(&lt;websitename&gt;.pubxml*檔案)。 程式碼會呼叫 Azure REST API，以取得發佈設定，並將儲存中的資訊 *.publishsettings*檔案。 然後它會使用從該檔案，以及範本檔案的資訊 (*pubxml.template*) 來建立 *.pubxml*所屬的發行設定檔的檔案。 此兩步驟程序會模擬您在 Visual Studio 中如何： 下載 *.publishsettings*檔案，並匯入，若要建立發行設定檔。

其他函式使用另一個範本檔 (網站 environment.template) 來建立*網站 environment.xml*檔案，其中包含的設定部署指令碼將使用連同 *.pubxml*檔案。

### <a name="troubleshooting-and-error-handling"></a>疑難排解和錯誤處理

指令碼就像是程式︰ 它們可能會失敗，以及這樣做時，您想要知道儘可能瞭解錯誤與造成它的原因。 基於這個理由，環境的建立指令碼的值變更`VerbosePreference`從變數`SilentlyContinue`到`Continue`以便顯示所有的詳細資訊訊息。 它也會變更的值`ErrorActionPreference`從變數`Continue`到`Stop`，如此一來，即使當它遇到非終止錯誤的指令碼停止：

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

它沒有任何工作之前，指令碼，讓它可以計算經過的時間，完成時，就會儲存開始時間：

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

完成其工作之後，指令碼會顯示已耗用時間：

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

並針對每個金鑰的作業指令碼寫入詳細資訊訊息，例如：

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>部署指令碼

什麼*新增 AzureWebsiteEnv.ps1*環境建立的指令碼會執行*發佈 AzureWebsite.ps1*指令碼會執行應用程式部署。

部署指令碼會取得從 web 應用程式名稱*網站 environment.xml*環境建立指令碼所建立的檔案。

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

它會取得部署的使用者密碼，從 *.publishsettings*檔案：

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

它會執行[MSBuild](http://msbuildbook.com/)建置和部署專案的命令：

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

如果您已指定`Launch`參數在命令列上的，它會呼叫`Show-AzureWebsite`cmdlet 來開啟預設瀏覽器的網站 url。

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

您可以使用與下列類似的命令來執行部署指令碼：

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

完成時，瀏覽器會開啟與在雲端中執行的站台和`<websitename>.azurewebsites.net`URL。

![修正應用程式部署至 Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>總結

這些指令碼與您可以確信一律會使用相同選項的相同順序執行相同的步驟。 這有助於確保小組的每位開發人員不會遺漏某些項目或弄亂的項目或部署在他自己實際上不會在另一個小組成員的環境或生產環境中運作方式相同的電腦上的自訂內容。

類似的方式，您可以將大部分的 Azure 的管理功能，您可以在管理入口網站中，使用 REST API、 Windows PowerShell 指令碼、.NET 語言 API 或您可以在 Linux 或 mac 執行的 Bash 公用程式自動化

在 [[下一步] 一章](source-control.md)我們將查看原始程式碼，並說明為何一定要在您的原始程式碼儲存機制中包含您的指令碼。

## <a name="resources"></a>資源

- [安裝和設定適用於 Azure 的 Windows PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)。 說明如何安裝 Azure PowerShell cmdlet，以及如何安裝憑證，您需要在您的電腦，以管理您的 Azure 帳戶。 這是因為它也有連結，了解 PowerShell 本身的資源開始著手的好地方。
- [Azure 指令碼中心](https://docs.microsoft.com/azure/automation/automation-runbook-gallery)。 WindowsAzure.com 開發指令碼管理 Azure 服務，取得教學課程中，cmdlet 參考文件來源的程式碼和範例指令碼的連結資源的入口網站
- [週末 Scripter： 開始使用 Azure 和 PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx)。 部落格專用的 Windows powershell 時，在這篇文章會提供使用 PowerShell for Azure 的管理功能的絕佳簡介。
- [安裝和設定 Azure 跨平台命令列介面](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。 快速入門教學課程適用於 Mac 和 Linux，以及 Windows 系統的 Azure 指令碼架構。
- [命令列工具區段下載 Azure Sdk 和工具的主題](https://azure.microsoft.com/downloads/)。 入口網站的文件和 Azure 命令列工具與相關的下載頁面。
- [將一切自動化使用 Azure 管理程式庫和.NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)。 Scott Hanselman 介紹 Azure 中的使用.NET 管理 API。
- [使用 Windows PowerShell 指令碼來發行至開發和測試環境](https://msdn.microsoft.com/library/azure/dn642480.aspx)。 MSDN 文件說明如何使用發佈 Visual Studio 會自動產生 web 專案的指令碼。
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597)。 Visual Studio 擴充功能在 Visual Studio 中新增適用於 Windows PowerShell 語言支援。

> [!div class="step-by-step"]
> [上一頁](introduction.md)
> [下一頁](source-control.md)
