---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: 啟用 ASP.NET Web API 2 中的跨原始來源要求 |Microsoft Docs
author: MikeWasson
description: 示範如何在 ASP.NET Web API 中支援跨原始資源共用 (CORS)。
ms.author: riande
ms.date: 10/10/2018
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 118b779c89edb874f7f928315d1094738be5f097
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348516"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>啟用 ASP.NET Web API 2 中的跨原始來源要求
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> 瀏覽器安全性可防止網頁對另一個網域提出 AJAX 要求。 這項限制稱為*同源原則*，可防止惡意網站從另一個網站讀取敏感性資料。 不過，有時候您可能想要讓呼叫您的 web API 的其他站台。
>
> [跨原始資源共用](http://www.w3.org/TR/cors/)(CORS) 是 W3C 標準，可讓伺服器放寬同源原則。 使用 CORS，伺服器可以明確允許某些跨源要求並拒絕其他。 CORS 可較為安全且更有彈性，比早期技術這類[JSONP](http://en.wikipedia.org/wiki/JSONP)。 本教學課程會示範如何在您的 Web API 應用程式中啟用 CORS。
>
> ## <a name="software-used-in-the-tutorial"></a>在本教學課程中使用的軟體
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web API 2.2

## <a name="introduction"></a>簡介

本教學課程會示範在 ASP.NET Web API 的 CORS 支援。 我們先建立兩個 ASP.NET 專案 – 一個稱為 「 web 服務 」，裝載的 Web API 控制器和其他呼叫 「 WebClient 」，它會呼叫 web 服務。 因為兩個應用程式裝載在不同網域時，web 服務的 AJAX 要求從 WebClient 會是跨原始來源要求。

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>什麼是 「 相同原始 」？

如果它們有相同的配置、 主機和連接埠，兩個 Url 會有相同的原點。 ([RFC 6454](http://tools.ietf.org/html/rfc6454))

這些兩個 Url 有相同的來源：

- `http://example.com/foo.html`
- `http://example.com/bar.html`

這些 Url 會有兩個不同的來源，比上一個：

- `http://example.net` -不同的網域
- `http://example.com:9000/foo.html` -不同的連接埠
- `https://example.com/foo.html` -不同的配置
- `http://www.example.com/foo.html` -不同的子網域

> [!NOTE]
> Internet Explorer 在比較來源時，不會考慮該連接埠。

## <a name="create-the-webservice-project"></a>建立 web 服務專案

> [!NOTE]
> 本節假設您已經知道如何建立 Web API 專案。 如果沒有，請參閱[Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。

1. 啟動 Visual Studio 並建立新**ASP.NET Web 應用程式 (.NET Framework)** 專案。
2. 在 **新的 ASP.NET Web 應用程式**對話方塊中，選取**空白**專案範本。 底下**新增的資料夾和核心參考**，選取**Web API**核取方塊。

   ![Visual Studio 中的新 ASP.NET 專案對話方塊](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. 新增名為 Web API 控制器`TestController`為下列程式碼：

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. 您可以在本機執行應用程式，或部署至 Azure。 （在本教學課程的螢幕擷取畫面，應用程式會部署至 Azure App Service Web Apps。）若要確認 web API 會正常運作，巡覽至`http://hostname/api/test/`，其中*hostname*是您用來部署應用程式的網域。 回應文字，您應該會看到&quot;取得： 測試訊息&quot;。

   ![Web 瀏覽器顯示測試訊息](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>建立 WebClient 專案

1. 建立另一個**ASP.NET Web 應用程式 (.NET Framework)** 專案，然後選取**MVC**專案範本。 （選擇性） 選取**變更驗證** > **不需要驗證**。 本教學課程中，您不需要驗證。

   ![在 Visual Studio 中的 [新增 ASP.NET 專案] 對話方塊中的 MVC 範本](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. 在 **方案總管**，開啟的檔案*Views/Home/Index.cshtml*。 您可以將此檔案中的程式碼取代為下列：

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   針對*serviceUrl*變數中，使用 web 服務應用程式的 URI。

3. 在本機執行 WebClient 應用程式，或將它發佈至另一個網站。

當您按一下 試試看 」 按鈕時，AJAX 要求提交至 web 服務應用程式使用中列出的 HTTP 方法 （GET、 POST 或 PUT） 的下拉式清單方塊。 這可讓您檢查不同的跨原始來源要求。 目前，web 服務應用程式不支援 CORS，因此如果您按一下按鈕，您就會收到錯誤。

![在瀏覽器中的 'Try' 時發生錯誤](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> 如果您觀察在工具中的 HTTP 流量要[Fiddler](http://www.telerik.com/fiddler)，您會看到瀏覽器會傳送 GET 要求，並要求成功，但 AJAX 呼叫會傳回錯誤。 請務必了解同源原則不會防止從瀏覽器*傳送*要求。 相反地，它會防止應用程式看到*回應*。

![Fiddler web 偵錯工具顯示 web 要求](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>啟用 CORS

現在讓我們的 web 服務應用程式中啟用 CORS。 首先，新增 CORS NuGet 套件。 在 Visual Studio 中，從**工具**功能表上，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

此命令會安裝最新的封裝，並更新所有相依性，包括 core Web API 程式庫。 使用`-Version`特定版本為目標的旗標。 CORS 封裝則需要 Web API 2.0 或更新版本。

開啟檔案*應用程式\_Start/WebApiConfig.cs*。 將下列程式碼加入**WebApiConfig.Register**方法：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

接下來，新增 **[EnableCors]** 屬性設定為`TestController`類別：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

針對*origins*參數，使用 WebClient 應用程式的部署位置的 URI。 這讓跨源要求 WebClient，同時仍然不允許所有其他的跨網域要求。 稍後，我將說明的參數 **[EnableCors]** 中更多詳細資料。

不包含結尾斜線*origins* URL。

重新部署更新的 web 服務應用程式。 您不需要更新 WebClient。 現在應該會成功從 WebClient 的 AJAX 要求。 所有允許的 GET、 PUT 和 POST 方法。

![Web 瀏覽器顯示成功的測試訊息](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>CORS 的運作方式

本章節描述 CORS 要求，在 HTTP 訊息的層級中發生的動作。 請務必了解 CORS 的運作方式，以便您可以設定 **[EnableCors]** 正確屬性，並進行疑難排解，如果無法運作如預期般。

CORS 規格引進了數個新的 HTTP 標頭啟用跨源要求。 如果瀏覽器支援 CORS，它會設定自動跨原始來源要求，這些標頭您不需要執行任何 JavaScript 程式碼中的特殊動作。

以下是跨原始要求的範例。 「 來源 」 標頭會提供提出要求的站台的網域。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

如果伺服器允許的要求，它會設定存取控制-允許-原始標頭。 此標頭的值符合 Origin 標頭，或者是萬用字元值"\*"，表示允許任何來源。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

如果回應不包含存取控制-允許-原始標頭，AJAX 要求將會失敗。 具體而言，瀏覽器不允許要求。 即使伺服器會傳回成功的回應，瀏覽器不會回應提供用戶端應用程式。

**預檢要求**

對於某些 CORS 要求，瀏覽器會傳送其他要求，稱為 「 預檢要求，」 後才會傳送實際要求的資源。

如果下列條件成立，則瀏覽器可以略過預檢要求：

- 要求方法是 GET、 HEAD 或 POST*和*
- 應用程式不會設定 Accept、 Accept-language、 內容語言、 以外任何要求標頭內容類型或最後一個事件識別碼、*和*
- Content-type 標頭 (如果設定) 是下列其中之一：

    - application/x-www-form-urlencoded
    - multipart/form-data
    - text/plain

要求標頭的相關規則有套用到應用程式設定所呼叫的標頭**setRequestHeader**上**XMLHttpRequest**物件。 （CORS 規格會呼叫這些 「 作者要求標頭 」）。規則不適用於標頭*瀏覽器*設定，例如使用者代理程式、 主機或內容長度。

預檢要求的範例如下：

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

事前要求使用 HTTP OPTIONS; 方法。 它包含兩個特殊標頭：

- 存取控制-要求的方法： 將實際的要求使用 HTTP 方法。
- 存取控制-要求的標頭： 要求標頭的清單，*應用程式*實際要求上設定。 （同樣地，這不包括瀏覽器設定的標頭。）

以下是範例回應，假設伺服器允許的要求：

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

此回應包含存取控制-允許-方法標頭，其中列出允許的方法，並選擇性地存取控制-允許-標頭的標頭，它會列出允許的標頭。 如果預檢要求成功，瀏覽器會傳送實際要求，如先前所述。

## <a name="scope-rules-for-enablecors"></a>[EnableCors] 的範圍規則

您可以在應用程式中，每個動作，每個控制站，或適用於所有的 Web API 控制器啟用 CORS。

**每個動作**

若要啟用 CORS 的單一動作時，將 **[EnableCors]** 動作方法上的屬性。 下列範例會啟用 CORS`GetItem`僅方法。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**每個控制站**

如果您設定 **[EnableCors]** 在控制器類別，它適用於控制站上的所有動作。 若要停用動作的 CORS，請新增 **[DisableCors]** 屬性的動作。 下列範例會針對每個方法，除了啟用 CORS `PutItem`。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**全域**

若要啟用 CORS 的應用程式中的所有 Web API 控制器時，傳遞**EnableCorsAttribute**執行個體**EnableCors**方法：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

如果您在一個以上的範圍設定的屬性，優先順序就會是：

1. 動作
2. 控制器
3. Global

## <a name="set-the-allowed-origins"></a>設定允許的來源

*Origins*的參數 **[EnableCors]** 屬性會指定哪些原始來源允許存取資源。 值是允許的來源的逗號分隔清單。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

您也可以使用萬用字元值"\*」 以允許來自任何來源的要求。

請仔細考慮，才能允許來自任何來源的要求。 這表示幾乎任何網站可以讓您的 web API 的 AJAX 呼叫。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

*方法*的參數 **[EnableCors]** 屬性會指定哪些 HTTP 方法才能存取資源。 若要允許所有方法，使用萬用字元值"\*」。 下列範例允許只有 GET 和 POST 要求。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

這篇文章所述稍早如何預檢要求可能會包含存取控制-access-control-request-headers 標標頭，列出應用程式設定的 HTTP 標頭 (所謂 「 撰寫要求標頭 」)。 *標頭*的參數 **[EnableCors]** 屬性可讓您指定允許哪些作者要求標頭。 若要允許任何標頭，設定*標頭*到 「\*"。 允許清單特定的標頭，設定*標頭*允許的標頭以逗號分隔清單：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

不過，瀏覽器並不會在它們如何設定存取控制-access-control-request-headers 標完全一致。 例如，Chrome 目前包含 「 來源 」。 FireFox 不包含標準標頭，例如"Accept"，即使應用程式會設定它們的指令碼中。

如果您設定*標頭*到以外的任何項目"\*」，您應該至少包含在 「 accept 」，"content-type"、"origin"，以及您想要支援的任何自訂標頭。

## <a name="set-the-allowed-response-headers"></a>設定允許的回應標頭

根據預設，瀏覽器不會公開所有應用程式的回應標頭。 是預設可用的回應標頭如下：

- Cache-Control
- 內容語言
- Content-Type
- 到期
- 上次修改
- Pragma

CORS 規格會呼叫這些[簡單的回應標頭](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header)。 若要讓其他標頭可供應用程式，設定*exposedHeaders*的參數 **[EnableCors]**。

在下列範例中，控制器的`Get`方法會設定名為 'X 自訂-標頭' 的自訂標頭。 根據預設，瀏覽器不會公開此標頭中的跨原始來源要求。 要使用的標頭，請納入 ' X-自訂-Header *exposedHeaders*。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>在跨原始來源要求中傳遞認證

認證需要在 CORS 要求的特殊處理。 根據預設，瀏覽器不會傳送任何與跨原始要求的認證。 認證包含 cookie，以及 HTTP 驗證配置。 若要傳送與跨原始要求的認證，用戶端必須將**XMLHttpRequest.withCredentials**設為 true。

使用**XMLHttpRequest**直接：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

在 jQuery 中：

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

此外，伺服器必須允許的認證。 若要在 Web API 中，允許跨原始來源的認證，請設定**SupportsCredentials**屬性設為 true，在 **[EnableCors]** 屬性：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

如果這個屬性為 true，則 HTTP 回應將包含存取控制-允許-認證標頭。 此標頭會告知瀏覽器伺服器允許跨原始來源要求認證。

如果瀏覽器傳送認證，但回應不包含有效的存取控制-允許-認證標頭，瀏覽器不會公開應用程式、 回應和 AJAX 要求失敗。

設定請小心**SupportsCredentials**為 true，因為這表示在另一個定義域的網站可以傳送給您的 Web API，代替使用者的登入的使用者的認證不會察覺使用者。 CORS 規格也會指出該設定*origins*要&quot; \* &quot;無效如果**SupportsCredentials**為 true。

## <a name="custom-cors-policy-providers"></a>自訂的 CORS 原則提供者

**[EnableCors]** 屬性會實作**ICorsPolicyProvider**介面。 您可以提供您自己的實作方式是建立衍生自類別**屬性**，並會實作**ICorsProlicyProvider**。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

現在您可以套用的屬性的任何放置，您會放置 **[EnableCors]**。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

例如，自訂的 CORS 原則提供者無法從組態檔讀取設定。

您可以使用屬性的替代方案，以註冊**ICorsPolicyProviderFactory**建立的物件**ICorsPolicyProvider**物件。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

若要設定**ICorsPolicyProviderFactory**，呼叫**SetCorsPolicyProviderFactory**擴充方法，在啟動時，如下所示：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>瀏覽器支援

Web API CORS 套件是一種伺服器端技術。 使用者的瀏覽器也必須支援 CORS。 幸運的是，所有主要瀏覽器的目前版本包括[cors 支援](http://caniuse.com/cors)。
