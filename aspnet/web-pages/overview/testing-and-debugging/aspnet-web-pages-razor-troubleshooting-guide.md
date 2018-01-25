---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: "ASP.NET Web Pages (Razor) 疑難排解指南 |Microsoft 文件"
author: tfitzmac
description: "本文說明使用 ASP.NET Web Pages (Razor) 和一些建議的解決方案時可能發生的問題。 軟體版本的 ASP.NET Web Pag..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: 72c49ed8b76b5fb5eb15bf01f57980b382607496
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web Pages (Razor) 疑難排解指南
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明使用 ASP.NET Web Pages (Razor) 和一些建議的解決方案時可能發生的問題。
> 
> ## <a name="software-versions"></a>軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2 和 ASP.NET Web Pages 1.0。


此主題包括下列章節：

- [執行網頁的問題](#Issues_Running_.cshtml_Pages)
- [Razor 程式碼問題](#IssuesWithRazorCode)
- [安全性與成員資格的問題](#membership)
- [傳送電子郵件問題](#email)
- [其他資源](#AdditionalResources)

一般問題，請參閱[ASP.NET Web Pages (Razor) 常見問題集](https://go.microsoft.com/fwlink/?LinkId=253000)。

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>執行網頁的問題

各種問題可以防止*.cshtml*和*.vbhtml*頁面無法正常執行。 本節列出常見的錯誤訊息，並有可能的原因。

### <a name="http-error-403---forbidden-access-is-denied"></a>HTTP 錯誤 403-禁止： 拒絕存取

*您沒有檢視此目錄或使用您提供的認證，網頁的權限。*

如果伺服器未執行的.NET framework 正確版本，就會發生此錯誤。 請確定正在執行伺服器 （本機或遠端） 的電腦至少已安裝.NET Framework 4。 也請確定應用程式本身設定為執行正確的版本。

如果您在本機 WebMatrix 中工作時看到此問題，請按一下**網站**工作區中，然後按一下樹狀檢視中的，按一下**設定**。 在**選取.NET Framework 版本**清單中，選取**.NET 4 （整合式）**。 如果此版本已設定，請以系統管理員身分執行 WebMatrix。

請確定您網站的根目錄具有至少一個*.cshtml*檔案。

如果遠端伺服器上的 web 伺服器時，您會看到此錯誤，請連絡伺服器系統管理員。 請確定伺服器具有.NET Framework 4 或更新的版本。 也請確定應用程式正在執行中應用程式集區設定為使用該版本的.net Framework。

如果您有伺服器的控制權，請確定它正在執行的.NET framework 正確版本。 您也可以嘗試執行修復安裝`aspnet_regiis -iru`命令。 （例如，如果您安裝.NET Framework 之後，您會安裝 IIS，IIS 會不正確設定來執行 ASP.NET 網頁。）如需詳細資訊，請參閱[ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx)。

### <a name="http-error-40314---forbidden"></a>HTTP 錯誤 403.14-禁止

*網頁伺服器設定為不列出此目錄的內容。*

如果要求保護的資源，可能會發生這個錯誤 (像是*Web.config*檔案) 或受保護的資料夾中 (類似*應用程式\_資料*或*應用程式\_程式碼*)。

### <a name="http-error-40417---not-found"></a>HTTP 錯誤 404.17-找不到

*要求的內容似乎是指令碼，並不會處理由靜態檔案處理常式。*

如果伺服器未正確使用.NET Framework 4 或更新版本中，因此無法辨識中的程式碼，可能會發生這個錯誤`@{ }`區塊。 請參閱稍早的描述*HTTP 錯誤 403-禁止： 拒絕存取*。

### <a name="http-error-4047---not-found"></a>HTTP 錯誤 404.7-找不到

*要求篩選模組設定為拒絕副檔名*

如果，可能會發生這個錯誤*.cshtml*或*.vbhtml*擴充功能已被明確封鎖在伺服器上。 此問題的徵兆時，該 Url 的工作並不包含延伸模組，但包含的 Url *.cshtml*或*.vbhtml*無法運作。 可能的解決方案是要重新啟用的擴充功能在網站的*Web.config*檔案。 下列範例示範如何啟用*.cshtml*延伸模組。

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>HTTP 錯誤 404.8-找不到

*要求篩選模組設定為拒絕包含 hiddenSegment 區段之 URL 中的路徑。*

如果要求保護的資源，可能會發生這個錯誤 (像是*Web.config*檔案) 或受保護的資料夾中 (類似*應用程式\_資料*或*應用程式\_程式碼*)。

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>這種類型的頁面不被服務 （伺服器錯誤 '/' 應用程式中）

請參閱 HTTP 錯誤 404.17 稍早的描述。

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Razor 程式碼問題

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>名稱 '*類別*' 不存在於目前的內容

通常，您會看到此錯誤的原因在於`class`參考協助程式，但協助專家未安裝。 例如，如果您嘗試使用協助程式，但如果您尚未安裝的 NuGet 封裝，您會看到這個錯誤。 使用在 WebMatrix 圖庫來尋找和安裝的協助程式。

如果協助專家已安裝，但頁面仍然無法辨識它，再試一次新增新增`using`陳述式的程式碼。 在`using`陳述式、 參考命名空間包含 helper。 例如，ASP.NET Web Helpers 封裝中的基本 helper 位於`System.Web.Helpers`命名空間。 在您要使用此 helper 頁面的頂端，加入這一行：

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>安全性與成員資格的問題

如果您使用內建的安全性 （成員資格） 系統中的 ASP.NET Web Pages (Razor)，您可能會遇到下列問題。

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>若要呼叫此方法時，"Membership.Provider"屬性必須是"ExtendedMembershipProvider"的執行個體

這個錯誤可能表示沒有`AspNetSqlMembershipProvider`類別設定。 （徵兆是站台在本機上可以正常運作，但當您發行到裝載提供者的伺服器時，會擲回這個錯誤）。一個修正這個問題是，明確地啟用簡單成員資格新增至站台的以下*Web.config*檔案：

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>傳送電子郵件問題

傳送電子郵件問題可能項挑戰，若要偵錯。 初始的問題可能是您無法連接到 SMTP 伺服器。 如果連接成功時，ASP.NET 將訊息交給關閉 SMTP 伺服器。 不過，可以是訊息本身，避免 SMTP 伺服器傳送的問題。

如果您的應用程式並不會成功地傳送電子郵件，請嘗試下列各項：

- SMTP 伺服器名稱通常會像下面`smtp.provider.com`或`smtp.provider.net`。 不過，如果您是主機提供者發行您的網站，SMTP 伺服器名稱此時可能`localhost`。 因為已發佈和提供者的伺服器上執行您的站台之後，可能從您的應用程式的觀點來看本機 SMTP 伺服器，就會發生這種情況。 伺服器名稱中的這項變更可能表示您已變更 SMTP 伺服器名稱做為您發佈程序的一部分。
- 連接埠號碼通常是 25。 不過，某些提供者需要您使用的連接埠 587 或某些其他連接埠。 檢查 SMTP 伺服器的擁有者哪個連接埠號碼他們期望應用程式使用。
- 請確定您使用正確的認證。 如果您已發行您的站台至主控提供者，使用提供者特別指出對於電子郵件的認證。 這些認證可能與您用來發行的認證不同。
- 有時候您完全不需要認證。 如果您要使用您個人的 ISP 傳送電子郵件，您的電子郵件提供者可能已經知道您的認證。 發行之後，您可能需要使用比您本機電腦上進行測試時有不同的認證。
- 如果您的電子郵件提供者會使用加密，設定`WebMail.EnableSsl`至`true`。

如果沒有傳送電子郵件時發生錯誤，您可能會看到標準 ASP.NET 錯誤訊息，看起來像這樣：

![電子郵件問題時，ASP.NET 錯誤訊息](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

您也可以偵錯使用傳送電子郵件問題`try-catch`區塊，如下列範例所示。 當您使用`try-catch`區塊中，ASP.NET 不會顯示其標準錯誤訊息。 相反地，您可以在此擷取中的錯誤`catch`區塊的部份。

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

取代為適當值`your-SMTP-server-name`，依此類推。 您可能會看到這種方式的錯誤訊息如下：

- *傳送郵件失敗。*

    -或-

    *連接嘗試失敗，因為連線對象並未正確回應之後一段時間，或已建立的連線失敗，因為已連線的主機無法回應*

    此錯誤通常表示應用程式無法連線到 SMTP 伺服器。 請檢查伺服器名稱和連接埠號碼。
- *信箱無法使用。伺服器回應為： 5.1.0 &lt; someuser@invaliddomain &gt;拒絕寄件者： 無效的寄件者網域*

    此訊息可能表示`From`位址不正確或遺失。
- *指定的字串不是所需的電子郵件地址的格式。*

    這個錯誤可能表示的值`To`或`From`屬性不會被視為電子郵件地址。 (ASP.NET 無法檢查電子郵件地址是否有效，只有它的正確格式，例如 *name@domain.com* 。)

> [!NOTE]
> 移除的標記，就會顯示錯誤 (`@errorMessage`) 頁面發佈到即時網站之前。 它不是個不錯的主意，讓使用者看到您從伺服器取得的錯誤訊息。


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>其他資源

[ASP.NET Web Pages (Razor) 常見問題集](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix 和 ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET 網站上的論壇
