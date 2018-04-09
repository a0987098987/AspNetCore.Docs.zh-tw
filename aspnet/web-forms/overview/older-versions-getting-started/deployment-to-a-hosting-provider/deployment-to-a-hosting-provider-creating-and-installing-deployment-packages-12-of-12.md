---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 疑難排解 (12 / 12) |Microsoft 文件
author: tdykstra
description: 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2a8342f026498a7cf3ff4a3c158ed177c15b7111
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 疑難排解 (12 / 12)
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Windows Azure 網站的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


此頁面描述，當您使用 Visual Studio 部署 ASP.NET web 應用程式時可能發生的一些常見問題。 每個都提供一個或多個可能原因和解決方案。

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>伺服器錯誤目前自訂錯誤設定 '/' 應用程式層中避免從遠端檢視錯誤詳細資料

### <a name="scenario"></a>情節

之後將網站部署至遠端主機，您會收到錯誤訊息所提及的 Web.config 檔案中的 customErrors 設定，而不會指出實際錯誤的原因是什麼：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

根據預設，ASP.NET 會顯示詳細的錯誤資訊，只有在本機電腦上執行您 web 應用程式時。 通常您不想 web 應用程式時網際網路上公開使用，因為駭客可能可以使用此資訊來尋找應用程式中的弱點可能會顯示詳細的錯誤資訊。 不過，當您部署至網站或更新到站台，有時東西會發生錯誤，而且您需要取得實際的錯誤訊息。

若要啟用應用程式的遠端主機上執行時顯示詳細的錯誤訊息，請編輯 Web.config 檔案，以設定`customErrors`模式關閉、 重新部署應用程式，並再次執行應用程式：

1. 如果應用程式 Web.config 檔案具有`customErrors`中的項目`system.web`項目，變更`mode`為 「 關閉 」 的屬性。 否則，請加入`customErrors`中的項目`system.web`具有項目`mode`屬性設為 「 關閉 」，如下列範例所示：

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. 部署應用程式。
3. 執行應用程式，並重複任何先前般導致發生錯誤。 現在您可以看到實際的錯誤訊息是什麼。
4. 當您解決錯誤時，還原原始`customErrors`設定並重新部署應用程式。

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>拒絕存取網頁中使用 SQL Server Compact

### <a name="scenario"></a>情節

當您部署使用 SQL Server Compact 的網站，而且您執行已部署的站台存取資料庫中的頁面時，您會看到下列錯誤訊息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

在伺服器上的網路服務帳戶必須能夠讀取 SQL Compact 服務中的原生二進位檔*bin\amd64*或*bin\x86*資料夾，但它沒有讀取這些資料夾的權限。 讀取網路服務的權限集*bin*資料夾中，務必要擴充到子資料夾的權限。

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>無法讀取設定檔案，因為權限不足

### <a name="scenario"></a>情節

當您按一下 [Visual Studio 發行應用程式部署至 IIS 在本機電腦上的按鈕發佈失敗而**輸出**] 視窗會顯示錯誤訊息如下所示：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

若要使用單鍵發行至 IIS 在本機電腦上，您必須以管理員權限執行 Visual Studio。 關閉 Visual Studio 並重新啟動它以管理員權限。

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>無法連接到目的電腦...使用指定的處理序

### <a name="scenario"></a>情節

當您按一下 [Visual Studio 發行應用程式部署的按鈕發佈失敗而**輸出**] 視窗會顯示錯誤訊息如下所示：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

