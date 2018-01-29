---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: "保護連接字串和其他組態資訊 (VB) |Microsoft 文件"
author: rick-anderson
description: "ASP.NET 應用程式通常會將組態資訊儲存在 Web.config 檔案中。 部分資訊是機密，保證保護。 依命名為 def。..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: f1514c4b6d041f6bbd83788e2110a95d3d831ff6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-vb"></a>保護連接字串和其他組態資訊 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip)或[下載 PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> ASP.NET 應用程式通常會將組態資訊儲存在 Web.config 檔案中。 部分資訊是機密，保證保護。 依預設這個檔案將不會處理網站訪客，但系統管理員或駭客可能會存取 Web 伺服器之檔案系統，並檢視檔案的內容。 在此教學課程中我們了解 ASP.NET 2.0 可讓我們來保護敏感資訊的加密 Web.config 檔案的區段。


## <a name="introduction"></a>簡介

ASP.NET 應用程式的組態資訊通常儲存在名為 XML 檔案`Web.config`。 透過這些教學課程的課程，我們已更新`Web.config`少數的次數。 建立時`Northwind`中具類型資料集[第一個教學課程](../introduction/creating-a-data-access-layer-vb.md)，連接字串資訊，例如自動加入至`Web.config`中`<connectionStrings>`> 一節。 稍後在[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-vb.md)教學課程中，我們以手動方式更新`Web.config`、 新增`<pages>`項目，指出應該使用我們的受測專案的 ASP.NET 網頁的所有`DataWebControls`佈景主題。

因為`Web.config`可能包含機密資料，例如連接字串，很重要的內容`Web.config`保持安全和隱藏從未經授權的檢視器。 根據預設，任何 HTTP 要求與檔案`.config`延伸模組由 ASP.NET 引擎，它會傳回*不提供這種類型的頁面*圖 1 所示的訊息。 這表示訪客無法檢視您`Web.config`檔案 s 內容，只要其 s 瀏覽器網址列輸入 http://www.YourServer.com/Web.config。


[![瀏覽 Web.config 透過瀏覽器傳回這類頁面不服務訊息](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**圖 1**： 瀏覽`Web.config`透過瀏覽器傳回這類頁面不服務訊息 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))


但如果攻擊者找不到某些其他攻擊，可檢視您`Web.config`檔案 s 內容嗎？ 功能無法攻擊者利用此資訊，以及可以採取哪些步驟來進一步保護內的機密資訊`Web.config`嗎？ 幸運的是，大部分的區段中`Web.config`不包含機密資訊。 哪些損害可以攻擊 perpetrate 知道您的 ASP.NET 網頁所使用的佈景主題的預設名稱？

某些`Web.config`章節中，不過，包含機密資訊，可能包含連接字串、 使用者名稱、 密碼、 伺服器名稱、 加密金鑰，以及其他等等。 這項資訊通常位於下列`Web.config`各節：

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

在此教學課程中我們將探討技術可保護這類的機密組態資訊。 如我們所見，.NET Framework 2.0 版包含受保護的組態系統，可幫助您輕鬆以程式設計的方式加密和解密選取的組態區段。

> [!NOTE]
> 本教學課程結束時，會查看 Microsoft 的建議從 ASP.NET 應用程式連接到資料庫。 除了加密連接字串，您可以協助您強化您連接到資料庫安全的方式，方法是確保系統。


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>步驟 1： 瀏覽 ASP.NET 2.0 s 受保護的組態選項

ASP.NET 2.0 包含受保護的組態系統，來加密和解密組態資訊。 這可用來以程式設計的方式加密或解密組態資訊的.NET Framework 中包含的方法。 受保護的組態系統會使用[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，可讓開發人員選擇使用何種密碼編譯實作。

.NET Framework 隨附兩個受保護的組態提供者：

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx)-使用非對稱[RSA 演算法](http://en.wikipedia.org/wiki/Rsa)加密和解密。
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx)-使用 Windows[資料保護 API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx)加密和解密。

因為受保護的組態系統會實作提供者設計模式，所以可以建立自己的受保護的組態提供者，並插入您的應用程式。 請參閱[實作受保護的組態提供者](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx)如需這個程序的詳細資訊。

RSA 和 DPAPI 的提供者會將金鑰用於其加密和解密的常式，以及這些金鑰可以儲存在電腦或使用者層級。 電腦層級索引鍵很適合做為它自己專用的伺服器的 web 應用程式執行所在的案例，或是否需要共用的伺服器上的多個應用程式加密的資訊。 使用者層級索引鍵是較安全的選項，在共用裝載環境中，在相同伺服器上的其他應用程式應該不可以解密您的應用程式保護的 s 組態區段。

在此教學課程中我們的範例會使用 DPAPI 提供者和電腦層級索引鍵。 具體來說，我們將探討加密`<connectionStrings>`一節中`Web.config`，不過，受保護的組態系統要用於加密任何大部分`Web.config`> 一節。 如需使用使用者層級索引鍵，或使用 RSA 提供者資訊，請參閱本教學課程結尾處的進一步讀取區段中的資源。

> [!NOTE]
> `RSAProtectedConfigurationProvider`和`DPAPIProtectedConfigurationProvider`中註冊的提供者`machine.config`與提供者名稱的檔案`RsaProtectedConfigurationProvider`和`DataProtectionConfigurationProvider`分別。 加密或解密我們必須提供適當的提供者名稱的組態資訊時 (`RsaProtectedConfigurationProvider`或`DataProtectionConfigurationProvider`) 而非實際的類型名稱 (`RSAProtectedConfigurationProvider`和`DPAPIProtectedConfigurationProvider`)。 您可以找到`machine.config`檔案`$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`資料夾。


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>步驟 2： 以程式設計的方式加密和解密組態區段

只要幾行程式碼中，我們可以加密或解密使用指定的提供者特定的組態區段。 程式碼中，我們將會看到在短時間內，只需要以程式設計方式參考適當的組態區段中，呼叫其`ProtectSection`或`UnprotectSection`方法，然後再呼叫`Save`方法，將所做的變更。 此外，.NET Framework 包含有用的命令列公用程式可加密和解密組態資訊。 我們將探討這個步驟 3 中的命令列公用程式。

為了說明以程式設計方式保護的組態資訊，讓 s 建立 ASP.NET 網頁，其中包含用於加密和解密的按鈕`<connectionStrings>`一節中`Web.config`。

先開啟`EncryptingConfigSections.aspx`頁面`AdvancedDAL`資料夾。 拖曳 TextBox 控制項從工具箱拖曳至設計工具中，設定其`ID`屬性`WebConfigContents`、 其`TextMode`屬性`MultiLine`，且其`Width`和`Rows`屬性至 95%與 15，分別。 此文字方塊控制項將顯示的內容`Web.config`讓我們快速查看 是否會加密內容。 當然，在實際的應用程式會永遠不會想要顯示的內容`Web.config`。

在文字方塊中，加入名為兩個按鈕控制項`EncryptConnStrings`和`DecryptConnStrings`。 設定其內容來加密的連接字串和解密連接字串。

此時您的畫面看起來應該類似於圖 2。


[![將文字方塊和兩個按鈕 Web 控制項加入頁面](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**圖 2**： 加入網頁中的文字方塊和兩個按鈕 Web 控制項 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))


接下來，我們必須撰寫程式碼，載入並顯示的內容`Web.config`中`WebConfigContents`文字方塊中，第一個頁面時載入。 將下列程式碼加入至頁面 s 程式碼後置類別。 這個程式碼加入方法，名為`DisplayWebConfig`呼叫它從`Page_Load`事件處理常式時`Page.IsPostBack`是`False`:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

`DisplayWebConfig`方法會使用[`File`類別](https://msdn.microsoft.com/library/system.io.file.aspx)開啟應用程式 s`Web.config`檔案， [ `StreamReader`類別](https://msdn.microsoft.com/library/system.io.streamreader.aspx)其內容讀入字串，而[`Path`類別](https://msdn.microsoft.com/library/system.io.path.aspx)產生的實體路徑`Web.config`檔案。 這三個類別都位在[`System.IO`命名空間](https://msdn.microsoft.com/library/system.io.aspx)。 因此，您必須新增`Imports``System.IO`頂端的程式碼後置類別或，或者，這些類別具有名稱的前置詞的陳述式`System.IO.`

接下來，我們必須加入兩個按鈕控制項的事件處理常式`Click`事件並新增必要的程式碼，來加密和解密`<connectionStrings>`區段電腦層級金鑰使用 DPAPI 提供者。 從設計工具中，按兩下每個要加入按鈕`Click`中程式碼後置的事件處理常式類別，然後再將下列程式碼：


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

在兩個事件處理常式中使用的程式碼是幾乎完全相同。 藉由取得關於目前應用程式的資訊都啟動`Web.config`檔案透過[`WebConfigurationManager`類別](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx)s [ `OpenWebConfiguration`方法](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx)。 這個方法會傳回指定的虛擬路徑的 web 組態檔。 下一步`Web.config`檔案 s`<connectionStrings>`區段存取透過[`Configuration`類別](https://msdn.microsoft.com/library/system.configuration.configuration.aspx)s [ `GetSection(sectionName)`方法](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx)，它會傳回[ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx)物件。

`ConfigurationSection`物件包含[`SectionInformation`屬性](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx)，提供其他資訊與相關的組態區段的功能。 為上方顯示的程式碼，我們可以判斷藉由檢查是否已加密的組態區段`SectionInformation`屬性的`IsProtected`屬性。 此外，區段可以加密或解密透過`SectionInformation`屬性 s`ProtectSection(provider)`和`UnprotectSection`方法。

`ProtectSection(provider)`方法接受做為輸入字串，指定要加密時所使用的受保護的組態提供者的名稱。 在`EncryptConnString`s 按鈕事件處理常式，我們會傳送到 DataProtectionConfigurationProvider`ProtectSection(provider)`方法，以便使用 DPAPI 提供者。 `UnprotectSection`方法可以判斷之前用來加密組態區段，因此不需要任何輸入參數的提供者。

在呼叫`ProtectSection(provider)`或`UnprotectSection`方法，您必須呼叫`Configuration`物件 s [ `Save`方法](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx)保存變更。 一旦組態資訊已加密或解密並儲存變更，我們稱之為`DisplayWebConfig`載入更新的`Web.config`到文字方塊控制項的內容。

當您有上述的程式碼中輸入測試造訪`EncryptingConfigSections.aspx`透過瀏覽器的頁面。 您應該一開始會看到列出的內容頁面`Web.config`與`<connectionStrings>`區段顯示純文字 （請參閱圖 3）。


[![將文字方塊和兩個按鈕 Web 控制項加入頁面](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**圖 3**： 加入網頁中的文字方塊和兩個按鈕 Web 控制項 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))


現在按一下 [加密連接字串] 按鈕。 如果已啟用要求驗證，從回傳標記`WebConfigContents`文字方塊中就會產生`HttpRequestValidationException`，這會顯示訊息，有潛在危險`Request.Form`從用戶端偵測到值。 要求驗證，根據預設，ASP.NET 2.0 中啟用時，會禁止回傳，包括未編碼的 HTML，設計來協助防止指令碼資料隱碼攻擊。 這項檢查可以停用在頁面或應用程式層級。 若要將它關閉此頁面，設定`ValidateRequest`設`False`中`@Page`指示詞。 `@Page`頁面 s 宣告式標記頂端找到指示詞。


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

如需有關要求驗證，其用途，在網頁和應用程式層級，以及如何為 HTML 編碼標記為停用，請參閱[要求驗證-防止指令碼攻擊](../../../../whitepapers/request-validation.md)。

停用頁面的要求驗證之後, 再試一次按一下 [加密連接字串] 按鈕。 在回傳時，將存取設定檔及其`<connectionStrings>`加密使用 DPAPI 提供者 > 一節。 文字方塊並更新為顯示新`Web.config`內容。 如圖 4 所示，`<connectionStrings>`現在會加密資訊。


[![按一下 加密連接字串按鈕加密&lt;connectionString&gt;區段](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**圖 4**： 按一下 加密連接字串按鈕加密`<connectionString>`> 一節 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))


加密`<connectionStrings>`我的電腦上產生的區段之後，雖然部分中的內容`<CipherData>`為畫面簡潔起見已移除項目：


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>`項目會指定用來執行加密的提供者 (`DataProtectionConfigurationProvider`)。 這項資訊由`UnprotectSection`解密連接字串按鈕的方法。


從存取的連接字串資訊時`Web.config`-可能是我們要撰寫程式碼，從 SqlDataSource 控制項，或在我們的輸入資料集中 Tableadapter 的自動產生程式碼-就會自動解密。 簡單地說，我們不需要新增任何額外的程式碼或邏輯來解密加密`<connectionString>`> 一節。 若要示範這點，請造訪較早的教學課程之一在這個階段，例如簡單顯示本教學課程中的基本報表區段從 (`~/BasicReporting/SimpleDisplay.aspx`)。 如圖 5 所示，本教學課程的運作方式完全如我們所預期，表示加密的連接字串資訊會被自動解密由 ASP.NET 網頁。


[![資料存取層會自動解密連接字串資訊](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**圖 5**： 資料存取層會自動解密連接字串資訊 ([按一下以檢視完整大小的影像](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))


若要還原`<connectionStrings>`回到其純文字表示區段中，按一下 [解密連接字串] 按鈕。 在回傳時您應該會看到中的連接字串`Web.config`以純文字。 此時您的畫面看起來應該如同第一次造訪此頁 （請參閱圖 3） 時。

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>步驟 3： 加密使用的組態區段`aspnet_regiis.exe`

.NET Framework 包含各種不同的命令列工具中`$WINDOWS$\Microsoft.NET\Framework\version\`資料夾。 在[使用 SQL 快取相依性](../caching-data/using-sql-cache-dependencies-vb.md)教學課程中，比方說，我們還在使用`aspnet_regsql.exe`命令列工具來加入所需的 SQL 快取相依性的基礎結構。 在這個資料夾中的另一個有用的命令列工具是[ASP.NET IIS 註冊工具 (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx)。 正如其名，ASP.NET IIS 註冊工具主要用來向 Microsoft s professional 等級網頁伺服器，IIS 註冊 ASP.NET 2.0 應用程式。 除了及其與 IIS 相關功能時，ASP.NET IIS 註冊工具也可用來加密或解密指定的組態區段中`Web.config`。

下列陳述式會顯示用來加密組態區段的一般語法`aspnet_regiis.exe`命令列工具：


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*區段*是組態區段 （例如 connectionStrings) 加密*實體\_目錄*是 web 應用程式的根目錄的完整的實體路徑和*提供者*使用 （例如 DataProtectionConfigurationProvider) 的受保護的組態提供者的名稱。 或者，如果在 IIS 中註冊 web 應用程式，您可以輸入而不是使用下列語法的實體路徑的虛擬路徑：


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

下列`aspnet_regiis.exe`範例會加密`<connectionStrings>`區段具有電腦層級索引鍵使用 DPAPI 提供者：


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

同樣地，`aspnet_regiis.exe`命令列工具可以用來解密組態區段。 而不是使用`-pef`切換，請使用`-pdf`(或 instead of `-pe`，使用`-pd`)。 另外請注意解密時不需要提供者名稱。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> 因為我們使用 DPAPI 提供者，會使用電腦特定的金鑰，您必須執行`aspnet_regiis.exe`來自相同的機器會被提供網頁。 比方說，如果您執行此命令列程式從您的本機開發電腦，然後將加密的 Web.config 檔案上傳到生產伺服器，在實際執行伺服器將無法解密連接字串資訊，因為它已加密使用您的開發電腦的特定索引鍵。 RSA 提供者並沒有這項限制，因為它是可能將 RSA 金鑰匯出到另一部電腦。


## <a name="understanding-database-authentication-options"></a>了解資料庫驗證選項

任何應用程式可以發出之前`SELECT`， `INSERT`， `UPDATE`，或`DELETE`查詢到 Microsoft SQL Server 資料庫，資料庫必須先識別要求者。 這個程序稱為*驗證*和 SQL Server 提供兩種驗證方法：

- **Windows 驗證**-要執行的應用程式的程序用來與資料庫通訊。 當執行 ASP.NET 應用程式透過 Visual Studio 2005 的 ASP.NET 程式開發伺服器時，ASP.NET 應用程式會假設目前登入使用者的身分識別。 ASP.NET 應用程式在 Microsoft Internet Information Server (IIS)，ASP.NET 應用程式通常取得的身份識別`domainName``\MachineName`或`domainName``\NETWORK SERVICE`，不過您可以自訂這個。
- **SQL 驗證**-提供做為認證進行驗證的使用者識別碼和密碼值。 使用 SQL 驗證的使用者識別碼和密碼提供連接字串中。

Windows 驗證是慣用透過 SQL 驗證，所以更安全。 使用 Windows 驗證連接字串是可用的使用者名稱和密碼，如果 web 伺服器和資料庫伺服器位於兩個不同的電腦，認證不會傳送透過網路以純文字。 使用 SQL 驗證，但是，驗證認證的硬式編碼的連接字串中而且從 web 伺服器傳輸到以純文字的資料庫伺服器。

這些教學課程都使用 Windows 驗證。 您可以判斷何種驗證模式下使用藉由檢查連接字串。 中的連接字串`Web.config`已經過的教學課程：

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

整合式安全性 = True 且缺乏使用者名稱和密碼會指出是否使用 Windows 驗證。 在某些連接字串詞彙信任連接 = 是 或 整合式的安全性 = 而非整合式安全性使用 SSPI = True，但這三個表示使用 Windows 驗證。

下列範例會使用 SQL 驗證的連接字串。 請注意連接字串中嵌入認證：

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

假設有攻擊者就可以檢視您的應用程式 s`Web.config`檔案。 如果您使用 SQL 驗證來連接到資料庫，可透過網際網路存取，攻擊者可以使用此連接字串連接到您的資料庫，透過 SQL Management Studio，或從自己的網站上的 ASP.NET 網頁。 若要降低此威脅，加密連接字串資訊中的`Web.config`使用受保護的組態系統。

> [!NOTE]
> 如需有關不同 SQL Server 中可用的驗證類型的詳細資訊，請參閱[建構安全 ASP.NET 應用程式： 驗證、 授權和安全通訊](https://msdn.microsoft.com/library/aa302392.aspx)。 如進一步連接字串範例，說明 Windows 和 SQL 驗證語法之間的差異，請參閱[ConnectionStrings.com](http://www.connectionstrings.com/)。


## <a name="summary"></a>總結

根據預設，檔案與`.config`ASP.NET 應用程式中的擴充功能無法透過瀏覽器來存取。 因為它們可能包含機密資訊，例如資料庫連接字串、 使用者名稱和密碼等，不會傳回這些類型的檔案。 .NET 2.0 中的受保護的組態系統可協助您進一步藉由使用指定的組態區段，以加密保護機密資訊。 有兩個內建的受保護的組態提供者： 使用 RSA 演算法，另一個則會使用 Windows 資料保護 API (DPAPI) 的其中一個。

在此教學課程中我們討論了如何加密和解密使用 DPAPI 提供者的組態設定。 這可以完成這兩種以程式設計的方式，正如我們所見在步驟 2 中，以及透過`aspnet_regiis.exe`命令列工具，所涵蓋的步驟 3。 如需有關使用使用者層級索引鍵，或改為使用 RSA 提供者的詳細資訊，請參閱進一步閱讀 > 一節中的資源。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [建立安全的 ASP.NET 應用程式： 驗證、 授權和安全通訊](https://msdn.microsoft.com/library/aa302392.aspx)
- [加密組態資訊，在 ASP.NET 2.0 應用程式](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [加密`Web.config`ASP.NET 2.0 中的值](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [如何： 加密組態區段，在 ASP.NET 2.0 使用 DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [如何： 加密組態區段，在 ASP.NET 2.0 使用 RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [.NET 2.0 中的組態 API](http://www.odetocode.com/Articles/418.aspx)
- [Windows 資料保護](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已本文菲和袁 Schmidt。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
[下一頁](debugging-stored-procedures-vb.md)
