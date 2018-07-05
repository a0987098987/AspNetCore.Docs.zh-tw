---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: 了解 ASP.NET AJAX 當地語系化 |Microsoft Docs
author: scottcate
description: 當地語系化是設計及整合應用程式或應用程式元件的支援特定語言和文化特性的程序。 Mic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 7f089e147ae9c4c42da0ca798149488043480a79
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382504"
---
<a name="understanding-aspnet-ajax-localization"></a>了解 ASP.NET AJAX 當地語系化
====================
藉由[Scott Cate](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> 當地語系化是設計及整合應用程式或應用程式元件的支援特定語言和文化特性的程序。 Microsoft ASP.NET 平台進行標準的 ASP.NET 應用程式的當地語系化可廣泛支援，藉由整合標準的.NET 當地語系化模型;Microsoft AJAX 架構利用整合式的模型，以支援各種可以在其中執行當地語系化的情況。


## <a name="introduction"></a>簡介

Microsoft ASP.NET 技術會以物件導向和事件導向的程式設計模型，以及結合編譯程式碼的優點。 不過，其伺服器端處理模型有幾個缺點固有的技術，而其中有許多您可以解決包含封裝在.NET Framework 中的 Microsoft AJAX 服務 System.Web.Extensions 命名空間中的新功能3.5。 這些擴充功能可讓許多豐富的用戶端可用的功能先前為 ASP.NET 2.0 AJAX Extensions 中的一部分，但現在的 Framework 基底類別庫的一部分。 控制項和功能，此命名空間中的包含部分呈現的頁面，而不需要重新整理整個頁面，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API），存取 Web 服務的能力，而且廣泛的用戶端 API 設計以鏡像的許多在 ASP.NET 伺服器端控制項集合中看到控制項配置。

本白皮書會檢查在 Microsoft AJAX Framework 和 Microsoft AJAX 指令碼程式庫，支援當地語系化和檢閱已經整合支援當地語系化 web 中的商務需求的內容中存在的當地語系化功能.NET Framework 所提供的應用程式。 Microsoft AJAX 指令碼程式庫會使用.resx 檔案格式已經由.NET 應用程式，可提供整合的 IDE 支援，以及可共用的資源類型。

本白皮書為基礎的 Microsoft Visual Studio 2008 Beta 2 版本上。 本白皮書也會假設您將使用 Visual Studio 2008，不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面。 某些程式碼範例會利用可能無法在 Visual Web Developer Express 的專案範本。

## <a name="the-need-for-localization"></a>*需要當地語系化*

特別是針對企業應用程式開發人員和元件開發人員，能夠建立工具，可以很清楚的文化特性和語言之間的差異變得越來越必要。 設計元件能夠適應的用戶端的地區設定時，會增加開發人員的生產力，並減少元件調節全域函式所需的工作量。

當地語系化是設計及整合應用程式或應用程式元件的支援特定語言和文化特性的程序。 Microsoft ASP.NET 平台進行標準的 ASP.NET 應用程式的當地語系化可廣泛支援，藉由整合標準的.NET 當地語系化模型;Microsoft AJAX 架構利用整合式的模型，以支援各種可以在其中執行當地語系化的情況。 Microsoft AJAX 架構中，所要部署到附屬組件，或藉由使用靜態檔案系統結構可能可以當地語系化指令碼。

## <a name="embedding-scripts-with-satellite-assemblies"></a>*使用附屬組件的內嵌指令碼*

標準的.NET Framework 當地語系化策略與一致，資源可以包含在附屬組件。 附屬組件提供數個優點高於傳統的資源包含二進位檔-在任何指定的本地化可以更新而不需要更新較大的映像，可以部署額外的當地語系化資源，只要安裝附屬組件載入可以部署的專案資料夾和附屬組件，而不會造成主要專案組件的重新載入。 特別是在 ASP.NET 專案，這會有幫助，因為它可大幅減少累加式更新時，所使用的系統資源數量，並以最低限度中斷生產網站。

指令碼時，會內嵌至組件上，它們包括在管理.resx （或編譯.resources） 的組件在編譯時期包含的檔案。 其資源會開放給指令碼應用程式，透過 AJAX 執行階段產生的程式碼，透過組件層級屬性

*內嵌指令碼檔案的命名慣例*

Microsoft AJAX 架構指令碼管理支援各種不同的選項，以用於部署和測試指令碼，並提供指導方針來加速這些選項。

*為了方便偵錯：*

發行 （生產） 指令碼不應包含`.debug`檔案名稱中的限定詞。 指令碼設計應包括偵錯`.debug`檔案名稱中。

*若要協助當地語系化：*

中性文化特性的指令碼不應包含任何檔案名稱的文化特性識別項。 針對包含當地語系化的資源的指令碼，應指定 ISO 語言程式碼中的檔案名稱。 比方說，`es-CO`代表西班牙文、 哥倫比亞省。

下表摘要說明的檔案命名慣例範例：

| Filename | 意義 |
| --- | --- |
| Script.js | 發行版本文化特性中性的指令碼。 |
| Script.debug.js | 偵錯版本的文化特性中性的指令碼。 |
| Script.en-US.js | 發行版英文，美國指令碼。 |
| Script.debug.es-CO.js | 偵錯版本西班牙文，哥倫比亞指令碼。 |

## <a name="walkthrough-create-an-localized-embedded-script"></a>逐步解說： 建立當地語系化的內嵌指令碼

*請注意： 本逐步解說需要 Visual Studio 2008 使用，因為 Visual Web Developer Express 不包括類別庫專案的專案範本。*

1. 使用整合的 ASP.NET AJAX Extensions 中建立新的網站專案。 建立另一個專案，稱為 LocalizingResources 方案內的類別庫專案。
2. 新增至 LocalizingResources 專案，稱為 VerifyDeletion.js Jscript 檔案，以及稱為 DeletionResources.resx 和 DeletionResources.es.resx.resx 資源檔。 前者會包含文化特性中性的資源;後者會包含西班牙文語言資源。
3. 將下列程式碼新增至 VerifyDeletion.js 中：

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

對於熟悉 JavaScript 的 Regex 語法，在單一的正斜線的文字 （在上述範例中，/FILENAME/ 是舉例） 表示的 RegExp 物件。 MSDN Library 包含廣泛的 JavaScript 參考，線上找到 JavaScript 原生物件上的資源。

1. 將下列的資源字串加入至 DeletionResources.resx 中： 

    **VerifyDelete**： 確定您想要刪除的檔案名稱嗎？

    **刪除**： 已刪除的檔案名稱。

1. 將下列的資源字串加入至 DeletionResources.es.resx 中： 

    **VerifyDelete**: Est seguro 查詢 desee quitar FILENAME 嗎？

    **刪除**: FILENAME se ha quitado。
2. AssemblyInfo 檔案中加入下列程式碼行：

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. LocalizingResources 專案中加入 System.Web 和 System.Web.Extensions 的參考。
2. 新增 LocalizingResources 專案中的網站專案的參考。
3. 在 default.aspx 中，在網站專案中，請使用下列額外的標記更新 ScriptManager 控制項：

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. 在 default.aspx 中，在頁面上，任何一處包含此標記：

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. 按 F5。 出現提示時，啟用 偵錯。 載入頁面時，請按 [刪除] 按鈕。 請注意，系統會提示您以英文 （除非您的電腦設定為慣用西班牙文語言資源上，依預設） 進行確認。
2. 關閉瀏覽器視窗並返回 default.aspx。 在 @Page標頭指示詞，取代附有的 Culture 與 UICulture ES-ES。 再次按 F5 以啟動 web 應用程式在瀏覽器中的一次。 這次請注意，系統會提示您刪除西班牙文中的檔案：


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-localization/_static/image6.png))


