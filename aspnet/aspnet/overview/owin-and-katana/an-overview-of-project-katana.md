---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: 專案 Katana 概觀 |Microsoft 文件
author: howarddierking
description: ASP.NET 架構之已超過十年，並在平台啟用無數網站和服務的開發。 為 Web 應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 3c2bcbbc6e506af759f6d77af17d015278cc0bdf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878759"
---
<a name="an-overview-of-project-katana"></a>專案 Katana 的概觀
====================
由[Howard Dierking](https://github.com/howarddierking)

> ASP.NET 架構之已超過十年，並在平台啟用無數網站和服務的開發。 Web 應用程式開發策略有發展，因為架構已經能夠持續發展與 ASP.NET MVC 和 ASP.NET Web API 等技術的步驟。 做 Web 應用程式開發雲端運算世界演化其下一個步驟，專案[Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)提供基礎的元件，以 ASP.NET 應用程式，讓他們能夠為彈性、 可攜性，輕量型的並提供更佳的效能-專案、 將另一種方式[Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)雲端最佳化您的 ASP.NET 應用程式。


## <a name="why-katana--why-now"></a>為什麼 Katana – 為何現在嗎？

 不論是否其中一個討論開發人員 framework 或使用者產品，務必了解建立基礎的升級過程中的與產品包含了解產品所建立。 請注意兩個客戶最初建立 ASP.NET。   
  
**第一個群組的客戶是傳統 ASP 開發人員。** 同時，ASP 就是一種主要的技術，用於建立網站和應用程式動態的資料導向 interweaving 標記和伺服器端指令碼。 ASP 執行階段提供抽象的基礎 HTTP 通訊協定和 Web 伺服器的核心層面，並且提供存取至其他服務這類工作階段和應用程式的狀態管理，快取，等等之物件的一組伺服器端指令碼。雖然強大、 傳統 ASP 應用程式變得容易管理，因為它們成長的大小和複雜度。 這是結構的主要是結構的因為缺少的指令碼結合所產生的程式碼和標記的交錯情形的程式碼的重複的環境中找到。 為了處理某些其挑戰時利用傳統 ASP 的優勢，ASP.NET 利用語言提供的物件導向的.NET framework 同時也保留的伺服器端程式設計模型的程式碼組織哪些傳統 asp 開發人員已成長習慣。

**適用於 ASP.NET 的目標客戶的第二個群組是 Windows 商務應用程式開發人員。** 不同於傳統 ASP 開發人員已習慣寫入 HTML 標記和程式碼，以產生更多的 HTML 標記，WinForms （如同之前 VB6 開發人員） 的開發人員已習慣於設計階段體驗包含畫布與一組豐富的使用者控制項的介面。 第一個版本的 ASP.NET – 也稱為 「 Web 表單 」 提供類似的設計階段經驗以及使用者介面元件的伺服器端事件模型和基礎結構功能 （例如 ViewState) 的一組以建立無縫式的開發人員體驗用戶端和伺服器端程式設計之間 Web Form 有效 hid Web 的無狀態的本質，在已熟悉 WinForms 開發人員可設定狀態的事件模型。

### <a name="challenges-raised-by-the-historical-model"></a>歷程記錄模型所引發的挑戰

**最後的結果是成熟、 具豐富功能的執行階段和開發人員的程式設計模型。** 不過，與功能豐富的來源值得注意的一些挑戰。 首先，架構已**龐大**，與不同邏輯單位所緊密結合在相同的 System.Web.dll 組件 （例如 Web form 架構的核心 HTTP 物件） 的功能。 其次，ASP.NET 隨附較大的.NET Framework，這表示，它的一部分**版本之間的時間為順序年而定。** 這並不容易，ASP.NET 跟上的所有變更中快速發展 Web 程式開發的情況。 最後，System.Web.dll 本身已結合至特定的 Web 裝載選項幾個不同的方式： 網際網路資訊服務 (IIS)。

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>演化的步驟執行： ASP.NET MVC 與 ASP.NET Web API

與 Web 程式開發中發生大量變更 ！ Web 應用程式已逐漸正在所開發的一系列的小型，已取得焦點的元件，而不是大型的架構。 元件，以及與所發行的頻率數目已增加速度過快。 很顯然與 Web 步調保留到需要架構，因此更大且更具豐富功能，而取得較小、 低耦合且更受關注**ASP.NET 小組花費幾個演化的步驟，以啟用 ASP.NET 以一系列隨插即用的 Web 元件，而不是單一架構**。

