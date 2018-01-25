---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: "如何： 將行動網頁加入 ASP.NET Web Form / MVC 應用程式 |Microsoft 文件"
author: rick-anderson
description: "此作法 」 說明各種方法來提供適合您的 ASP.NET Web Form 中的行動裝置的網頁 / MVC 應用程式，並建議架構和..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: aac359b26c508784793a67260dc2e65c30db687a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>如何： 將行動網頁加入 ASP.NET Web Form / MVC 應用程式
====================
> **適用於**
> 
> - ASP.NET Web Form 4.0 版
> - ASP.NET MVC 3.0 版
> 
> **摘要**
> 
> 此作法 」 說明各種方法來提供適合您的 ASP.NET Web Form 中的行動裝置的網頁 / MVC 應用程式，並建議架構和設計各種裝置為目標時應該考量的問題。 本文件也會說明為何 ASP.NET 行動控制項從 ASP.NET 2.0 3.5 已經過時，並討論一些現代的替代方案。


## <a name="contents"></a>內容

- 總覽
- 架構選項
- 瀏覽器及裝置偵測
- ASP.NET Web Form 應用程式可以如何呈現行動特定頁面
- ASP.NET MVC 應用程式可以如何呈現行動特定頁面
- 其他資源

如需示範這份技術白皮書的技術，ASP.NET Web Form 和 MVC 的可下載程式碼範例，請參閱[行動應用程式 （& s) 與 ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview)。

## <a name="overview"></a>總覽

– 智慧型手機、 功能手機和平板電腦 – 行動裝置繼續中做為存取 Web 的人氣指數成長。 許多 web 開發人員及 web 導向公司，這表示它是提供絕佳的瀏覽體驗訪客使用這些裝置越來越重要。

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>如何較早版本的 ASP.NET 支援行動瀏覽器

ASP.NET 版本 2.0 到 3.5，包含*ASP.NET 行動控制項*： 一組伺服器控制項中的行動裝置的*System.Web.Mobile.dll*組件和*System.Web.UI.MobileControls*命名空間。 在 ASP.NET 4 中，仍然包含該組件，但已被取代。 開發人員建議移轉到更現代的方法，例如這份文件中所述。

ASP.NET 行動控制項為什麼標示為過時的原因是其設計導向周圍常見周圍 2005年和更早版本的行動電話。 該紀元的 WAP 瀏覽器，控制項主要被設計來呈現 WML 或 cHTML 標記 （而不是標準 HTML)。 但 WAP、 WML 和 cHTML 不再相關的大部分的專案，因為 HTML 現在已成為行動和桌面瀏覽器，類似於無所不在標記語言。

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>現在支援行動裝置的挑戰

雖然行動瀏覽器時，現在幾乎都支援 HTML，您仍然會面臨許多挑戰時以期能讓建立絕佳的行動瀏覽經驗：

- ***螢幕大小***-行動裝置明顯不同表單中，和他們的螢幕通常是大部分小於桌上型監視器。 因此，您可能需要設計完全不同的版面配置。
- ***輸入方法***– 某些裝置具有 keypads，某些 styluses，而其他則使用觸控。 您可能需要考慮多個瀏覽機制和資料的輸入的法。
- ***標準相容性***– 多行動瀏覽器不支援的最新的 HTML、 CSS 或 JavaScript 標準。
- ***頻寬***– 行動數據網路效能變化 （implementation），而某些使用者位於關稅收費的 mb 數。

沒有一體適用的解決方案。您的應用程式必須的外觀和行為以不同的方式根據裝置存取它。 根據您要何種行動裝置層級，這可能是支援的更大的挑戰 web 開發人員比桌面 「 瀏覽器戰爭"曾經。

