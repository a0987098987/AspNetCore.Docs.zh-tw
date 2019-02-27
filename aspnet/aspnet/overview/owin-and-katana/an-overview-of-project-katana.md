---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Katana 專案概觀 |Microsoft Docs
author: howarddierking
description: ASP.NET Framework 已存在超過十年，且平台已啟用了無數的網站和服務。 為 Web 應用程式...
ms.author: riande
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 52007eba109de28c6d178505b82b1d5ff2883b47
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823597"
---
<a name="an-overview-of-project-katana"></a>Katana 專案概觀
====================
藉由[Howard Dierking](https://github.com/howarddierking)

> ASP.NET Framework 已存在超過十年，且平台已啟用了無數的網站和服務。 進化的 Web 應用程式開發策略，因為架構已經能夠發展在 ASP.NET MVC 和 ASP.NET Web API 等技術的步驟。 開發 Web 應用程式會將其下一個進化步驟納入雲端運算的世界，因為專案[Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)提供一組基礎的 ASP.NET 應用程式，可靈活、 可攜性，讓他們的元件輕量級，並提供更佳的效能 – 將另一種方法，專案[Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)雲端最佳化您的 ASP.NET 應用程式。


## <a name="why-katana--why-now"></a>為什麼 Katana – 為什麼現在嗎？

 不論是否其中會討論開發人員 framework 或使用者的產品，請務必了解基礎有關為何要建立的產品 – 和這個過程包含了解針對所建立的產品。 ASP.NET 原本是以兩個客戶的需求而建立。   
  
**第一個群組的客戶是傳統的 ASP 開發人員。** 同時，ASP 已由 interweaving 標記和伺服器端指令碼中建立動態、 資料導向網站和應用程式的主要技術之一。 ASP 執行階段會提供一組物件抽象化基礎 HTTP 通訊協定和 Web 伺服器的核心層面，並存取其他服務這類工作階段和應用程式的狀態管理，提供快取等伺服器端指令碼。雖然功能強大，傳統的 ASP 應用程式就會成為管理它們成長的大小和複雜度的一項挑戰。 這是結構的主要是結構的因為缺乏指令碼結合所產生的程式碼和標記的交錯情形的程式碼的重複的環境中找到。 為了充分利用的傳統 ASP 功能為基礎，還可因應其挑戰，ASP.NET 利用物件導向語言的.NET framework 所提供，同時也保留 伺服器端程式設計模型的程式碼組織哪一個典型的 asp 開發人員已習慣。

**適用於 ASP.NET 的目標客戶的第二個群組是 Windows 商務應用程式開發人員。** 不同於傳統 ASP 開發人員而言，之前習慣撰寫 HTML 標記和程式碼以產生更多的 HTML 標記，WinForms 開發人員 （例如之前的 VB6 開發人員） 已習慣於設計階段體驗包含畫布和一組豐富的使用者介面的控制項。 ASP.NET – 的第一個版本也稱為 「 Web 表單 」 提供類似設計階段經驗，以及使用者介面元件的伺服器端事件模型和一組基礎結構功能 （例如 ViewState) 來建立無縫式的開發人員體驗之間用戶端和伺服器端程式設計。 Web Form 有效 hid Web 的無狀態性質，底下是 WinForms 開發人員所熟悉的可設定狀態的事件模型。

### <a name="challenges-raised-by-the-historical-model"></a>歷程記錄模型所引發的挑戰

**最後的結果是成熟且功能豐富的執行階段和開發人員的程式設計模型。** 不過，與增添功能豐富性提出幾個值得注意的挑戰。 首先，架構就已經**單體式**，具有以邏輯方式不同單位會緊密結合在相同的 System.Web.dll 組件 （例如，使用 Web form 架構的核心 HTTP 物件） 的功能。 其次，ASP.NET 已隨附的較大的.NET framework，這意味著，**版本之間的時間為年的順序。** 這會很難跟上的所有變更發生在快速發展的 Web 開發的 asp.net。 最後，System.Web.dll 本身結合在特定的 Web 裝載選項的幾個不同方式： 網際網路資訊服務 (IIS)。

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>進化的步驟執行： ASP.NET MVC 和 ASP.NET Web API

