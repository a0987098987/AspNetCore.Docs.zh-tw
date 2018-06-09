---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: 啟用 ASP.NET Web API 2 中的跨原始要求 |Microsoft 文件
author: MikeWasson
description: 示範如何跨原始資源共用 (CORS) 支援在 ASP.NET Web API。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508377"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>啟用 ASP.NET Web API 2 中的跨原始要求
====================
由[Mike Wasson](https://github.com/MikeWasson)

> 瀏覽器安全性防止網頁 AJAX 要求到另一個網域。 這項限制稱為*相同來源原則*，並可防止惡意網站從另一個站台讀取的機密資料。 不過，有時候您可能想要讓呼叫您的 web API 的其他站台。
> 
> [跨原始資源共用](http://www.w3.org/TR/cors/)」 (CORS) 是一種 W3C 標準，可讓放寬的相同來源原則的伺服器。 使用 CORS，伺服器可以明確地允許某些跨原始要求時拒絕其他人。 CORS 是更安全且更彈性與較早的技術如[JSONP](http://en.wikipedia.org/wiki/JSONP)。 本教學課程會示範如何在 Web API 應用程式中啟用 CORS。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2.2


<a id="intro"></a>
## <a name="introduction"></a>簡介

本教學課程會示範在 ASP.NET Web API 的 CORS 支援。 我們會先建立兩個 ASP.NET 專案-一個稱為 「 web 服務 」，裝載的 Web API 控制器，與其他稱為 「 WebClient"，它會呼叫 web 服務。 因為兩個應用程式裝載在不同網域時，從 WebClient AJAX 要求 web 服務是跨原始要求。

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>什麼是 「 相同來源 」？

如果它們有相同的配置、 主機和連接埠，兩個 Url 會有相同的原始主機。 ([RFC 6454](http://tools.ietf.org/html/rfc6454))

這些兩個 Url 有相同的原始主機：

- `http://example.com/foo.html`
- `http://example.com/bar.html`

這些 Url 會有兩個不同來源比上一個：

- `http://example.net` 為不同的網域
- `http://example.com:9000/foo.html` 為不同的通訊埠
- `https://example.com/foo.html` 為不同的配置
- `http://www.example.com/foo.html` 為不同的子網域

> [!NOTE]
> 比較來源時，Internet Explorer 不會將連接埠。


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>建立 web 服務專案

> [!NOTE]
> 本節假設您已經知道如何建立 Web API 專案。 如果沒有，請參閱[開始使用 ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。


啟動 Visual Studio 並建立新**ASP.NET Web 應用程式**專案。 選取**空**專案範本。 在 < 加入資料夾和核心參考 > 下選取**Web API**核取方塊。 （選擇性） 選取 Mircosoft azure 中部署應用程式的 「 主機中雲端 」 選項。 Microsoft 提供的免費 web 裝載最多 10 個網站中[免費試用的 Azure 帳戶](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

加入名為此 Web API 控制器`TestController`為下列程式碼：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

您可以在本機執行應用程式或部署至 Azure。 （在本教學課程螢幕擷取畫面，我部署至 Azure App Service Web 應用程式。）若要驗證是否運作 web API，導覽至`http://hostname/api/test/`，其中*hostname*是應用程式的部署所在的網域。 回應文字，您應該會看到&quot;取得： 測試訊息&quot;。

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>建立 WebClient 專案

建立另一個 ASP.NET Web 應用程式專案，然後選取**MVC**專案範本。 （選擇性） 選取**變更驗證** > **非驗證**。 此教學課程中，您不需要驗證。

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

在 [方案總管] 中，開啟檔案 Views/Home/Index.cshtml。 以下列內容取代此檔案中的程式碼：

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

如*serviceUrl*變數中，使用 web 服務應用程式的 URI。 現在在本機執行 WebClient 應用程式，或將它發行到另一個網站。

按一下 「 試試看 」 按鈕送出使用中列出的 HTTP 方法的 web 服務應用程式的 AJAX 要求 （GET、 POST 或 PUT） 的下拉式方塊。 這可讓我們檢查不同的跨原始要求。 現在，web 服務應用程式不支援 CORS，因此如果您按一下按鈕時，您會收到錯誤。

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> 若您監看工具中的 HTTP 流量，例如[Fiddler](http://www.telerik.com/fiddler)，您會看到瀏覽器並傳送 GET 要求，並要求成功，但 AJAX 呼叫會傳回錯誤。 請務必了解相同來源原則不會防止瀏覽器從*傳送*要求。 相反地，它會防止應用程式看到*回應*。


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>啟用 CORS

現在讓我們的 web 服務應用程式中啟用 CORS。 首先，新增 CORS NuGet 封裝。 在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

此命令會安裝最新的封裝，並更新所有相依性，包括核心 Web 應用程式開發介面程式庫。 使用者為目標的特定版本的版本旗標。 CORS 封裝需要 Web API 2.0 或更新版本。

開啟檔案的應用程式\_Start/WebApiConfig.cs。 將下列程式碼加入**WebApiConfig.Register**方法。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

接下來，加入 **[EnableCors]** 屬性`TestController`類別：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

如*origins*參數，使用 WebClient 應用程式的部署所在的 URI。 這可從 WebClient，讓跨原始要求時仍然不允許所有其他的跨網域要求。 我將在稍後說明的參數 **[EnableCors]** 中更多詳細資料。

不包含結尾的斜線*origins* URL。

重新部署更新的 web 服務應用程式。 您不需要更新 WebClient。 現在從 WebClient 要求應該都會成功。 所有允許的 GET、 PUT 和 POST 方法。

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>CORS 的運作方式

本章節描述 CORS 要求，在 HTTP 訊息的層級中發生的事。 請務必了解 CORS 的運作方式，讓您可以設定 **[EnableCors]** 屬性是否正確，以及疑難排解如果項目不在您預期方式運作。

CORS 規格導入了幾個新的 HTTP 標頭啟用跨原始要求。 如果瀏覽器支援 CORS，它會設定這些標頭會自動針對跨原始要求。您不需要執行任何 JavaScript 程式碼中的特殊的動作。

以下是跨原始要求的範例。 「 原始 」 標頭提供提出要求的站台的網域。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

如果伺服器允許的要求，它會設定存取控制-允許的原始標頭。 此標頭的值符合 Origin 標頭，或為萬用字元值"\*"，允許任何來源的意義。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

如果回應不包含存取控制-允許的原始標頭，要求將會失敗。 具體來說，瀏覽器不允許要求。 即使伺服器會傳回成功的回應，瀏覽器不會回應提供給用戶端應用程式。

**預檢要求**

對於某些 CORS 要求，瀏覽器會傳送其他要求，傳送實際要求的資源之前稱為 「 預檢要求，」。

如果下列條件成立，瀏覽器可以略過預檢要求：

- 要求方法為 GET、 HEAD 或 POST，*和*
- 應用程式不會設定 Accept，接受語言內容語言，以外的任何要求標頭的內容類型或最後一個事件識別碼、*和*
- Content-type 標頭 (如果設定) 是下列其中之一： 

    - application/x-www-form-urlencoded
    - multipart/表單資料
    - 文字/純文字

要求標頭的相關規則有套用到應用程式會藉由呼叫設定的標頭**setRequestHeader**上**XMLHttpRequest**物件。 （CORS 規格會呼叫這些 「 作者要求標頭 」）。規則不適用於標頭*瀏覽器*可以設定，例如使用者代理程式、 主機、 或內容長度。

預檢要求的範例如下：

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

事前要求會使用 OPTIONS HTTP 方法。 它包含兩個特殊標頭：

- 存取控制-要求的方法： 將實際的要求使用 HTTP 方法。
- 存取控制-要求的標頭： 要求標頭的清單，*應用程式*實際要求上設定。 （同樣地，這不包括瀏覽器設定的標頭。）

以下是範例回應，假設伺服器允許要求：

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

回應包括列出允許的方法，存取控制-允許-方法標頭和選擇性地存取控制-允許的標頭標頭，其中會列出允許的標頭。 如果預檢要求成功，瀏覽器會傳送實際要求，如先前所述。

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>[EnableCors] 的範圍規則

您可以在應用程式中，每個動作，每個控制站，或全域所有的 Web API 控制器啟用 CORS。

**每個動作**

若要啟用 CORS 單一動作，將 **[EnableCors]** 動作方法上的屬性。 下列範例會啟用 CORS`GetItem`只方法。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**每個控制站**

如果您設定 **[EnableCors]** 控制器類別，它會套用至控制器上的所有動作。 若要停用 CORS 的動作，將 **[DisableCors]** 動作屬性。 下列範例會針對每個方法，除了啟用 CORS `PutItem`。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**全域**

若要啟用您的應用程式中的所有 Web API 控制器的 CORS，傳遞**EnableCorsAttribute**執行個體**EnableCors**方法：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

如果您在一個以上的範圍內設定屬性，是優先順序：

1. 動作
2. 控制器
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>設定允許的來源

*Origins*參數 **[EnableCors]** 屬性會指定哪一個原始來源允許存取資源。 值為允許出處的逗號分隔清單。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

您也可以使用萬用字元值"\*」 允許從任何來源的要求。

請仔細考慮，才能允許來自任何來源的要求。 這表示幾乎任何網站可以讓您的 web API 的 AJAX 呼叫。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

*方法*參數 **[EnableCors]** 屬性會指定允許存取資源的 HTTP 方法。 若要讓所有方法，使用萬用字元值"\*"。 下列範例可讓只有 GET 和 POST 要求。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

稍早所述方式預檢要求可能包括存取控制-頭 access-control-request-headers 標頭中，列出應用程式所設定的 HTTP 標頭 (所謂的 「 撰寫要求標頭 」)。 *標頭*參數 **[EnableCors]** 屬性會指定允許哪些作者要求標頭。 若要允許任何標頭，設定*標頭*至 「\*"。 允許清單特定的標頭，設定*標頭*允許的標頭以逗號分隔清單：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

不過，瀏覽器不在它們如何設定存取控制-access-control-request-headers 標完全一致。 例如，Chrome 目前包含"origin";而 FireFox 不包含標準標頭，例如 「 接受 」，即使應用程式會設定這些指令碼中。

如果您設定*標頭*以外任何內容 」\*」，您至少應該包含 [接受]，「 內容型別 」 和 「 原始 」，再加上您想要支援的任何自訂標頭。

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>設定允許的回應標頭

根據預設，在瀏覽器不會公開所有應用程式的回應標頭。 預設可用的回應標頭如下：

- Cache-Control
- 內容語言
- Content-Type
- 到期
- 上次修改
- Pragma

CORS 規格會呼叫這些[簡單的回應標頭](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header)。 若要讓其他標頭使用應用程式，請設定*exposedHeaders*參數 **[EnableCors]**。

在下列範例中，控制器的`Get`方法會設定名為 'X 自訂的標頭' 自訂標頭。 根據預設，瀏覽器將不會公開此標頭中的跨原始要求。 若要使用的標頭，在中包含 'X 自訂的標頭' *exposedHeaders*。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>跨原始要求中傳遞認證

認證需要特殊處理 CORS 要求中。 根據預設，在瀏覽器不會傳送跨原始要求的任何認證。 認證會包括 cookie，以及 HTTP 驗證配置。 若要傳送與跨原始要求的認證，用戶端必須設定**XMLHttpRequest.withCredentials**為 true。

使用**XMLHttpRequest**直接：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

JQuery： 中

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

此外，伺服器必須允許認證。 若要允許跨原始認證 Web API 中，設定**SupportsCredentials**屬性設定為 true，在 **[EnableCors]** 屬性：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

如果這個屬性為 true，則 HTTP 回應將包含存取控制-允許-認證標頭。 此標頭會告知瀏覽器伺服器允許跨原始要求的認證。

如果瀏覽器傳送認證，但回應不包含有效的存取控制-允許-認證標頭，瀏覽器不會公開至應用程式中，回應和 AJAX 要求失敗。

非常謹慎設定**SupportsCredentials**為 true，因為這表示網站，位於另一個網域可以傳送到您的 Web API 使用者的身分登入之使用者的認證而使用者不知道。 CORS 規格也會指出該設定*origins*至&quot; \* &quot;無效如果**SupportsCredentials**為 true。

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>自訂的 CORS 原則提供者

**[EnableCors]** 屬性會實作**ICorsPolicyProvider**介面。 您可以藉由建立衍生自的類別提供您自己的實作**屬性**並實作**ICorsProlicyProvider**。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

現在您可以將任何地方，您會將屬性套用 **[EnableCors]**。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

例如，自訂的 CORS 原則提供者無法從組態檔中讀取設定。

使用屬性的替代方案，您可以向註冊**ICorsPolicyProviderFactory**建立物件**ICorsPolicyProvider**物件。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

若要設定**ICorsPolicyProviderFactory**，呼叫**SetCorsPolicyProviderFactory**擴充方法，在啟動時，如下所示：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>瀏覽器支援

Web API CORS 封裝是伺服器端技術。 使用者的瀏覽器也必須支援 CORS。 幸運的是，所有主要瀏覽器的目前版本包含[支援 CORS](http://caniuse.com/cors)。

Internet Explorer 8 和 Internet Explorer 9 已部分支援 CORS，而不 XMLHttpRequest 使用舊版 XDomainRequest 物件。 如需詳細資訊，請參閱[XDomainRequest-限制、 限制和因應措施](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx)。