一開始通常接近的第一次行動瀏覽器支援的開發人員認為務必只支援最新和最高性能智慧型手機 （例如 Windows Phone 7、 iPhone 或 Android） 時，可能是因為開發人員通常個人擁有這類裝置。 不過，仍非常受歡迎，成本較低的電話，其擁有者執行並用來瀏覽網頁 – 尤其是在您更輕鬆地取得比寬頻連線的行動電話所在的國家 （地區）。 您的業務需要決定哪些範圍的裝置來支援以方法為考慮其可能購買的客戶。 如果您要建置的 luxury 健全狀況 spa 線上冊，您就可能會讓只為目標進階的智慧型手機，商業決策，而如果您要建立影片的票證預約系統，您可能需要較不強大的功能與訪客帳戶電話。

## <a name="architectural-options"></a>架構選項

我們要的 ASP.NET Web Form 或 MVC 的特定技術詳細資料之前，請注意，web 開發人員通常有三個主要的可能選項，可支援行動瀏覽器：

1. ***不執行任何動作-***可以只建立標準的桌面導向的 web 應用程式，並依賴行動瀏覽器進行水平時轉譯。 

    - **利用**： 它是實作和維護 – 不需要額外工作最便宜的選項
    - **缺點**： 提供的最差的使用者體驗： 

        - 最新的智慧型手機可能呈現您的 HTML 一樣桌面瀏覽器，但縮放和捲動水平與垂直取用您小型螢幕上的內容，仍會強制使用者。 這是遠低於最佳化。
        - 較舊的裝置和功能的電話可能無法令人滿意的方式呈現您的標記。
        - 即使在最新平板電腦的裝置上 （其螢幕可以是膝上型電腦螢幕的大小），套用不同的互動規則。 觸控輸入最適合具有較大的按鈕連結擴散的進一步切割，並沒有任何方法可以將滑鼠游標暫留飛出視窗功能表。
