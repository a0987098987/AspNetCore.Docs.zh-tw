---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 使用 Visual Studio 的 ASP.NET Web 部署： 疑難排解 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 65cd5cd9f7d1f9c5fdaea9b0d16bdfd84259efdd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834287"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>使用 Visual Studio 的 ASP.NET Web 部署： 疑難排解
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


此頁面描述一些常見的問題，當您使用 Visual Studio 部署 ASP.NET web 應用程式時可能發生。 每個會提供一或多個可能原因和對應的解決方案。

顯示案例適用於 Azure 和協力廠商主機服務提供者。 如需有關如何疑難排解 Azure App Service 中的 web 應用程式的詳細資訊，請參閱下列資源：

- [使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [監視 Azure App Service 中的 Web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [發表最新的 Windows Azure SDK 2.0 for.NET](http:// https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) （ScottGu 的部落格，會示範如何取得 Visual Studio 中的診斷記錄檔）

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>伺服器錯誤 '/' 應用程式-在目前的自訂錯誤設定防止錯誤的詳細資料從遠端檢視

### <a name="scenario"></a>情節

部署至遠端主機的站台之後，您會收到錯誤訊息，提及 customErrors 設定 Web.config 檔案中的，而不會指出實際錯誤的原因是什麼：

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

根據預設，ASP.NET 會顯示詳細的錯誤資訊，只有在本機電腦上執行您的 web 應用程式時。 通常您不想要顯示詳細的錯誤資訊，您的 web 應用程式時，透過網際網路公開可用，因為駭客可能無法使用此資訊來尋找應用程式中的弱點。 不過，當您部署或更新的站台至站台，有時會發生錯誤了，而且您需要取得實際的錯誤訊息。

若要啟用的應用程式，以顯示詳細的錯誤訊息，遠端主機上執行時，編輯 Web.config 檔，以便將 customErrors 模式設定、 重新部署應用程式，並再次執行應用程式：

1. 如果應用程式 Web.config 檔案有 acustomErrors 元素 thesystem.web 項目中，變更為 「 關閉 」 themode 屬性。 否則 acustomErrors 項目中新增 thesystem.web 項目並 themode 屬性設定為 「 關閉 」，如下列範例所示： 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. 部署應用程式。
3. 執行應用程式並重複任何您先前導致發生錯誤。 現在您可以看到實際的錯誤訊息的功能。
4. 當您解決錯誤時，請還原原始的 customErrors 設定，並重新部署應用程式。

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>無法建立/陰影複製 'ContosoUniversity' 已經存在的檔案。

### <a name="scenario"></a>情節

當您嘗試在 Visual Studio 中執行專案時您會得到錯誤頁面的訊息，如下列範例所示：

'/' 應用程式中的伺服器錯誤。 無法建立/陰影複製 'ContosoUniversity' 已經存在的檔案。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

等候一分鐘的時間和重新整理瀏覽器中，或重新編譯的站台並嘗試再次執行它。

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>存取遭到拒絕在網頁中使用 SQL Server Compact

### <a name="scenario"></a>情節

當您部署使用 SQL Server Compact 的網站和您在存取資料庫的已部署站台執行頁面時，您會看到下列錯誤訊息：

存取被拒絕。 (來自 HRESULT 的例外狀況： 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

在伺服器上的網路服務帳戶必須能夠讀取 SQL Compact 服務中的原生二進位檔*bin\amd64*或是*bin\x86*資料夾中，但沒有讀取這些資料夾的權限。 讀取網路服務的權限集*bin*資料夾，並確認其擴充至子資料夾的權限。

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>無法讀取組態檔，因為沒有足夠的權限

### <a name="scenario"></a>情節

當您按一下 Visual Studio 發行應用程式部署至 IIS 在您的本機電腦上的按鈕發行會失敗並**輸出**視窗會顯示一則錯誤訊息如下所示：

讀取 IIS 設定檔 'MACHINE/重新導向' 時，就會發生錯誤。 執行此作業的身分識別為...錯誤： 無法讀取組態檔，因為沒有足夠的權限。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

若要使用單鍵發行至 IIS 在本機電腦上，您必須以系統管理員權限執行 Visual Studio。 關閉 Visual Studio 並重新啟動系統管理員權限。

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>無法連接到目的地電腦...使用指定的處理序

### <a name="scenario"></a>情節

當您按一下 Visual Studio 發佈按鈕來部署應用程式時，發行會失敗並**輸出**視窗會顯示一則錯誤訊息如下所示：

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

Proxy 伺服器會中斷與目的地伺服器的通訊。 從 Windows 控制台或 Internet Explorer 中，選取**網際網路選項**，然後選取**連線** 索引標籤。在 **網際網路內容** 對話方塊中，按一下**LAN 設定**。 在 **區域網路 (LAN) 設定**對話方塊中，清除**自動偵測設定**核取方塊。 然後再按一下 [發行] 按鈕。

如果問題持續發生，請連絡您的系統管理員，以判斷哪些可以使用 proxy 或防火牆設定。 問題是因為 Web Deploy Web 管理服務部署 (8172); 使用非標準連接埠對於其他連線，Web Deploy，將會使用連接埠 80。 當您要部署到協力廠商主機服務提供者時，通常使用 Web 管理服務。

## <a name="default-net-40-application-pool-does-not-exist"></a>預設.NET 4.0 的應用程式集區不存在

### <a name="scenario"></a>情節

當您部署的應用程式需要.NET Framework 4 時，您會看到下列錯誤訊息：

預設.NET 4.0 應用程式集區不存在或無法加入應用程式。 請確認 ASP.NET 4.0 安裝在這部電腦上。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

在 IIS 中未安裝 ASP.NET 4。 如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 的電腦上已安裝，但可能未安裝在 IIS 中。 在您要部署伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

您也可能需要手動設定預設應用程式集區的.NET Framework 版本。 如需詳細資訊，請參閱作為測試環境教學課程，在這一系列的 iis 部署。

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>初始化字串的格式不符合從索引 0 處開始的規格。

### <a name="scenario"></a>情節

部署應用程式使用一種單鍵之後發行，當您執行的頁面存取資料庫，得到下列錯誤訊息：

初始化字串的格式不符合從索引 0 處開始的規格。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

開啟*Web.config*檔案中的已部署的站台和檢查連接字串值是否會以 $ 開頭 (ReplacableToken\_，如下列範例所示：

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

如果連接字串看起來像此範例中，編輯專案檔，並針對所有組建組態的 PropertyGroup 項目中加入下列屬性：

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

然後重新部署應用程式。

## <a name="http-500-internal-server-error"></a>HTTP 500 內部伺服器錯誤

### <a name="scenario"></a>情節

當您執行已部署的站台時，您會看到下列錯誤訊息沒有指出錯誤的原因的特定資訊：

HTTP 錯誤 500-內部伺服器錯誤。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

原因有很多的 500 錯誤，但如果您要遵循這些教學課程中的其中一個可能原因是您將 XML 項目放在其中一個 Web.config 轉換檔中錯誤的位置。 例如，您會收到這個錯誤如果您將插入的轉換&lt;位置&gt;下方的項目&lt;system.web&gt;而不是正下方&lt;組態&gt;。 若要確認轉換如預期般運作，您可以使用 Web.config 轉換預覽功能。 如果您發現不正確的原始轉換解決方案是更正轉換檔案，然後重新部署。 如果錯誤不明顯，請嘗試 標記為註解轉換，然後重新部署，以查看哪一個導致 500 錯誤。

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 內部伺服器錯誤

### <a name="scenario"></a>情節

當您執行已部署的站台時，您會看到下列錯誤訊息：

HTTP 錯誤 500.21-內部伺服器錯誤。 「 Pagehandlerfactory-isapi-整合式 」 的處理常式有不正確的模組"ManagedPipelineHandler 」 模組清單中。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

站台，您已部署 ASP.NET 4 中，但 ASP.NET 4 未註冊在 IIS 伺服器的目標。 在伺服器上開啟提升權限的命令提示字元，並執行下列命令註冊 ASP.NET 4:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

您也可能需要手動設定預設應用程式集區的.NET Framework 版本。 如需詳細資訊，請參閱作為測試環境教學課程，在這一系列的 iis 部署。

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>登入失敗的應用程式中的開啟 SQL Server Express Database\_資料

### <a name="scenario"></a>情節

您已更新*Web.config*檔案以指向做為 SQL Server Express 資料庫的連接字串 *.mdf*檔案中您*應用程式\_資料*資料夾，然後第一個您執行應用程式，您會看到下列錯誤訊息的時間：

「 System.Data.SqlClient.SqlException： 無法開啟資料庫"DatabaseName 」 登入所要求。 登入失敗。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

名稱 *.mdf*檔案無法比對任何具有曾經存在過您的電腦的 SQL Server Express 資料庫的名稱，即使您刪除 *.mdf*先前已存在的資料庫檔案。 變更的名稱 *.mdf*從未使用做為資料庫名稱變更為名稱的檔案*Web.config*檔案，以使用新的名稱。 或者，您可以使用[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除先前已經有 SQL Server Express 資料庫。

## <a name="model-compatibility-cannot-be-checked"></a>模型的相容性無法檢查

### <a name="scenario"></a>情節

您已更新*Web.config*檔案連接字串以指向新的 SQL Server Express 資料庫，並執行應用程式的第一次您會看到下列錯誤訊息：

無法檢查模型相容性，因為資料庫不包含模型中繼資料。 請確定 IncludeMetadataConvention 確認已新增 DbModelBuilder 慣例。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

如果之前您在電腦上，資料庫可能已經存在的某些資料表中，曾經使用您放在 Web.config 檔案中的資料庫名稱。 選取您電腦之前變更尚未使用的新名稱*Web.config*檔以指向使用這個新的資料庫名稱。 或者，您可以使用[SQL Server Express 公用程式](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)或是[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除現有的資料庫。

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>SQL 錯誤時指令碼會嘗試建立使用者或角色

### <a name="scenario"></a>情節

您使用上設定的資料庫部署**封裝/發行 SQL**  索引標籤，在部署期間執行的 SQL 指令碼包含 Create User 或 Create Role 命令和指令碼執行失敗時執行這些命令。 您可能會看到更多詳細的訊息，如下所示：

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

此錯誤發生時，您已設定中的資料庫部署**發佈 Web**精靈而非**封裝/發行 SQL**索引標籤上，建立中的執行緒[組態和部署](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)論壇和解決方案將會新增至本疑難排解頁面。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

您用來執行部署的使用者帳戶沒有建立使用者或角色的權限。 比方說，控管公司的資料庫可能會被指派\_datareader，db\_資料寫入元，與 db\_ddladmin 角色，它會設定為您的使用者帳戶。 這些是足夠用於建立大多數的資料庫物件，但不適用於建立使用者或角色。 若要避免錯誤的方法之一是藉由從資料庫部署中排除使用者和角色。 您可以藉由編輯資料庫的自動產生的指令碼的 PreSource 項目，使其包含下列屬性：

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

如需如何編輯專案檔中的 PreSource 項目資訊，請參閱[如何： 編輯專案檔中的部署設定](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)。 如果需要在目的地資料庫中的使用者或開發資料庫中的角色，請連絡您的主機服務提供者尋求協助。

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server 逾時錯誤時在部署期間執行自訂指令碼

### <a name="scenario"></a>情節

您已指定在部署期間，執行自訂 SQL 指令碼和 Web Deploy 執行時它們，它們的逾時間。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

執行具有不同的交易模式的多個指令碼，可能會導致逾時錯誤。 根據預設，自動產生的指令碼執行在交易中，但不是這麼做的自訂指令碼。 如果您選取**提取資料及/或從現有的資料庫結構描述**選項**封裝/發行 SQL**索引標籤上，而且如果您加入自訂的 SQL 指令碼，您必須變更一些指令碼的交易設定以便所有的指令碼會使用相同的交易設定。 如需詳細資訊，請參閱 <<c0> [ 如何： 資料庫使用 Web 應用程式專案部署](https://msdn.microsoft.com/library/dd465343.aspx)。

如果您已設定交易設定，使所有都相同，但仍然收到這個錯誤，可能的解決方法是分別執行指令碼。 在 **資料庫指令碼**方格中的**封裝/發行**SQL 索引標籤上，清除**Include**造成逾時錯誤，指令碼的核取方塊，然後發行專案。 然後移回**資料庫指令碼**方格中，選取該指令碼**Include**核取方塊，然後清除**Include**其他指令碼的核取方塊。 然後重新發佈專案。 此時，當您發行時，只選取自訂的指令碼執行。

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>站台資訊清單的 Stream 資料尚無法使用

### <a name="scenario"></a>情節

當您要安裝封裝，使用*deploy.cmd*檔案與 t （測試） 選項中，您會看到下列錯誤訊息：

錯誤： 資料流資料的 ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' 尚無法使用。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

錯誤訊息表示命令無法產生測試報告。 不過，如果您使用 y （實際安裝） 選項，可能會執行的命令。 訊息僅會指示是在測試模式中執行命令的問題。

## <a name="this-application-requires-managedruntimeversion-v40"></a>此應用程式需要 ManagedRuntimeVersion v4.0

### <a name="scenario"></a>情節

當您嘗試部署時，您會看到下列錯誤訊息：

您嘗試使用應用程式集區已設定為 'v2.0' 的 'managedRuntimeVersion' 屬性。 此應用程式需要 'v4.0'。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

在 IIS 中未安裝 ASP.NET 4。 如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 的電腦上已安裝，但可能未安裝在 IIS 中。 在您要部署伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>無法轉換 Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>情節

當您部署封裝時，您會看到下列錯誤訊息：

無法將類型 'Microsoft.Web.Deployment.DeploymentProviderOptions' 到 'Microsoft.Web.Deployment.DeploymentProviderOptions' 的物件。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

您嘗試部署從 IIS 管理員 」 中使用 Web 部署 1.1 UI 已安裝的 Web Deploy 2.0 的伺服器。 如果您使用 IIS 的遠端系統管理工具來部署匯入封裝時，檢查**新提供的功能**對話方塊中，當您建立連線。 （此對話方塊可能只會顯示一次第一次建立連接時。 若要清除連線，然後再重新啟動，關閉 IIS 管理員並啟動它一次輸入 inetmgr/重設在命令提示字元。）列出的其中一項功能會**Web 部署 UI**，其版本號碼低於 8，您要部署到伺服器可能會有 1.1 和 2.0 的 Web Deploy 安裝的版本。 若要從已安裝的 2.0 用戶端部署，伺服器必須只有 Web Deploy 2.0 安裝。 您必須連絡主控提供者，若要解決此問題。

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>無法載入 SQL Server Compact 的原生元件

### <a name="scenario"></a>情節

當您執行已部署的站台時，您會看到下列錯誤訊息：

無法載入 SQL Server Compact 對應至版本 8482 的 ADO.NET 提供者的原生元件。 安裝 SQL Server Compact 的正確版本。 請參閱 KB 文章 974247 如需詳細資訊。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

已部署的站台沒有*amd64*並*x86*及其子資料夾中它們的應用程式的原生組件*bin*資料夾。 SQL Server Compact 安裝的電腦，在原生組件位於*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*。 若要取得正確的檔案到正確的資料夾，Visual Studio 專案中的最佳方式是安裝 NuGet SqlServerCompact 封裝。 套件安裝新增建置後指令碼來複製原生組件，載入*amd64*並*x86*。 為了讓這些部署，不過，您必須手動將其包含在專案中。 如需詳細資訊，請參閱 <<c0> [ 部署 SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教學課程。

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>部署 Entity Framework Code First 應用程式之後的 「 路徑不正確 」 錯誤

### <a name="scenario"></a>情節

您部署的應用程式，例如 SQL Server Compact 將其資料庫儲存於應用程式中的檔案就會使用 Entity Framework Code First 移轉和 DBMS\_Data 資料夾。 您必須設定為在您第一次部署之後建立資料庫的 Code First 移轉。 當您執行應用程式時您會收到錯誤訊息如下列範例所示：

路徑不是有效的。 請檢查資料庫目錄。 [路徑 = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

程式碼第一次嘗試建立資料庫，但應用程式\_資料資料夾不存在。 您未有任何檔案*應用程式\_資料*當您部署，或您所選取的資料夾**排除應用程式\_資料**上**封裝/發行 Web**  索引標籤的 專案屬性 視窗。 部署程序不會在伺服器上建立資料夾，如果要複製到伺服器的資料夾中不有任何檔案。 如果您已經在網站中設定的資料庫，在部署程序會刪除的檔案和*應用程式\_資料*本身如果您選取的資料夾**移除目的地上的其他檔案**中發行設定檔。 若要解決此問題，將預留位置檔案放在.txt 檔案*應用程式\_資料*資料夾，請確定您沒有**排除應用程式\_資料**選取，並重新部署。

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>「 無法使用分開基礎 RCW 的 COM 物件。 」

### <a name="scenario"></a>情節

您已順利使用單鍵發行來部署您的應用程式啟動，而您收到此錯誤：

Web deployment 工作失敗。 (無法完成遠端代理程式的 url 要求 '<https://serverurl.com/msdeploy.axd?site=sitename>'。)  
 無法完成遠端代理程式的 url 要求 '<https://url/msdeploy.axd?site=sitename>'。  
要求已中止： 已取消要求。  
不能分開基礎 RCW 的 COM 物件。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

關閉並重新啟動 Visual Studio 通常是所有所需解決此錯誤。

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>部署失敗因為使用者認證用於發佈的並沒有 setACL 授權單位

### <a name="scenario"></a>情節

發行失敗，發生錯誤，指出您沒有權限設定資料夾權限 （您使用的使用者帳戶沒有 setACL 授權單位）。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限的應用程式\_Data 資料夾。 若您知道的站台資料夾的預設權限是否正確，而且不需要設定，您加入停用此行為**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或.wpp.targets 檔案 （以會影響所有設定檔）。 如需有關如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>當應用程式嘗試寫入應用程式資料夾時，存取被拒錯誤

### <a name="scenario"></a>情節

您的應用程式錯誤時嘗試建立或編輯的檔案中的其中一個應用程式資料夾中，因為它並沒有該資料夾的寫入授權單位。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限的應用程式\_Data 資料夾。 如果您的應用程式需要的子資料夾的寫入權限，您可以設定為該資料夾的權限中所示設定資料夾權限並部署至生產環境教學課程，在這一系列。 如果您的應用程式需要的站台的根資料夾的寫入權限，您必須防止根資料夾上設定唯讀存取權，加上**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或.wpp.targets 檔案 （以會影響所有設定檔）。 如需有關如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>組態錯誤-targetFramework 屬性參考的版本晚於安裝的.NET Framework 版本

### <a name="scenario"></a>情節

您已成功發佈 web 專案以 ASP.NET 4.5 為目標，但是 （使用 customErrors 模式設定為 「 關閉 」 的 Web.config 檔案中） 執行應用程式時您會收到下列錯誤：

'targetFramework' 屬性中&lt;編譯&gt;才能目標版本 4.0 和更新版本的.NET Framework 使用 Web.config 檔案的項目 (例如，'&lt;編譯 targetFramework ="4.0"&gt;'). 'TargetFramework' 屬性目前參考的版本晚於安裝的.NET Framework 版本。 指定有效的目標版本的.NET Framework 中，或安裝必要的.NET Framework 版本。

來源錯誤中的 [錯誤] 頁面中，反白顯示下行從 Web.config 做為錯誤的原因：

&lt;compilation targetFramework="4.5" /&gt;

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

伺服器不支援 ASP.NET 4.5。 請連絡來判斷何時及是否可以加入支援 ASP.NET 4.5 的主機服務提供者。 如果升級伺服器不是一個選項，您必須部署之 web 專案的目標 ASP.NET 4 或更早版本改。

如果您將 ASP.NET 4 或較早的 web 專案部署到相同的目的地時，選取**移除其他檔案目的地** 核取方塊**設定**索引標籤**發佈 Web**精靈。 如果您未選取**移除目的地上的其他檔案**，您仍會繼續設定錯誤頁面。

專案**屬性**windows 包含目標 framework 下拉式清單中，但您不能解決這個問題，只要變更，從 **.NET Framework 4.5**到 **.NET Framework 4**. 如果您將目標 framework 變更為較早的 framework 版本時，專案還是會有較新的 framework 版本的組件的參考，並不會執行。 您必須手動變更這些參考，或建立新的專案以.NET Framework 4 或更早版本為目標。 如需詳細資訊，請參閱 <<c0> [ 網站的.NET Framework 目標](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)。

## <a name="medium-trust-errors"></a>中度信任錯誤

### <a name="scenario"></a>情節

當您在生產環境中執行您的應用程式時，它會取得與中度信任相關的錯誤。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

許多協力廠商裝載提供者會將您的網站以中度信任，這表示它不允許執行的一些事項。 例如，應用程式程式碼無法存取 Windows 登錄和無法讀取或寫入您的應用程式資料夾階層之外的檔案。 應用程式執行的預設*完全信任*您本機電腦上，這表示應用程式可能無法執行一些作業，當您將它部署到生產環境時將會失敗。

您可以設定應用程式以在本機 IIS 環境中的中度信任執行，以進行疑難排解。 若要這樣做，請開啟 應用程式*Web.config*檔案，並新增**信任**中的項目**system.web**項目，在此範例中所示。

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

現在應用程式會在 IIS 中的中度信任上執行，即使在您的本機電腦上。

沒有此資料庫，如果您要部署至 Azure App Service，因為 Azure 不需要中度信任。 在本教學課程以 2 月版，2012 年的時間，讓您的應用程式的中度信任執行使用這個方法會造成錯誤在 Azure 中。

如果您使用 Entity Framework Code First 移轉，而且您要部署至主機服務提供者，在中度信任執行您的應用程式，請確定您有 5.0 版或更新的版本。 在 Entity Framework 4.3 版中，移轉會需要完全信任，才能更新資料庫結構描述。

## <a name="http-40417-not-found-error"></a>找不到 HTTP 404.17 錯誤

### <a name="scenario"></a>情節

當您在 IIS 中的開發電腦上執行已部署的站台時，您會看到下列報表伺服器無法處理 Default.aspx 的錯誤訊息：

找不到的 HTTP 錯誤 404.17-

要求的內容似乎是指令碼，並不會處理靜態檔案處理常式。

### <a name="possible-cause-and-solution"></a>可能的原因和解決方案

在您的電腦上可能未安裝 ASP.NET 4.5。 為測試環境本系列教學課程說明如何安裝 ASP.NET 4.5，請參閱 IIS 中部署的步驟。

> [!div class="step-by-step"]
> [上一步](deploying-extra-files.md)
