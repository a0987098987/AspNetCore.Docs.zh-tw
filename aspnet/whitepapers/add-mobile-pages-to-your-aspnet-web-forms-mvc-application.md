---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 如何： 將行動網頁新增至 ASP.NET Web Form / MVC 應用程式 |Microsoft Docs
author: rick-anderson
description: 本 How To 說明會提供適用於行動裝置，從您的 ASP.NET Web Form 網頁的各種方法 / MVC 應用程式，並建議架構和...
ms.author: aspnetcontent
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 59b81184852a7fe0ad2dcad9718b572a8c756918
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823351"
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>如何： 將行動網頁新增至 ASP.NET Web Form / MVC 應用程式
====================
> **適用於**
> 
> - ASP.NET Web Form 版本 4.0
> - ASP.NET MVC 3.0 版
> 
> **摘要**
> 
> 本 How To 說明會提供適用於行動裝置，從您的 ASP.NET Web Form 網頁的各種方法 / MVC 應用程式，並建議架構和設計以各種裝置為目標時應該考量的問題。 本文件也說明為何從 ASP.NET 2.0 ASP.NET 行動控制項，為 3.5 已經過時，並討論一些現代的替代方案。


## <a name="contents"></a>內容

- 總覽
- 架構選項
- 瀏覽器及裝置的偵測
- ASP.NET Web Forms 應用程式會如何呈現行動裝置專屬頁面
- ASP.NET MVC 應用程式會如何呈現行動裝置專屬頁面
- 其他資源

如需可下載的程式碼範例示範針對 ASP.NET Web Form 和 MVC 的這份白皮書的技術，請參閱[行動應用程式和 ASP.NET 網站](https://docs.microsoft.com/aspnet/mobile/overview)。

## <a name="overview"></a>總覽

行動裝置 – 智慧型手機、 功能手機和平板電腦 – 繼續在受歡迎，做為存取 Web。 對於許多 web 開發人員和 web 導向的企業，這表示它是越來越重要，無法使用這些裝置的訪客提供絕佳的瀏覽經驗。

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>如何較早版本的 ASP.NET 支援行動瀏覽器

ASP.NET 版本 2.0 到 3.5，包含*ASP.NET 行動控制項*： 伺服器控制項中的行動裝置的一組*System.Web.Mobile.dll*組件和*System.Web.UI.MobileControls*命名空間。 組件仍會包含在 ASP.NET 4 中，但它已被取代。 開發人員建議移轉至更現代化的方法，例如這份文件中所述。

為什麼 ASP.NET 行動控制項已標示為過時的原因是它們的設計很常見周圍 2005年和更早版本的行動電話周圍導向。 WAP 瀏覽器，那個時代，控制項主要被設計來呈現 WML 或 cHTML 標記 （而不是一般的 HTML)。 但 WAP、 WML 和 cHTML 不再相關的大部分專案，因為 HTML 現在已成為行動和桌面瀏覽器，類似的通用標記語言。

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>目前支援的行動裝置上的挑戰

雖然行動瀏覽器時，現在幾乎都支援 HTML，您仍會在建立絕佳的行動瀏覽體驗為目標時面臨許多挑戰：

- ***螢幕大小***-行動裝置明顯不同格式，和他們的螢幕通常是許多小於桌上型監視器。 因此，您可能需要它們的設計完全不同的頁面配置。
- ***輸入方法***– 有些裝置有提功，某些 styluses，而其他人使用觸控功能。 您可能需要考慮多個導覽機制和資料的輸入的方法。
- ***標準相容性***– 許多行動瀏覽器不支援的最新的 HTML、 CSS 或 JavaScript 標準。
- ***頻寬***-行動數據網路效能的變化很大的差異，和某些使用者位於關稅的 mb 數來收取費用。

沒有一體適用的解決方案;您的應用程式會有的外觀和行為以不同的方式，根據裝置進行存取。 根據您要何種行動裝置層級，這可以是支援的挑戰更大的 web 開發人員比一直以來桌面 「 瀏覽器大戰 」。