2. ***解決這個問題，在用戶端上的*–**小心使用 CSS 和[漸進式增強功能](http://en.wikipedia.org/wiki/Progressive_enhancement)標記、 樣式和指令碼，以調整為任何瀏覽器執行它們，您可以建立。 例如，與[CSS 3 媒體查詢](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/)，您可以建立多欄版面配置，以開啟到單一資料行配置裝置的螢幕較窄比選擇的閾值。 

    - **利用**： 最佳化中使用，即使根據任何螢幕和輸入的特性它們有未知的新款裝置的特定裝置的轉譯
    - **利用**： 輕鬆可讓您跨所有裝置類型 – 最小的重複程式碼或投入時間的共用伺服器端邏輯
    - **缺點**： 行動裝置會因此不同桌上型裝置，您可能會真的要行動頁面是完全不同於您桌面的網頁，顯示不同的相關資訊。 這類變化可能會沒有效率或不可能達成穩當地 CSS 光是透過特別考慮如何不一致較舊的裝置解譯 CSS 規則。 這是非常有用的 CSS 3 屬性。
    - **缺點**： 不提供支援各種不同的伺服器端邏輯和工作流程中的不同的裝置。 您，例如無法透過單獨 CSS 實作簡化購物車簽出工作流程為行動使用者。
    - **缺點**： 效率不佳的頻寬用量。 您的伺服器可能要傳輸的標記和樣式套用至所有可能的裝置，即使目標裝置只會使用該資訊的子集。
3. ***解決這個問題，在伺服器上的*–**如果您的伺服器可讓您知道哪些裝置正在存取它-，或在該裝置，例如其螢幕大小和輸入的法，最低的特性，無論是行動裝置-網域控制站可以執行不同的邏輯和輸出不同的 HTML 標記。 

    - **優點：**最大的彈性。 沒有任何限制，您可以在多少改變您的伺服器端邏輯的 asp.net 或最佳化您想要的裝置特定配置的標記。
    - **優點：**有效率的頻寬用量。 您只需要傳輸的標記和目標裝置所要使用的樣式資訊。
    - **缺點：**有時候會強制投入時間量或程式碼的重複 （例如，讓您建立 Web Form 網頁的 MVC 檢視相似但稍微不同的複本）。 其中可能會分解一般邏輯為基礎的圖層或服務，但儘管如此，您的 UI 程式碼或標記的某些部分可能需要複製而且以平行方式加以維護。
    - **缺點：**裝置偵測並非易事。 它需要的清單或已知的裝置類型和它們的特性 （這不一定完全保持最新狀態） 的資料庫，並不保證能夠正確地符合每個傳入要求。 本文件稍後說明某些選項，且其犯的錯誤。

若要取得最佳結果，大部分的開發人員會發現他們需要結合選項 (2) 和 (3)。 次要文體差異使用 CSS 或甚至 JavaScript，在用戶端上最適合可調整，而資料、 工作流程或標記中的主要差異最有效地實作伺服器端程式碼中。

### <a name="this-paper-focuses-on-server-side-techniques"></a>本文著重於伺服器端技術

因為 ASP.NET Web Form 和 MVC 是這兩種主要伺服器端技術，這份技術白皮書將著重於伺服器端技術，可讓您產生不同的標記和行動瀏覽器的邏輯。 當然，您也可以結合這與任何用戶端技術 （例如，CSS 3 媒體查詢，JavaScript 漸進式增強功能），但是，就是更網頁設計比 ASP.NET 程式開發。

## <a name="browser-and-device-detection"></a>瀏覽器及裝置偵測

如需支援行動裝置的所有伺服器端技術金鑰先決條件是，知道您的訪客使用哪一種裝置。 事實上，甚至更好了解該裝置的製造商和型號號碼知道*特性*的裝置。 特性可能包括：

- 為行動裝置嗎？
- 輸入的法 （滑鼠] / [鍵盤、 觸控、 數字鍵台、 搖桿，...）
- 螢幕大小 （實體和像素為單位）
- 支援的媒體和資料格式
- Etc.

最好是讓特性與模型數目為基礎的決策，因為，然後您會更有能力處理新款裝置。

### <a name="using-aspnets-built-in-browser-detection-support"></a>使用 ASP。網路的內建瀏覽器偵測支援

ASP.NET Web Form 和 MVC 的開發人員可以藉由檢查的內容立即探索造訪瀏覽器的重要特性*Request.Browser*物件。 例如，請參閱

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ..和許多其他

在幕後，ASP.NET 平台符合傳入*User-agent* (UAC) 的 HTTP 標頭，針對一組瀏覽器定義 XML 檔案中的規則運算式。 預設平台包括定義許多常見的行動裝置，而且您可以加入自訂瀏覽器定義檔案代表您想要識別的其他項目。 如需詳細資訊，請參閱 MSDN 頁面[ASP.NET Web 伺服器控制項和瀏覽器能力](https://msdn.microsoft.com/library/x3k2ssx2.aspx)。

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>使用透過 51Degrees.mobi Foundation WURFL 裝置資料庫

雖然 ASP。網路的內建瀏覽器偵測支援將足以說明許多應用程式時，有兩種主要的情況下它可能不夠：

- ***您想要識別最新裝置***（而不需要手動建立它們的瀏覽器定義檔案）。 請注意，.NET 4 的瀏覽器定義檔案不是不夠新，無法辨識 Windows Phone 7、 Android 手機、 Opera Mobile 瀏覽器或 Apple Ipad。
- ***您需要裝置功能的詳細的資訊***。 您可能需要了解裝置的輸入法 （例如觸控 vs 字鍵台），或哪些音訊格式瀏覽器支援。 這項資訊無法使用標準的瀏覽器定義檔案中。

[*無線通用資源檔*(WURFL) 專案](http://wurfl.sourceforge.net/)維護更多保持最新狀態和詳細資訊在使用行動裝置今天。

適用於.NET 開發人員最棒的工具是該 ASP。網路的瀏覽器偵測功能，所以可強化克服這些問題。 例如，您可以在其中加入開放原始碼[ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection)至您的專案程式庫。 是瀏覽器功能提供者 （可用於 Web Form 和 MVC 應用程式），會直接讀取 WURFL 資料並將它連結到 ASP 或 ASP.NET IHttpModule。網路的內建瀏覽器偵測機制。 一旦您已安裝模組， *Request.Browser*會突然包含許多比較精確和詳細資訊： 則會正確辨識許多先前所述的裝置，並列出其功能 （包括其他功能，例如輸入法）。 請參閱專案的文件，如需詳細資訊。

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Web Form 應用程式可以如何呈現行動特定頁面

根據預設，以下是常見的行動裝置上顯示新的 Web Form 應用程式的方式：

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

很明顯地，未配置看起來非常行動設備友善 – 此頁面針對大型、 橫向的監視器，不適用於小型縱向導向螢幕的設計。 因此可以您做什麼資訊？

如本文稍早所述，有許多方式來調整您的行動裝置的網頁。 有些技術則是伺服器為基礎，其他用戶端上執行。

### <a name="creating-a-mobile-specific-master-page"></a>建立行動裝置特定的主版頁面

根據您的需求，您可以使用相同的 Web Form 所有造訪者，但有兩個不同的主版頁面： 一個用於桌上型電腦的訪客，另一個用於行動裝置的訪客。 這可讓您變更 CSS 樣式表或您最上層的 HTML 標記，以符合行動裝置，而不未強制您重複的任何頁面邏輯的彈性。

這是容易的。 例如，您可以加入 PreInit 處理常式，如下所示的 Web 表單：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

現在，建立您的應用程式的最上層資料夾中呼叫 Mobile.Master 主版頁面，將會在偵測到行動裝置時使用。 視您行動主要頁面可以參考特定行動裝置版的 CSS 樣式表。 桌面的訪客還是會看到您預設主版頁面上，行動非。

### <a name="creating-independent-mobile-specific-web-forms"></a>建立獨立的行動裝置特定 Web Form

最大的彈性，您可以深入比有不同的主版頁面進行不同的裝置類型。 您可以實作兩個*完全分隔的 Web Form 網頁的設定*– 一個設定的桌面瀏覽器，另一個設定行動裝置的 asp.net。 這最適合如果您想要呈現給行動裝置的訪客的非常不同的資訊或工作流程。 本章節的其餘部分將說明這種方法，在詳細資料。

假設您已經有的桌面瀏覽器所設計的 Web Form 應用程式，是建立在您的專案中，稱為 「 行動 」 的子資料夾，並建置行動頁面有最簡單的方式繼續進行。 您可以建構完整的子站台，與它自己的主版頁面、 樣式表和頁面，使用完全相同的技巧，您會使用任何其他 Web Form 應用程式。 您不需要產生行動裝置對等*每*頁面中您桌面的網站; 您可以選擇哪些功能子集的合理行動訪客。

您的行動網頁可以共用一般靜態資源 (例如影像、 JavaScript 或 CSS 檔案) 您一般的頁面，如果您想使用。 「 行動 」 資料夾將會因為*不*標示為個別的應用程式裝載在 IIS 中 （它是只 Web Form 網頁的簡單子資料夾），它會也共用所有相同設定、 工作階段資料和其他基礎結構做為您桌面的頁面。

> [!NOTE]
> 由於這種方法通常牽涉到某些程式碼的重複 （行動頁面可能會與桌面的網頁共用一些相似之處），請務必出任何一般商務邏輯或資料存取程式碼至共用的基礎層或服務的因素。 否則，您將 double 建立和維護您的應用程式的投入時間。


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>重新導向至您的行動網頁行動訪客

通常很方便只在重新導向行動頁面行動訪客*第一個*要求其瀏覽工作階段中，不是在其工作階段中的每個要求，因為：

- 您可以輕鬆地讓行動裝置存取您桌面的網頁，當中 – 只要您前往 「 桌面版本 」 的主版頁面上放置連結訪客。 造訪者將不會重新導向回到行動裝置 頁面上，因為它已不再第一個要求其工作階段中。
- 它可避免干擾要求的任何動態資源 （例如，如果您有網站的桌面和行動裝置版的組件會顯示在 IFRAME 中或特定 Ajax 處理常式中的通用 Web 表單），您的站台桌面和行動裝置版的組件之間共用的風險

若要這樣做，您可以將放在您重新導向邏輯**工作階段\_啟動**方法。 例如，將下列方法加入 Global.asax.cs 檔案：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>設定表單驗證以尊重您的行動裝置頁面

請注意，表單驗證會使特定假設，它可以將重新導向訪客期間和之後驗證程序：

- 當使用者需要驗證時，表單驗證將會重新導向至您的桌面登入頁面，不論它們是否桌面或行動裝置的使用者 (因為它只需要的概念*一個*登入 URL)。 假設您想要以不同的方式設定您的行動裝置的登入頁面的樣式，您需要加強桌面登入頁面，以便將行動裝置的使用者重新導向至不同的行動裝置登入頁面。 例如，加入下列程式碼加入您**桌面**登入頁面程式碼後置： 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- 使用者成功登入之後，表單驗證會根據預設將他們重新導向到您桌面的首頁 (因為它只需要的概念*一個*預設頁面)。 您要增強您的行動裝置的登入頁面，使它會重新導向至行動首頁之後成功登入。 例如，加入下列程式碼加入您**行動**登入頁面程式碼後置： 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
 此程式碼假設您的網頁有稱為 LoginUser，如所示的預設專案範本 」 的登入伺服器控制項。

### <a name="working-with-output-caching"></a>使用輸出快取

如果您使用輸出快取，請注意，根據預設，它是桌面的使用者，可以造訪 （導致快取其輸出） 的特定的 URL，後面接著就會接收的快取的桌面輸出的行動使用者。 不論您只是不同的主版頁面依裝置類型或實作完全不同的 Web Form，每個裝置類型，這項警告皆適用。

若要避免此問題，您可以指示要變更快取項目，根據是否訪客使用行動裝置的 ASP.NET。 VaryByCustom 將參數加入至您的頁面 OutputCache 宣告，如下所示：

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

接下來，定義*isMobileDevice*做為自訂快取新增下列方法的參數會覆寫以 Global.asax.cs 檔案：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

這可確保行動網頁訪客沒有收到先前置於快取中的桌面的訪客的輸出。

### <a name="a-working-example"></a>工作範例

若要查看作用中的這些技術，請下載[這份技術白皮書的程式碼範例](https://docs.microsoft.com/aspnet/mobile/overview)。 Web Form 範例應用程式會自動重新導向至行動使用者一組稱為 「 行動裝置版的子資料夾中的行動特定頁面。 標記和那些頁面的樣式更適合行動瀏覽器，您可以看到從下列螢幕擷取畫面：

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

更多秘訣行動瀏覽器最佳化您的標記和 CSS 的詳細資訊，請參閱 「 樣式行動頁面行動瀏覽器 」 在本文件稍後的一節。

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>ASP.NET MVC 應用程式可以如何呈現行動特定頁面

由於模型-檢視-控制器模式可分隔 （在控制站） 的應用程式邏輯，從呈現邏輯 （在檢視中），您可以選擇從任何下列方式來處理伺服器端程式碼中的行動支援：

1. ***桌上型電腦和行動瀏覽器，使用相同的控制器和檢視，但會呈現不同的 Razor 版面配置，根據裝置類型 * 檢視。** 如果您在所有的裝置上顯示相同的資料，但只想要提供不同的 CSS 樣式表，或是變更幾個最上層的 HTML 項目，行動裝置的 asp.net，這個選項最有用。
2. ***對於桌面和行動裝置版的瀏覽器，使用相同的控制站，但會呈現每種裝置類型的不同檢視***。 如果您使用大約顯示相同的資料，對於使用者，提供相同的工作流程，這個選項最有用，但要轉譯非常不同的 HTML 標記，以符合所用的裝置。
3. ***建立桌面和行動瀏覽器，實作無關的控制器和檢視每個不同的區域 *。** 如果您正在顯示非常不同的螢幕，包含不同的資訊，導致使用者透過不同的工作流程適合他們的裝置類型，這個選項最有用。 這可能表示部分重複程式碼，但您可以最小化，析出至基礎的圖層或服務的一般邏輯。

如果您想要採取**第一個**選項而有所不同 Razor 版面配置每個裝置類型，是非常簡單。 只需修改您\_ViewStart.cshtml 檔案，如下所示：

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

現在您可以建立行動裝置特定版面配置，稱為\_LayoutMobile.cshtml 與頁面結構和 CSS 規則針對行動裝置最佳化。

如果您想要採取**第二個**選項呈現根據訪客的裝置類型完全不同的檢視，請參閱[Scott Hanselman 部落格文章](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx)。

這份文件的其餘部分著重於**第三個**選項 – 建立個別的控制站*和*行動裝置 – 讓您能夠控制對行動裝置的訪客提供哪些功能子集的完全檢視。

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>設定 ASP.NET MVC 應用程式中的行動裝置的區域

您可以新增以一般方式稱為 「 行動 」，在現有的 ASP.NET MVC 應用程式的區域： 在方案總管] 中，您的專案名稱上按一下滑鼠右鍵，然後選擇 [新增 à 區域。 在 ASP.NET MVC 應用程式內的任何其他區域一樣，您可以再新增控制器和檢視。 例如，將加入至您行動區域新的控制器呼叫 HomeController 作為行動裝置的訪客的首頁。

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>確保 URL /Mobile 到達行動首頁

如果您想 URL /Mobile 到達 HomeController 您行動區域內的索引動作，您必須進行兩個小變更路由設定。 首先，更新 MobileAreaRegistration 類別使 HomeController 預設的控制站在行動裝置的區域中，如下列程式碼所示：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

這表示行動首頁現在即將放置 /Mobile，而非行動/首頁，因為 「 家用 」 現在是以隱含方式控制站的預設名稱行動頁面。

接下來，請注意，將第二個 HomeController 加入至您的應用程式 （也就是行動一，除了現有的桌面一個），您將會中斷一般桌面首頁。 它將會失敗，發生錯誤 「*符合控制器名稱為 '首頁' 找到多個型別*"。 若要解決此問題，更新 （中 Global.asax.cs) 的最上層路由組態來指定您的桌面 HomeController 在模稜兩可時，應該採取優先順序：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

現在會離開和 URL http:// 錯誤*您的站台*會到達桌面的首頁和 http:// /*您的站台*/mobile/ 會到達行動首頁。

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>重新導向至您行動區域行動訪客

因此有許多可能的方法，將重新導向邏輯中 ASP.NET MVC 中，有許多不同的擴充點。 一個很棒的選項是建立一個篩選屬性、 [RedirectMobileDevicesToMobileArea]，執行重新導向，如果符合下列條件：

1. 它是使用者工作階段中的第一個要求 （亦即，Session.IsNewSession 等於 true）
2. 要求來自行動瀏覽器 （亦即，Request.Browser.IsMobileDevice 等於 true）
3. 使用者不已要求行動區域中的資源 (亦即，*路徑*URL 部分的開頭不是 /Mobile)

包含與這份技術白皮書的可下載範例包含實作此邏輯。 它會實作為衍生自 AuthorizeAttribute，這表示它在即使您使用輸出快取 （否則如果桌面訪客第一次存取特定 URL，桌面的輸出可能會快取，並提供服務到可以正確運作的授權篩選條件後續行動訪客）。

因為它是篩選器，您可以選擇要套用到特定的控制器和動作例如

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… 或套用至所有控制器和動作為 MVC 3*全域篩選*Global.asax.cs 檔案中：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

可下載的範例也示範如何建立此屬性的重新導向至您行動區域內的特定位置的子類別。 這表示，例如，您可以：

- 註冊全域篩選為顯示上述的預設傳送行動首頁行動訪客。
- 也要採取行動的訪客便具有要求所有產品網頁的行動裝置版本的 「 檢視產品 」 動作套用特殊的 [RedirectMobileDevicesToMobileProductPage] 篩選器。
- 也適用於特定的動作，重新導向至相同行動頁面行動訪客的 篩選器的其他特殊子類別

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>設定表單驗證以尊重您的行動裝置頁面

如果您使用表單驗證，您應該注意，當使用者需要登入，它會自動將使用者導向至單一特定 「 登入 」 的 URL，其預設值是**/帳戶/登入**。 這表示行動使用者，可能會重新導向到您桌面 「 登入 」 的動作。

若要避免這個問題，邏輯加入您桌面的 「 登入 」 動作，以便將重新導向行動使用者一次行動裝置的 「 登入 」 動作。 如果您使用預設 ASP.NET MVC 應用程式範本，更新 AccountController 的登入的動作，如下所示：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… 然後實作適當的行動裝置特定 「 登入 」 動作稱為 AccountController 行動當地的控制站上。

### <a name="working-with-output-caching"></a>使用輸出快取

如果您使用 [OutputCache] 篩選器，您必須強制快取項目，若要依裝置類型。 例如，寫入：

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

然後，將下列方法加入至 Global.asax.cs 檔案：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

這可確保行動網頁訪客沒有收到先前置於快取中的桌面的訪客的輸出。

### <a name="a-working-example"></a>工作範例

若要查看作用中的這些技術，請下載[這份技術白皮書的程式碼相關聯的範例](https://docs.microsoft.com/aspnet/mobile/overview)。 此範例包含 ASP.NET MVC 3 （發行候選版本） 應用程式增強為支援行動裝置使用上面所述的方法。

## <a name="further-guidance-and-suggestions"></a>進一步的指引和建議

下列討論適用於 Web Form 和 MVC 開發人員使用本文件涵蓋的技術。

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>強化您的重新導向邏輯使用 51Degrees.mobi Foundation

本文件中所示的重新導向邏輯可能完全足以用於您的應用程式，但如果您要停用工作階段，將無法運作或與拒絕的 cookie （這些不能有工作階段），因為它不會知道是否指定要求的行動瀏覽器第一個從該訪客。

您已學到如何改善開放原始碼 51Degrees.mobi Foundation ASP 的精確度。網路的瀏覽器偵測。 它也有內建的功能，將重新導向至特定位置，在 Web.config 中設定的行動訪客。可以運作，而不根據 ASP.NET 工作階段 (因此 cookie) 儲存的雜湊訪客 HTTP 標頭及 IP 位址的暫存記錄檔，使它知道每一個要求是否從給定 vistor 的第一個。

下列項目新增至 web.config 檔 fiftyOne 區段將會重新導向至頁面偵測到的行動裝置從第一個要求 ~ / Mobile/Default.aspx。 將頁面行動資料夾下的任何要求*不*重新導向，無論裝置類型。 如果行動裝置未作用一段時間的 20 分鐘或更多裝置將遭到忽略，後續要求將會被視為新的用途的重新導向。

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

如需詳細資訊，請參閱[51degrees.mobi Foundation 文件](https://github.com/51Degrees/dotNET-Device-Detection)。

> [!NOTE]
> 您*可以*使用 51Degrees.mobi Foundation 的重新導向功能，在 ASP.NET MVC 應用程式，但是您必須定義以純文字的 Url，不會根據路由的參數或將 MVC 篩選條件放入您重新導向設定在 [動作]。 這是因為 （在寫入時間） 51Degrees.mobi Foundation 無法辨識或路由篩選器。


### <a name="disabling-transcoders-and-proxy-servers"></a>停用轉錄和 Proxy 伺服器

行動網路業者行動網際網路的方法中有兩個廣泛的目標：

1. 提供做為儘可能多相關內容
2. 可以共用限制的廣播網路頻寬的客戶數目最大化

由於大部分的網頁是針對較大的桌面大小螢幕並快速修正列寬頻連線所設計，許多運算子會使用*轉錄*或*proxy 伺服器*，動態變更 web 內容。 它們可能會修改您的 HTML 標記或 CSS 以配合較小的螢幕 （特別是針對 「 功能的電話 」 缺少處理能力來處理複雜的配置），和它們可能會重新映像 （大幅減少其品質） 壓縮以改善網頁傳遞的速度。

但是，如果您已取得所花的心力產生行動最佳化您站台的版本，您可能不想干擾它任何進一步的網路業者。 您可以將下行加入頁面\_中任何 ASP.NET Web Form 的載入事件：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

或者，ASP.NET MVC 控制器，您可以加入下列方法覆寫，讓它套用到該控制器上的所有動作：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

產生的 HTTP 訊息告知 W3C 相容轉錄且不改變內容的 proxy。 當然，是行動網路運算子會遵守此訊息不保證。

### <a name="styling-mobile-pages-for-mobile-browsers"></a>設定樣式行動瀏覽器的行動裝置的頁面

它超出範圍的這份文件中有詳細描述哪些類型的 HTML 標記工作正確，或哪些 web 設計技巧將特定裝置上的可用性最大化。 它具有向上，尋找夠簡單的版面配置，讓您最佳化行動大小螢幕，而不使用不可靠 HTML 或 CSS 位置訣竅。 不過，是一個重要的技術，值得一提，*檢視區 meta 標記*。

某些現代化行動瀏覽器，適用於桌上型監視器，投入時間顯示網頁中的呈現在虛擬畫布上，也稱為 「 檢視區 」 頁面 （例如虛擬檢視區為 980 像素寬、 iPhone、 上和 850 像素寬、 Opera Mobile 上依預設），然後向下調整結果，以符合畫面的實體像素。 使用者即可放大或放大的檢視區。 這樣做的好處，它可讓瀏覽器顯示頁面在預定的配置中，但也有缺點，它會強制縮放和平移的使用者很不方便。 如果您在設計行動應用程式，最好設計窄的畫面，因此不需要的任何縮放或水平捲軸。

指示行動瀏覽器檢視區應該是寬度的方法是透過使用非標準*檢視區*meta 標記。 例如，如果您的頁面標頭 區段中，加入下列

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… 然後支援智慧型手機瀏覽器會配置 480 像素的範圍虛擬畫布上的頁面。 這表示，如果您的 HTML 項目會定義其寬度，以百分比表示，此百分比會解譯相對於這個 480 像素寬度，沒有預設檢視區寬度。 如此一來，使用者是比較不可能對縮放和平移水平 – 大幅改善行動瀏覽經驗。

如果您想要比對裝置的實體像素的檢視區寬度時，您可以指定下列各項：

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

此正常運作，您必須明確地強制超過該寬度的項目 (例如，使用*寬度*屬性或 CSS 屬性)，否則瀏覽器將會強制在使用較大的檢視區而定。 另請參閱：[更多詳細的非標準的檢視區標記](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html)。

大多數最新型的智慧型手機支援*雙重方向*： 直向或橫向模式中可以保存它們。 因此，務必不提出任何假設的螢幕寬度 （像素）。 請勿甚至假設的螢幕寬度固定的因為使用者可以重新設定他們的裝置，都在頁面上。

下列中繼標籤中，通知他們內容的頁面已經過最佳化，行動應用程式，因此不應該轉換，可能也會接受較舊的 Windows Mobile 和 Blackberry 裝置。

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>其他資源

如需行動裝置模擬器，模擬器可用來測試您行動裝置的 ASP.NET web 應用程式的清單，請參閱網頁[模擬熱門的行動裝置進行測試](../mobile/device-simulators.md)。

## <a name="credits"></a>參與名單

- 主要作者： Steven Sanderson
- 檢閱者 / 其他內容寫入器： James Rosewell、 黃冠 Söderström、 Scott Hanselman、 Scott 獵人