請注意，在此逐步解說的數個變化。 比方說，指令碼無法向 ScriptManager 控制項以程式設計方式在載入頁面時。

## <a name="including-a-static-script-file-structure"></a>*包含靜態指令碼檔案結構*

當部署使用靜態指令碼檔案，可能會遺失部分使用的固有的.NET 當地語系化配置的優點。 顯示主要是讓您將無法自動從包含指令碼資源檔; 產生的型別例如，在上述逐步解說中，資源已公開所自動產生的型別呼叫從 ScriptManager 控制項的訊息。

有，不過，若要使用靜態指令碼檔案結構的一些優點。 可以執行更新，不必重新編譯和重新部署附屬組件，並使用靜態檔案結構也可以覆寫內嵌指令碼，將次要的一種可能尚未運送的功能與元件。

Microsoft 建議您避免版本控制問題，自動在專案編譯期間產生的指令碼資源。 當維護廣泛的指令碼程式碼基底，它可能會變得越來越難確保程式碼變更會反映在每個當地語系化的指令碼。 或者，您可以只維護一個邏輯的指令碼和多個當地語系化的指令碼，建置專案時，合併檔案。

因為不是以宣告方式包含的資源，靜態指令碼檔案都應參考藉由新增`<asp:ScriptElement>`元素的子系`<Scripts>`標記的 ScriptManager 控制項，或以程式設計方式加入`ScriptReference`物件若要`Scripts`屬性`ScriptManager`在執行階段頁面上的控制項。

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager 和它在當地語系化的角色*

