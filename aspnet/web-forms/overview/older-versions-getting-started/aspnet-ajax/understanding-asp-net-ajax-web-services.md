---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: 了解 ASP.NET AJAX Web 服務 |Microsoft Docs
author: scottcate
description: Web 服務是分散式系統之間交換資料中提供跨平台解決方案的.NET framework 中不可或缺的一部分。 雖然 Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: c3874e19ef55ffd4949f9bc280d32a3da7a00271
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376381"
---
<a name="understanding-aspnet-ajax-web-services"></a>了解 ASP.NET AJAX Web 服務
====================
藉由[Scott Cate](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Web 服務是分散式系統之間交換資料中提供跨平台解決方案的.NET framework 中不可或缺的一部分。 雖然 Web 服務用來允許不同的作業系統、 物件模型和程式設計語言來傳送和接收資料，但它們也可用來以動態方式將資料插入 ASP.NET AJAX 頁面，或將資料從頁面傳送至後端系統。 這一切都可以在不訴諸於回傳作業完成。


## <a name="calling-web-services-with-aspnet-ajax"></a>使用 ASP.NET AJAX 呼叫 Web 服務

Dan Wahlin

Web 服務是分散式系統之間交換資料中提供跨平台解決方案的.NET framework 中不可或缺的一部分。 雖然 Web 服務用來允許不同的作業系統、 物件模型和程式設計語言來傳送和接收資料，但它們也可用來以動態方式將資料插入 ASP.NET AJAX 頁面，或將資料從頁面傳送至後端系統。 這一切都可以在不訴諸於回傳作業完成。

而 ASP.NET AJAX UpdatePanel 控制項提供簡單的方式，ajax 啟用任何 ASP.NET 頁面，可能是當您需要以動態方式存取伺服器上的資料，而不使用 UpdatePanel。 在本文中，您會看到如何完成這項作業所建立和使用 ASP.NET AJAX 頁面內的 Web 服務。

這篇文章將著重於提供核心的 ASP.NET AJAX Extensions，以及呼叫 AutoCompleteExtender ASP.NET AJAX Toolkit 中的 Web 服務啟用控制項的功能。 涵蓋的主題包括定義啟用 AJAX 的 Web 服務，建立用戶端 proxy，然後呼叫使用 JavaScript 的 Web 服務。 您也會看到如何發出 Web 服務呼叫，直接向 ASP.NET 頁面方法。

## <a name="web-services-configuration"></a>Web 服務設定

使用 Visual Studio 2008 建立新的網站專案時，web.config 檔案會有一些新增的項目可能不熟悉舊版 Visual Studio 的使用者。 因此他們可以使用頁面中其他項目會定義必要 HttpHandlers 和 HttpModules，這些修改部分可以對應到 ASP.NET AJAX 控制項的"asp"前置詞。 列表 1 顯示所做的修改`<httpHandlers>`會影響 Web 服務呼叫的 web.config 中的項目。 預設的 HttpHandler 用來處理.asmx 呼叫會移除，並取代為 ScriptHandlerFactory 類別，位於 System.Web.Extensions.dll 組件。 System.Web.Extensions.dll 內含所有使用 ASP.NET AJAX 的核心功能。

**列表 1。ASP.NET AJAX Web 服務處理常式組態**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

此 HttpHandler 取代對 ASP.NET AJAX 頁面從.NET Web 服務的 JavaScript Object Notation (JSON) 呼叫，以便使用 JavaScript 的 Web 服務 proxy。 ASP.NET AJAX Web 服務會傳送 JSON 訊息，而不是標準的簡易物件存取通訊協定 (SOAP) 呼叫，一般與 Web 服務相關聯。 這會導致較小的要求和整體的回應訊息。 它也允許更有效率的用戶端處理的資料因為 ASP.NET AJAX JavaScript 程式庫已最佳化來處理 JSON 物件。 列表 2 和 Web 服務要求和回應訊息序列化為 JSON 格式列表 3 顯示範例。 列表 3 中的回應訊息會將客戶物件的陣列，以及其相關聯的屬性時，則列表 2 所示的要求訊息會將傳送的國家/地區參數，且"比利時"的值。

**列表 2。Web 服務要求訊息序列化為 JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] 作業名稱被定義為 web 服務 URL; 的一部分此外，要求訊息不一定提交透過 JSON。Web 服務可以使用 ScriptMethod 屬性，並 UseHttpGet 參數設定為 true，這會導致透過傳遞參數的查詢字串參數。*


