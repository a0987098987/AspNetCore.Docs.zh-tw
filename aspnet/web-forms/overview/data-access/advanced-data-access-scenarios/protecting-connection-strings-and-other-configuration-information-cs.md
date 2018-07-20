---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: 保護連接字串和其他組態資訊 (C#) |Microsoft Docs
author: rick-anderson
description: ASP.NET 應用程式通常會儲存在 Web.config 檔案中的組態資訊。 其中有些資訊是機密，保證保護。 由 def。...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 24a066a4c9d60e3dc1897bd3143b382b876e027c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842217"
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>保護連接字串和其他組態資訊 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip)或[下載 PDF](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> ASP.NET 應用程式通常會儲存在 Web.config 檔案中的組態資訊。 其中有些資訊是機密，保證保護。 依預設這個檔案將不會處理網站訪客，但系統管理員或駭客可能會存取 Web 伺服器的檔案系統，並檢視檔案的內容。 在本教學課程中我們了解 ASP.NET 2.0 可讓我們來保護機密資訊加密 Web.config 檔案的區段。


## <a name="introduction"></a>簡介

ASP.NET 應用程式的組態資訊通常儲存在名為 XML 檔案`Web.config`。 這些教學課程期間，我們已更新`Web.config`少數的次數。 建立時`Northwind`型別中的資料集[第一個教學課程](../introduction/creating-a-data-access-layer-cs.md)，連接字串資訊，例如自動加入至`Web.config`在`<connectionStrings>`一節。 稍後，在[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中，我們以手動方式更新`Web.config`，來加入`<pages>`項目會指出應該使用我們的專案中的 ASP.NET 網頁的所有`DataWebControls`佈景主題。

由於`Web.config`可能包含機密資料，例如連接字串，很重要的內容`Web.config`保持安全且隱藏未經授權的檢視器。 根據預設，任何 HTTP 要求與檔案`.config`延伸模組由 ASP.NET 引擎，它會傳回*不提供這種類型的頁面*[圖 1] 所示的訊息。 這表示訪客不能檢視您`Web.config`只要輸入檔案 s 內容 http://www.YourServer.com/Web.config其 s 的瀏覽器網址列。


[![瀏覽 Web.config 透過瀏覽器傳回這類頁面是不會處理訊息](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**圖 1**： 瀏覽`Web.config`透過瀏覽器傳回這類頁面不會處理訊息 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


但如果攻擊者能夠找到一些其他攻擊，讓她檢視您`Web.config`檔案 s 內容嗎？ 可能的攻擊者用來做什麼這項資訊，以及可以採取哪些步驟來進一步保護中的機密資訊`Web.config`嗎？ 幸運的是，大部分的各節`Web.config`未包含機密資訊。 什麼傷害可以攻擊者 perpetrate 若他們知道使用 ASP.NET 網頁的佈景主題的預設名稱？

某些`Web.config`章節中，不過，包含機密資訊可能包含連接字串、 使用者名稱、 密碼、 伺服器名稱、 加密金鑰和其他等等。 這項資訊通常位於下列`Web.config`各節：

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

在本教學課程中我們將探討技術可保護這類的機密組態資訊。 如我們所見，.NET Framework 2.0 版包含了能以程式設計方式加密及解密選取的組態區段，即可輕易地的受保護的組態系統。

> [!NOTE]
> 本教學課程中隨附了解 Microsoft 建議從 ASP.NET 應用程式連接到資料庫。 除了加密您的連接字串，您可以協助強化系統藉由確保您要連線到安全的方式中的資料庫。


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>步驟 1： 探索 ASP.NET 2.0 s 受保護的組態選項

ASP.NET 2.0 包含加密和解密組態資訊的受保護的組態系統。 這包括在.NET Framework 中可用來以程式設計方式加密或解密組態資訊的方法。 受保護的組態系統會使用[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，可讓開發人員選擇使用何種密碼編譯的實作。

.NET Framework 隨附兩個受保護的組態提供者：

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -使用非對稱[RSA 演算法](http://en.wikipedia.org/wiki/Rsa)加密和解密。
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -使用 Windows[的資料保護 API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx)加密和解密。

由於受保護的組態系統會實作提供者設計模式，就可以建立自己的受保護的組態提供者，並插入您的應用程式。 請參閱[實作受保護組態提供者](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx)如需此程序的詳細資訊。

RSA 和 DPAPI 的提供者會將金鑰用於其加密和解密的常式，以及這些金鑰可以儲存在電腦或使用者層級。 電腦層級索引鍵是適用於其自己專用的伺服器的 web 應用程式執行所在的案例，或者如果有多個應用程式需要共用的伺服器上加密的資訊。 使用者層級索引鍵是在相同的伺服器上的其他應用程式不應該儲存能夠解密您的應用程式的受保護的 s 組態區段的共用裝載環境中更安全的選項。

在本教學課程中的 DPAPI 提供者和電腦層級索引鍵，將使用我們的範例。 具體來說，我們將探討加密`<connectionStrings>`一節`Web.config`，不過，受保護的組態系統可用來加密任何大部分`Web.config`一節。 如需使用使用者層級索引鍵，或使用 RSA 提供者的資訊，請參閱在本教學課程結尾處的其他讀數一節中的資源。

> [!NOTE]
> `RSAProtectedConfigurationProvider`並`DPAPIProtectedConfigurationProvider`中註冊提供者`machine.config`提供者名稱的副檔名`RsaProtectedConfigurationProvider`和`DataProtectionConfigurationProvider`分別。 加密或解密我們必須提供適當的提供者名稱的組態資訊時 (`RsaProtectedConfigurationProvider`或是`DataProtectionConfigurationProvider`) 而非實際的型別名稱 (`RSAProtectedConfigurationProvider`和`DPAPIProtectedConfigurationProvider`)。 您可以找到`machine.config`檔案中`$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`資料夾。


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>步驟 2： 以程式設計方式加密和解密組態區段

只要幾行程式碼中，我們可以加密或解密使用指定的提供者的特定組態區段。 程式碼中，我們會在短時間內，看到只需要以程式設計方式參考適當的組態 區段中，呼叫其`ProtectSection`或是`UnprotectSection`方法，然後再呼叫`Save`方法，將所做的變更。 此外，.NET Framework 包含有用的命令列公用程式可加密和解密組態資訊。 我們將探討在步驟 3 中的這個命令列公用程式。

為了說明以程式設計方式保護的組態資訊，可讓 s 建立 ASP.NET 網頁，其中包含用於加密和解密的按鈕`<connectionStrings>`一節中`Web.config`。

首先開啟`EncryptingConfigSections.aspx`頁面中`AdvancedDAL`資料夾。 將文字方塊控制項從工具箱拖曳至設計工具中，設定其`ID`屬性，以`WebConfigContents`、 其`TextMode`屬性設`MultiLine`，並將其`Width`和`Rows`屬性給 95%和 15，會分別。 此文字方塊控制項會顯示的內容`Web.config`讓我們快速查看是否，是否加密內容。 當然，在實際的應用程式會永遠不會想要顯示的內容`Web.config`。

下方的文字方塊中，新增名為兩個按鈕控制項`EncryptConnStrings`和`DecryptConnStrings`。 若要加密連接字串及解密連接字串設定其 Text 屬性。

此時您的畫面應該看起來類似 圖 2。


[![將文字方塊和兩個按鈕 Web 控制項加入頁面](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**圖 2**： 新增至頁面的文字方塊和兩個按鈕 Web 控制項 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


接下來，我們必須撰寫程式碼來載入及顯示的內容`Web.config`在`WebConfigContents`文字方塊中，第一個頁面時載入。 頁面 s 程式碼後置類別中加入下列程式碼。 此程式碼會新增一個名為方法`DisplayWebConfig`，並呼叫從`Page_Load`事件處理常式時`Page.IsPostBack`是`false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

`DisplayWebConfig`方法會使用[`File`類別](https://msdn.microsoft.com/library/system.io.file.aspx)以開啟應用程式 s`Web.config`檔案[`StreamReader`類別](https://msdn.microsoft.com/library/system.io.streamreader.aspx)其內容讀入字串，而[`Path`類別](https://msdn.microsoft.com/library/system.io.path.aspx)產生的實體路徑`Web.config`檔案。 這三個類別會在找到的所有[`System.IO`命名空間](https://msdn.microsoft.com/library/system.io.aspx)。 因此，您必須新增`using``System.IO`陳述式的程式碼後置類別，或者這些類別具有名稱的前置詞`System.IO.`。

接下來，我們需要加入兩個按鈕控制項的事件處理常式`Click`事件，並新增必要的程式碼，來加密和解密`<connectionStrings>`區段使用 DPAPI 的提供者的電腦層級金鑰。 從設計工具中，按兩下每個按鈕來新增`Click`中程式碼後置的事件處理常式類別，然後再將下列程式碼：


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

使用中的兩個事件處理常式的程式碼是幾乎完全相同。 它們都會啟動藉由取得目前的應用程式 s 的相關資訊`Web.config`透過檔案[`WebConfigurationManager`類別](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx)s [ `OpenWebConfiguration`方法](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx)。 這個方法會傳回指定的虛擬路徑的 web 組態檔。 下一步`Web.config`檔案 s`<connectionStrings>`一節透過存取[`Configuration`類別](https://msdn.microsoft.com/library/system.configuration.configuration.aspx)s [ `GetSection(sectionName)`方法](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx)，以傳回[ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx)物件。

`ConfigurationSection`物件包含[`SectionInformation`屬性](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx)，提供其他資訊和相關的組態區段的功能。 上面顯示的程式碼，我們可以判斷是否藉由檢查加密的組態區段`SectionInformation`屬性的`IsProtected`屬性。 此外，區段可以要加密或解密透過`SectionInformation`屬性 s`ProtectSection(provider)`和`UnprotectSection`方法。

`ProtectSection(provider)`方法接受做為輸入字串，指定要加密時所使用的受保護的組態提供者的名稱。 在  `EncryptConnString` s 按鈕事件處理常式，我們將傳遞到 DataProtectionConfigurationProvider`ProtectSection(provider)`方法，以便使用 DPAPI 的提供者。 `UnprotectSection`方法可以判斷用來加密組態區段，因此不需要任何輸入參數的提供者。

之後呼叫`ProtectSection(provider)`或是`UnprotectSection`方法，您必須呼叫`Configuration`物件 s [ `Save`方法](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx)來保存變更。 一旦已加密或解密的組態資訊並儲存變更，我們呼叫`DisplayWebConfig`載入更新`Web.config`到 TextBox 控制項的內容。

一旦您輸入上述的程式碼，測試它，請造訪`EncryptingConfigSections.aspx`透過瀏覽器的頁面。 您應該一開始會看到列出的內容頁面`Web.config`與`<connectionStrings>`以純文字顯示的區段 （請參閱 [圖 3]）。


[![將文字方塊和兩個按鈕 Web 控制項加入頁面](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**圖 3**： 新增至頁面的文字方塊和兩個按鈕 Web 控制項 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


現在按一下 [加密連接字串] 按鈕。 標記已啟用要求驗證，如果從回傳`WebConfigContents`文字方塊中會產生`HttpRequestValidationException`，其中顯示訊息，有潛在危險`Request.Form`從用戶端偵測到值。 要求驗證，ASP.NET 2.0 中的預設會啟用，禁止包含未編碼 HTML 的回傳，並可協助防止指令碼資料隱碼攻擊。 這項檢查可以停用在頁面或應用程式層級。 若要將它關閉此頁面，設定`ValidateRequest`設為`false`在`@Page`指示詞。 `@Page`指示詞位於頂端的 [s] 頁面的宣告式標記。


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

如需有關要求驗證，它的目的，在網頁和應用程式層級，以及如何為 HTML 編碼標記為停用，請參閱[要求驗證-防止指令碼攻擊](../../../../whitepapers/request-validation.md)。

停用頁面的要求驗證之後, 再試一次按一下 [加密連接字串] 按鈕。 在回傳時，將存取設定檔並將其`<connectionStrings>`加密使用 DPAPI 的提供者的一節。 文字方塊則會更新以顯示新`Web.config`內容。 如 [圖 4] 所示，`<connectionStrings>`現在都已加密資訊。


[![按一下 加密連接字串按鈕會加密&lt;connectionString&gt;區段](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**圖 4**： 按一下 加密連接字串按鈕加密`<connectionString>`一節 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


加密`<connectionStrings>`我的電腦上產生的區段之後，雖然部分中的內容`<CipherData>`為了簡潔起見已移除項目：


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>`項目會指定用來執行加密提供者 (`DataProtectionConfigurationProvider`)。 這項資訊由`UnprotectSection`方法時按一下 [解密連接字串] 按鈕。


當連接字串資訊從存取`Web.config`-可能是由我們所撰寫程式碼，從 SqlDataSource 控制項或在我們的輸入資料集 Tableadapter 的自動產生程式碼-就會自動解密。 簡單地說，我們不需要新增任何額外的程式碼或邏輯來解密加密`<connectionString>`一節。 若要進行示範，請瀏覽先前的教學課程的其中一個在這個階段，例如簡單顯示教學課程中，從基本報告 區段 (`~/BasicReporting/SimpleDisplay.aspx`)。 如 [圖 5] 所示，本教學課程的運作方式完全如我們所預期，表示加密的連接字串資訊時所自動解密由 ASP.NET 網頁。


[![資料存取層會自動解密連接字串資訊](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**圖 5**： 資料存取層會自動解密連接字串資訊 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


若要還原`<connectionStrings>`回到其純文字來表示區段中，按一下 [解密連接字串] 按鈕。 在回傳中，您應該看到中的連接字串`Web.config`以純文字。 此時您的畫面看起來應該像第一次瀏覽此頁面 （請參閱 圖 3） 時。

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>步驟 3： 加密使用的組態區段`aspnet_regiis.exe`

.NET Framework 包含各種不同的命令列工具，在`$WINDOWS$\Microsoft.NET\Framework\version\`資料夾。 在 [使用 SQL 快取相依性](../caching-data/using-sql-cache-dependencies-cs.md)教學課程中，比方說，我們探討使用`aspnet_regsql.exe`命令列工具來新增必要的 SQL 快取相依性的基礎結構。 在此資料夾中的另一個有用的命令列工具是[ASP.NET IIS 註冊工具 (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx)。 正如其名，則 ASP.NET IIS 註冊工具是主要用來與 Microsoft 的專業級 Web 伺服器，IIS 註冊 ASP.NET 2.0 應用程式中。 它與 IIS 相關的功能，除了 ASP.NET IIS 註冊工具也可用來加密或解密指定的組態區段中`Web.config`。

下列陳述式會顯示用來加密組態區段的一般語法`aspnet_regiis.exe`命令列工具：


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*一節*是要加密 （例如 connectionStrings) 的組態區段*實體\_目錄*是 web 應用程式的根目錄的完整的實體路徑和*提供者*用 （例如 DataProtectionConfigurationProvider) 的受保護的組態提供者的名稱。 或者，如果在 IIS 中註冊 web 應用程式，您可以輸入而不是使用下列語法的實體路徑的虛擬路徑：


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

下列`aspnet_regiis.exe`範例會加密`<connectionStrings>`區段具有電腦層級索引鍵使用 DPAPI 的提供者：


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

同樣地，`aspnet_regiis.exe`命令列工具可用來解密組態區段。 而不是使用`-pef`切換，請使用`-pdf`(或 instead of `-pe`，使用`-pd`)。 此外，請注意解密時不需要提供者名稱。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> 因為我們使用 DPAPI 的提供者，它會使用金鑰的電腦，您必須執行`aspnet_regiis.exe`來自相同的機器會在提供網頁。 比方說，如果您從本機開發電腦上執行此命令列程式，然後將加密的 Web.config 檔案上傳到生產環境伺服器時，實際執行伺服器將無法再解密連接字串資訊，因為它已加密使用您的開發電腦的特定索引鍵。 RSA 提供者並沒有這項限制，即能將 RSA 金鑰匯出到另一部電腦。


## <a name="understanding-database-authentication-options"></a>了解資料庫驗證選項

任何應用程式可以發出之前`SELECT`， `INSERT`， `UPDATE`，或`DELETE`查詢到 Microsoft SQL Server 資料庫，資料庫必須先識別要求者。 此程序稱為*驗證*和 SQL Server 提供兩種驗證方法：

- **Windows 驗證**-應用程式執行所在的程序用來與資料庫通訊。 當執行 ASP.NET 應用程式透過 Visual Studio 2005 的 ASP.NET 程式開發伺服器時，ASP.NET 應用程式會假設目前登入使用者的身分識別。 ASP.NET 應用程式上 Microsoft Internet Information Server (IIS)，ASP.NET 應用程式通常是假設的身分識別`domainName``\MachineName`或`domainName``\NETWORK SERVICE`，不過您可以自訂這個。
- **SQL 驗證**-使用者 ID 和密碼值會提供做為認證進行驗證。 使用 SQL 驗證的使用者識別碼和密碼提供連接字串中。

Windows 驗證是慣用透過 SQL 驗證更安全。 使用 Windows 驗證連接字串是免費的使用者名稱和密碼，並在 web 伺服器和資料庫伺服器位於兩個不同的電腦上，如果認證不會傳送透過網路以純文字。 使用 SQL 驗證，不過，驗證認證是硬式編碼的連接字串中，從 web 伺服器傳輸至資料庫伺服器，以純文字。

這些教學課程都使用 Windows 驗證。 您可以分辨哪一種驗證模式所檢查的連接字串。 中的連接字串`Web.config`針對我們的教學課程已：

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Integrated Security = True 且缺乏使用者名稱和密碼會指出，正在使用 Windows 驗證。 在某些連接字串 受信任連線一詞 = Yes 或整合式的安全性 = 而非整合式安全性使用 SSPI = True，但這三個指示使用 Windows 驗證。

下列範例會示範使用 SQL 驗證的連接字串。 請注意連接字串中內嵌的認證：

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

假設有攻擊者可檢視您的應用程式 s`Web.config`檔案。 如果您使用 SQL 驗證來連接到可透過網際網路存取的資料庫時，攻擊者可以使用此連接字串來連接到您的資料庫，透過 SQL Management Studio 或從自己的網站上的 ASP.NET 網頁。 若要降低此威脅，來加密中的連接字串資訊`Web.config`使用受保護的組態系統。

> [!NOTE]
> 如需有關不同 SQL Server 中可用的驗證類型的詳細資訊，請參閱[建置安全的 ASP.NET 應用程式： 驗證、 授權和安全通訊](https://msdn.microsoft.com/library/aa302392.aspx)。 如進一步連接字串範例，說明 Windows 和 SQL 驗證 」 語法之間的差異，請參閱[ConnectionStrings.com](http://www.connectionstrings.com/)。


## <a name="summary"></a>總結

根據預設，檔案與`.config`無法透過瀏覽器存取 ASP.NET 應用程式中的延伸模組。 因為它們可能包含機密資訊，例如資料庫連接字串、 使用者名稱和密碼，然後依此類推，不會傳回這些類型的檔案。 .NET 2.0 中的受保護的組態系統可協助您進一步藉由使用指定的組態區段，以加密保護機密資訊。 有兩個內建的受保護的組態提供者： 使用 RSA 演算法，另一個使用 Windows 資料保護 API (DPAPI) 的其中一個。

在本教學課程中，我們討論過如何加密和解密使用 DPAPI 的提供者的組態設定。 這可以完成以程式設計的方式，如我們在步驟 2 中所見，以及透過`aspnet_regiis.exe`涵蓋在步驟 3 中的命令列工具。 如需有關使用使用者層級索引鍵，或改為使用 RSA 提供者的詳細資訊，請參閱 < 進一步閱讀 > 一節中的資源。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [建置安全的 ASP.NET 應用程式： 驗證、 授權和安全通訊](https://msdn.microsoft.com/library/aa302392.aspx)
- [ASP.NET 2.0 中的組態資訊加密應用程式](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [加密`Web.config`ASP.NET 2.0 中的值](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [如何： Encrypt Configuration Sections in ASP.NET 2.0 使用 DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [如何： Encrypt Configuration Sections in ASP.NET 2.0 使用 RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [.NET 2.0 中的組態 API](http://www.odetocode.com/Articles/418.aspx)
- [Windows 資料保護](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa Murphy 和 Randy Schmidt。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
> [下一頁](debugging-stored-procedures-cs.md)