Proxy 伺服器會中斷與目的地伺服器的通訊。 從 Windows [控制台] 或在 Internet Explorer 中，選取**網際網路選項**選取**連線**] 索引標籤。在**網際網路內容**對話方塊中，按一下 [ **LAN 設定**。 在**區域網路 (LAN) 設定**對話方塊中，清除**自動偵測設定**核取方塊。 然後再按一下 [發行] 按鈕。

如果此問題持續發生，請連絡系統管理員，以判斷哪些可以透過 proxy 或防火牆設定。 問題發生，因為 Web Deploy Web 管理服務部署 (8172); 使用非標準連接埠為其他連線，Web Deploy，將會使用連接埠 80。 當您部署到協力廠商主機服務提供者時，通常使用 Web 管理服務。

## <a name="default-net-40-application-pool-does-not-exist"></a>預設.NET 4.0 的應用程式集區不存在

### <a name="scenario"></a>情節

當您部署的應用程式需要.NET Framework 4 時，您會看到下列錯誤訊息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

在 IIS 中未安裝 ASP.NET 4。 如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 已安裝在電腦上，但可能未安裝在 IIS 中。 在您要部署至伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

您也可能需要手動設定的預設應用程式集區的.NET Framework 版本。 如需詳細資訊，請參閱[部署至 IIS 做為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>初始化字串的格式不符合索引 0 處開始的規格。

### <a name="scenario"></a>情節

在您部署應用程式使用一種單鍵之後發行，當您執行存取資料庫的頁面您收到下列錯誤訊息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

開啟*Web.config*檔案中的已部署的站台和檢查連接字串值是否以開頭`$(ReplacableToken_`，如下列範例所示：

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

如果連接字串類似下列範例中，編輯專案檔，並加入下列屬性加入`PropertyGroup`的所有組建組態的項目：

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

然後重新部署應用程式。

## <a name="http-500-internal-server-error"></a>HTTP 500 內部伺服器錯誤

### <a name="scenario"></a>情節

當您執行已部署的站台時，您會看到下列錯誤訊息，不含特定資訊指出錯誤的原因：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

有許多原因會造成 500 錯誤，但如果您要遵照這些教學課程的其中一個可能原因是您將 XML 項目放在錯誤中的其中一個 XML 轉換檔案的位置。 例如，您會收到這個錯誤如果您將插入的轉換`<location>`項目底下`<system.web>`而不是直接在`<configuration>`。 方案在此情況下是更正 XML 轉換檔案，然後重新部署。

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 內部伺服器錯誤

### <a name="scenario"></a>情節

當您執行已部署的站台時，您會看到下列錯誤訊息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

網站已部署 ASP.NET 4 中，但 ASP.NET 4 未登錄於 IIS 伺服器的目標。 在伺服器上開啟提升權限的命令提示字元並執行下列命令註冊 ASP.NET 4:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

您也可能需要手動設定的預設應用程式集區的.NET Framework 版本。 如需詳細資訊，請參閱[部署至 IIS 做為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>登入失敗的應用程式中的開啟 SQL Server Express 資料庫\_資料

### <a name="scenario"></a>情節

您更新*Web.config*檔案連接字串以指向做為 SQL Server Express 資料庫*.mdf*檔案中您*應用程式\_資料*資料夾中，而且是第一個執行應用程式，您會看到下列錯誤訊息的時間：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

名稱*.mdf*檔案無法符合曾經已存在您的電腦的任何 SQL Server Express 資料庫的名稱，即使您刪除*.mdf*先前已存在的資料庫檔案。 變更名稱*.mdf*從未使用過為資料庫名稱以及變更名稱的檔案*Web.config*檔案，以使用新的名稱。 或者，您可以使用[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除先前已存在 SQL Server Express 資料庫。

## <a name="model-compatibility-cannot-be-checked"></a>模型的相容性無法檢查

### <a name="scenario"></a>情節

您更新*Web.config*檔案連接字串以指向新的 SQL Server Express 資料庫，並執行應用程式第一次您會看到下列錯誤訊息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

如果您將 Web.config 檔案中的資料庫名稱已用過您的電腦上的資料庫可能已存在的某些資料表之前。 選取新的名稱，未在電腦上之前變更使用*Web.config*指向要使用這個新的資料庫名稱的檔案。 或者，您可以使用[SQL Server Express 公用程式](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)或[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除現有的資料庫。

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>當指令碼會嘗試建立使用者或角色的 SQL 錯誤

### <a name="scenario"></a>情節

您使用上設定的資料庫部署**封裝/發行 SQL**索引標籤上，在部署期間執行的 SQL 指令碼包括 Create User 或 Create Role 命令和指令碼執行失敗時執行這些命令。 您可能會看到更多詳細的訊息，如下所示：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

當您完成設定資料庫部署中的，如果會發生這個錯誤**發行 Web**精靈而不是**封裝/發行 SQL**索引標籤上，建立的執行緒[組態和部署](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)論壇，而使用的方案將會加入至本疑難排解頁面。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

您用來執行部署的使用者帳戶沒有建立使用者或角色的權限。 比方說，控管公司可能會指派`db_datareader`， `db_datawriter`，和`db_ddladmin`它設定為您的使用者帳戶的角色。 這些是足夠建立大多數的資料庫物件，但無法建立使用者或角色。 若要避免此錯誤的方法之一是從資料庫部署中排除使用者和角色。 您可以藉由編輯`PreSource`項目為資料庫自動產生的指令碼，使其包含下列屬性：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

如需有關如何編輯資訊`PreSource`項目在專案檔中，請參閱[如何： 編輯專案檔中的部署設定](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)。 如果需要會在目的地資料庫中的使用者或開發資料庫中的角色，請連絡您的主機服務提供者尋求協助。

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>在部署期間執行自訂指令碼時發生 SQL Server 逾時錯誤

### <a name="scenario"></a>情節

您已指定自訂 SQL 指令碼，在部署期間，執行，而且當 Web Deploy 執行它們，階段逾時。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

執行多個有不同的交易模式的指令碼可能會導致逾時錯誤。 根據預設，自動產生的指令碼執行在交易中，但自訂指令碼不這麼做。 如果您選取**提取資料和/或從現有的資料庫結構描述**選項**封裝/發行 SQL**索引標籤上，而且如果您新增自訂 SQL 指令碼，您必須變更一些指令碼的交易設定，讓所有指令碼會使用相同的交易設定。 如需詳細資訊，請參閱[How to： 部署資料庫與 Web 應用程式專案](https://msdn.microsoft.com/library/dd465343.aspx)。

如果您已設定的交易，讓所有都相同，但是仍然收到這個錯誤，可能的解決方法是分別執行指令碼。 在**資料庫指令碼**方格中的**封裝/發行**SQL 索引標籤上，清除**Include**造成逾時錯誤，指令碼 核取方塊，然後發行專案。 然後移回至**資料庫指令碼**方格中，選取該指令碼**Include**核取方塊，然後清除**Include**其他指令碼的核取方塊。 然後將專案發行一次。 此時，當您發行時，只有在選取的自訂指令碼執行。

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>資料流資料的站台資訊清單尚無法使用

### <a name="scenario"></a>情節

當您要安裝封裝，使用*deploy.cmd*檔案搭配`t`（測試） 選項，您會看到下列錯誤訊息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

錯誤訊息表示命令無法產生測試報表。 不過，命令可能會執行如果您使用`y`（實際的安裝） 選項。 訊息只會指出沒有測試模式中執行命令的問題。

## <a name="this-application-requires-managedruntimeversion-v40"></a>此應用程式需要 ManagedRuntimeVersion v4.0

### <a name="scenario"></a>情節

當您嘗試部署時，您會看到下列錯誤訊息：

 錯誤： 資料流資料的 ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' 尚無法使用。 您嘗試使用應用程式集區已設定為 'v2.0' 的 'managedRuntimeVersion' 屬性。 此應用程式需要 'v4.0'。 

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

在 IIS 中未安裝 ASP.NET 4。 如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 已安裝在電腦上，但可能未安裝在 IIS 中。 在您要部署至伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>無法轉換 Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>情節

當您部署封裝時，您會看到下列錯誤訊息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

您嘗試部署從 IIS 管理員 」 中使用 Web 部署 1.1 UI，以已安裝的 Web Deploy 2.0 的伺服器。 如果您使用 IIS 的遠端系統管理工具來部署的封裝，檢查匯入**新提供的功能**對話方塊中，當您建立連接。 （此對話方塊可能只會顯示一次第一次建立連線時。 若要清除連接並重新開始，關閉 IIS 管理員和它再次啟動輸入`inetmgr /reset`在命令提示字元。)列出的其中一項功能會**Web 部署 UI**，而且它具有低於 8 的版本號碼，您要部署到伺服器可能會有 1.1 和 2.0 版本的 Web Deploy 安裝。 若要從已安裝 2.0 用戶端部署，伺服器必須只有 Web Deploy 2.0 安裝。 您必須連絡裝載提供者來解決這個問題。

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>無法載入 SQL Server Compact 的原生元件

### <a name="scenario"></a>情節

當您執行已部署的站台時，您會看到下列錯誤訊息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

已部署的站台沒有*amd64*和*x86*及其子資料夾中這些應用程式 下的原生組件*bin*資料夾。 SQL Server Compact 安裝的電腦，在原生組件位於*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*。 若要取得正確的檔案到正確的資料夾，Visual Studio 專案中的最佳方式是安裝 NuGet SqlServerCompact 封裝。 封裝安裝可新增建置後指令碼複製到原生組件*amd64*和*x86*。 為了讓這些部署，不過，您必須手動將它們包含在專案。 如需詳細資訊，請參閱[部署 SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教學課程。

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>部署 Entity Framework Code First 應用程式後的 「 路徑不正確 」 錯誤

### <a name="scenario"></a>情節

您部署的應用程式，例如 SQL Server Compact 的檔案中的應用程式儲存其資料庫會使用 Entity Framework Code First 移轉和 DBMS\_Data 資料夾。 您必須設定為在您第一次部署之後建立資料庫的 Code First 移轉。 當您執行應用程式會取得錯誤訊息，如下列範例所示：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

程式碼第一次嘗試建立資料庫，但應用程式\_資料資料夾不存在。 您未有任何檔案*應用程式\_資料*資料夾，當您部署，或您選取**排除應用程式\_資料**上**封裝/發行 Web**  索引標籤**專案屬性**視窗。 部署程序將不會在伺服器上建立資料夾，如果要複製到伺服器的資料夾中不有任何檔案。 如果您已經在資料庫中的站台設定，部署程序會刪除的檔案和*應用程式\_資料*資料夾本身如果您選取**移除目的端的其他檔案**中發行設定檔。 若要解決此問題，將預留位置檔案放在.txt 檔案等*應用程式\_資料*資料夾，請確定您沒有**排除應用程式\_資料**選取，並重新部署。 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>「 無法使用具有已經從其基礎 RCW 分離的 COM 物件。 」

### <a name="scenario"></a>情節

您已順利使用單鍵發行來部署您的應用程式啟動，而您收到這個錯誤：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

關閉並重新啟動 Visual Studio 通常是所有需要來解決這個錯誤。

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>部署失敗因為使用者認證用於發行沒有 setACL 授權單位

### <a name="scenario"></a>情節

發行失敗，發生錯誤，指出您沒有授權單位設定資料夾的權限 （您要使用的使用者帳戶沒有 setACL 授權單位）。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限應用程式\_Data 資料夾。 如果您知道網站資料夾的預設權限是否正確，而且不需要設定，您加入停用此行為**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;**至發行設定檔 （以會影響單一設定檔） 或 wpp.targets 檔案 （以會影響所有設定檔）。 如需如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>當應用程式嘗試寫入應用程式的資料夾時，存取被拒錯誤

### <a name="scenario"></a>情節

您的應用程式錯誤時嘗試建立或編輯的檔案中的其中一個應用程式資料夾中，因為它並沒有該資料夾的寫入授權單位。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限應用程式\_Data 資料夾。 如果您的應用程式需要的子資料夾的寫入權限，您就可以設定為該資料夾的權限，如中所示[設定資料夾權限](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)和[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。 如果您的應用程式需要站台的根資料夾的寫入權限，您必須防止從根資料夾上設定唯讀存取權，藉由新增**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;**至發行設定檔 （以會影響單一設定檔） 或 wpp.targets 檔案 （以會影響所有設定檔）。 如需如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。 <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>組態錯誤-targetFramework 屬性參考不晚於已安裝.NET Framework 版本的版本

### <a name="scenario"></a>情節

您已成功發佈之 web 專案的目標 ASP.NET 4.5，但當您執行應用程式 (具有`customErrors`模式設為 「 關閉 」 的 Web.config 檔案中) 您收到下列錯誤：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

錯誤頁面的 [來源錯誤] 方塊中，反白顯示從 Web.config 下行錯誤的原因為：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

伺服器不支援 ASP.NET 4.5。 請連絡來判斷何時及是否可以加入 ASP.NET 4.5 支援主機服務提供者。 如果升級伺服器不是一個選項，您必須部署 ASP.NET 4 或更早版本為目標的 web 專案改用。如果您將 ASP.NET 4 或較早的 web 專案部署到相同的目的地時，選取**移除目的端的其他檔案** 核取方塊**設定** 索引標籤**發行 Web**精靈。 如果您沒有選取**移除目的端的其他檔案**，您仍會繼續組態錯誤頁面。

專案**屬性**windows 包含目標 framework 下拉式清單中，但您無法解決這個問題只變更與**.NET Framework 4.5**至**.NET Framework 4**. 如果您的目標 framework 變更為舊版 framework 時，專案還是會有較新的 framework 版本的組件的參考，並不會執行。 您必須手動變更這些參考，或建立新的專案以.NET Framework 4 或更早版本為目標。 如需詳細資訊，請參閱[Web sites 的.NET Framework 目標](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)。

> [!div class="step-by-step"]
> [上一步](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