其中一個最早變更是搭配滑軌安裝在 Web 開發架構，例如 Ruby 由於已知的模型檢視控制器 (MVC) 設計模式的普及率升高。 這種樣式的建置 Web 應用程式提供開發人員應用程式的標記，同時仍然保留的標記和商務邏輯，這是一個適用於 ASP.NET 的初始賣點分隔較大的控制權。 為了要符合需求的這種樣式的 Web 應用程式開發，Microsoft 花費得以將其自身更適合未來， **out of band 開發 ASP.NET MVC** （並不包括.NET Framework 中）。 ASP.NET MVC 已發行以獨立下載項目。 這會讓工程團隊的彈性來傳遞更新，便是先前可能比更頻繁。

Web 應用程式開發中的另一個主要 shift 已從動態的伺服器產生的網頁 shift 鍵，以使用動態產生通訊的用戶端指令碼頁面區段的靜態初始標記**與後端 Web 應用程式開發介面透過AJAX 要求**。 架構的轉換有助於駭人的 Web Api 和 ASP.NET Web API framework 的開發。 如果 ASP.NET MVC 是 ASP.NET Web API 的版本會提供另一個有機會進一步做更模組化架構發展 ASP.NET。 工程團隊已利用機會和**建置 ASP.NET Web API 時，它有沒有相依性 System.Web.dll 中找到的核心架構型別的任何**。 此功能啟用兩件事： 首先，它表示 ASP.NET Web API 可能完全獨立的方式發展 （和快速重複，因為它透過 NuGet 傳遞時，它可以繼續）。 第二，因為有個 System.Web.dll，沒有外部相依性，因此，沒有任何相依性到 IIS，ASP.NET Web API 包含自訂的主控件 （例如主控台應用程式、 Windows 服務等） 中執行的功能

### <a name="the-future-a-nimble-framework"></a>未來： 靈活架構

分離互相 framework 元件並再釋放它們需由 NuGet，架構無法立即**更獨立且更快速地逐一查看**。 此外的功能和彈性的 Web API 的自我裝載功能證實深具吸引力的開發人員想**小型、 輕量級主機**其服務。 已證實該吸引，事實上，其他架構也會想要這項功能，而且這提出新的挑戰，也就是說，每個架構已在它自己的主控件程序中執行它自己的基底位址上，並且需要管理 （啟動、 停止等） 獨立。 現代的 Web 應用程式通常會支援靜態檔案服務、 產生動態頁面、 Web API 和更最近即時-單次/推播通知。 每個服務，應執行並分開管理，必須要有不只是實際。

情況所需的單一裝載抽象的會讓開發人員撰寫的應用程式，從各種不同的元件和架構，然後執行該應用程式支援的主機上。