並在 Web 開發過程中發生許多變更 ！ Web 應用程式已逐漸正在開發一系列的小型、 著重元件，而不是大型的架構。 元件，以及與已發行的頻率數目已增加以以往更快的速率。 很顯然與 Web 步調維持需要架構，來取得較小、 低耦合且更受關注而不是更大且功能更豐富，因此**ASP.NET 團隊花了數個進化的步驟來啟用 ASP.NET 為一系列隨插即用的 Web 元件，而不是單一架構**。

其中一項最早的變更是 Rails 的 Web 開發架構，像是 Ruby 由於已知的模型-檢視-控制器 (MVC) 設計模式的熱門程度升高。 這種建置 Web 應用程式提供給開發人員更能控制其應用程式的同時仍然保留的標記和商務邏輯，就是一種適用於 ASP.NET 初始的賣點分隔的標記。 為了符合此樣式的 Web 應用程式開發需求，Microsoft 藉此機會來決定本身的位置更美好的未來**超出開發 ASP.NET MVC** （和不包含.NET Framework 中）。 ASP.NET MVC 已發行為獨立下載。 這提供給工程團隊的彈性，頻率遠比先前可能已提供更新。

另一個重大的轉變，Web 應用程式開發中已從動態、 伺服器產生的網頁排班，靜態與動態區段之間的通訊用戶端指令碼所產生的網頁的初始標記**與後端 Web Api 透過AJAX 要求**。 此架構變化，可協助駭人 Web Api 的興起和 ASP.NET Web API 架構的開發。 如果 ASP.NET MVC 是 ASP.NET Web API 的版本會提供進一步的更接近模組化架構發展 ASP.NET 的另一個機會。 工程小組利用這個機會並**建置 ASP.NET Web API 時，任何找到 System.Web.dll 中的核心架構類型上有任何相依性**。 啟用此兩件事情： 首先，這表示，ASP.NET Web API 可以持續演化完全獨立的方式 （和其可能會持續快速反覆，因為它透過 NuGet 提供）。 其次，因為沒有要 System.Web.dll，沒有外部相依性，因此，IIS 沒有相依性，ASP.NET Web API 會包含執行自訂主應用程式 （例如主控台應用程式、 Windows 服務等） 中的功能

### <a name="the-future-a-nimble-framework"></a>敏捷 Framework 的未來：

藉由減少 framework 元件，從另一個，並再將它們釋放在 NuGet 上，架構無法立即**更彼此獨立，而且更快速地逐一查看**。 此外的威力與彈性的 Web API 的自我裝載的功能已證明非常吸引人的開發人員想**小型、 輕量級主機**其服務。 的確如此吸引人，事實上，其他架構也想要這項功能，而這會顯示新的挑戰，在於每個架構在它自己的主控件程序中執行它自己的基底位址上及所需的管理 （啟動、 停止等） 分開。 現代的 Web 應用程式通常會支援提供靜態檔案、 產生動態頁面、 Web API 和更近期即時-一次性/推播通知。 應該執行每項服務的資料並分開進行管理，必須要有不只是實際。

所需的是主控的抽象概念，讓開發人員撰寫應用程式從各種不同的元件和架構，並接著執行支援的主機上的 這個應用程式。