一開始通常接近第一次行動瀏覽器支援的開發人員認為務必只支援最新且最精確的 smartphone （例如，Windows Phone 7、 iPhone 或 Android） 時，可能是因為開發人員通常是個人擁有這類裝置。 不過，成本較低的電話仍是很受歡迎，而且其擁有者使用它們來瀏覽網頁 – 特別是在行動電話所在位置，您更輕鬆地取得比寬頻連線的國家/地區。 您的業務需要決定哪些範圍要考慮其可能購買的客戶支援的裝置。 如果您正在建置為 luxury 健全狀況 spa 線上手冊，您可能會進行業務決策才能目標進階的智慧型手機，而如果您要建立杜比劇院效果的票證預約系統，您可能需要較不強大的功能與訪客帳戶電話。

## <a name="architectural-options"></a>架構選項

我們的 ASP.NET Web Form 或 MVC 特定的技術細節之前，請注意，web 開發人員通常會有三個主要的可能選項，可支援行動瀏覽器：

1. ***不執行任何動作 –*** 您只可以建立標準的桌面導向的 web 應用程式，並依賴來呈現它接受的行動瀏覽器。 

    - **利用**： 它是最便宜的選項，來實作和維護 – 不需要額外工作
    - **缺點**： 提供的最差的使用者體驗： 

        - 最新的智慧型手機可能會讓您的 HTML 與桌面瀏覽器，一樣，但縮放，以及水平捲動並以垂直方式取用您在小型螢幕上的內容，仍會強制使用者。 這是遠離最佳。
        - 較舊的裝置和功能的電話可能無法令人滿意的方式呈現您的標記。
        - 即使在最新的平板電腦裝置 （其畫面可以是膝上型電腦螢幕的大小），套用不同的互動規則。 觸控式的輸入最適合搭配較大的按鈕連結擴散的進一步，並沒有任何方法可以將滑鼠游標停留在飛出功能表。