ScriptManager 可讓多個當地語系化的應用程式的自動行為：

- 它會自動找出設定，以及命名慣例; 為基礎的指令碼檔案比方說，它會載入偵錯啟用在偵錯模式中的指令碼，並載入當地語系化根據瀏覽器的使用者介面選項的指令碼。
- 它讓您定義的文化特性，包括自訂文化特性。
- 它會透過 HTTP 啟用指令碼檔案的壓縮。
- 它會快取指令碼，以有效率地管理許多要求。
- 它會加入一層間接取值的指令碼傳遞它們透過加密的 URL。

指令碼參考可以透過程式設計方式或宣告式標記加入 ScriptManager 控制項。 宣告式標記會特別有用的而非網站專案本身，內嵌在組件時使用指令碼時，指令碼的名稱可能不會變更為推入修訂。

## <a name="summary"></a>總結

Web 應用程式成長到大量對象，必須能夠連線到更廣泛的文化特性和社群會變得核心商業模型;電子商務 web 應用程式必須能夠處理外部貨幣，內容管理系統需要其內容，但也他們瀏覽提示和其他語言，與公司中的表單欄位需要知道這項需求是能夠不只存在可存取。

.NET Framework 本質上支援豐富的當地語系化的架構，利用附屬組件和 XML 資源 (.resx) 檔案來提供統一的方式，來查閱資源字串和映像。 ASP.NET AJAX Extensions，包括 Microsoft AJAX 架構和 Microsoft AJAX 指令碼程式庫，提供支援對此程式設計模型為用戶端程式碼，讓您輕鬆的資源字串查閱。 附屬組件會支援自動包含透過 ScriptResource.axd 的指令碼資源 （實際的.js 檔案），只要檔案名稱依照指定的命名配置。 透過這項支援，ASP.NET AJAX 擴充功能會簡化的指令碼當地語系化和全球化應用程式。

## <a name="bio"></a>*簡歷*

Scott Cate 從事 Microsoft Web 技術工作自 1997 年，而且 myKB.com 總裁 ([www.myKB.com](http://www.myKB.com))，專精於撰寫 ASP.NET 架構著重於知識庫軟體解決方案的應用程式。 可以透過電子郵件連絡 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [上一頁](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [下一頁](understanding-asp-net-ajax-web-services.md)