## <a name="the-open-web-interface-for-net-owin"></a>適用於 .NET 的開放 Web 介面 (OWIN)

 所達成的優點[機架](http://rack.github.io/)在 Ruby 社群中，數個.NET 社群成員將設定來建立 Web 伺服器與 framework 元件之間的抽象概念。 兩個 OWIN 抽象概念的設計目標是很簡單，但在其他 framework 型別上卻花費最少可能的相依性。 這兩個目標有助於確保：

- 新元件可以更輕鬆地開發和使用。
- 主機和可能整個平台/作業系統之間，可以更輕鬆地移植應用程式。

產生抽象層是由兩個核心項目所組成。 第一個是環境字典。 此資料結構會負責儲存所有處理 HTTP 要求和回應，以及任何相關伺服器狀態所需的狀態。 環境字典定義，如下所示：

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

必須填入資料，例如內文資料流和 HTTP 要求和回應的標頭集合的環境字典 OWIN 相容的網頁伺服器。 然後是應用程式或架構元件，以填入或其他值來更新字典並寫入回應主體資料流的責任。

除了指定的環境字典的型別，OWIN 規格定義了核心字典索引鍵值組的清單。 比方說下, 表會顯示 HTTP 要求所需的字典索引鍵：

| 索引鍵名稱 | 值描述 |
| --- | --- |
| `"owin.RequestBody"` | 要求本文中，如果有任何 Stream。 Stream.Null 可能做為預留位置，如果沒有要求內文。 請參閱[要求本文](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics)。 |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>`之要求標頭。 請參閱[標頭](http://owin.org/html/owin.html#3-3-headers)。 |
| `"owin.RequestMethod"` | A`string`包含要求的 HTTP 要求方法 (例如`"GET"`， `"POST"`)。 |
| `"owin.RequestPath"` | A`string`包含要求路徑。 路徑必須相對於應用程式委派中; 「 根 」請參閱[路徑](http://owin.org/html/owin.html#5-3-paths)。 |
| `"owin.RequestPathBase"` | A`string`包含對應至應用程式委派中; 「 根 」 的要求路徑的部分請參閱[路徑](http://owin.org/html/owin.html#5-3-paths)。 |
| `"owin.RequestProtocol"` | A`string`包含的通訊協定名稱和版本 (例如`"HTTP/1.0"`或`"HTTP/1.1"`)。 |
| `"owin.RequestQueryString"` | A`string`包含查詢字串元件，HTTP 要求的 URI，而不需要前置的"？"(例如`"foo=bar&baz=quux"`)。 值可能是空字串。 |
| `"owin.RequestScheme"` | A`string`包含要求所使用的 URI 配置 (例如`"http"`， `"https"`)，請參閱[URI 配置](http://owin.org/html/owin.html#5-1-uri-scheme)。 |

OWIN 的第二個索引鍵項目是應用程式委派。 這是函式簽章會做為 OWIN 應用程式中的所有元件之間的主要介面。 應用程式委派的定義如下所示：

`Func<IDictionary<string, object>, Task>;`

應用程式委派則只需 Func 委派型別函式接受做為輸入的環境字典的位置，並傳回工作的實作。 這項設計有適用於開發人員的幾個含意：

- 有極少數才能撰寫 OWIN 元件的類型相依性。 這會大幅增加 OWIN 開發人員的存取範圍。
- 非同步的設計可讓要有效率地使用它處理的運算資源，特別是在更多的 I/O 密集作業的抽象概念。
- 因為應用程式委派是不可部分完成的執行單位，因為做為參數的委派上執行的環境字典時，OWIN 元件可以輕鬆地鏈結在一起，以建立複雜的 HTTP 處理管線。

從實作觀點來看，OWIN 是一種規格 ([http://owin.org/html/owin.html](http://owin.org/html/owin.html))。 其目標並不是成為下一代 的 Web 架構，而是 Web 架構與 Web 伺服器如何互動的規格。

如果您已調查[OWIN](http://owin.org/)或是[Katana](https://github.com/aspnet/AspNetKatana/wiki)，您可能也會發現[Owin NuGet 套件](http://nuget.org/packages/Owin)和 Owin.dll。 此程式庫包含單一的介面， [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)，它會正式制定並制訂中所述的啟動順序[區段 4](http://owin.org/html/owin.html#4-application-startup)的 OWIN 規格。 雖然不需要以組建 OWIN 伺服器[IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)介面提供具體的參考點，以及它正由 Katana 專案元件。

## <a name="project-katana"></a>專案 Katana

而同時[OWIN](http://owin.org/html/owin.html)規格並*Owin.dll*是擁有的社群，社群執行開放原始碼成果[Katana](https://github.com/aspnet/AspNetKatana/wiki)專案表示 OWIN 的集合，雖然仍開放原始碼、 建置及 Microsoft 所發行的元件。 這些元件可包含基礎結構元件，例如主機和伺服器，以及功能的元件，例如驗證元件和架構的繫結，例如[SignalR](../../../signalr/index.md)和[ASP.NET WebAPI](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md)。 專案具有下列三個高層級目標： 

- **可攜式**– 應該能夠輕鬆地取代為新的元件可供元件。 這包括所有類型的元件，從伺服器和主機的架構。 此目標的含意是，協力廠商架構可以順暢地執行 Microsoft 伺服器上時 Microsoft 架構可以在第三方伺服器和主機上執行。
- **模組化/彈性**– Katana 專案元件不同於許多架構包含大量的功能，預設會開啟，應該是小型且具有焦點，讓控制項應用程式開發人員，判斷哪些元件在應用程式中使用。
- **輕量型、 高效能/可擴充**– 分成小型、 一組的傳統架構的概念會明確新增應用程式開發人員，所產生的 Katana 應用程式可以取用較少的運算元件已取得焦點資源，如此一來，比使用其他類型伺服器和架構來處理更多的負載，和。 如應用程式的需求需要更多的功能，從基礎結構，這些可以新增至 OWIN 管線中，，但這應該是明確的決策，部分應用程式開發人員。 此外，較低層級的元件的替代表示，當它們變成可用，新的高效能伺服器可以順暢地導入以改善 OWIN 應用程式的效能，而不會中斷這些應用程式。

## <a name="getting-started-with-katana-components"></a>開始使用 Katana 元件

第一次引進，其中一個層面[Node.js](http://nodejs.org/)立即吸引人的注意的架構是為了簡單起見，其中一個可能撰寫及執行的 Web 伺服器。 如果 Katana 目標已框架處理的 light [Node.js](http://nodejs.org/)，其中可能指出 Katana 帶來許多優點總結[Node.js](http://nodejs.org/) （和類似的架構） 不強迫開發人員丟出她知道開發 ASP.NET Web 應用程式的所有服務。 此陳述式，才會保存，則為 true，開始使用 Katana 專案應該也一樣簡單性質，為了[Node.js](http://nodejs.org/)。

## <a name="creating-hello-world"></a>建立"Hello World ！"

JavaScript 和.NET 開發之間的一個顯著的差異是編譯器的目前狀態 （或不存在）。 因此，簡單的 Katana 伺服器的起始點是 Visual Studio 專案。 不過，我們可以開始使用最基本的專案類型： 空白的 ASP.NET Web 應用程式。

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

接下來，我們將安裝[Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)到專案的 NuGet 套件。 此套件會提供在 ASP.NET 要求管線中執行 OWIN 伺服器。 它可以在找到[NuGet 資源庫](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)，可以使用 [Visual Studio 套件管理員] 對話方塊或 [套件管理員] 主控台，使用下列命令安裝：

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

安裝`Microsoft.Owin.Host.SystemWeb`套件將會安裝一些額外的套件作為相依項目。 這些相依性的其中一個是`Microsoft.Owin`，提供數個協助程式類型和 OWIN 應用程式的開發方法的程式庫。 我們可以快速地撰寫下列的"hello world"伺服器使用這些類型。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

這個非常簡單的 Web 伺服器現在可以執行使用 Visual Studio **F5**命令，並包含偵錯的完整支援。

## <a name="switching-hosts"></a>切換主機

根據預設前, 一個"hello world"範例會執行在 ASP.NET 要求管線中，使用 IIS 的內容中的 System.Web。 這可以本身加入極大值，因為它可讓我們受益於彈性和 OWIN 管線的管理功能的編寫性和整體的成熟度，IIS。 不過，可能不需要 IIS 所提供的好處，而且需求之一，是較小、 更精簡的主機。 所需，然後執行 IIS 和 System.Web 以外，我們簡單的 Web 伺服器？

為了說明可攜性目標，從命令列的主應用程式的 Web 伺服器主機移動需要只需將新的伺服器與主應用程式相依性新增至專案的輸出資料夾，然後啟動主機。 在此範例中，我們會裝載在 Katana 主應用程式呼叫我們的 Web 伺服器`OwinHost.exe`且將使用 Katana HttpListener 為基礎的伺服器。 同樣的其他 Katana 的元件，這些會取得從 NuGet 使用下列命令：

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

我們可以從命令列中，然後瀏覽至專案根資料夾，並只需執行`OwinHost.exe`（其中已安裝在其個別的 NuGet 套件的 [工具] 資料夾）。 根據預設，`OwinHost.exe`已設定為尋找 HttpListener 為基礎的伺服器，因此不需要任何額外的設定。 在 Web 瀏覽器中瀏覽`http://localhost:5000/`顯示現在透過主控台執行的應用程式。

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana 架構

 Katana 元件架構分割成四個邏輯層，應用程式，如下所述：*主應用程式、 伺服器、 中介軟體*並*應用程式*。 元件架構納入為重要因素的方式，這些層級的實作，可輕鬆地取代，在許多情況下，不需要重新編譯的應用程式。   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>主機

 主機負責：

- 管理基礎程序。
- 處理協調會導致選取的伺服器，以及哪些要求透過 OWIN 管線建構工作流程。

  目前，有 3 個 Katana 型應用程式的主要裝載選項：  
  
**Iis/ASP.NET**： 使用標準的 HttpModule 和 HttpHandler 類型時，OWIN 管線可以在 IIS 上執行的 ASP.NET 要求流程的一部分。 裝載支援的 ASP.NET 會啟用的 Microsoft.AspNet.Host.SystemWeb NuGet 套件安裝到 Web 應用程式專案。 此外，因為 IIS 做為主機和伺服器，OWIN 伺服器/主機區別 」 混為一談是 NuGet 套件內，這表示如果使用的 SystemWeb 主機，開發人員不能取代替代伺服器實作。  
  
**自訂主機**: Katana 元件套件可讓開發人員在自己的自訂程序，主應用程式是主控台應用程式、 Windows 服務等。這項功能類似於提供 Web API 的自我裝載功能。 下列範例會示範自訂主機的 Web API 程式碼：  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Katana 應用程式的自我裝載安裝程式會類似：

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

其中一個差異值得注意的 Web API 和 Katana 自我裝載範例是遺漏 Katana 自我裝載範例的 Web API 組態程式碼。 若要讓可攜性和複合性，Katana 分隔的程式碼，從設定要求處理管線的程式碼會啟動伺服器。 設定 Web API 的程式碼然後包含在啟動時，此外 WebApplication.Start 中的類型參數指定的類別。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

啟動類別將在本文稍後更詳細地討論。 不過，程式碼，才能啟動 Katana 自我裝載的程序和您在 ASP.NET Web API 自我裝載的應用程式中可能會立即使用的程式碼的極為類似。

**OwinHost.exe**： 部分會想要撰寫自訂的程序，來執行 Katana Web 應用程式，而許多想要直接啟動預先建置的可執行檔可以啟動伺服器，並執行其應用程式。 針對此案例，包括 Katana 元件套件`OwinHost.exe`。 當在專案的根目錄中的執行，此可執行檔將開始 （它預設會使用 HttpListener 伺服器） 的伺服器，而使用慣例來尋找並執行使用者的啟動類別。 取得更細微的控制，可執行檔會提供一些其他命令列參數。

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>伺服器

 主機負責啟動和維護應用程式執行所在，伺服器的責任的程序時若要開啟網路通訊端、 接聽要求，並將它們傳送到 OWIN 元件的管線所指定 （做為您的使用者嗎可能已經發現，此管線在 應用程式開發人員的啟動類別中指定）。 目前，Katana 專案包含兩個伺服器實作： 

- **Microsoft.Owin.Host.SystemWeb**： 如先前所述，IIS 與 ASP.NET 管線做為主機和伺服器。 因此，在選擇這個裝載選項，IIS 管理主機層級考量，例如處理程序啟動和會接聽 HTTP 要求。 ASP.NET Web 應用程式，它接著會傳送要求至 ASP.NET 管線。 Katana SystemWeb 主機註冊 ASP.NET HttpModule 和 HttpHandler 攔截要求，因為它們透過 HTTP 管線流動，並將它們傳送到使用者指定 OWIN 管線。
- **Microsoft.Owin.Host.HttpListener**： 此 Katana 伺服器如其名稱所指示，會使用.NET Framework 的 HttpListener 類別，來開啟通訊端，並將要求傳送到開發人員指定 OWIN 管線。 這是目前 Katana 自我裝載的 API 和 OwinHost.exe 預設伺服器選取項目。

## <a name="middlewareframework"></a>中介軟體/架構

 如先前所述，如果伺服器接受要求，以從用戶端，它會負責通過管線的由開發人員的啟始程式碼所指定的 OWIN 元件。 這些管線元件稱為中介軟體。  
 在非常基本的層級，OWIN 中介軟體元件只需要實作 OWIN 應用程式委派，使其可呼叫。

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

不過，為了簡化開發和中介軟體元件的組合，Katana 支援少數幾個慣例和協助程式型別中介軟體元件。 最常見的是`OwinMiddleware`類別。 使用這個類別所建立的自訂中介軟體元件看起來會如下所示： 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 這個類別衍生自`OwinMiddleware`，實作做為其引數，其中會接受管線中的下一個中介軟體的執行個體，然後將它傳遞給基底建構函式的建構函式。 在下一個中介軟體參數之後，用來設定中介軟體的其他引數也會宣告為建構函式參數。   
  
中介軟體會執行在執行階段，透過覆寫`Invoke`方法。 這個方法會採用單一引數的型別`OwinContext`。 這個內容物件由提供`Microsoft.Owin`NuGet 封裝稍早所述，並提供要求、 回應和環境的字典，以及一些額外的協助程式類型的強型別存取。   
  
中介軟體類別可以輕鬆地新增至 OWIN 管線中的應用程式啟動程式碼，如下所示：   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

由於 Katana 基礎結構只會建立管線的 OWIN 中介軟體元件，而且元件只需要支援參與管線的應用程式委派中, 介軟體元件的範圍可以從簡單的複雜性記錄器將整個架構，例如 ASP.NET，Web API 或[SignalR](../../../signalr/index.md)。 例如，將 ASP.NET Web API 新增至先前的 OWIN 管線需要加入下列的啟始程式碼：

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Katana 基礎結構將會建立已加入到設定方法的 IAppBuilder 物件的順序為基礎的中介軟體元件的管線。 然後，在我們的範例，LoggerMiddleware 可以處理流經管線中，不論這些要求最終的處理方式的所有要求。 這可讓擁有強大的功能的中介軟體元件 （例如驗證元件） 可以處理包含多個元件和架構 （例如 ASP.NET Web API、 SignalR 和靜態檔案伺服器） 的管線要求的地方。
 
## <a name="applications"></a>應用程式

如先前範例所示，OWIN 和 Katana 專案應該不想成新的應用程式設計模型，只將分離應用程式撰寫模型和伺服器與裝載基礎結構的架構的抽象概念。 比方說，當建置 Web API 應用程式，開發人員架構會繼續使用 ASP.NET Web API 架構，不論使用 Katana 專案元件 OWIN 管線中執行應用程式。 OWIN 相關的程式碼將會看見應用程式開發人員的一個位置會是應用程式啟動程式碼，其中的開發人員撰寫 OWIN 管線。 在啟動程式碼中，開發人員會登錄一系列 UseXx 陳述式，通常是一個針對每個中介軟體元件會處理連入要求。 這項體驗必須註冊在目前的 System.Web 世界中的 HTTP 模組相同的效果。 一般而言，較大型架構中的介軟體，例如 ASP.NET Web API 或[SignalR](../../../signalr/index.md)會註冊在管線結尾處。 跨領域中介軟體元件，例如驗證或快取，朝向管線的開頭通常已註冊，使它們可處理所有的架構和元件註冊管線中稍後的要求。 這種區隔的中介軟體元件彼此，以及從基礎的基礎結構元件可讓在不同的速度發展，同時確保整體系統會保持穩定的元件。

## <a name="components--nuget-packages"></a>元件 – NuGet 套件

如同許多目前程式庫和架構，Katana 專案元件是以一組 NuGet 套件形式傳遞。 即將推出的版本 2.0，Katana 套件相依性關係圖看起來像這樣。 （按一下影像以放大檢視）。

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Katana 專案中的幾乎每個套件而定，直接或間接地 Owin 套件。 您可能還記得，這是包含提供 OWIN 規格的第 4 節中所述的應用程式啟動順序的具體實作的 IAppBuilder 介面的套件。 此外，許多封裝相依於 Microsoft.Owin，提供一組協助程式類型使用 HTTP 要求和回應。 封裝的其餘部分可以歸類為裝載基礎結構套件 （伺服器或主機） 或中介軟體。 封裝及 Katana 專案外部的相依性會以橙色顯示。

Katana 2.0 的裝載基礎結構包括 SystemWeb 和 HttpListener 為基礎的伺服器、 執行 OWIN 應用程式使用 OwinHost.exe，OwinHost 封裝和 Microsoft.Owin.Hosting 封裝自我裝載在 OWIN 應用程式自訂主應用程式 （例如主控台應用程式、 Windows 服務等。）

Katana 2.0 中, 介軟體元件主要著重在提供不同的驗證方法。 會提供一個診斷的其他中介軟體元件，可讓開始與錯誤頁面的支援。 隨著 OWIN 成長到實際的裝載抽象概念中, 介軟體元件，這兩個是那些由 Microsoft 和協力廠商開發的生態系統也會成長數字。

## <a name="conclusion"></a>結論

 從開始，Katana 專案的目標已建立，並藉此強制開發人員若要了解另一個 Web 架構。 相反地，其目標是要建立更多的選擇，比先前一直無法讓.NET Web 應用程式開發人員的抽象概念。 藉由一般的 Web 應用程式堆疊插入其中的一組可取代元件的邏輯層，Katana 專案可讓元件在整個堆疊在任何頻率最適合這些元件改善中。 藉由建置簡單的 OWIN 抽象周圍的所有元件，Katana 可讓架構和建置的應用程式在其上跨各種不同的伺服器和主機具有可攜性。 藉由將開發人員放在堆疊的控制項，Katana 可確保開發人員，會使最終決定如何輕量的相關或她 Web 堆疊應該是功能豐富。  
  

## <a name="for-more-information-about-katana"></a>如需有關 Katana

- GitHub 上的 Katana 專案： [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/)。
- 影片： [Katana 專案-適用於 ASP.NET 的 OWIN](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)，由 Howard Dierking。

## <a name="acknowledgements"></a>謝誌

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick 是將焦點放在 Azure 和 MVC 的 microsoft 的資深程式設計作家。
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
