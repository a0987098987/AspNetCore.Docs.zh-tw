---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: 了解 ASP.NET AJAX 當地語系化 |Microsoft 文件
author: scottcate
description: 當地語系化是設計並將特定語言和文化特性的支援整合到應用程式或應用程式元件的程序。 Mic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 565b0294f57b784bc592b286b3d8b28504110415
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="understanding-aspnet-ajax-localization"></a>了解 ASP.NET AJAX 當地語系化
====================
由[Scott 類別](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> 當地語系化是設計並將特定語言和文化特性的支援整合到應用程式或應用程式元件的程序。 Microsoft ASP.NET 平台提供適用於標準的 ASP.NET 應用程式的當地語系化藉由整合標準的.NET 當地語系化模型; 廣泛的支援Microsoft AJAX Framework 利用整合式的模型，以支援各種不同的案例可以在其中執行當地語系化。


## <a name="introduction"></a>簡介

Microsoft ASP.NET 技術，帶來物件導向和事件驅動的程式設計模型，與聯集它已編譯程式碼的優點。 不過，其伺服器端處理模型有幾個缺點固有的技術，其中有許多可以定址 System.Web.Extensions 命名空間，封裝在.NET Framework 中的 Microsoft AJAX 服務中包含的新功能3.5。 這些延伸可讓許多豐富型用戶端可用的功能先前為 ASP.NET 2.0 AJAX Extensions 中的一部分，但現在的架構基底類別程式庫的一部分。 控制項和功能，此命名空間中的包含局部網頁呈現，而不需要完整頁面重新整理，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API），存取 Web 服務的能力，並且鏡像的許多可廣泛的用戶端 API控制配置，ASP.NET 伺服器端控制項集所示。

本白皮書會檢查存在於 Microsoft AJAX Framework 和 Microsoft AJAX 指令碼程式庫，支援當地語系化和檢閱已經整合支援當地語系化 web 中的商務需求的內容中的當地語系化功能.NET Framework 所提供的應用程式。 Microsoft AJAX 指令碼程式庫會利用.resx 檔案格式已經由.NET 應用程式，它提供整合的 IDE 支援，可共用的資源類型。

本白皮書以 Microsoft Visual Studio 2008 Beta 2 版本上。 本白皮書也會假設您將使用 Visual Studio 2008 中，不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面。 某些程式碼範例會利用可能無法在 Visual Web Developer Express 中的專案範本。

## <a name="the-need-for-localization"></a>*需要當地語系化*

特別是針對企業應用程式開發人員，元件開發人員能夠建立工具可感知文化特性和語言之間的差異變得越來越必要。 設計元件，以套用到用戶端的地區設定的能力會增加開發人員生產力，並減少調元件的全域函式所需的工作量。

當地語系化是設計並將特定語言和文化特性的支援整合到應用程式或應用程式元件的程序。 Microsoft ASP.NET 平台提供適用於標準的 ASP.NET 應用程式的當地語系化藉由整合標準的.NET 當地語系化模型; 廣泛的支援Microsoft AJAX Framework 利用整合式的模型，以支援各種不同的案例可以在其中執行當地語系化。 Microsoft AJAX 架構中，所要部署至附屬組件，或藉由使用靜態檔案系統結構可能可以當地語系化指令碼。

## <a name="embedding-scripts-with-satellite-assemblies"></a>*具有附屬組件的內嵌指令碼*

與標準.NET Framework 當地語系化策略一致，資源可以包含在附屬組件。 附屬組件提供數個優點高於傳統資源包含二進位檔案位在任何給定的當地語系化可以更新而無須更新更大的影像，您可以部署額外的當地語系化資源，只要安裝至附屬組件可以部署的專案資料夾和附屬組件，而不會造成主要專案組件的重新載入。 特別是在 ASP.NET 專案，這會是有幫助，因為它可以大幅降低累加式更新時，所使用的系統資源數量，並最少中斷生產網站的使用方式。

它們包括指令碼內嵌至組件中受管理的.resx （或編譯.resources） 檔案，會包含在編譯時期組件。 其資源則可透過 AJAX 執行階段產生程式碼，透過組件層級屬性的指令碼應用程式

*內嵌指令碼檔案的命名慣例*

Microsoft AJAX Framework 指令碼管理以用於部署和測試指令碼支援各種不同的選項，這些選項，以便提供指導方針。

*為了方便偵錯：*

發行 （生產環境） 指令碼不應包含`.debug`檔案名稱中的限定詞。 指令碼偵錯應該包含`.debug`檔案名稱中。

*若要協助當地語系化：*

中性文化特性的指令碼不應該包含檔案的名稱的任何文化特性識別項。 包含當地語系化的資源指令碼，應指定 ISO 語言程式碼中的檔案名稱。 例如，`es-CO`代表西班牙文、 哥倫比亞省。

下表摘要說明的檔案命名慣例範例：

| Filename | 意義 |
| --- | --- |
| Script.js | 發行版本文化特性中性的指令碼。 |
| Script.debug.js | 偵錯版本文化特性中性的指令碼。 |
| Script.en-US.js | 發行版英文，美國指令碼。 |
| Script.debug.es-CO.js | 偵錯版本西班牙文，哥倫比亞省的指令碼。 |

## <a name="walkthrough-create-an-localized-embedded-script"></a>逐步解說： 建立當地語系化的內嵌指令碼

*請注意： 本逐步解說需要使用 Visual Studio 2008，因為 Visual Web Developer Express 不包括類別庫專案的專案範本。*

1. 建立新的網站專案與 ASP.NET AJAX 擴充功能整合。 建立另一個專案，類別庫專案，稱為 LocalizingResources 方案中。
2. 加入至 LocalizingResources 專案，稱為 VerifyDeletion.js Jscript 檔案，以及稱為 DeletionResources.resx 和 DeletionResources.es.resx.resx 資源檔。 前者會包含文化特性中性的資源;後者會包含西班牙文語言資源。
3. 將下列程式碼加入至 VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

熟悉 JavaScript Regex 語法中，單一正斜線內文字的 （在上述範例中，/FILENAME/ 是範例） 代表 RegExp 物件。 MSDN Library 包含廣泛的 JavaScript 參考，並可以線上找到 JavaScript 原生物件上的資源。

1. 將下列的資源字串加入至 DeletionResources.resx: 

    **VerifyDelete**： 確定您想要刪除的檔案名稱嗎？

    **刪除**： 已刪除的檔案名稱。

1. 將下列的資源字串加入至 DeletionResources.es.resx: 

    **VerifyDelete**: Est seguro 佇列 desee quitar FILENAME 嗎？

    **刪除**: FILENAME se ha quitado。
2. 將下列程式碼加入至 AssemblyInfo 檔案：

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. LocalizingResources 專案中加入 System.Web 和 System.Web.Extensions 的參考。
2. 新增 LocalizingResources 專案中的網站專案的參考。
3. 在 default.aspx 中，在網站專案，請使用下列額外的標記更新 ScriptManager 控制項：

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. 在 default.aspx 中，任何位置在頁面上，包含這個標記：

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. 按 F5。 如果出現提示，請啟用偵錯。 當網頁載入時，請按 [刪除] 按鈕。 請注意，系統會提示您以英文 （除非您的電腦設定為慣用西班牙文語言資源上，依預設） 確認。
2. 關閉瀏覽器視窗並返回 default.aspx。 在@Page標頭指示詞，取代附有 Culture 和 UICulture ES-ES。 按 F5 再次啟動 web 應用程式瀏覽器中的一次。 這次請注意，系統會提示您刪除西班牙文中的檔案：


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-localization/_static/image6.png))


