---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: "了解 ASP.NET AJAX Web 服務 |Microsoft 文件"
author: scottcate
description: "Web 服務是.NET framework 的跨平台解決方案提供分散式系統之間交換資料中不可或缺的一部分。 雖然 Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 8eb3486c9b3f4ddb6a8bc2c1cdcac774a6852574
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-web-services"></a>了解 ASP.NET AJAX Web 服務
====================
由[Scott 類別](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Web 服務是.NET framework 的跨平台解決方案提供分散式系統之間交換資料中不可或缺的一部分。 雖然 Web 服務通常用來允許不同的作業系統、 物件模型和程式設計語言來傳送和接收資料，但它們也可用來以動態方式將資料插入至 ASP.NET AJAX 頁面，或將資料從網頁傳送到後端系統。 這全都可以進行需要回傳作業就可以做到。


## <a name="calling-web-services-with-aspnet-ajax"></a>與 ASP.NET AJAX 呼叫 Web 服務

Dan Wahlin

Web 服務是.NET framework 的跨平台解決方案提供分散式系統之間交換資料中不可或缺的一部分。 雖然 Web 服務通常用來允許不同的作業系統、 物件模型和程式設計語言來傳送和接收資料，但它們也可用來以動態方式將資料插入至 ASP.NET AJAX 頁面，或將資料從網頁傳送到後端系統。 這全都可以進行需要回傳作業就可以做到。

雖然 ASP.NET AJAX UpdatePanel 控制項提供簡單的方式來 AJAX 啟用任何 ASP.NET 網頁，可能是當您需要以動態方式存取伺服器上的資料，而不使用 UpdatePanel 的時間。 在本文中，您會看到如何完成這項作業所建立和使用 ASP.NET AJAX 頁面內的 Web 服務。

本文將著重在核心 ASP.NET AJAX 擴充功能，以及啟用 Web 服務中的控制項稱為 AutoCompleteExtender ASP.NET AJAX Toolkit 中的可用功能。 涵蓋的主題包括定義啟用 AJAX 的 Web 服務、 建立用戶端 proxy，然後呼叫 JavaScript 的 Web 服務。 您也會看到如何 Web 服務呼叫，可以藉由直接 ASP.NET 網頁方法。

## <a name="web-services-configuration"></a>Web 服務組態

Visual Studio 2008 建立新的網站專案時，web.config 檔的數目可能不熟悉舊版 Visual Studio 的使用者新增。 這些修改的某些"asp 」 前置詞對應至 ASP.NET AJAX 控制項讓它們可以用於在網頁中其他項目會定義必要 HttpHandlers 和 HttpModules。 清單 1 顯示所做的修改`<httpHandlers>`影響 Web 服務呼叫的 web.config 中的項目。 移除和取代位於 System.Web.Extensions.dll 組件中的 ScriptHandlerFactory 類別 HttpHandler 用來處理.asmx 呼叫預設值。 System.Web.Extensions.dll 包含所有 ASP.NET AJAX 所使用的核心功能。

**清單 1。ASP.NET AJAX Web 服務處理常式組態**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

此 HttpHandler 取代為了讓 JavaScript 物件標記法 (JSON) 呼叫必須從 ASP.NET AJAX 頁面對.NET Web 服務使用 JavaScript 的 Web 服務 proxy。 ASP.NET AJAX 的 Web 服務會傳送 JSON 訊息，而不是標準的簡易物件存取通訊協定 (SOAP) 呼叫一般與 Web 服務相關聯。 這會導致較小的要求和回應訊息整體。 它也可以更有效率的用戶端處理的資料因為 ASP.NET AJAX JavaScript 程式庫已最佳化，可使用的 JSON 物件。 列出 2 和 Web 服務要求和回應訊息序列化為 JSON 格式列出 3 顯示範例。 列出 2 所示的要求訊息傳遞國家 （地區） 參數值是"比利時 「 客戶 」 物件的陣列和其相關聯的屬性，會將傳遞回應訊息中列出的 3 時。

**表 2。Web 服務要求訊息序列化為 JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *>[!NOTE]作業名稱定義為 web 服務 URL 的一部分; 此外，要求訊息不一定提交透過 JSON。Web 服務可以利用 ScriptMethod 屬性 UseHttpGet 參數設為 true 時，這會導致透過傳遞參數的查詢字串參數。*


**列出 3。Web 服務的回應訊息序列化為 JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

下一節中，您會看到如何建立 Web 服務能夠處理 JSON 要求訊息和回應以簡單和複雜類型。

## <a name="creating-ajax-enabled-web-services"></a>建立啟用 AJAX 的 Web 服務

ASP.NET AJAX framework 提供數個不同的方式，來呼叫 Web 服務。 您可以使用 JavaScript 的 AutoCompleteExtender 控制項 （適用於 ASP.NET AJAX 工具組）。 不過，呼叫服務之前您必須 AJAX 啟用它，讓它可以由用戶端指令碼的程式碼呼叫。

不論您還不熟悉 ASP.NET Web 服務，您會發現它直接建立並啟用 AJAX 功能的服務。 .NET framework 已支援的自 2002年初始發行版本的 ASP.NET Web 服務建立和 ASP.NET AJAX 擴充功能提供額外的 AJAX 功能建置在.NET framework 的預設集的功能時。 Visual Studio.NET 2008 Beta 2 已建立.asmx Web 服務檔案的內建支援，並自動從 System.Web.Services.WebService 類別中衍生相關聯的程式碼旁邊的類別。 當您將方法加入至類別，您必須套用 WebMethod 屬性，使其由 Web 服務取用者呼叫。

列出 4 顯示將 WebMethod 屬性套用至名為 GetCustomersByCountry() 方法的範例。

**列出 4。Web 服務中使用 WebMethod 屬性**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

GetCustomersByCountry() 方法接受國家 （地區） 參數，並傳回客戶物件陣列。 傳遞至方法的國家 （地區） 值被轉送到接著呼叫從資料庫擷取資料的資料層類別客戶物件屬性中填入資料，並傳回陣列的商業層類別。

## <a name="using-the-scriptservice-attribute"></a>使用 ScriptService 屬性

當加入的 WebMethod 屬性可讓 GetCustomersByCountry() 方法呼叫的標準 SOAP 訊息傳送至 Web 服務用戶端，它並不允許 JSON 呼叫將會從 ASP.NET AJAX 應用程式立即可用的。 若要允許進行需要套用 ASP.NET AJAX 擴充功能的 JSON 呼叫`ScriptService`屬性設定為 Web 服務類別。 這可讓 Web 服務以傳送回應使用 JSON 格式化的訊息，並可讓用戶端指令碼來呼叫服務所傳送 JSON 訊息。

列出 5 顯示 ScriptService 屬性套用至名為 CustomersService Web 服務類別的範例。

**列出 5。使用 ScriptService 屬性 AJAX 啟用 Web 服務**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

ScriptService 屬性做為標記，表示它可以從 AJAX 指令碼呼叫。 它實際上不會處理任何幕後發生的 JSON 序列化或還原序列化工作。 ScriptHandlerFactory （在 web.config 中設定） 和其他相關的類別執行大量的 JSON 處理。

## <a name="using-the-scriptmethod-attribute"></a>使用 ScriptMethod 屬性

ScriptService 屬性是定義在.NET Web 服務，才能讓它是由 ASP.NET AJAX 頁面所使用的唯一 ASP.NET AJAX 屬性。 不過，另一個屬性的具名 ScriptMethod 可以也會直接套用到服務中的 Web 方法。 ScriptMethod 定義三個屬性，包括`UseHttpGet`，`ResponseFormat`和`XmlSerializeString`。 變更這些屬性的值可以是有用的 Web 方法所接受的要求類型需要 Web 方法需要的形式傳回原始的 XML 資料時，必須變更為 GET、`XmlDocument`或`XmlElement`物件，或當從傳回的資料 服務一律應序列化為 XML，而不是 JSON。

Web 方法應該接受時，就可以使用屬性 UseHttpGet GET 要求而不是 POST 要求。 要求會傳送具有 Web 方法的輸入參數中使用 URL 轉換成 QueryString 參數。 UseHttpGet 屬性預設為 false，應該只設定為`true`當作業都已知為安全且機密資料時未傳遞至 Web 服務。 列出 6 顯示 UseHttpGet 屬性使用 ScriptMethod 屬性的範例。

**列出 6。搭配 UseHttpGet 屬性使用 ScriptMethod 屬性。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

列出 6 所示 HttpGetEcho Web 方法呼叫時所傳送的標頭的範例會顯示 下一步：

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

除了可讓 Web 方法，以接受 HTTP GET 要求，XML 回應需要傳回從服務，而不是 JSON 時也可以使用 ScriptMethod 屬性。 例如，Web 服務可能會從遠端站台擷取 RSS 摘要，並將其當做 XmlDocument 或 XmlElement 物件。 處理 XML 的資料可以再出現在用戶端上。

列出 7 顯示使用 ResponseFormat 屬性來指定應傳回 Web 方法中的 XML 資料的範例。

**列出 7。搭配 ResponseFormat 屬性使用 ScriptMethod 屬性。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

ResponseFormat 屬性也可以搭配 XmlSerializeString 屬性。 XmlSerializeString 屬性具有預設值是 false 表示全部傳回除了字串從 Web 方法傳回的型別會序列化為 XML 時`ResponseFormat`屬性設定為`ResponseFormat.Xml`。 當`XmlSerializeString`設`true`，從 Web 方法傳回的所有型別會序列化為 XML，包括字串類型。 如果 ResponseFormat 屬性的值為`ResponseFormat.Json`XmlSerializeString 屬性會被忽略。

列出 8 顯示使用 XmlSerializeString 屬性以強制序列化為 XML 字串的範例。

**列出 8。搭配 XmlSerializeString 屬性使用 ScriptMethod 屬性**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

隨即會顯示從呼叫程式碼範例 8 所示 GetXmlString Web 方法傳回的值：

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

雖然預設的 JSON 格式要求和回應訊息的整體大小降到最低，並更輕易地取用 ASP.NET AJAX 用戶端跨瀏覽器的方式，ResponseFormat 和 XmlSerializeString 屬性可以是過低時用戶端例如 Internet Explorer 5 或更高版本的應用程式預期要從 Web 方法傳回的 XML 資料。

## <a name="working-with-complex-types"></a>使用複雜型別

列出 5 示範了範例傳回名為 Customer 從 Web 服務複雜類型。 「 客戶 」 類別定義為屬性，例如 FirstName 和 LastName 內部幾種不同的簡單類型。 複雜型別做為輸入參數，或啟用 AJAX 的 Web 方法的傳回型別都會自動為 JSON 序列化再傳送至用戶端。 不過，巢狀複雜類型 （其在內部定義另一個型別中） 不會提供給用戶端做為獨立物件預設。

在其中 Web 服務所使用的巢狀複雜類型必須也可用於用戶端頁面的情況下，ASP.NET AJAX GenerateScriptType 屬性可以加入至 Web 服務。 列出 9 所示 CustomerDetails 類別包含的位址和 Gender 屬性，例如其***代表巢狀複雜類型。***

**列出 9。如下所示 CustomerDetails 類別包含兩個巢狀複雜類型。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

列出 9 所示的 CustomerDetails 類別內定義的位址和性別物件將不會自動成為可供使用透過 JavaScript 用戶端因為它們都是巢狀的類型 （地址是類別和性別是列舉型別）。 在使用 Web 服務中的巢狀的類型必須是在用戶端可用的情況下，可以使用先前所述的 GenerateScriptType 屬性 （請參閱列出 10）。 這個屬性可以在其中從服務傳回不同的巢狀複雜類型的情況下多次新增。 它可以套用 Web 服務類別直接或更新特定 Web 方法。

**列出 10。使用 GenerateScriptService 屬性來定義可供用戶端使用的巢狀型別。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

藉由套用`GenerateScriptType`Web 服務、 地址和性別類型的屬性會自動可供使用的用戶端 ASP.NET AJAX JavaScript 程式碼。 自動產生並由 Web 服務上加入 GenerateScriptType 屬性傳送至用戶端 JavaScript 的範例所示 11。 您會看到如何使用巢狀複雜類型，在本文稍後。

**列出 11。提供給 ASP.NET AJAX 頁面的巢狀複雜類型。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

既然您已經看到如何建立 Web 服務，並使其可存取的 ASP.NET AJAX 網頁，讓我們看看如何建立和使用 JavaScript proxy，以便資料可以擷取或傳送至 Web 服務。

## <a name="creating-javascript-proxies"></a>建立 JavaScript Proxy

通常呼叫標準的 Web 服務 （.NET 或另一個平台），包括建立傳送 SOAP 要求和回應訊息的複雜性時保護您的 proxy 物件。 透過 ASP.NET AJAX Web 服務呼叫，可建立及用來輕鬆地呼叫服務，而不需擔心序列化和還原序列化 JSON 訊息 JavaScript proxy。 可以使用 ASP.NET AJAX ScriptManager 控制項自動產生 JavaScript proxy。

建立 JavaScript proxy 可以呼叫 Web 服務被透過使用在 ScriptManager 的服務屬性。 這個屬性可讓您定義一個或多個 ASP.NET AJAX 頁面可以傳送或接收資料，而不需回傳作業以非同步方式呼叫的服務。 您可以定義服務使用 ASP.NET AJAX`ServiceReference`控制項並將 Web 服務 URL 指派給控制項的`Path`屬性。 列出 12 顯示參考名為 CustomersService.asmx 服務的範例。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**列出 12。定義使用 ASP.NET AJAX 頁面的 Web 服務。**

將參考加入至透過 ScriptManager 控制項 CustomersService.asmx 會導致以動態方式產生並此網頁所參考的 JavaScript proxy。 Proxy 會使用內嵌&lt;指令碼&gt;標記，並以動態方式載入呼叫 CustomersService.asmx 檔案，以及將 /js 附加至結尾。 下列範例會示範如何 JavaScript proxy 內嵌於頁面在 web.config 中停用偵錯時：

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *>[!NOTE]如果您想要查看實際 JavaScript proxy 的程式碼會產生您可以 Internet Explorer 網址方塊中輸入所需的.NET Web 服務的 URL，並將 /js 附加至結尾。*


如果 JavaScript proxy 的偵錯版本會內嵌在網頁中做的 web.config 中啟用偵錯時顯示 下一步：

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

ScriptManager 所建立的 JavaScript proxy 也可以直接在網頁內嵌，而非使用參考&lt;指令碼&gt;標記的 src 屬性。 ServiceReference 控制項的 InlineScript 屬性設定為 true （預設值為 false） 可以完成此動作。 Proxy 不共用跨多個網頁和您想要降低對伺服器進行網路呼叫的數目時，這非常有用。 當 InlineScript 設定為 true proxy 指令碼將不會快取瀏覽器，在 ASP.NET AJAX 應用程式中的多個頁面所使用的 proxy 的情況，建議使用預設值為 false。 使用 InlineScript 屬性的範例會顯示下一步:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>使用 JavaScript Proxy

一旦使用 ScriptManager 控制項的 ASP.NET AJAX 網頁參照 Web 服務，可以呼叫 Web 服務，而且可以使用回呼函式處理傳回的資料。 會呼叫 Web 服務 （如果有的話） 參考其命名空間、 類別名稱和 Web 方法名稱。 可以定義傳遞至 Web 服務的任何參數，以及處理傳回的資料的回呼函式。

使用 JavaScript proxy 來呼叫名為 GetCustomersByCountry() 的 Web 方法的範例所示列出 13。 GetCustomersByCountry() 函式是在終端使用者按一下頁面上的按鈕時呼叫。

**列出 13。JavaScript proxy，以呼叫 Web 服務。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

此呼叫參考 InterfaceTraining 命名空間、 服務中定義的 CustomersService 類別和 GetCustomersByCountry Web 方法。 從文字方塊取得國家 （地區） 值，以及名為 OnWSRequestComplete 非同步的 Web 服務呼叫傳回時要叫用的回呼函式，會將它傳遞。 OnWSRequestComplete 處理從服務傳回的客戶物件的陣列，並將其轉換都會顯示在頁面的資料表。 呼叫產生的輸出是以圖 1 所示。


[![進行非同步 AJAX 呼叫的 Web 服務所取得的資料繫結。](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**圖 1**： 將資料繫結來進行非同步 AJAX 呼叫的 Web 服務取得。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-web-services/_static/image3.png))


JavaScript proxy 也可以在其中應該呼叫 Web 方法，但 proxy 不應該等候回應的情況下進行單向呼叫 Web 服務。 例如，您可以呼叫 Web 服務，以啟動處理程序，例如工作流程，但不是等候來自服務的傳回值。 在對服務進行單向呼叫需要的地方的情況下，列出 13 所示的回呼函式可以被忽略。 因為沒有回呼函式定義為 proxy 物件將不會等待 Web 服務傳回的資料。

## <a name="handling-errors"></a>處理錯誤

Web 服務的非同步回呼可能會遇到各種錯誤，例如網路故障，Web 服務無法使用或傳回例外狀況類型。 幸運的是，ScriptManager 所產生的 JavaScript proxy 物件可讓多個回呼來處理錯誤和失敗除了稍早所示的 success 回呼函式定義。 錯誤回呼函式可以定義 Web 方法的呼叫中的標準的回呼函式之後立即列出 14 中所示。

**列出 14。定義錯誤回呼函式，並顯示錯誤。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

呼叫 Web 服務時，發生任何錯誤都將會觸發 OnWSRequestFailed() 回呼函式呼叫可接受的物件，代表做為參數的錯誤。 Error 物件會公開數個不同的函數，以判斷錯誤的原因，以及在呼叫已逾時。使用不同的錯誤函式的範例會顯示列出 14 和圖 2 顯示的函式所產生的輸出範例。


[![藉由呼叫 ASP.NET AJAX 錯誤函數所產生的輸出。](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**圖 2**： 呼叫 ASP.NET AJAX 錯誤函式所產生的輸出。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>處理從 Web 服務傳回的 XML 資料

您稍早已如何 Web 方法時，無法使用 ScriptMethod 屬性以及其 ResponseFormat 屬性來傳回原始的 XML 資料。 當 ResponseFormat 設 ResponseFormat.Xml 時，從 Web 服務傳回的資料會序列化為 XML，而不是 JSON。 當 XML 資料必須直接傳遞至用戶端使用 JavaScript 或 XSLT 的處理，這非常有用。 目前的時間，在 Internet Explorer 5 或更高版本會提供最佳的用戶端物件模型，用來剖析和篩選 XML 資料，因為它對 MSXML 的內建支援。

擷取 XML 資料從 Web 服務並無不同擷取其他資料型別。 啟動叫用 JavaScript proxy 來呼叫適當的函式，並定義的回呼函式。 一旦呼叫會傳回您可以再處理回呼函式中的資料。

列出 15 顯示呼叫名為 GetRssFeed() 傳回 XmlElement 物件的 Web 方法的範例。 GetRssFeed() 接受單一參數代表之 rss 摘要擷取的 URL。

**列出 15。從 Web 服務傳回的 XML 資料搭配使用。**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

這個範例會將 URL 傳遞至 RSS 摘要，且處理 OnWSRequestComplete() 函式傳回的 XML 資料。 OnWSRequestComplete() 會先檢查瀏覽器是否要知道 MSXML 剖析器的 Internet Explorer。 如果是，XPath 陳述式用來尋找所有&lt;項目&gt;RSS 摘要中的標記。 然後，會逐一查看每個項目與相關聯&lt;標題&gt;和&lt;連結&gt;標記找出和處理，以顯示每個項目資料。 圖 3 顯示進行 ASP.NET AJAX 透過 JavaScript proxy GetRssFeed() Web 方法呼叫所產生的輸出的範例。

## <a name="handling-complex-types"></a>處理複雜型別

接受或 Web 服務所傳回的複雜型別會自動公開 JavaScript proxy。 不過，巢狀複雜類型不是在用戶端直接存取除非 GenerateScriptType 屬性套用至服務先前所述。 您為什麼要使用巢狀複雜類型，用戶端上？

若要回答這個問題的答案，假設 ASP.NET AJAX 頁面會顯示客戶資料，並可讓使用者更新客戶地址。 如果 Web 服務可讓您指定位址類型 （CustomerDetails 類別內定義的複雜型別），可傳送給用戶端更新程序可以分割成個別的函式，供重複使用較佳程式碼。


[![從呼叫傳回 RSS 資料的 Web 服務建立的輸出。](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**圖 3**： 從呼叫傳回 RSS 資料的 Web 服務建立的輸出。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-web-services/_static/image9.png))


列出 16 顯示模型命名空間中定義的位址物件會叫用的用戶端程式碼範例填入已更新的資料，並指派給 CustomerDetails 物件的位址屬性。 CustomerDetails 物件接著傳遞至 Web 服務進行處理。

**列出 16。使用巢狀複雜類型**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>建立和使用頁面方法

Web 服務提供公開各種不同的用戶端包括 ASP.NET AJAX 網頁來重複使用服務的絕佳方式。 不過，可能的需要擷取永遠不會使用或其他頁面所共用的資料頁面。 在此情況下，進行以允許存取的資料頁的.asmx 檔案看起來好像大材小用因為服務才會使用單一頁面。

ASP.NET AJAX 提供另一個機制來呼叫 Web 服務類似不需建立獨立.asmx 檔案。 這是使用稱為 「 頁面方法 」 的技術。 頁面方法是直接在網頁或程式碼除外的檔案中內嵌的 WebMethod 屬性套用至這些靜態 (共用中 VB.NET) 方法。 藉由套用 WebMethod 屬性呼叫使用特殊的 JavaScript 物件，名為 PageMethods 在執行階段動態建立。 PageMethods 物件做為從 JSON 序列化/還原序列化程序時保護您的 proxy。 請注意，為了使用 PageMethods 物件，您必須將 ScriptManager EnablePageMethods 屬性為 true。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

列出 17 顯示在 ASP.NET 之外的程式碼類別中定義兩個頁面方法的範例。 這些方法會擷取位在應用程式的商業層類別中的資料\_之網站的程式碼資料夾。

**列出 17。定義頁面的方法。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

當 ScriptManager 偵測到頁面中的 Web 方法的存在就會產生動態參考先前所述的 PageMethods 物件。 呼叫 Web 方法是藉由參考後面接著名稱的方法和任何必要的參數資料應該傳遞 PageMethods 類別來完成。 列出 18 顯示呼叫兩個稍早所示的分頁方法的範例。

**列出 18。呼叫頁面與 PageMethods JavaScript 物件的方法。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

使用 PageMethods 物件是非常類似於使用 JavaScript proxy 物件。 先指定所有參數的資料應該傳遞至該頁面的方法，然後定義非同步呼叫傳回時，應該呼叫回呼函式。 您也可以指定錯誤回呼 （請參閱列出 14 處理失敗的範例）。

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender 和 ASP.NET AJAX 工具組

ASP.NET AJAX Toolkit (可從[http://ajax.asp.net](http://ajax.asp.net)) 提供數個控制項，可用來存取 Web 服務。 具體而言，此工具組包含有用的控制項，名為`AutoCompleteExtender`，可以用來呼叫 Web 服務，並在頁面中顯示資料，而不需要撰寫任何 JavaScript 程式碼完全。

AutoCompleteExtender 控制項可用來擴充現有功能的文字方塊，並幫助使用者更輕鬆地找到自己要找的資料。 當他們在文字方塊中輸入的控制項可以用來查詢 Web 服務，並以動態方式顯示文字方塊底下的結果。 圖 4 顯示使用 AutoCompleteExtender 控制項來顯示支援應用程式的客戶 id 的範例。 當使用者在文字方塊中輸入不同的字元，就會顯示不同的項目下方根據其輸入。 然後，使用者可以選取所需的客戶識別碼。

使用 ASP.NET AJAX 頁面內 AutoCompleteExtender 需要 AjaxControlToolkit.dll 組件加入至網站的 bin 資料夾。 一旦加入 toolkit 組件，您需要參考在 web.config 中，以便用於應用程式中的所有頁面，您可以使用它所包含的控制項。 這可藉由加入下列標記內 web.config 的&lt;控制項&gt;標記：

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

在您只需要使用特定的頁面中的控制項的情況下您可以將 Reference 指示詞加入至頁面頂端，如下所示，而不是更新 web.config 參考：

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![使用 AutoCompleteExtender 控制項。](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**圖 4**： 使用 AutoCompleteExtender 控制項。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-web-services/_static/image12.png))


網站已設定為使用 ASP.NET AJAX 工具組，一旦 AutoCompleteExtender 控制項可以加入至網頁更像您可以加入規則的 ASP.NET 伺服器控制項。 列出 19 顯示使用控制項來呼叫 Web 服務的範例。

**列出 19。使用 ASP.NET AJAX Toolkit AutoCompleteExtender 控制項。**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender 有數個不同的屬性，包括在伺服器上的控制項上找到的標準識別碼和 runat 屬性。 除了這些項目，它可讓您定義的使用者類型在 Web 服務之前會查詢資料的字元數。 列出 19 所示的 MinimumPrefixLength 屬性，會導致服務呼叫每次的文字方塊中輸入一個字元。 您會想應特別小心，因為每次使用者輸入的字元 Web 服務，將此值設定將被呼叫來搜尋比對的字元，在文字方塊中的值。 分別使用 ServicePath 和 ServiceMethod 屬性會定義要呼叫的 Web 服務，以及目標 Web 方法。 最後，TargetControlID 屬性會識別哪些連結 AutoCompleteExtender 控制項的文字方塊。

被呼叫之 Web 服務必須有套用先前所述的 ScriptService 屬性，而且目標 Web 方法必須接受兩個名為 prefixText 和計數的參數。 PrefixText 參數代表使用者輸入的字元，而計數參數代表要傳回項目數目 （預設值為 10）。 列出 20 顯示 GetCustomerIDs Web 方法，稍早所述列出 19 AutoCompleteExtender 控制項會呼叫此方法的範例。 Web 方法呼叫的商務層方法，接著呼叫資料圖層會處理篩選資料，以及傳回比對結果的方法。 資料層方法的程式碼所示列出 21。

**列出 20。從 AutoCompleteExtender 控制項傳送的資料進行篩選。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**列出 21。篩選根據使用者輸入的結果。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>結論

ASP.NET AJAX 呼叫 Web 服務，而不需要撰寫自訂的 JavaScript 程式碼來處理要求和回應訊息的許多提供極佳的支援。 在本文中，您已經看到如何 AJAX 啟用.NET Web 服務，讓它們來處理 JSON 訊息，以及如何定義使用 ScriptManager 控制項的 JavaScript proxy。 您已看過的 JavaScript proxy 可以用來呼叫 Web 服務如何處理簡單和複雜類型和處理失敗。 最後，您已經看到如何頁面方法可用來簡化建立，並讓 Web 服務呼叫的程序，以及如何 AutoCompleteExtender 控制項可以提供使用者說明他們輸入。 雖然 ASP.NET AJAX 中可用的 UpdatePanel 當然會由於其簡易性許多 AJAX 程式設計人員所選擇的控制項，必須了解如何透過 JavaScript proxy 呼叫 Web 服務可用於許多應用程式。

## <a name="bio"></a>簡歷

Dan Wahlin （Microsoft 最有價值專家適用於 ASP.NET 和 XML Web Service） 是.NET 開發講師和架構顧問在介面技術訓練 ([http://www.interfacett.com](http://www.interfacett.com))。 Dan 所建構的 XML for ASP.NET 開發人員網站 ([www.XMLforASP.NET](http://www.XMLforASP.NET))，位於 INETA 喇叭局所免費提供，並在數個所做的心得。 Dan 共同撰寫 Professional Windows DNA (Wrox)，ASP.NET： 秘訣、 教學課程與程式碼 (Sam)、 ASP.NET 1.1 測試人員方案、 Professional ASP.NET 2.0 AJAX (Wrox)、 ASP.NET 2.0 MVP 缺失，以及 ASP.NET 開發人員 (Sam) 撰寫的 XML。 當他不撰寫程式碼、 文件或活頁簿時，Dan 喜歡的休閒活動撰寫和錄製音樂和播放高爾夫球和籃球與他的妻子小孩。

Scott 是否已從 1997 年使用 Microsoft Web 技術，且 myKB.com 總統 ([www.myKB.com](http://www.myKB.com)) 擅長撰寫 ASP.NET 架構的重點 Knowledge Base 軟體解決方案的應用程式。 透過在電子郵件，即可以聯繫 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在他的部落格[ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[上一頁](understanding-asp-net-ajax-localization.md)
[下一頁](understanding-asp-net-ajax-debugging-capabilities.md)