**列表 3。Web 服務的回應訊息序列化為 JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

下一節中，您會看到如何建立 Web 服務能夠處理 JSON 要求訊息，並回應與簡單和複雜型別。

## <a name="creating-ajax-enabled-web-services"></a>建立啟用 AJAX 的 Web 服務

ASP.NET AJAX 架構會提供數個不同的方式，來呼叫 Web 服務。 您可以使用 AutoCompleteExtender 控制項 （適用於 ASP.NET AJAX Toolkit） 或 JavaScript。 不過，呼叫服務之前必須 AJAX 啟用它，以便它可以由用戶端指令碼來呼叫。

不論是否您還不熟悉 ASP.NET Web 服務，您會發現它直接建立與 AJAX 啟用服務。 .NET framework 已支援從初始發行 2002年中建立 ASP.NET Web 服務和 ASP.NET AJAX Extensions 提供額外的 AJAX 功能建置在.NET framework 的一組預設功能。 Visual Studio.NET 2008 Beta 2 已內建支援，讓您建立的.asmx Web 服務檔案，並會自動衍生自 stockservice 類別的 相關聯的程式碼除外的類別檔案。 當您新增到類別的方法，您必須套用 WebMethod 屬性，使其可由 Web 服務取用者呼叫。

列表 4 顯示將 WebMethod 屬性套用至名為 GetCustomersByCountry() 方法的範例。

**列表 4。在 Web 服務中使用 WebMethod 屬性**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

GetCustomersByCountry() 方法接受的國家/地區參數，並傳回客戶物件陣列。 傳遞至方法的國家/地區值轉送給呼叫資料層的類別，從資料庫擷取資料以資料填入客戶物件屬性，並傳回陣列的商務層類別。

## <a name="using-the-scriptservice-attribute"></a>使用 ScriptService 屬性

同時新增的 WebMethod 屬性可讓 GetCustomersByCountry() 方法，將標準的 SOAP 訊息傳送至 Web 服務的用戶端呼叫，它並不允許從內建的 ASP.NET AJAX 應用程式進行 JSON 呼叫。 若要允許您就必須套用 ASP.NET AJAX 擴充功能進行 JSON 呼叫`ScriptService`屬性設定為 Web 服務類別。 這可讓 Web 服務以傳送回應訊息使用 JSON 來格式化，並可讓用戶端指令碼來呼叫服務傳送 JSON 訊息。

列表 5 顯示 ScriptService 屬性套用至名為 CustomersService Web 服務類別的範例。

**列表 5。使用 AJAX 啟用 Web 服務的 ScriptService 屬性**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

ScriptService 屬性做為標記，以表示它可以在從 AJAX 指令碼的程式碼呼叫。 它實際上不處理任何幕後發生的 JSON 序列化或還原序列化工作。 （在 web.config 中設定） ScriptHandlerFactory 和其他相關的類別執行大量的 JSON 處理。

## <a name="using-the-scriptmethod-attribute"></a>使用 ScriptMethod 屬性

ScriptService 屬性是唯一必須定義在.NET Web 服務，才能讓它以供 ASP.NET AJAX 頁面中的 ASP.NET AJAX 屬性。 不過，名為 ScriptMethod 的另一個屬性可以也直接套用到服務中的 Web 方法。 ScriptMethod 定義三個屬性，包括`UseHttpGet`，`ResponseFormat`和`XmlSerializeString`。 變更這些屬性的值可以是在何處時 Web 方法必須傳回原始的 XML 資料的形式會變更為 GET、 Web 方法所接受的要求類型的情況下有用`XmlDocument`或`XmlElement`物件，或當從傳回的資料 服務一律應序列化為 XML，而不是 JSON。

Web 方法應該接受時，就可以使用屬性 UseHttpGet 取得而不是 POST 要求的要求。 要求會傳送 Web 方法的輸入參數中使用的 URL 轉換成查詢字串參數。 UseHttpGet 屬性預設為 false，應該只設定為`true`當作業都已知為安全且當機密資料不會傳遞給 Web 服務。 列表 6 顯示 UseHttpGet 屬性搭配使用 ScriptMethod 屬性的範例。