## <a name="the-open-web-interface-for-net-owin"></a>開啟 Web 介面 for.NET (OWIN)

 啟發的優點達成[機架](http://rack.github.io/)拼音社群中，.NET 社群的數個成員設定來建立網頁伺服器和 framework 元件之間的抽象概念。 OWIN 抽象的兩個設計目標是很簡單，與所花費的最少的可能相依性的其他 framework 型別。 這兩個目標協助確保：

- 新的元件可以更輕鬆地開發並取用。
- 應用程式可以更輕鬆地移植主機與潛在整個平台/作業系統之間。

產生的抽象概念是由兩個核心項目所組成。 第一個是環境目錄。 這個資料結構會負責處理 HTTP 要求和回應，以及任何相關伺服器狀態所需的狀態的所有儲存。 環境字典定義，如下所示：

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

OWIN 相容的網頁伺服器會負責填入資料的內文資料流和 HTTP 要求和回應標頭集合如環境字典。 然後會擴展或更新的字典，包含額外的值，並寫入回應主體資料流的應用程式或架構元件的責任。

除了指定環境目錄的類型，OWIN 規格定義核心字典的索引鍵值組的清單。 例如下, 表顯示 HTTP 要求所需的字典索引鍵：

| 索引鍵名稱 | 值描述 |
| --- | --- |
| `"owin.RequestBody"` | 包含要求主體中，如果有任何資料流。 Stream.Null 可能做為預留位置，如果沒有要求主體。 請參閱[要求本文](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics)。 |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>`要求標頭。 請參閱[標頭](http://owin.org/html/owin.html#3-3-headers)。 |
| `"owin.RequestMethod"` | A`string`包含要求的 HTTP 要求方法 (例如`"GET"`， `"POST"`)。 |
| `"owin.RequestPath"` | A`string`包含要求路徑。 路徑必須相對於 「 根 」 的應用程式委派。請參閱[路徑](http://owin.org/html/owin.html#5-3-paths)。 |
| `"owin.RequestPathBase"` | A`string`包含對應的應用程式委派; 「 根 」 的要求路徑的一部分，請參閱[路徑](http://owin.org/html/owin.html#5-3-paths)。 |
| `"owin.RequestProtocol"` | A`string`包含通訊協定名稱和版本 (例如`"HTTP/1.0"`或`"HTTP/1.1"`)。 |
| `"owin.RequestQueryString"` | A`string`包含查詢字串元件 HTTP 要求的 URI，而不前置的"？"(例如`"foo=bar&baz=quux"`)。 值可能是空字串。 |
| `"owin.RequestScheme"` | A`string`包含用於要求的 URI 配置 (例如`"http"`， `"https"`); 請參閱[URI 配置](http://owin.org/html/owin.html#5-1-uri-scheme)。 |

OWIN 的第二個項目是應用程式的委派。 這是函式簽章會做為 OWIN 應用程式中的所有元件之間的主要介面。 應用程式委派的定義如下所示：

`Func<IDictionary<string, object>, Task>;`

應用程式委派則只是其中函式接受環境目錄，做為輸入，並傳回工作的 Func 委派類型的實作。 此設計有適用於開發人員的幾個含意：

- 有極少數的類型相依性才能撰寫 OWIN 元件。 這樣可大幅增加 OWIN 開發人員的存取範圍。
- 非同步設計可讓您能夠有效率地使用它的運算資源，特別是在多個 I/O 密集作業的處理資料的抽象。
- 因為應用程式委派是不可部分完成的執行單位，而且環境目錄做為參數的委派上執行，因為 OWIN 元件可以輕鬆地鏈結在一起以建立複雜的 HTTP 處理管線。

從實作觀點來看，OWIN 是一種規格 ([http://owin.org/html/owin.html](http://owin.org/html/owin.html))。 其目的是為下一個 Web 架構，但規格而不是 Web 架構和 Web 伺服器互動的方式。

如果您已經調查[OWIN](http://owin.org/)或[Katana](https://github.com/aspnet/AspNetKatana/wiki)，您可能也會注意到[Owin NuGet 封裝](http://nuget.org/packages/Owin)和 Owin.dll。 此程式庫包含單一介面， [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)，」 會正式制定以及制定中所述的啟動順序[區段 4](http://owin.org/html/owin.html#4-application-startup)的 OWIN 規格。 而不需以組建 OWIN 伺服器[IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)介面提供具體的參考點，且 Katana 專案元件會使用它。

## <a name="project-katana"></a>專案 Katana

而同時[OWIN](http://owin.org/html/owin.html)規格和*Owin.dll*為社群所擁有和社群執行開放原始碼努力[Katana](https://github.com/aspnet/AspNetKatana/wiki)專案表示 OWIN 集合針對，同時仍開放原始碼，建立與 Microsoft 所發行的元件。 這些元件可包含基礎結構元件，例如主機和伺服器，以及功能的元件，例如驗證元件和架構繫結，例如[SignalR](../../../signalr/index.md)和[ASP.NET 網頁應用程式開發介面](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md)。 專案具有下列三個高階的目標： 

- **可攜式**– 元件應該要能夠輕鬆地取代成新的元件可以使用。 這包括所有類型的元件，從伺服器和主機的架構。 此目標的是，第三方架構可以順暢地執行 Microsoft 伺服器上時 Microsoft 架構可能會執行協力廠商伺服器與主機上。
- **模組化/彈性**– Katana 專案元件不同於許多架構包括各種功能，預設會開啟，應該是小型和焦點，提供控制應用程式開發人員決定要哪些元件在應用程式中使用。
- **輕量型/高效能/可擴充**– 傳統架構的概念分成小型、 一組元件會明確新增應用程式，開發人員所產生的 Katana 應用程式可以使用較少的電腦已取得焦點資源，如此一來，比處理更多的負載，與其他類型的伺服器和架構。 如需求的應用程式需要更多的功能，從基礎結構，那些可以新增至 OWIN 管線中，但那應該是部分應用程式開發人員明確決策。 此外，較低層級元件的替代表示，當它們變成可用，新的高效能伺服器可以順暢地導入以改善 OWIN 應用程式的效能，而不會中斷這些應用程式。

## <a name="getting-started-with-katana-components"></a>開始使用 Katana 元件

第一次引進，某層面的[Node.js](http://nodejs.org/)立即吸引人的注意的架構是的簡化的其中一個無法撰寫及執行 Web 伺服器。 如果 Katana 目標所包覆 light 的[Node.js](http://nodejs.org/)，其中可能會依所說 Katana 帶來許多好處進行摘要[Node.js](http://nodejs.org/) （及與它類似的架構） 而不未強制開發人員擲出她知道開發 ASP.NET Web 應用程式相關的一切。 此陳述式來保存，則為 true，開始使用 Katana 專案應該同樣簡單性質，為了[Node.js](http://nodejs.org/)。

## <a name="creating-hello-world"></a>建立"Hello World ！"

JavaScript 和.NET 開發之間的明顯差異是編譯器的目前狀態 （或不存在）。 因此，簡單 Katana 伺服器的起始點是 Visual Studio 專案。 不過，我們可以開始使用最小的專案類型： 空的 ASP.NET Web 應用程式。

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

接下來，我們將會安裝[Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)專案將 NuGet 封裝。 此封裝提供在 ASP.NET 要求管線中執行的 OWIN 伺服器。 您可以找到上[NuGet gallery](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) ，可以使用 Visual Studio 封裝管理員 對話方塊或 [封裝管理員] 主控台使用下列命令安裝：

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

安裝`Microsoft.Owin.Host.SystemWeb`封裝將會安裝為相依性的幾個額外的封裝。 這些相依性的其中一個是`Microsoft.Owin`，程式庫，並且提供數個協助程式類型和方法來開發 OWIN 應用程式。 若要快速地撰寫下列"hello world"伺服器，我們可以使用這些型別。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

這個非常簡單的 Web 伺服器現在可以執行使用 Visual Studio **F5**命令，並包含偵錯的完整支援。

## <a name="switching-hosts"></a>切換主機

根據預設，上述的"hello world"範例執行 IIS 的內容中使用 System.Web ASP.NET 要求管線中。 這可以單獨使用時新增極大的值因為它可讓我們受益於彈性與撰寫性 OWIN 管線的管理功能和整體成熟度的 IIS。 不過，可能不需要 IIS 的好處，而且想要為較小、 更精簡的主機。 所需，然後執行 IIS 和 System.Web 之外，我們簡單的 Web 伺服器？

為了說明的可攜性的目標，從命令列的主應用程式的 Web 伺服器主機移動需要直接將新的伺服器和主機相依性加入至專案的輸出資料夾，然後啟動主機。 在此範例中，我們將會裝載我們稱為 Katana 主應用程式中的 Web 伺服器`OwinHost.exe`且將會使用 Katana HttpListener 架構的伺服器。 同樣地其他 Katana 元件，這些都能取得從 NuGet 使用下列命令：

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

我們可以從命令列，然後瀏覽至專案根資料夾，並只需執行`OwinHost.exe`（其已安裝在其個別的 NuGet 套件的 [工具] 資料夾）。 根據預設，`OwinHost.exe`設定要尋找的 HttpListener 伺服器，因此不需要任何額外的設定。 在網頁瀏覽器中瀏覽`http://localhost:5000/`顯示現在透過主控台執行的應用程式。

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana 架構

 Katana 元件架構將分割成四個邏輯層，應用程式，如底下所述：*主機、 伺服器、 中介軟體，* 和*應用程式*。 元件架構被考量的方式，這些層級實作可以輕鬆地取代，在許多情況下，而不需要重新編譯的應用程式。   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>主機

 主機負責：

- 管理基礎程序。
- 會處理選取的伺服器，以及哪些要求透過 OWIN 管線建構會導致工作流程整合，以完成。

  目前，有 3 Katana 基礎的應用程式的主要裝載選項：  
  
**ASP.NET**： 使用標準的 HttpModule 和 HttpHandler 型別，OWIN 管線可以在 IIS 上執行的 ASP.NET 要求流程的一部分。 Microsoft.AspNet.Host.SystemWeb NuGet 封裝安裝到 Web 應用程式專案時，會啟用 ASP.NET 裝載支援。 此外，因為 IIS 做為主機和伺服器，OWIN 伺服器/主控件區別 」 混為一談是在此 NuGet 封裝中，這表示如果使用的 SystemWeb 主機，開發人員無法替換替代伺服器實作。  
  
**自訂主機**: Katana 元件套件可讓開發人員主應用程式在自己的自訂程序，不論是主控台應用程式、 Windows 服務和其他內容。這項功能提供 Web API 的自我裝載功能類似。 下列範例會示範自訂主機的 Web API 的程式碼：  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

自我裝載 Katana 應用程式安裝程式會類似：

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

之間的 Web API 和 Katana 自我裝載範例有一個值得注意的差異是缺少 Katana 自我裝載範例的 Web API 組態程式碼。 若要啟用可攜性和撰寫，Katana 分隔啟動伺服器設定要求處理管線的程式碼的程式碼。 設定 Web API 的程式碼然後包含在啟動時，此外 WebApplication.Start 中的類型參數指定的類別。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

啟動類別將在本文稍後的更詳細地討論。 不過，需要啟動 Katana 自我裝載的處理程序看起來極為類似您在 ASP.NET Web API 的自我裝載應用程式可能現今使用的程式碼的程式碼。

**OwinHost.exe**： 許多部分會想要撰寫自訂的程序，來執行 Katana Web 應用程式，而希望只要啟動預先建置的可執行檔可以啟動伺服器，並執行其應用程式。 針對此案例，包括 Katana 元件套件`OwinHost.exe`。 當從專案的根目錄內執行，此可執行檔將會啟動 （它預設會使用 HttpListener 伺服器） 的伺服器，並使用慣例來尋找並執行使用者啟動的類別。 對於更細微的控制，可執行檔會提供幾個其他命令列參數。

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>伺服器

 主機負責啟動和維護應用程式執行所在，伺服器的責任的程序時若要開啟網路通訊端，接聽要求，透過 OWIN 元件的管線傳送它們所指定 （做為您的使用者嗎可能已注意到，此管線在應用程式開發人員的啟動類別中指定）。 目前，Katana 專案包含兩個伺服器實作： 

- **Microsoft.Owin.Host.SystemWeb**： 如先前所述，IIS 與 ASP.NET 管線搭配做為主機和伺服器。 因此，當選擇這個裝載選項，IIS 管理主機層級的考量，如處理序啟用和接聽 HTTP 要求。 ASP.NET Web 應用程式，並再將要求傳送至 ASP.NET 管線。 Katana SystemWeb 主機登錄 ASP.NET HttpModule HttpHandler 攔截要求，因為它們透過 HTTP 管線流動，並將它們傳送到指定使用者的 OWIN 管線。
- **Microsoft.Owin.Host.HttpListener**： 如其名稱所指示，此 Katana 伺服器會使用.NET Framework 的 HttpListener 類別若要開啟通訊端，並將要求傳送至開發人員指定的 OWIN 管線。 這是目前 Katana 自我裝載的 API 和 OwinHost.exe 預設伺服器選取項目。

## <a name="middlewareframework"></a>中介軟體/架構

 如先前所述，當伺服器接受的要求從用戶端，它會負責通過管線，開發人員的啟始程式碼所指定的 OWIN 元件。 這些管線元件稱為中介軟體。  
 在非常基本的層級的 OWIN 中介軟體元件只需要使其可呼叫實作 OWIN 應用程式委派。

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

不過，為了簡化開發和中介軟體元件的組合，Katana 會支援中介軟體元件的少數幾個慣例和協助程式類型。 最常見的這些是`OwinMiddleware`類別。 使用這個類別來建置自訂的中介軟體元件看起來會如下所示： 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 此類別衍生自`OwinMiddleware`，實作做為其引數，其中接受管線中的下一個中介軟體的執行個體，然後將它傳遞給基底建構函式的建構函式。 之後的下一個中介軟體參數，用來設定中介軟體的其他引數也會宣告為建構函式參數。   
  
中介軟體會執行在執行階段，透過覆寫`Invoke`方法。 這個方法會採用單一引數的型別`OwinContext`。 這個內容物件由`Microsoft.Owin`NuGet 封裝稍早所述，並提供要求、 回應和環境的字典，以及一些額外的 helper 類型的強型別存取。   
  
中介軟體類別可以輕鬆地加入至 OWIN 管線中的應用程式啟動程式碼，如下所示：   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

由於 Katana 基礎結構只建立管線的 OWIN 中介軟體元件，而且元件只需要以支援參與管線的應用程式委派中, 介軟體元件的範圍可以從簡單的複雜性記錄器將整個架構，像 ASP.NET，Web API 或[SignalR](../../../signalr/index.md)。 例如，將 ASP.NET Web API 新增至先前的 OWIN 管線需要加入下列啟動程式碼：

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Katana 基礎結構會建置到設定方法的 IAppBuilder 物件已加入的順序為基礎的中介軟體元件的管線。 然後，在本例中，LoggerMiddleware 可以處理流程會通過管線，無論最終如何處理這些要求的所有要求。 這可讓在的中介軟體元件 （例如驗證元件） 可以處理包含多個元件和架構 （例如 ASP.NET Web API、 SignalR 和靜態檔案伺服器） 的管線要求的強大案例。
 
## <a name="applications"></a>應用程式

如先前範例所示，OWIN 和 Katana 專案應該不被視為新應用程式的程式設計模型，但而不是以分離應用程式撰寫模型和伺服器與裝載基礎結構架構的抽象概念。 比方說，當建置 Web API 應用程式，開發人員架構會繼續使用 ASP.NET Web API framework，無論使用元件從 Katana 專案 OWIN 管線中執行應用程式中。 會看到應用程式開發人員 OWIN 相關的程式碼的位置將應用程式啟動程式碼中，開發人員撰寫 OWIN 管線的位置。 在啟動程式碼中，開發人員會登錄一系列的 UseXx 陳述式，通常是一個針對每個中介軟體元件會處理傳入的要求。 這項體驗將會有註冊目前 System.Web 世界中的 HTTP 模組相同的效果。 一般而言，較大架構中的介軟體，例如 ASP.NET Web API 或[SignalR](../../../signalr/index.md)會註冊在管線結尾處。 跨領域中介軟體元件，例如驗證或快取中，通常註冊朝向管線的開頭，使它們將會處理所有架構及稍後在管線中的已註冊元件的要求。 這項分隔的中介軟體元件彼此，並從基本的基礎結構元件可讓在不同時發展，同時確保整體系統會保持穩定的元件。

## <a name="components--nuget-packages"></a>元件 – NuGet 封裝

如同許多的目前媒體櫃和架構，Katana 專案元件都會以一組 NuGet 封裝來提供。 如需近期版本 2.0，Katana 封裝相依性圖形看起來如下。 （按一下放大的影像）。

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Katana 專案中的幾乎每個封裝而定，直接或間接地 Owin 封裝。 您可能記得這是包含提供 OWIN 規格的第 4 節中所述的應用程式啟動順序的具象實作的 IAppBuilder 介面的套件。 此外，許多封裝相依 Microsoft.Owin，提供一組協助程式類型使用的 HTTP 要求和回應。 封裝的其餘部分可以歸類為裝載基礎結構的封裝 （伺服器或主機） 或中介軟體。 封裝和 Katana 專案外部的相依性會顯示為橙色。

Katana 2.0 裝載基礎結構包括 SystemWeb 與 HttpListener 為基礎的伺服器、 執行 OWIN 應用程式使用 OwinHost.exe，OwinHost 封裝及 Microsoft.Owin.Hosting 套件的是自我裝載中的 OWIN 應用程式自訂主應用程式 （例如主控台應用程式、 Windows 服務等）

Katana 2.0 中介軟體元件主要焦點放在提供不同的驗證方式。 提供一個診斷的其他中介軟體元件，可讓開始與錯誤頁面的支援。 OWIN 隨著成既定的裝載抽象概念中, 介軟體元件，這兩個那些由 Microsoft 和協力廠商所開發的生態系統也會成長數字。

## <a name="conclusion"></a>結論

 從開始，Katana 專案的目標尚未建立，並藉此強制開發人員若要了解另一個 Web 架構。 相反地，其目標是要建立的抽象概念，讓.NET Web 應用程式開發人員選項比先前一直。 透過邏輯層級中的典型的 Web 應用程式堆疊成一組可取代的元件，Katana 專案可讓元件在整個堆疊在任何速率最適合這些元件改善。 藉由建置簡單的 OWIN 抽象周圍的所有元件，Katana 可讓架構建置的應用程式在其上能夠移植到各種不同的伺服器與主機不同。 藉由將開發人員放入堆疊的控制項，Katana 可確保，開發人員會使最終選擇如何輕量型或她 Web 堆疊應該是如何豐富的功能。  
  

## <a name="for-more-information-about-katana"></a>如需有關 Katana

- GitHub 上的 Katana 專案： [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/)。
- 影片： [Katana 專案-適用於 ASP.NET 的 OWIN](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)，由 Howard Dierking。

## <a name="acknowledgements"></a>謝誌

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick 是將焦點放在 Azure 和 MVC microsoft 資深程式寫入器。
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