2. ***解決此問題，在用戶端*–** 小心使用 CSS 並[漸進式增強](http://en.wikipedia.org/wiki/Progressive_enhancement)標記、 樣式和適應它們執行的任何瀏覽器的指令碼，您可以建立。 例如，使用[CSS 3 的媒體查詢](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/)，您可以建立多重資料行版面配置，其畫面位於所選的臨界值較窄的裝置上會變成單一資料行版面配置。 

    - **利用**： 最佳化呈現特定裝置使用，甚至是根據它們的任何畫面和輸入的特性有未知的未來裝置
    - **利用**： 輕鬆可讓您跨所有裝置類型 – 最小的程式碼或工作的重複共用伺服器端邏輯
    - **缺點**： 行動裝置並不讓桌面裝置，您可能希望是完全不同於您桌面的網頁，讓行動網頁顯示不同的資訊。 這類變化可能會沒有效率或不可能達成穩固透過 CSS，尤其考慮一致較舊的裝置解譯 CSS 規則。 這是特別的 CSS 3 屬性。
    - **缺點**： 不提供支援各種不同的伺服器端邏輯和適用於不同裝置的工作流程。 您比方說，不能透過 CSS 單獨實作簡單購物車簽出的工作流程為行動使用者。
    - **缺點**： 效率不佳的頻寬用量。 您的伺服器可能要傳輸的標記和樣式套用至所有可能的裝置，即使目標裝置只會使用該資訊的子集。
3. ***解決此問題，在伺服器上的*–** 如果您的伺服器可讓您知道哪些裝置存取它-，或在該裝置，例如其螢幕大小和輸入的法，最低的特性，且無論是行動裝置，像是網域控制站可以執行不同的邏輯和輸出不同的 HTML 標記。 

    - **優點：** 最大的彈性。 沒有多少，您便可以改變您的 asp.net 的伺服器端邏輯上，或最佳化您如有需要，裝置特定配置的標記來無限制。
    - **優點：** 有效率的頻寬用量。 您只需要傳輸的標記和目標裝置將使用的樣式資訊。
    - **缺點：** 有時候會強制執行的工作或程式碼的重複 （例如，讓您建立 Web Form 頁面或 MVC 檢視類似但稍微不同的複本）。 其中可能會排除一般邏輯為基礎的圖層或服務，但儘管如此，您的 UI 程式碼或標記的某些部分可能重複，並以平行方式加以維護。
    - **缺點：** 裝置偵測並非易事。 它需要的已知的裝置類型和它們的特性 （這不一定完全最新狀態） 的資料庫或資料清單，並不保證會精確地符合每個傳入要求。 本文件稍後所述部分選項和其所犯的錯誤。

若要取得最佳結果，大部分的開發人員會發現他們需要結合選項 (2) 和 (3)。 樣式的細微差異使用 CSS 或甚至是 JavaScript，在用戶端上最可調整，而在伺服器端程式碼中最有效地實作資料、 工作流程或標記中的主要差異。

### <a name="this-paper-focuses-on-server-side-techniques"></a>此白皮書著重於伺服器端技術

由於 ASP.NET Web Form 和 MVC 是這兩種主要的伺服器端技術，這份白皮書著重於伺服器端技術，可讓您產生不同的標記和行動瀏覽器的邏輯。 當然，您也可以結合此使用任何用戶端技術 （例如，CSS 3 的媒體查詢，漸進式增強功能的 JavaScript），但，相當多的 web 設計比 ASP.NET 程式開發。

## <a name="browser-and-device-detection"></a>瀏覽器及裝置的偵測

支援行動裝置的所有伺服器端技術的重要先決條件是知道您的訪客使用哪一種裝置。 事實上，比起了解該裝置的製造商和型號號碼了解更好*特性*的裝置。 特性可能包括：

- 它是行動裝置嗎？
- 輸入的法 （滑鼠] / [鍵盤、 觸控、 數字鍵台、 搖桿，...）
- 螢幕大小，（實際和像素為單位）
- 支援的媒體和資料格式
- 等。

最好是因為進行特性與模型數目為基礎的決策，然後您就更有能力來處理未來的裝置。

### <a name="using-aspnets-built-in-browser-detection-support"></a>使用 ASP。NET 的內建的瀏覽器偵測支援

ASP.NET Web Form 和 MVC 的開發人員立即可以藉由檢查屬性探索正在瀏覽的瀏覽器的重要特性*Request.Browser*物件。 例如，請參閱

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...還有許多其他項目

在幕後，ASP.NET 平台符合傳入*User-agent* (UA) 的 HTTP 標頭，針對一組瀏覽器定義 XML 檔案中的規則運算式。 根據預設，此平台包括定義許多常見的行動裝置，以及您可以為您想要辨識的其他項目中新增自訂的瀏覽器定義檔案。 如需詳細資訊，請參閱 MSDN 頁面[ASP.NET Web 伺服器控制項和瀏覽器能力](https://msdn.microsoft.com/library/x3k2ssx2.aspx)。

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>使用 WURFL 裝置資料庫透過 51Degrees.mobi Foundation

雖然 ASP。NET 的內建的瀏覽器偵測支援將足以說明許多應用程式時，有兩種主要的情況下它可能不太夠：

- ***您想要辨識最新的裝置***（而不需要手動建立它們的瀏覽器定義檔案）。 請注意，.NET 4 的瀏覽器定義檔案不是不夠新，無法辨識 Windows Phone 7、 Android 手機、 Opera Mobile 瀏覽器或 Apple Ipad。
- ***您需要裝置功能的詳細的資訊***。 您可能需要知道裝置的輸入法 （例如觸控與數字鍵台），或哪些音訊格式的瀏覽器支援。 標準的瀏覽器定義檔案中，無法使用這項資訊。

[*無線通用資源檔*(WURFL) 專案](http://wurfl.sourceforge.net/)維護更多最新狀態和詳細資訊中使用的行動裝置的相關今天。

適用於.NET 開發人員最棒的消息是該 ASP。NET 的瀏覽器偵測功能，所以就可以增強程式碼來克服這些問題。 例如，您可以在其中加入開放原始碼[ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection)至您的專案程式庫。 它是 ASP.NET IHttpModule 或瀏覽器功能提供者 （可使用 Web Form 和 MVC 應用程式），會直接讀取 WURFL 資料並將它連結到 ASP。NET 的內建瀏覽器偵測機制。 一旦您安裝該模組，請*Request.Browser*突然將包含許多更精確且更詳細的資訊： 它會正確地辨識許多先前所述的裝置，並列出其功能 （包括其他功能，例如輸入法）。 請參閱專案的文件，如需詳細資訊。

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Web Form 應用程式會如何呈現行動裝置專屬頁面

根據預設，以下是常見的行動裝置上的全新的 Web Form 應用程式的顯示方式：

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

很明顯地，未配置看起來非常適合行動 – 此頁面專為大型、 橫向的監視器，不適用於小型的直向導向螢幕。 因此可以您做什麼資訊？

如本文稍早所述，有很多種，量身訂做您的行動裝置的網頁。 有些技術則是伺服器架構，其他用戶端上執行。

### <a name="creating-a-mobile-specific-master-page"></a>建立行動專用的主版頁面

根據您的需求，您可以為所有訪客，使用相同的 Web Form，但有兩個不同的主版頁面： 一個用於桌面的訪客，另一個用於行動裝置的訪客。 這可讓您彈性地變更 CSS 樣式表或您最上層的 HTML 標記，以符合行動裝置，而不需要強迫您重複的任何網頁邏輯。

這很簡單。 例如，您可以將如下所示的 PreInit 處理常式加入 Web Form:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

現在，建立您的應用程式的最上層資料夾中稱為 Mobile.Master 主版頁面，並將在偵測到行動裝置時使用。 如有必要，您行動裝置的主版頁面可以參考的行動裝置專屬的 CSS 樣式表。 桌面的訪客仍然會看到您預設主版頁面，不是行動裝置的一個。

### <a name="creating-independent-mobile-specific-web-forms"></a>建立獨立的行動裝置專屬 Web Form

最大的彈性，您即可更進一步比有個別的主版頁面，針對不同裝置類型。 您可以實作兩個*完全不同的 Web Form 頁面集*– 一個設定的桌面瀏覽器，另一組行動裝置的 asp.net。 這適用於最適合您想要呈現給行動裝置的訪客的非常不同的資訊或工作流程。 本節的其餘部分將說明這種方式詳細資料。

假設您已經有專為桌面瀏覽器的 Web Form 應用程式，繼續進行的最簡單方式是建立在您專案中，名為"Mobile"子資料夾，並建置行動頁面有。 您可以建構整個的子網站時，它自己的主版頁面、 樣式表和頁面，使用完全相同的技巧，您可以使用任何其他 Web Form 應用程式。 您不一定要產生行動裝置對等*每個*在桌面網站頁面上，您可以選擇哪些功能的子集有意義的行動裝置的訪客。

您的行動網頁可以共用通用的靜態資源 (例如影像、 JavaScript 或 CSS 檔案) 與您一般的頁面，如果您想要。 因為您的 「 行動 」 資料夾將會*不*標示為個別的應用程式裝載在 IIS 中 （它是只是 Web Form 網頁的簡單子資料夾），它也會分享相同的所有設定、 工作階段資料和其他基礎結構即程式桌面的頁面。

> [!NOTE]
> 由於這種方法通常會涉及到的程式碼的一些重複項目 （行動網頁可能會將桌面的網頁中分享一些相似之處），請務必將任何一般商務邏輯或資料存取程式碼到共用的基礎層或服務。 否則，您將會兩倍的建立和維護您的應用程式的工作。


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>重新導向至您的行動頁面的行動訪客

通常很方便將行動裝置的訪客的行動裝置的頁面重新導向僅*第一個*要求在其瀏覽工作階段 （而不是在其工作階段中的每個要求上），因為：

- 您接著可以輕鬆地讓行動裝置的訪客存取您桌面的網頁，如果他們想要 – 只要將連結放在您移至 [桌面版本] 的主版頁面。 訪客不會重新導向回到行動裝置的頁面上，因為它不再第一個要求在其工作階段中。
- 它可避免干擾要求 （例如，如果您有一個常見的 Web 表單，桌上型電腦和行動裝置的組件，您的網站會顯示在 IFRAME 中或某些 Ajax 處理常式中），您的站台桌上型電腦和行動裝置的組件之間共用的所有動態資源的風險

若要這樣做，您可以將放在您重新導向邏輯**工作階段\_啟動**方法。 比方說，將下列方法新增至 Global.asax.cs 檔案中：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>設定表單驗證，應遵守您的行動裝置頁面

請注意，表單驗證會使某些假設，它可以重新導向訪客期間和之後的驗證程序：

- 當使用者需要進行驗證時，表單驗證將會重新導向至您的桌面登入頁面，不論它們位桌面或行動裝置的使用者 (因為它只會有的概念*一個*登入 URL)。 假設您想要以不同的方式設定您的行動裝置的登入頁面的樣式，您需要增強您的桌面登入頁面，讓行動使用者重新導向至不同的行動裝置登入頁面。 例如，加入下列程式碼，您**桌面**登入頁面的程式碼後置： 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- 使用者成功登入之後，表單驗證會根據預設重新導向至您的桌面首頁 (因為它只會有的概念*一個*預設網頁)。 您要增強您的行動裝置的登入頁面，讓它將重新導向至行動裝置首頁之後成功登入。 例如，加入下列程式碼，您**行動**登入頁面的程式碼後置： 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  此程式碼假設您的網頁有呼叫 LoginUser，如所示的預設專案範本的 Login 伺服器控制項。

### <a name="working-with-output-caching"></a>使用輸出快取

如果您使用輸出快取，請注意，根據預設，它是桌上型電腦的使用者，可以造訪特定 URL （導致快取其輸出），後面則會收到快取的桌面輸出的行動使用者。 無論您只是改變您的裝置類型的主版頁面，或實作完全不同的 Web Form，每種裝置類型，適用於這項警告。

若要避免此問題，您可以指示 ASP.NET 來變更快取項目，根據造訪者是否正在使用行動裝置。 Hodnota VaryByCustom 將參數新增至您的頁面 OutputCache 宣告，如下所示：

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

接下來，定義*isMobileDevice*做為自訂快取參數，將下列方法新增覆寫至 Global.asax.cs 檔案：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

這可確保行動裝置的訪客頁面不會收到先前桌面的訪客，以放入快取的輸出。

### <a name="a-working-example"></a>實用的範例

若要查看作用中的這些技巧，請下載[這份白皮書的程式碼範例](https://docs.microsoft.com/aspnet/mobile/overview)。 Web Form 範例應用程式會自動將行動裝置的使用者一組稱為 「 行動裝置的子資料夾中的行動裝置專屬頁面重新導向。 標記和樣式的那些頁面更適合行動瀏覽器，您可以看到與下列螢幕擷取畫面：

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

更多秘訣行動瀏覽器最佳化您的標記和 CSS 的詳細資訊，請參閱 「 樣式行動頁面的行動瀏覽器 」 在本文件稍後的一節。

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>ASP.NET MVC 應用程式會如何呈現行動裝置專屬頁面

因為 「 模型-檢視-控制器模式以減少從展示邏輯 （在檢視中） 的應用程式邏輯 （在控制站），您可以選擇任何下列其中一個方法來處理伺服器端程式碼中的行動支援：

1. ***桌上型電腦和行動瀏覽器中，使用相同的控制器和檢視，但會呈現不同的 Razor 版面配置，根據裝置類型 * 檢視。** 如果您在所有的裝置上顯示相同的資料，但只是想要提供不同的 CSS 樣式表，或是變更幾個最上層的 HTML 項目，行動裝置的 asp.net，這個選項最有用。
2. ***桌上型電腦和行動瀏覽器中，使用相同的控制站，但會呈現每種裝置類型的不同檢視***。 如果您是顯示大致上相同的資料，而且對於使用者，提供相同的工作流程，這個選項最有用，但想要轉譯非常不同的 HTML 標記，以符合所使用的裝置。
3. ***建立桌面和行動瀏覽器，實作獨立的控制器和檢視每個不同的區域 *。** 若您使用顯示非常不同的畫面，內含不同資訊而導致使用者透過不同的工作流程適用於其裝置類型，這個選項最有用。 這可能表示部分重複程式碼，但您可以最小化那個隔離一般邏輯為基礎的圖層或服務。

如果您想要採取**第一個**選項出入 Razor 版面配置每種裝置類型，它是非常簡單。 只要修改您\_ViewStart.cshtml 檔案，如下所示：

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

現在您可以建立行動專用的配置，稱為\_LayoutMobile.cshtml 頁面結構和 CSS 規則 適用於行動裝置。

如果您想要採取**第二個**選項根據訪客的裝置類型的轉譯完全不同檢視，請參閱[Scott Hanselman 的部落格文章](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx)。

這份文件的其餘部分著重於**第三個**選項 – 建立個別的控制器*和*行動裝置 – 讓您能夠控制其確實提供哪些功能的子集來進行行動裝置的訪客的檢視。

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>在 ASP.NET MVC 應用程式內的行動裝置區域設定

您可以新增以一般方式稱為 「 行動 」，在現有的 ASP.NET MVC 應用程式的區域： 您在 方案總管的專案名稱上按一下滑鼠右鍵，然後選擇 新增相對區域。 以您平常的 ASP.NET MVC 應用程式內的任何其他區域，您可以再新增控制器和檢視。 比方說，新增至您行動裝置的區域稱為 HomeController 作為行動裝置的訪客首頁的新控制器。

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>確保 URL /Mobile 達到行動裝置的首頁

如果您想要連線到您行動裝置的區域內 HomeController 的索引動作 URL /Mobile 時，您必須對路由組態的兩個的小型變更。 首先，更新 MobileAreaRegistration 類別使 HomeController 預設控制器在行動裝置的區域中，如下列程式碼所示：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

這表示在行動裝置的首頁現在即將放置在 /Mobile，而不是行動/首頁，因為 「 首頁 」 現在是隱含預設控制器名稱行動頁面。

接下來，請注意，加入您的應用程式 （亦即，行動裝置一，除了現有的桌面一個） 中的第二個 HomeController，您將會中斷您的一般桌面首頁。 它將會失敗並出現錯誤 「*符合控制器名稱為 'Home' 找到多個類型*"。 若要解決此問題，請指定您的桌面 HomeController 在模稜兩可時，應該會優先更新最上層路由組態 （在 Global.asax.cs):

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

現在錯誤將會淘汰過，和 URL http://<em>yoursite</em>/ 會到達桌面的首頁和 http://<em>您的站台</em>/mobile/ 會連線到行動裝置的首頁。

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>重新導向至您的行動裝置區域的行動訪客

因此有許多可能的方法，將重新導向邏輯在 ASP.NET MVC 中，有許多不同的擴充點。 其中一個不錯的選項是建立一個篩選屬性、 [RedirectMobileDevicesToMobileArea]，執行重新導向，如果符合下列條件：

1. 它是使用者工作階段中的第一個要求 （亦即，Session.IsNewSession 等於 true）
2. 要求來自行動瀏覽器 （亦即，Request.Browser.IsMobileDevice 等於 true）
3. 使用者已不要求行動裝置的區域中的資源 (亦即*路徑*URL 部分的開頭不是 /Mobile)

本白皮書所隨附的可下載範例包含此邏輯的實作。 它會實作為衍生自 AuthorizeAttribute，這表示它在即使您使用輸出快取 （否則如果桌面的訪客第一次存取特定 URL，桌面的輸出可能會快取，並再提供給可以正確運作的授權篩選條件，後續行動訪客）。

因為它是篩選條件時，您可以選擇要套用至特定的控制器和動作例如

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… 或您可以將它套用到所有控制器和動作的 MVC 3*全域篩選*Global.asax.cs 檔案中：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

可下載的範例也會示範如何建立此屬性的重新導向至您行動裝置的區域內的特定位置的子類別。 這表示，例如，您可以：

- 註冊全域篩選器如下所示上述的預設傳送行動裝置的訪客在行動裝置的首頁。
- 以採取行動的訪客要求它們有任何產品頁面的行動裝置版本的 「 檢視產品 」 動作，也適用於特殊的 [RedirectMobileDevicesToMobileProductPage] 篩選器。
- 也適用於特定的動作，重新導向行動訪客的對等的行動頁面的 篩選器的其他特殊子類別

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>設定表單驗證，應遵守您的行動裝置頁面

如果您使用表單驗證，您應該注意，當使用者需要登入，它會自動將使用者重新導向至單一特定 「 登入 」 的 URL，其預設值是 **/帳戶/登入**。 這表示行動使用者，可能會重新導向至您的桌面登入 動作。

若要避免這個問題，使它會重新導向行動使用者一次在行動裝置的 「 登入 」 動作，在桌面的登入 動作加入邏輯。 如果您使用預設 ASP.NET MVC 應用程式範本，更新 AccountController 的登入動作，如下所示：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… 取代預設文字，然後再呼叫 AccountController 行動的區域中的控制站上實作適當的行動裝置專屬 「 登入 」 動作。

### <a name="working-with-output-caching"></a>使用輸出快取

如果您使用 [OutputCache] 篩選器，您必須強制因裝置類型的快取項目。 比方說，撰寫程式碼：

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

然後，新增至 Global.asax.cs 檔案的下列方法：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

這可確保行動裝置的訪客頁面不會收到先前桌面的訪客，以放入快取的輸出。

### <a name="a-working-example"></a>實用的範例

若要查看作用中的這些技巧，請下載[這份白皮書的程式碼相關聯的範例](https://docs.microsoft.com/aspnet/mobile/overview)。 此範例包含 ASP.NET MVC 3 （發行候選版本） 應用程式增強為支援行動裝置使用上面所述的方法。

## <a name="further-guidance-and-suggestions"></a>進一步的指引和建議

下列討論適用於 Web Form 和 MVC 的開發人員使用這份文件所述的技巧。

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>增強您的重新導向邏輯使用 51Degrees.mobi Foundation

這份文件中所示的重新導向邏輯可能是完全滿足您的應用程式，但如果您要停用工作階段，將無法運作或與拒絕的 cookie （這些不能有工作階段），因為它不會知道是否為指定的要求的行動瀏覽器第一個來自該訪客。

此外，您已了解開放原始碼 51Degrees.mobi Foundation 如何改善 ASP 的精確度。NET 的瀏覽器偵測。 它也有內建的能力，來重新導向至特定位置，在 Web.config 中設定的行動訪客。就能夠運作，而不根據 ASP.NET 工作階段 (並因此 cookie) 所儲存的訪客的 HTTP 標頭和 IP 位址的雜湊的暫存記錄檔，使它知道每一個要求是否從給定造訪的第一個。

下列項目加入至 web.config 檔案 fiftyOne 區段將會重新導向至頁面偵測到的行動裝置的第一個要求 ~ / Mobile/Default.aspx。 頁面 [行動] 資料夾下的任何要求都會*不*重新導向，無論何種裝置類型。 如果行動裝置已經過一段非作用中的 20 分鐘或更多裝置將會被遺忘，後續要求將會被視為新的目的，重新導向。

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

如需詳細資訊，請參閱 < [51degrees.mobi Foundation 文件](https://github.com/51Degrees/dotNET-Device-Detection)。

> [!NOTE]
> 您*可以*使用 51Degrees.mobi Foundation 的重新導向功能，在 ASP.NET MVC 應用程式，但您必須定義您的重新導向設定，以純文字的 Url，不是由路由參數，或將 MVC 篩選條件上的動作。 這是因為 （在本文撰寫之際） 51Degrees.mobi Foundation 無法辨識或路由篩選器。


### <a name="disabling-transcoders-and-proxy-servers"></a>停用的轉碼器和 Proxy 伺服器

行動網路業者他們行動裝置的網際網路的方法有兩個廣泛的目標：

1. 提供做為盡可能更相關內容
2. 可以共用限制的廣播網路頻寬的客戶數目最大化

由於大部分的網頁設計大型桌面大小的螢幕和列快速修正寬頻連線時，許多運算子會使用*轉錄器*或是*proxy 伺服器*，動態變更網頁內容。 他們可能會修改您的 HTML 標記或 CSS 以符合較小的螢幕 （特別是針對 「 功能電話 」 缺少的處理能力來處理複雜的版面配置），和他們可能會重新映像 （大幅降低其品質） 壓縮以改善網頁傳送速度。

但是，如果您所採取的工作，以產生您的站台的行動裝置最佳化版本，您可能不想干擾它任何進一步的網路業者。 您可以將下行加入頁面\_任何 ASP.NET Web Form 中的 Load 事件：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

或者，您也可以針對 ASP.NET MVC 控制站，您可以新增下列方法覆寫，讓它套用至該控制器上的所有動作：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

產生的 HTTP 訊息會通知 W3C 相容轉錄器並不是用來改變內容的 proxy。 當然，則行動網路運算子會遵守此訊息無法保證。

### <a name="styling-mobile-pages-for-mobile-browsers"></a>行動瀏覽器的樣式設定行動裝置頁面

它超出範圍的這份文件中，有詳細描述哪些類型的 HTML 標記工作正確，或哪些 web 設計技術最大化在特定的裝置上的可用性。 它有增加，尋找夠簡單版面配置，讓您適合行動大小 畫面中，而不使用不可靠 HTML 或 CSS 技巧。 不過，是一個重要的技術，值得一提*檢視區的中繼標記*。

某些現代行動瀏覽器，在適用於桌面的監視器，投入時間顯示網頁中呈現的頁面上的虛擬的畫布，也稱為 「 檢視區 」 （例如，虛擬的檢視區為 980 像素寬在 iPhone 上和 850 個像素寬 Opera Mobile 上依預設），然後相應減少的結果，以符合螢幕的實體像素為單位。 使用者可以拉近然後該檢視區前後移動瀏覽。 這樣做的好處，它可讓瀏覽器顯示頁面在其預期的配置中，但也有缺點，它會強制縮放和取景功能不適用於使用者。 如果您要設計適用於行動裝置，最好設計窄的畫面，因此不需要的任何縮放或水平捲動。

告訴行動瀏覽器中檢視區應該是寬度的方式是透過使用非標準*viewport*中繼標籤。 例如，如果您將下列新增至您的頁面標頭 區段中，

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… 然後支援 smartphone 瀏覽器會配置到 480 像素寬虛擬畫布上的頁面。 這表示，如果您的 HTML 項目會定義其寬度，以百分比表示，百分比會解譯這個 480 像素的寬度，不預設檢視區寬度方面。 如此一來，使用者是比較不可能需要縮放和取景位置調整 水平 – 大幅改善行動瀏覽體驗。

如果您想檢視區寬度，以符合裝置的實體像素為單位，您可以指定下列項目：

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

這麼做才能正常運作，您必須明確地強制超過該寬度的項目 (例如，使用*寬度*屬性或 CSS 屬性)，否則瀏覽器將會強制在使用較大的檢視區不論。 另請參閱：[非標準的檢視區標記的更多詳細](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html)。

現今大部分的智慧型手機支援*雙向*： 它們可以保留在直向或橫向模式。 因此，務必不要像素為單位的螢幕寬度的相關假設。 不要甚至是假設的螢幕寬度固定的因為使用者可以重新導向他們的裝置，都在頁面上。

下列 meta 標記中，通知他們內容的頁面已經過最佳化，適用於行動裝置，並因此不應該轉換，可能也會接受較舊的 Windows Mobile 和 Blackberry 裝置。

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>其他資源

如需行動裝置模擬器，模擬器可用來測試行動裝置的 ASP.NET web 應用程式的清單，請參閱網頁[模擬熱門的行動裝置，以測試](../mobile/device-simulators.md)。

## <a name="credits"></a>參與名單

- 主要作者： Steven Sanderson
- 檢閱者 / 其他內容作者： James Rosewell、 Mikael Söderström、 Scott Hanselman、 Scott Hunter