**列表 6。您可以使用 ScriptMethod 屬性搭配 UseHttpGet 屬性。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

列表 6 所示 HttpGetEcho Web 方法呼叫時所傳送的標頭的範例會顯示下一步:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

除了允許接受 HTTP GET 要求的 Web 方法，需要從服務，而不是 JSON 傳回 XML 回應時也可以用 ScriptMethod 屬性。 比方說，Web 服務可能會擷取與遠端站台的 RSS 摘要，並傳回做為 XmlDocument 或 XmlElement 物件。 處理 XML 資料可以進行用戶端上。

列表 7 顯示使用 ResponseFormat 屬性來指定應該從 Web 方法傳回的 XML 資料的範例。

**列表 7。您可以使用 ScriptMethod 屬性與 ResponseFormat 屬性。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

ResponseFormat 屬性也可以搭配 XmlSerializeString 屬性。 XmlSerializeString 屬性具有預設值是 false 表示所有傳回的資料類型，除了從 Web 方法傳回的字串會序列化為 XML 時`ResponseFormat`屬性設定為`ResponseFormat.Xml`。 當`XmlSerializeString`設為`true`，從 Web 方法傳回的所有型別序列化為 XML，包括字串類型。 如果 ResponseFormat 屬性值為`ResponseFormat.Json`XmlSerializeString 屬性會被忽略。

列表 8 顯示使用 XmlSerializeString 屬性，以強制序列化為 XML 字串的範例。

**列表 8。搭配 XmlSerializeString 屬性使用 ScriptMethod 屬性**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

從呼叫列表 8 所示 GetXmlString Web 方法傳回的值會顯示下一步:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

雖然預設的 JSON 格式要求和回應訊息的整體大小降到最低，而且更容易取用 ASP.NET AJAX 用戶端的跨瀏覽器的方式，可以是 ResponseFormat 和 XmlSerializeString 屬性使用時用戶端例如 Internet Explorer 5 或更高版本的應用程式會預期要從 Web 方法傳回的 XML 資料。

## <a name="working-with-complex-types"></a>使用複雜型別

列表 5 示範傳回名為客戶的 Web 服務的複雜類型的範例。 Customer 類別定義數個不同的簡單型別在內部會為屬性，例如 FirstName 和 LastName。 複雜型別做為輸入參數，或在啟用 AJAX 的 Web 方法的傳回型別會自動序列化為 JSON 再傳送至用戶端。 不過，巢狀複雜型別 （其定義在內部，另一個類型內） 不會提供給用戶端做為獨立物件的預設值。

在其中的 Web 服務所使用的巢狀複雜型別也必須使用用戶端頁面中的情況下，ASP.NET AJAX GenerateScriptType 屬性可以加入 Web 服務的影響。 例如，列表 9 所示的 CustomerDetails 類別包含位址和 Gender 屬性其中***代表巢狀複雜類型。***

**列表 9。如下所示的 CustomerDetails 類別包含兩個巢狀的複雜型別。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

列表 9 所示的 CustomerDetails 類別中定義的位址和性別物件將不會自動可供使用透過 JavaScript 用戶端因為它們是巢狀型別 （地址是一種類別和性別是列舉型別）。 在使用 Web Service 內的巢狀的類型必須是用於用戶端的情況下，先前所述 GenerateScriptType 屬性都可以使用 （請參閱 [列表 10]）。 這個屬性可以在其中從服務傳回不同的巢狀複雜類型的情況下多次加入。 直接到 Web 服務類別或更新特定的 Web 方法可以套用它。

**列表 10。您可以使用 GenerateScriptService 屬性來定義應可供用戶端的巢狀型別。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

藉由套用`GenerateScriptType`Web 服務、 地址和性別類型的屬性會自動可供使用由用戶端 ASP.NET AJAX JavaScript 程式碼。 JavaScript，會自動產生並傳送至用戶端上 Web 服務新增 GenerateScriptType 屬性的範例是以列表 11 所示。 您會看到如何使用巢狀複雜類型，在本文稍後。

**列表 11。提供給 ASP.NET AJAX 頁面的巢狀複雜類型。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