請注意，此逐步解說中的數個變化。 比方說，指令碼無法向 ScriptManager 控制項以程式設計方式在載入頁面時。

## <a name="including-a-static-script-file-structure"></a>*包括靜態指令碼檔案結構*

使用靜態指令碼檔案進行部署，便會失去某些使用固有的.NET 當地語系化配置的優點。 主要可見時，就會失去自動包括指令碼資源檔; 所產生的型別比方說，在上述的逐步解說中，資源已公開呼叫訊息從 ScriptManager 控制項自動產生型別。

有，不過，若要使用的靜態指令碼檔案結構的一些優點。 可以執行更新，而不需要重新編譯和重新部署附屬組件，並使用靜態檔案結構也可以透過覆寫內嵌指令碼，來整合一次要項可能不已寄出的功能與元件。

Microsoft 建議您避免版本控制問題自動在專案的編譯期間產生的指令碼資源。 當維護廣泛的指令碼程式碼基底會變得愈來愈困難，以確保程式碼變更會反映在每個當地語系化的指令碼。 或者，您可以只維護一個邏輯指令碼和多個當地語系化的指令碼，合併的檔案建立專案時。

因為不是以宣告方式包含的資源，靜態指令碼檔案應該參考藉由加入`<asp:ScriptElement>`做為子系的項目`<Scripts>`標記 ScriptManager 控制項，或以程式設計方式加入`ScriptReference`物件若要`Scripts`屬性`ScriptManager`在執行階段頁面上的控制項。

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager 及當地語系化其角色*

ScriptManager 可讓多個當地語系化的應用程式的自動行為：

- 自動找出指令碼檔案，根據設定和命名慣例。比方說，它會載入偵錯功能的指令碼在偵錯模式中，而且載入當地語系化根據瀏覽器的使用者介面選項的指令碼。
- 它讓您定義的文化特性，包括自訂文化特性。
- 它可以透過 HTTP 的指令碼檔案的壓縮。
- 它會快取，有效地管理多要求的指令碼。
- 將會加入一層間接取值指令碼經由管道它們透過加密的 URL。

指令碼參考可以加入 ScriptManager 控制項，以程式設計方式或宣告式標記。 宣告式標記為特別有用內嵌在組件的網站專案本身以外使用指令碼時的指令碼名稱可能不會變更為推入修訂。

## <a name="summary"></a>總結

當 web 應用程式成長以達到更大的對象時，必須能夠連線到更廣泛的文化特性和 「 社群 」 會變成核心商務模型;電子商務 web 應用程式需要能夠處理外幣，內容管理系統需要以其內容，但也其導覽提示和其他語言和公司中的表單欄位必須知道這項需求是能夠不只存在可存取。

.NET Framework 在本質上支援豐富的當地語系化架構，利用附屬組件和 XML 資源 (.resx) 檔案提供統一的方式來尋找資源字串和影像。 ASP.NET AJAX 擴充功能，包括 Microsoft AJAX Framework 和 Microsoft AJAX 指令碼程式庫提供支援對此程式設計模型到用戶端程式碼中，啟用簡單的資源字串查閱。 附屬組件支援在自動包含透過 ScriptResource.axd 指令碼資源 （實際的.js 檔案），只要檔案名稱依照指定的命名配置。 這項支援，ASP.NET AJAX 擴充功能簡化將指令碼的當地語系化和全球化應用程式。

## <a name="bio"></a>*Bio*

Scott 是否已從 1997 年使用 Microsoft Web 技術，且 myKB.com 總統 ([www.myKB.com](http://www.myKB.com)) 擅長撰寫 ASP.NET 架構的重點 Knowledge Base 軟體解決方案的應用程式。 透過在電子郵件，即可以聯繫 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [上一頁](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [下一頁](understanding-asp-net-ajax-web-services.md)