既然您已了解如何建立 Web 服務，並且讓它們存取 ASP.NET AJAX 頁面，讓我們看看如何建立和使用 JavaScript proxy，讓資料可以擷取或傳送至 Web 服務。

## <a name="creating-javascript-proxies"></a>建立 JavaScript Proxy

通常呼叫標準的 Web 服務 （.NET 或其他平台），牽涉到建立 proxy 物件，以保護您抵禦傳送 SOAP 要求和回應訊息的複雜性。 ASP.NET AJAX Web 服務呼叫，JavaScript proxy 可以建立和用來輕鬆地呼叫服務，而不需擔心序列化和還原序列化 JSON 訊息。 藉由使用 ASP.NET AJAX ScriptManager 控制項，可以自動產生 JavaScript proxy。

建立可呼叫 Web 服務的 JavaScript proxy 是透過 ScriptManager 的服務屬性來完成。 這個屬性可讓您定義一或多個 ASP.NET AJAX 頁面可以傳送或接收資料，而不需要回傳作業以非同步方式呼叫的服務。 定義服務使用 ASP.NET AJAX`ServiceReference`控制項，並將 Web 服務 URL 指派給控制項的`Path`屬性。 列表 12 顯示範例參考名為 CustomersService.asmx 服務。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**列表 12。定義 ASP.NET AJAX 頁面所使用之 Web 服務。**

將參考加入 ScriptManager 控制項透過 CustomersService.asmx 會導致以動態方式產生並由頁面所參考的 JavaScript proxy。 Proxy 會使用內嵌&lt;指令碼&gt;標記，並以動態方式載入呼叫 CustomersService.asmx 檔案，並附加 /js 字結尾。 下列範例會示範如何 JavaScript proxy 內嵌於頁面偵錯 web.config 中停用時：

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] 如果您想要查看實際 JavaScript proxy 程式碼，就會產生您可以在 Internet Explorer 的 [位址] 方塊中輸入所需的.NET Web 服務的 URL，並附加 /js 字結尾。*


如果已啟用偵錯 JavaScript proxy 的偵錯版本會內嵌在網頁中做的 web.config 中如下所示：

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

ScriptManager 所建立的 JavaScript proxy 也可以直接在網頁內嵌，而非使用參考&lt;指令碼&gt;標記的 src 屬性。 這可以由 ServiceReference 控制項的 InlineScript 屬性設定為 true （預設值為 false）。 當 proxy 不會共用跨多個頁面，以及當您想要減少對伺服器的網路呼叫的數目時，這非常有用。 當設定為 true 的 proxy 指令碼的 InlineScript 不會快取瀏覽器以便在 ASP.NET AJAX 應用程式中的多個頁面所使用的 proxy 的情況，建議的預設值為 false。 使用 InlineScript 屬性的範例會顯示下一步:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>使用 JavaScript Proxy

一旦 Web 服務頁面，參考了 ASP.NET AJAX 使用 ScriptManager 控制項，可以呼叫 Web 服務，並使用回呼函式就可以處理傳回的資料。 Web 服務會呼叫 （如果有的話） 參考其命名空間、 類別名稱和 Web 方法名稱。 可以定義傳遞至 Web 服務的任何參數，以及處理傳回的資料的回呼函式。

使用 JavaScript proxy 來呼叫名為 GetCustomersByCountry() Web 方法的範例是以列表 13 所示。 當使用者按一下網頁上的按鈕時，會呼叫 GetCustomersByCountry() 函式。

**列表 13。使用 JavaScript proxy 呼叫 Web 服務。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

這個呼叫會參照 InterfaceTraining 命名空間，在服務中定義的 CustomersService 類別和 GetCustomersByCountry Web 方法。 從 textbox 取得的國家/地區值，以及名為 OnWSRequestComplete 非同步 Web 服務呼叫傳回時應該叫用的回呼函式，會將它傳遞。 OnWSRequestComplete 會處理從服務傳回的客戶物件的陣列，並將它們轉換為顯示在頁面的資料表。 圖 1 顯示呼叫所產生的輸出。


[![取得 Web 服務來執行非同步 AJAX 呼叫的資料繫結。](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**圖 1**： 將資料繫結取得的 Web 服務來執行非同步 AJAX 呼叫。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-web-services/_static/image3.png))


JavaScript proxy 也可以在單向呼叫 Web 服務，應該呼叫 Web 方法，而 proxy 不應該等待回應的情況下。 例如，您可能要呼叫 Web 服務，以啟動處理程序，例如工作流程，但不是會等待來自服務的傳回值。 在單向呼叫要對服務的情況下，只是可以省略列表 13 所示的回呼函式。 因為沒有回呼函式定義 proxy 物件將不會等候 Web 服務傳回的資料。

## <a name="handling-errors"></a>處理錯誤

Web 服務的非同步回呼可能會遇到的不同類型的錯誤，例如網路故障，Web 服務無法使用或傳回例外狀況。 幸運的是，ScriptManager 所產生的 JavaScript proxy 物件可讓多個回呼，以便定義來處理錯誤和失敗除了稍早所示的成功回呼。 錯誤回呼函式可以定義 Web 方法的呼叫中的標準的回呼函式的後面，如 [列表 14] 所示。

**列表 14。定義的錯誤回呼函式，並顯示錯誤。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

呼叫 Web 服務時，發生任何錯誤會觸發 OnWSRequestFailed() 回呼函式呼叫可接受物件，代表做為參數的錯誤。 Error 物件會公開數個不同的函數，來判斷錯誤的原因，以及在呼叫已逾時。列表 14 顯示使用不同的錯誤函式的範例，以及 圖 2 顯示的函式所產生的輸出範例。


[![藉由呼叫 ASP.NET AJAX 錯誤函數所產生的輸出。](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**圖 2**： 藉由呼叫 ASP.NET AJAX 錯誤函數所產生的輸出。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>處理從 Web 服務傳回的 XML 資料

先前您已看到如何 Web 方法時，可以使用 ScriptMethod 屬性以及其 ResponseFormat 屬性傳回原始的 XML 資料。 當 ResponseFormat 設定為 ResponseFormat.Xml 時，從 Web 服務傳回的資料會序列化為 XML，而不是 JSON。 XML 資料需要直接傳遞給用戶端針對使用 JavaScript 或 XSLT 處理時，這非常有用。 在目前的階段，Internet Explorer 5 或更高版本會提供最佳的用戶端物件模型進行剖析和篩選 XML 資料，因為它對 MSXML 的內建支援。

擷取 XML 資料從 Web 服務是無不同之處擷取其他資料型別。 啟動叫用 JavaScript proxy 來呼叫適當的函式，並定義回呼函式。 一旦呼叫傳回之後您可以接著處理回呼函式中的資料。

列表 15 示範呼叫 Web 方法，名為 GetRssFeed() 會傳回 XmlElement 物件。 GetRssFeed() 接受單一參數表示之 rss 摘要擷取的 URL。

**列表 15。使用 Web 服務所傳回的 XML 資料。**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

此範例會傳遞 rss 摘要的 URL，並處理 OnWSRequestComplete() 函式傳回的 XML 資料。 OnWSRequestComplete() 會先檢查有瀏覽器知道 MSXML 剖析器可以使用 Internet Explorer。 如果是，XPath 陳述式用來尋找所有&lt;項目&gt;RSS 摘要中的標記。 然後，會逐一查看每個項目和相關聯&lt;標題&gt;並&lt;連結&gt;標記會位於和處理，以顯示每個項目資料。 圖 3 顯示進行透過 GetRssFeed() Web 方法的 JavaScript proxy 呼叫 ASP.NET AJAX 所產生的輸出範例。

## <a name="handling-complex-types"></a>處理複雜的類型

接受或 Web 服務所傳回的複雜型別會自動公開透過 JavaScript proxy。 不過，巢狀複雜類型不是直接存取用戶端上除非 GenerateScriptType 屬性套用至服務稍早所述。 您為什麼要使用巢狀的複雜型別用戶端上？

為了回答這個問題，假設 ASP.NET AJAX 頁面會顯示客戶資料，並可讓使用者更新客戶地址。 如果 Web 服務可讓您指定位址類型 （CustomerDetails 類別內定義的複雜型別） 都可以傳送至用戶端的更新程序可以分割成個別的函式重複使用更好的程式碼。


[![從呼叫傳回 RSS 資料的 Web 服務建立的輸出。](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**圖 3**： 輸出從呼叫傳回 RSS 資料的 Web 服務建立。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-web-services/_static/image9.png))


列表 16 顯示叫用位址物件模型命名空間中定義的用戶端程式碼範例填入已更新的資料，並將它指派給 CustomerDetails 物件的位址屬性。 然後 CustomerDetails 物件會傳遞到 Web 服務以進行處理。

**列表 16。使用巢狀複雜類型**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>建立和使用頁面方法

Web 服務提供重複使用的服務，以各種不同的用戶端，包括 ASP.NET AJAX 頁面公開 （expose） 的絕佳方式。 不過，可能的需要擷取以往無法使用或由其他頁面共用的資料頁面。 在此情況下，進行以允許存取的資料頁的.asmx 檔案看起來好像大材小用因為服務只使用單一頁面。

ASP.NET AJAX 提供另一個機制來進行 Web 服務式的呼叫，而不需建立獨立的.asmx 檔案。 這是由使用稱為 「 頁面方法 」 的技術。 頁面方法是直接在網頁或程式碼除外檔案中內嵌的 WebMethod 屬性套用至它們靜態 (在中共用 VB.NET) 方法。 藉由套用 WebMethod 屬性它們可以呼叫使用名為 PageMethods 在執行階段動態建立的特殊 JavaScript 物件。 PageMethods 物件做為保護您抵禦 JSON 序列化/還原序列化程序的 proxy。 請注意，若要使用 PageMethods 物件，您必須設定 ScriptManager 的 EnablePageMethods 屬性設為 true。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

列表 17 顯示在 ASP.NET 之外的程式碼的類別中定義兩個頁面方法的範例。 這些方法會擷取位於應用程式中的商務層類別中的資料\_網站的程式碼資料夾。

**列表 17。定義頁面方法。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

當 ScriptManager 偵測到頁面中的 Web 方法的存在就會產生先前所述的 PageMethods 物件的動態參考。 呼叫 Web 方法是藉由參考後面接著名稱的方法，並應傳遞任何必要的參數資料的 PageMethods 類別來完成。 列表 18 顯示呼叫兩個稍早所示的頁面方法的範例。

**列出 18。呼叫頁面方法與 PageMethods JavaScript 物件。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

使用 PageMethods 物件是非常類似於使用的 JavaScript proxy 物件。 您第一次指定所有應該傳遞至頁面方法，然後定義 非同步呼叫傳回時，應該呼叫在回呼函式的參數資料。 您也可以指定失敗回呼 （請參閱列表 14 處理失敗的範例）。

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender 和 ASP.NET AJAX Toolkit

ASP.NET AJAX Toolkit (可從[ http://ajax.asp.net ](http://ajax.asp.net)) 提供數個控制項，可用來存取 Web 服務。 具體來說，此工具組包含實用的控制項，名為`AutoCompleteExtender`，可用來呼叫 Web 服務，並顯示在頁面中的資料，而不需要撰寫任何 JavaScript 程式碼完全。

AutoCompleteExtender 控制項可用來擴充現有功能的 textbox，並協助使用者更輕鬆地找到他們要尋找的資料。 當他們輸入到文字方塊控制項可用來查詢 Web 服務，並動態顯示文字方塊下方的結果。 [圖 4] 顯示使用 AutoCompleteExtender 控制項以顯示支援應用程式的客戶識別碼的範例。 當使用者在文字方塊輸入不同的字元，就會顯示不同的項目根據其輸入其下。 使用者接著可以選取所需的客戶識別碼。

使用 ASP.NET AJAX 頁面內 AutoCompleteExtender 需要 AjaxControlToolkit.dll 組件會加入至網站的 bin 資料夾。 一旦新增工具組的組件之後，您會想要使它所包含的控制項可用於應用程式中的所有頁面在 web.config 中參考它。 這可以藉由新增下列標記中 web.config 的完成&lt;控制項&gt;標記：

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

在您只需要使用特定的頁面中的控制項的情況下您可以將它參考將參考指示詞加入至頁面頂端，而不是更新 web.config，如下所示：

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![使用 AutoCompleteExtender 控制項。](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**圖 4**： 使用 AutoCompleteExtender 控制項。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-web-services/_static/image12.png))


一旦網站已設定為使用 ASP.NET AJAX Toolkit，AutoCompleteExtender 控制項可以加入至頁面更像加入規則的 ASP.NET 伺服器控制項。 列表 19 顯示使用控制項來呼叫 Web 服務的範例。

**列出 19。使用 ASP.NET AJAX Toolkit AutoCompleteExtender 控制項。**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender 有數個不同的屬性，包括伺服器控制項上找到的標準識別碼與 runat 屬性。 除了這些之外，它可讓您定義使用者類型，Web 服務之前會查詢資料的字元數。 列出 19 所示的 MinimumPrefixLength 屬性會導致服務呼叫每次在文字方塊中輸入一個字元。 您會想要小心設定此值，因為每次使用者輸入的字元 Web 服務會呼叫以搜尋符合的字元，在文字方塊中的值。 要呼叫的 Web 服務，以及目標 Web 方法，會定義分別使用 ServicePath] 和 [ServiceMethod 屬性。 最後，TargetControlID 屬性會識別哪一個連結 AutoCompleteExtender 控制項的文字方塊。

被呼叫的 Web 服務必須要有套用稍早所述的 ScriptService 屬性和目標的 Web 方法必須接受兩個名為 prefixText 和計數的參數。 PrefixText 參數代表終端使用者所輸入的字元，而計數參數代表要傳回多少項目 （預設值為 10）。 列表 20 顯示 GetCustomerIDs Web 方法呼叫由 AutoCompleteExtender 控制項列出 19 稍早所示的範例。 Web 方法呼叫的商務層方法，接著呼叫資料層方法會處理篩選的資料，並傳回相符的結果。 資料層方法的程式碼是由列出 21 所示。

**列出 20。從 AutoCompleteExtender 控制項傳送的資料進行篩選。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**列出 21。篩選根據使用者輸入的結果。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>結論

ASP.NET AJAX 會提供絕佳的支援，而不需要撰寫大量自訂的 JavaScript 程式碼來處理要求和回應訊息中呼叫 Web 服務。 這篇文章中您已了解如何啟用 AJAX 功能.NET Web 服務將讓他們能夠處理 JSON 訊息，以及如何定義使用 ScriptManager 控制項的 JavaScript proxy。 您已了解的 JavaScript proxy 可以用來呼叫 Web 服務，如何處理簡單和複雜型別和處理失敗。 最後，您已了解如何使用頁面方法，來簡化建立，並讓 Web 服務呼叫的程序和如何 AutoCompleteExtender 控制項可以提供幫助使用者當他們輸入。 雖然可在 ASP.NET AJAX UpdatePanel 肯定會基於其單純性許多 AJAX 程式設計人員所選擇的控制項，了解如何透過 JavaScript proxy 呼叫 Web 服務可用於許多應用程式。

## <a name="bio"></a>簡歷

Dan Wahlin （Microsoft 最有價值專家適用於 ASP.NET 和 XML Web Service） 是介面的技術訓練的.NET 開發的講師和架構顧問 ([http://www.interfacett.com](http://www.interfacett.com))。 Dan 建構的 XML for ASP.NET 開發人員網站 ([www.XMLforASP.NET](http://www.XMLforASP.NET)) 上的 INETA 講師機構，在數個會議上發表演說。 Dan 合著 Professional Windows DNA (Wrox)，ASP.NET： 秘訣、 教學課程和程式碼 (Sam)、 ASP.NET 1.1 Insider 解決方案、 Professional ASP.NET 2.0 AJAX (Wrox)、 ASP.NET 2.0 MVP Hacks 和 ASP.NET 開發人員 (Sam) 撰寫的 XML。 當他不撰寫程式碼、 文章或書籍時，Dan 工作之餘喜歡撰寫和錄製音樂和播放高爾夫球和他的妻子和孩子的籃球。

Scott Cate 從事 Microsoft Web 技術工作自 1997 年，而且 myKB.com 總裁 ([www.myKB.com](http://www.myKB.com))，專精於撰寫 ASP.NET 架構著重於知識庫軟體解決方案的應用程式。 可以透過電子郵件連絡 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [上一頁](understanding-asp-net-ajax-localization.md)
> [下一頁](understanding-asp-net-ajax-debugging-capabilities.md)
