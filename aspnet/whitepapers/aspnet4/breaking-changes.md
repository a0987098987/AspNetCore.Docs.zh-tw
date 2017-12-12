---
uid: whitepapers/aspnet4/breaking-changes
title: "ASP.NET 4 的重大變更 |Microsoft 文件"
author: rick-anderson
description: "本文件說明已為.NET Framework 版本 4 可能會影響使用所建立的應用程式的發行版本的變更..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: a0f25ed3c996b73e362177b196539c6f2b143739
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 的重大變更
====================
> 本文件描述所做之.NET Framework 版本 4 可能會影響使用較早的版本，包括 ASP.NET 4 版 Beta 1 和 Beta 2 版本所建立的應用程式的發行的變更。
> 
> [下載此技術白皮書](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>內容

[在 Web.config 檔案中的 ControlRenderingCompatibilityVersion 設定](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode 變更](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode 和進行 urlencode 處理現在編碼單引號](#0.1__Toc256770143 "_Toc256770143")  
[ASP.NET 網頁 (.aspx) 剖析器為 Stricter](#0.1__Toc256770144 "_Toc256770144")  
[瀏覽器定義檔案更新](#0.1__Toc256770145 "_Toc256770145")  
[從根 Web 組態檔中移除 System.Web.Mobile.dll](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET 要求驗證](#0.1__Toc256770147 "_Toc256770147")  
[雜湊演算法的預設值為 Now HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[新的 ASP.NET 4 根組態相關組態錯誤](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 個子應用程式無法啟動時在 ASP.NET 2.0 或 ASP.NET 3.5 應用程式](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 Web Sites 無法安裝 SharePoint 的電腦上啟動](#0.1__Toc256770151 "_Toc256770151")  
[HttpRequest.FilePath 屬性不再包含 PathInfo 值](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 應用程式可能會產生參考 eurl.axd HttpException 錯誤](#0.1__Toc256770153 "_Toc256770153")  
[事件處理常式可能不會不引發 IIS 7 或 IIS 7.5 中的預設文件在整合模式](#0.1__Toc256770154 "_Toc256770154")  
[ASP.NET 程式碼存取安全性 (CAS) 實作變更](#0.1__Toc256770155 "_Toc256770155")  
[移動 MembershipUser 和 System.Web.Security 命名空間中的其他類型](#0.1__Toc256770156 "_Toc256770156")  
[輸出快取的變更來改變\*HTTP 標頭](#0.1__Toc256770157 "_Toc256770157")  
[Passport System.Web.Security 類型為過時](#0.1__Toc256770158 "_Toc256770158")  
[無法轉譯 ASP.NET 4 中的映像 MenuItem.PopOutImageUrl 屬性](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 容錯移轉至呈現影像時路徑包含反斜線](#0.1__Toc256770160 "_Toc256770160")  
[免責聲明](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>ControlRenderingCompatibilityVersion 設定 Web.config 檔案中

ASP.NET 控制項，讓您指定更精確地說如何轉譯標記已修改.NET Framework 第 4 版中。 在舊版的.NET framework 中，某些控制項來發出您並沒有方式可以停用的標記。 根據預設，不會再產生 ASP.NET 4 這種類型的標記。

如果您使用 Visual Studio 2010 來升級您的應用程式從 ASP.NET 2.0 或 ASP.NET 3.5 為目標時，此工具會自動加入的設定`Web.config`保留舊版的轉譯檔案。 不過，如果您將 IIS 中的應用程式集區變更成以.NET Framework 4 為目標來升級應用程式，則 ASP.NET 預設會使用新轉譯模式。 若要停用新的呈現模式，新增下列設定中的`Web.config`檔案：

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

新的行為會將主要轉譯變更如下：

- **映像**和**ImageButton**控制項不再呈現`border="0"`屬性。
- **BaseValidator**從中衍生的類別和驗證控制項不再預設呈現紅色文字。
- **HtmlForm**控制項不會呈現**名稱**屬性。
- **資料表**控制不再呈現`border="0"`屬性。
- 未專為使用者輸入的控制項 (例如，**標籤**控制項) 不再呈現`disabled="disabled"`屬性如果其**啟用**屬性設定為**false**（或如果它們是從容器控制項繼承此設定）。

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode 變更

**ClientIDMode** ASP.NET 4 中的設定可讓您指定如何產生 ASP.NET**識別碼**HTML 元素的屬性。 在舊版 ASP.NET 中，預設行為是相當於**AutoID**設定**ClientIDMode**。 不過，預設值是現在**可預測**。

如果您使用 Visual Studio 2010 來升級您的應用程式從 ASP.NET 2.0 或 ASP.NET 3.5 為目標時，此工具會自動加入的設定`Web.config`檔案會保留舊版的.NET Framework 的行為。 不過，如果您將 IIS 中的應用程式集區變更成以.NET Framework 4 為目標來升級應用程式，則 ASP.NET 預設會使用新模式。 若要停用新的用戶端識別碼模式，新增下列設定中的`Web.config`檔案：

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode 和進行 urlencode 處理現在編碼單引號

在 ASP.NET 4 **HtmlEncode**和**進行 urlencode 處理**方法**HttpUtility**和**HttpServerUtility**類別已被更新為編碼的單引號字元 （'），如下所示：

- **HtmlEncode**方法編碼單引號的執行個體 」。
- **進行 urlencode 處理**方法編碼為 %27 單引號的執行個體。

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>ASP.NET 網頁 (.aspx) 剖析器為 Stricter

ASP.NET 網頁的網頁剖析器 (`.aspx`檔案) 和使用者控制項 (`.ascx`檔案) 更為嚴格 ASP.NET 4 中，將會拒絕多個執行個體無效的標記。 例如，下列兩個程式碼片段會成功剖析在舊版 ASP.NET 中，但現在將會引發在 ASP.NET 4 中的剖析器錯誤。

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

請注意結尾的分號無效**HiddenField**標記。

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

請注意封閉**樣式**會碰到的屬性**CssClass**屬性。

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>更新的瀏覽器定義檔案

瀏覽器定義檔已更新成包含新增和已更新瀏覽器及裝置的資訊。 已移除 Netscape Navigator 這類較舊的瀏覽器和裝置，並已新增 Google Chrome 和 Apple iPhone 這類較新的瀏覽器和裝置。

如果您的應用程式包含繼承自其中一個已移除瀏覽器定義的自訂瀏覽器定義，則您會看到錯誤。 例如，如果`App_Browsers`資料夾包含瀏覽器定義繼承自 IE2 瀏覽器的定義，您會收到下列的組態錯誤訊息：

- 找不到識別碼為 'IE2' 的瀏覽器或閘道項目。

> [!NOTE]
> **HttpBrowserCapabilities**物件 (它由頁面**Request.Browser**屬性) 由瀏覽器定義檔案所驅動。 因此，存取這個物件在 ASP.NET 4 中的屬性所傳回的資訊可能不同於在舊版 ASP.NET 中傳回的資訊。


您可以從下列資料夾複製瀏覽器定義檔案還原到舊的瀏覽器定義檔案：

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

將檔案複製到對應`\CONFIG\Browsers`針對 ASP.NET 4 的資料夾。 複製檔案之後，請執行 Aspnet\_regbrowsers.exe 命令列工具。

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll 從根 Web 組態檔中移除

在舊版的 ASP.NET，System.Web.Mobile.dll 組件的參考已包含在根`Web.config`檔案**組件**下一節。 為了改善效能，已移除此組件的參考。

System.Web.Mobile.dll 組件會包含在 ASP.NET 4，但是它已被取代。 如果您想要使用來自 System.Web.Mobile.dll 組件的類型，將這個組件的參考加入任一個根`Web.config`檔案或應用程式`Web.config`檔案。 例如，如果您想要使用任何 （已過時） 的 ASP.NET 行動控制項，您必須加入 System.Web.Mobile.dll 組件，以參考`Web.config`檔案。

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>ASP.NET 要求驗證

在 ASP.NET 要求驗證的功能提供特定層級的預設保護，防止跨網站指令碼 (XSS) 攻擊。 在舊版 ASP.NET 中，依預設會啟用要求驗證。 不過，它只套用到 ASP.NET 網頁 (`.aspx`檔和其類別檔案) 以及只執行那些頁面所。

在 ASP.NET 4 中，依預設，已啟用要求驗證的所有要求，因為它已啟用之前**BeginRequest** HTTP 要求的階段。 如此一來，要求驗證適用於所有 ASP.NET 資源，不只是.aspx 網頁要求的要求。 這包括例如 Web 服務呼叫和自訂 HTTP 處理常式的要求。 自訂 HTTP 模組會讀取 HTTP 要求的內容時，也是使用中要求驗證。

如此一來，先前未不會觸發錯誤的要求現在可能會要求驗證錯誤。 若要還原的 ASP.NET 2.0 要求驗證功能的行為，新增下列設定中的`Web.config`檔案：

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

不過，我們建議您分析以判斷是否有現有的處理常式、 模組或其他自訂程式碼可能會存取不安全的 HTTP 輸入可能 XSS 攻擊向量的任何要求驗證錯誤。

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>雜湊演算法的預設值為 Now HMACSHA256

ASP.NET 使用加密和雜湊演算法來協助保護資料，例如表單驗證 Cookie 和檢視狀態。 根據預設，ASP.NET 4 現在使用 HMACSHA256 演算法 cookie 和檢視狀態上的雜湊作業。 舊版 ASP.NET 會使用較舊的 HMACSHA1 演算法。

如果您執行混合的 ASP.NET 2.0/ASP.NET 4，可能會影響您的應用程式資料，例如表單驗證 cookie 必須工作 across.NET Framework 版本的所在的環境。 若要設定 ASP.NET 4 Web 應用程式使用較舊的 HMACSHA1 演算法，加入下列設定中的`Web.config`檔案：

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>新的 ASP.NET 4 根組態相關的組態錯誤

根組態檔 (`machine.config`檔案及根`Web.config`檔案) 的.NET Framework 4 和 （因此 ASP.NET 4） 更新包含大部分的重複使用組態資訊，可在 ASP.NET 3.5 中找不到應用程式`Web.config`檔案。 由於受管理的 IIS 7 和 IIS 7.5 組態系統的複雜度，執行 ASP.NET 4 下方和 IIS 7 和 IIS 7.5 的 ASP.NET 3.5 應用程式可能會導致 ASP.NET 或 IIS 組態錯誤。

我們建議您在 Visual Studio 2010 中，使用專案升級工具來升級 ASP.NET 3.5 應用程式以 ASP.NET 4，如果可行的話。 Visual Studio 2010 會自動修改 ASP.NET 3.5 應用程式的`Web.config`檔案以包含適當的設定，針對 ASP.NET 4。

不過，它是支援的案例來執行 ASP.NET 3.5 應用程式使用.NET Framework 4，而不必重新編譯。 在此情況下，您可能必須手動修改應用程式的`Web.config`檔案然後再執行 .NET Framework 4 下方和 IIS 7 或 IIS 7.5 的應用程式。

在下兩節描述您可能需要不同組合的軟體進行的變更。

**安裝既不 hotfix KB958854 或 SP2 的 Windows Vista SP1 或 Windows Server 2008 SP1。** 在此組態中，IIS 7 組態系統未正確應用程式的受管理的設定的合併比較應用程式層級`Web.config`檔案，以 ASP.NET 2.0`machine.config`檔案。 因為如此，應用程式層級`Web.config`來自.NET Framework 3.5 或更新版本的檔案必須有**system.web.extensions**以避免導致 IIS 7 驗證失敗的組態區段定義 （項目）。

不過，以手動方式修改應用程式層級`Web.config`不精確地符合 Visual Studio 2008 所導入的原始未定案組態 > 一節定義的檔案項目會導致 ASP.NET 組態錯誤。 （預設組態項目所產生的 Visual Studio 2008 正常運作。）常見的問題是，以手動方式修改`Web.config`檔案遺漏**allowDefinition**和**requirePermission**各種設定區段找到的組態屬性定義。 這會導致應用程式層級中的縮寫的組態區段不符`Web.config`檔案和 ASP.NET 4 中的完整定義`machine.config`檔案。 如此一來，在執行階段，ASP.NET 4 組態系統，會擲回組態錯誤。

**Windows Vista SP2、 Windows Server 2008 SP2、 Windows 7、 Windows Server 2008 R2，並也 Windows Vista SP1 和 Windows Server 2008 SP1 已安裝 hotfix KB958854。**

在此案例中，IIS 7 和 IIS 7.5 的原生的組態系統傳回組態錯誤，因為它對文字比較**類型**定義為受管理的組態區段處理常式的屬性。 因為所有`Web.config`檔案所產生的 Visual Studio 2008 與 Visual Studio 2008 SP1 已 「 3.5"中的型別字串**system.web.extensions** （及相關） 的組態區段處理常式，而且因為 ASP.NET4`machine.config`檔案中有"4.0"**類型**相同組態區段的處理常式，一律會產生 Visual Studio 2008 或 Visual Studio 2008 SP1 中的應用程式無法在 IIS 7 中的設定驗證屬性和IIS 7.5。

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>解決這些問題

第一個案例的因應措施是，更新應用程式層級`Web.config`檔案中的未定案設定文字，藉以`Web.config`由 Visual Studio 2008 會自動產生的檔案。

替代的解決方法的第一個案例是在電腦上安裝 Service Pack 2 for Vista 或 Windows Server 2008 或 hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) 若要修正的不正確IIS 組態系統的組態合併行為。 不過，您執行這些動作之後，您的應用程式可能會遇到的組態錯誤，因為第二個案例所述的問題。

第二個案例的因應措施是將其刪除或標記為註解所有**system.web.extensions**組態區段定義和組態區段群組中應用程式層級定義`Web.config`檔案。 這些定義通常是在應用程式層級頂端`Web.config`檔案，並可以藉由識別**c**元素和其子系。

如需這兩種案例中，建議您也可以手動刪除**system.codedom**區段中，雖然這並非必要。

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 個子應用程式無法啟動時在 ASP.NET 2.0 或 ASP.NET 3.5 應用程式

因為發生組態或編譯錯誤，所以可能無法啟動設定為執行舊版 ASP.NET 之應用程式子系的 ASP.NET 4 應用程式。 下列範例會顯示受影響的應用程式的目錄結構。

`/parentwebapp`（設定為使用 ASP.NET 2.0 或 ASP.NET 3.5）  
`/childwebapp`（設定為使用 ASP.NET 4）

中的應用程式`childwebapp`資料夾將無法啟動 IIS 7 或 IIS 7.5 上，並會回報組態錯誤。 錯誤文字將包含類似下列訊息：

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

在 IIS 6 中的應用程式上`childwebapp`資料夾也將無法啟動，但是它會報告不同的錯誤。 例如，錯誤文字可能會說明下列：

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

因為發生這些狀況中的父應用程式的組態資訊`parentwebapp`資料夾是決定所使用的子網路的最後一個合併的組態設定的組態資訊的階層架構的一部分應用程式中`childwebapp`資料夾。 根據 ASP.NET 4 Web 應用程式是否執行 IIS 6 或 IIS 7 （或 IIS 7.5） 上，IIS 組態系統或 ASP.NET 4 編譯系統會傳回錯誤。

若要解決此問題，並取得子 ASP.NET 4 應用程式以使用，您必須遵循的步驟取決於 ASP.NET 4 應用程式是否執行 IIS 6 或 IIS 7 （或 IIS 7.5）。

### <a name="step-1-iis-7-or-iis-75-only"></a>步驟 1 （IIS 7 或 IIS 7.5 只）

這個步驟是在執行 IIS 7 的作業系統或 IIS 7.5，包括 Windows Vista、 Windows Server 2008、 Windows 7 和 Windows Server 2008 R2 上必要的。

移動**c**中的定義`Web.config`上層應用程式 （執行 ASP.NET 2.0 或 ASP.NET 3.5 為目標的應用程式） 的檔案到根`Web.config`.net Framework 2.0 的檔案。 IIS 7 和 IIS 7.5 的原生組態系統會掃描**c**時它會合併組態檔的階層項目。 移動**c**從上層 Web 應用程式定義`Web.config`根檔案`Web.config`檔案可以有效地隱藏的組態合併處理程序之子系 ASP.NET 4 中的項目應用程式。

在 32 位元作業系統上或 32 位元應用程式集區，根`Web.config`ASP.NET 2.0 與 ASP.NET 3.5 檔通常位於下列資料夾中：

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

在 64 位元作業系統上或 64 位元應用程式集區，根`Web.config`ASP.NET 2.0 與 ASP.NET 3.5 檔通常位於下列資料夾中：

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

如果您在 64 位元電腦上執行 32 位元和 64 位元的 Web 應用程式，您必須移動**c**項目上移至根`Web.config`32 位元和 64 位元系統檔案。

當您將**c**根目錄中的項目`Web.config`檔案中，貼上一節之後立即**組態**項目。 下列範例顯示哪些上半部根`Web.config`檔案應該看起來像當您完成移動項目。

> [!NOTE]
> 在下列範例中，已針對可讀性包裝行。


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>步驟 2 （所有版本的 IIS）

這個步驟是必要的 ASP.NET 4 子系 Web 應用程式是否正在執行 IIS 6 或 IIS 7 （或 IIS 7.5）。

在`Web.config`父 Web 應用程式正在使用 ASP.NET 2 或 ASP.NET 3.5 為目標的檔案加入**位置**（適用於 IIS 及 ASP.NET 組態系統） 會明確指定的標記，只能的組態項目適用於父 Web 應用程式。 下列範例示範的語法**位置**来加入項目：

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

下列範例會示範如何**位置**標記用來包裝開頭的所有設定區段**appSettings**區段，以結束**system.webServer**> 一節。

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

當您完成步驟 1 和 2 時，子 ASP.NET 4 Web 應用程式會啟動，沒有錯誤。

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 Web Sites 無法安裝 SharePoint 的電腦上啟動

執行 SharePoint 的網頁伺服器則有`Web.config`部署在 SharePoint 網站的根目錄的檔案 (例如，`c:\inetpub\wwwroot\web.config`預設的網站)。 在這個`Web.config`檔案中，SharePoint 設定自訂的部分信任層級名為 WSS\_最少。

如果您嘗試以這種類型的 SharePoint 網站的子系執行部署 ASP.NET 4 Web 站台，您會看到下列錯誤：

`Could not find permission set named 'ASP.NET'.`

因為 ASP.NET 4 程式碼存取安全性 (CAS) 基礎結構會尋找名為 ASP.NET 的權限集合，就會發生此錯誤。 不過，部分信任 WSS 所參考的組態檔\_最少不包含具有該名稱的任何權限集。

目前沒有任何可用的 SharePoint 與 ASP.NET 相容的版本。 如此一來，您應該嘗試執行 ASP.NET 4 Web 站台作為子站台之下 SharePoint Web 站台。

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>HttpRequest.FilePath 屬性不再包含 PathInfo 值

舊版 ASP.NET 包含**PathInfo**從各種不同的檔案路徑相關的屬性，包括傳回值中的值**HttpRequest.FilePath**， **HttpRequest.AppRelativeCurrentExecutionFilePath**，和**HttpRequest.CurrentExecutionFilePath**。 ASP.NET 4 已不再包含**PathInfo**這些屬性的傳回值中的值。 相反地， **PathInfo**資訊可用於**HttpRequest.PathInfo**。 例如，假設有下列 URL 片段：

`/testapp/Action.mvc/SomeAction`

在舊版 ASP.NET， **HttpRequest**屬性有下列值：

**HttpRequest.FilePath**:`/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: （空白）

在 ASP.NET 4 **HttpRequest**屬性改為包含下列值：

**HttpRequest.FilePath**:`/testapp/Action.mvc`

**HttpRequest.PathInfo**:`SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 應用程式可能會產生參考 eurl.axd HttpException 錯誤

ASP.NET 4 啟用 IIS 6 上之後，在 IIS 6 （在 Windows Server 2003 或 Windows Server 2003 R2） 執行的 ASP.NET 2.0 應用程式可能會產生錯誤，如下所示：

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

當 ASP.NET 偵測到網站已設定為使用 ASP.NET 4，原生元件的 ASP.NET 4 將無副檔名 URL ASP.NET 的 managed 部分進行進一步的處理，就會發生這個錯誤。 不過，如果 ASP.NET 4 Web 站台之下的虛擬目錄設定為使用 ASP.NET 2.0，正在處理中修改過的 URL，其中包含字串"eurl.axd"此方法會產生無副檔名 URL。 這個修改過的 URL，然後會傳送至 ASP.NET 2.0 應用程式。 ASP.NET 2.0 無法辨識"eurl.axd 」 格式。 因此，ASP.NET 2.0 會尋找名為`eurl.axd`並加以執行。 沒有這類檔案存在，因此要求失敗與**HttpException**例外狀況。

您可以暫時解決此問題，使用下列選項之一。

### <a name="option-1"></a>選項 1

如果 ASP.NET 4 不需要執行的網站，請重新對應站台改為使用 ASP.NET 2.0。

### <a name="option-2"></a>選項 2

如果 ASP.NET 4，才能執行網站，請將任何子 ASP.NET 2.0 的虛擬目錄移至不同的網站，則對應到 ASP.NET 2.0。

### <a name="option-3"></a>選項 3

如果您不太實用，若要重新對應至 ASP.NET 2.0 網站，或變更虛擬目錄的位置，明確停用 ASP.NET 4 中的處理無副檔名 URL。 使用下列程序：

1. 在 Windows 登錄中，開啟下列節點：

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. 建立新**DWORD**名為值**EnableExtensionlessUrls**。
2. 設定**EnableExtensionlessUrls**設為 0。 這會停用無副檔名 URL 行為。
3. 儲存登錄值並關閉登錄編輯程式。
4. 執行**iisreset**命令列工具，這會導致 IIS 以讀取新的登錄值。

> [!NOTE]
> 設定**EnableExtensionlessUrls** 1 會讓無副檔名 URL 行為。 這是未不指定任何值時的預設值。


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>事件處理常式可能不會不引發 IIS 7 或 IIS 7.5 中的預設文件在整合模式

ASP.NET 4 包含變更的修改如何**動作**屬性的 html**表單**項目會呈現時的無副檔名 URL 解析為預設文件。 無副檔名 URL 解析為預設文件的範例是[http://contoso.com/](http://contoso.com/)，產生的要求中[http://contoso.com/Default.aspx](http://contoso.com/Default.aspx)。

ASP.NET 4 現在會呈現 HTML**表單**項目的**動作**至已對應到它的預設文件的無副檔名 URL 提出要求時，屬性值為空字串。 例如，在較舊版本的 ASP.NET，要求[http://contoso.com](http://contoso.com)的要求會導致`Default.aspx`。 在文件中開啟**表單**會呈現標記，如下列範例所示：

`<form action="Default.aspx" />`

在 ASP.NET 4 中，要求[http://contoso.com](http://contoso.com)也會導致的要求`Default.aspx`。 不過，ASP.NET 現在會呈現 HTML 開啟**表單**標記，如下列範例所示：

`<form action="" />`

如何在這項差異**動作**轉譯屬性會導致表單張貼的 IIS 和 ASP.NET 的處理方式稍有變更。 當**動作**屬性是空字串，IIS **DefaultDocumentModule**物件將會建立的子要求`Default.aspx`。 在大部分情況中，這個子要求是對應用程式的程式碼，而`Default.aspx`頁面將會正常執行。

不過，Managed 程式碼與 IIS 7 或 IIS 7.5 整合模式之間的可能互動可能會在子要求期間讓受管理 .aspx 頁面適當地停止運作。 如果發生下列狀況，子要求`Default.aspx`文件將會導致錯誤或非預期的行為：

1. .aspx 網頁傳送到瀏覽器中使用**表單**項目的**動作**屬性設為""。
2. 表單回傳至 ASP.NET。
3. 受管理的 HTTP 模組會讀取實體內容的某些部分。 例如，模組會讀取**Request.Form**或**Request.Params**。 這會將 POST 要求的實體主體讀入受管理記憶體中。 因此，任何以 IIS 7 或 IIS 7.5 整合模式執行的機器碼模組都無法再使用實體主體。
4. IIS **DefaultDocumentModule**物件最後執行並在建立的子要求`Default.aspx`文件。 不過，因為 Managed 程式碼的某個部分已經讀取實體主體，所以沒有實體主體可用來傳送至子要求。
5. 針對子要求的處理常式的 HTTP 管線執行時`.aspx`handler-execute 階段期間執行的檔案。
6. 沒有實體主體，因為有任何形式的變數和檢視狀態，，因此沒有資訊可用來判斷哪些事件 （如果有的話） 應該要引發之.aspx 網頁處理常式。 因此，未執行受影響 .aspx 頁面的回傳事件處理常式。

您可以下列方式來解決這個問題：

- 識別在預設文件要求期間存取該要求實體主體的 HTTP 模組，並判斷是否可以設定要執行只會針對受管理的要求。 在 IIS 7 和 IIS 7.5 的整合式模式，可以標示 HTTP 模組執行只會針對受管理的要求，將下列屬性加入至模組的**system.webserver/modules**項目：

- `precondition="managedHandler"`

- 針對此模組要求 IIS 7 和 IIS 7.5 判斷為不符合這個設定會停用受管理的要求。 預設文件的要求，第一個要求是無副檔名的 URL。 因此，IIS 不會標示任何受管理的模組會以執行 managed 處理常式的前置條件初始要求處理期間。 如此一來，受管理的模組將不會意外讀取實體內容，因此實體仍然可以使用，並傳遞沿著子要求，並預設文件。

- 如果有問題的 HTTP 模組都必須執行的所有要求 (用於靜態檔案，解析成無副檔名 url **DefaultDocumentModule**物件，針對受管理的要求等)，明確修改受影響的.aspx 頁面設定**動作**屬性時，頁面**System.Web.UI.HtmlControls.HtmlForm**控制項為非空白字串。 例如，如果預設文件是`Default.aspx`，修改頁面的程式碼，以明確地設定**HtmlForm**控制項的**動作**"Default.aspx"的屬性。

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>ASP.NET 程式碼存取安全性 (CAS) 實作的變更

ASP.NET 2.0 中，並由延伸模組 3.5 中所加入的 ASP.NET 功能會使用.NET Framework 1.1 和 2.0 程式碼存取安全性 (CAS) 模型。 不過，已大幅全面檢查 ASP.NET 4 中 CAS 的實作。 如此一來，在全域組件快取 (GAC) 中執行的受信任程式碼所依賴的部分信任 ASP.NET 應用程式可能會因各種不同的安全性例外狀況。 部分信任的應用程式廣泛修改機器 CAS 原則，也可能會因安全性例外狀況。

您可以還原部分信任 ASP.NET 4 應用程式行為的 ASP.NET 1.1 和 2.0 版使用新**legacyCasModel**屬性**信任**組態項目，如下列範例所示:

`<trust level= "Medium" legacyCasModel="true" />`

當您還原舊版的 CAS 模型時，會啟用下列的舊 CA 的行為：

- 機器 CAS 原則已生效。
- 允許在單一應用程式定義域中的多個不同的權限集。
- 不需要的組件在 GAC 中，會在堆疊上只有 ASP.NET 或其他.NET Framework 程式碼時叫用明確的權限的判斷提示。

無法還原情形的情境之一，在.NET Framework 4： 非 Web 部分信任應用程式不再可以呼叫 System.Web.dll 與 System.Web.Extensions.dll 中的某些 Api。 在舊版的.NET framework 中，是讓非 Web 部分信任應用程式，可以明確授與**AspNetHostingPermission**權限。 這些應用程式無法再使用**System.Web.HttpUtility**中的型別**System.Web.ClientServices。\***命名空間和類型與成員資格、 角色和設定檔。 不再支援.NET Framework 4 中從非 Web 部分信任應用程式呼叫這些類型。

> [!NOTE]
> **HtmlEncode**和**HtmlDecode**功能**System.Web.HttpUtility**類別已移至新的.NET Framework 4 **System.Net.WebUtility**類別。 如果這是所使用的唯一 ASP.NET 功能，修改為使用新的應用程式的程式碼**WebUtility**類別。


高層級的預設 CA 實作 ASP.NET 4 中所做的變更摘要如下：

- ASP.NET 應用程式定義域現在是同質性的應用程式定義域。 只有部分信任和完全信任的授權集可用應用程式定義域中。
- ASP.NET 部分信任的授權集為獨立於任何企業層級、 電腦層級或使用者層級的 CAS 原則。
- ASP.NET 3.5 和 3.5 SP1 隨附的組件已轉換成使用.NET Framework 4 的透明度模型。
- 使用 ASP.NET **AspNetHostingPermission**屬性已大幅降低。 已從公用的 ASP.NET 應用程式開發介面中移除這個屬性的大多數執行個體。
- 動態編譯的組件所建立的 ASP.NET 組建提供者已明確地將標示為透明的組件更新。
- 現在標記的方式接受 APTCA 屬性時，則只有在 Web 裝載環境中的所有 ASP.NET 組件。 部分信任的非 Web 裝載環境類似 ClickOnce 不能呼叫 ASP.NET 組件。

如需有關新的 ASP.NET 4 程式碼存取安全性模型的詳細資訊，請參閱[ASP.NET 應用程式中使用程式碼存取安全性](https://msdn.microsoft.com/en-us/library/dd984947%28VS.100%29.aspx)MSDN 網站上。

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>移動 MembershipUser 和 System.Web.Security 命名空間中的其他類型

ASP.NET 成員資格中使用某些類型已從移動`System.Web.dll`新 System.Web.ApplicationServices.dll 組件。 已移動類型，以解析用戶端和擴充 .NET Framework SKU 中類型之間的架構層相依性。

網站專案沒有問題，因為將這些類型，因為 System.Web.ApplicationServices.dll ASP.NET 編譯系統，已加入至預設會使用參考組件的清單。 如果您升級使用較早版本的 ASP.NET 至 ASP.NET 4 開啟 Visual Studio 2010 中建立的網站專案，專案會編譯無誤。

同樣地，如果您升級舊版 ASP.NET 至 ASP.NET 4 中開啟 Visual Studio 2010 中建立的 Web 應用程式專案時，升級程序會將 System.Web.ApplicationServices.dll 的參考加入至專案。 因此，升級的 Web 應用程式專案也會編譯無誤。

無誤編譯使用較早版本的 ASP.NET 所建立的 （二進位） 檔案也在 ASP.NET 4 執行，即使成員資格類型移到不同的組件。 類型轉送資訊已加入至 ASP.NET 4 新版`System.Web.dll`，會自動傳送這些類型的執行階段參考類型的新位置。

不過，使用特定的成員資格類型，並從舊版的 ASP.NET 已經升級的類別庫將無法編譯 ASP.NET 4 專案中使用時。 例如，類別庫專案可能無法編譯，並回報錯誤，如下所示：

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

您可以將您的類別庫專案中的參考，加入 System.Web.ApplicationServices.dll 來解決這個問題。

下列清單顯示*System.Web.Security*從其中移動的型別`System.Web.dll`System.Web.ApplicationServices.dll 至：

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *System.Web.Security.MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>輸出快取的變更來改變\*HTTP 標頭

在 ASP.NET 1.0 中造成的錯誤快取指定的頁面`Location="ServerAndClient"`做為輸出 – 快取設定，來發出`Vary:*`回應的 HTTP 標頭。 這會影響如何告知用戶端瀏覽器永遠不要在本機快取頁面。

在 ASP.NET 1.1 **System.Web.HttpCachePolicy.SetOmitVaryStar**加入方法，您可以放心地呼叫`Vary:*`標頭。 因為可能最新變更的發出的 HTTP 標頭視為選擇此方法時的變更。 不過，開發人員有困惑在 ASP.NET 中，行為和問題報告建議開發人員不知道現有**SetOmitVaryStar**行為。

在 ASP.NET 4 中，已修正根本問題所決定。 `Vary:*` HTTP 標頭由指定下列指示詞的回應所不再發出：

`<%@OutputCache Location="ServerAndClient" %>`

如此一來， **SetOmitVaryStar**不再需要為了隱藏`Vary:*`標頭。

在指定的應用程式中`Location="ServerAndClient"`中**@ OutputCache**指示詞在頁面上，您現在可以看到的名稱所隱含的行為**位置**屬性的值-也就是說，將頁面在您呼叫而不需要瀏覽器快取**SetOmitVaryStar**方法。

如果您的應用程式中的頁面必須發出`Vary:*`，呼叫**AppendHeader**方法，如下列範例所示：

`HttpResponse.AppendHeader("Vary","*");`

或者，您可以變更輸出快取的值**位置**屬性設定為 「 伺服器 」。

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Passport System.Web.Security 類型為過時

內建於 ASP.NET 2.0 的 Passport 支援已經過時，不支援幾年因為 Passport (現在 LiveID) 已變更。 如此一來，五種類型與 Passport 中**System.Web.Security**此時會標示為**ObsoleteAttribute**屬性。

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>MenuItem.PopOutImageUrl 屬性無法轉譯 ASP.NET 4 中的映像

在 ASP.NET 3.5 *MenuItem.PopOutImageUrl*屬性可讓您指定功能表項目來表示功能表項目具有動態子功能表中顯示影像的 URL。 下列範例會示範如何在 ASP.NET 3.5 中的標記中指定這個屬性。

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

因為在 ASP.NET 4 設計變更，呈現沒有輸出*PopOutImageUrl*如果設定的屬性*MenuItem*類別。 相反地，您必須指定影像 URL 直接在*功能表*控制使用*StaticPopOutImageUrl*屬性或*DynamicPopOutImageUrl*屬性。 當您使用靜態功能表中， *Menu.StaticPopOutImageUrl*屬性指定的映像，會顯示以表示靜態功能表項目具有子功能表，如下列範例所示的 URL:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

如果您正在使用動態功能表中，您使用*Menu.DynamicPopOutImageUrl*屬性來指定表示動態功能表項目具有子功能表的影像 URL。 下列範例類似，但示範如何設定*DynamicPopOutImageUrl*針對動態功能表的屬性。

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

如果*Menu.DynamicPopOutImageUrl*屬性未設定而*Menu.DynamicEnableDefaultPopOutImage*屬性設定為*false*，不會顯示影像。 同樣地，如果*StaticPopOutImageUrl*屬性未設定而*StaticEnableDefaultPopOutImage*屬性設定為*false*，不會顯示影像。

當您設定這些屬性的路徑時，使用正斜線 （/），而不是反斜線 (\)。 如需詳細資訊，請參閱[Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 容錯移轉至呈現影像時的路徑包含反斜線](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu。") 其他位置中這份文件。

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl Menu.DynamicPopOutImageUrl 無法呈現影像時路徑包含反斜線

在 ASP.NET 4 中，使用指定的映像*Menu.StaticPopOutImageUrl*和*Menu.DynamicPopOutImageUrl*屬性將不會呈現如果路徑包含 backlashes (\)。 這是從舊版的 ASP.NET 的變更。

下列範例會*功能表*控制標記顯示*StaticPopOutImageUrl*屬性設定使用的路徑包含反斜線。 在 ASP.NET 4 中，將不會呈現在屬性中指定的映像。

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

若要更正此問題，變更在所指定的路徑值*StaticPopOutImageUrl*和*DynamicPopOutImageUrl*內容，以使用正斜線 （/）。 下列範例會示範這項變更：

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

請注意，較早版本的 ASP.NET 至 ASP.NET 4 已移轉的應用程式可能也會受到影響，因為*MenuItem.PopOutImageUrl*屬性已變更。 如需詳細資訊，請參閱[MenuItem.PopOutImageUrl 屬性無法轉譯 ASP.NET 4 中的映像](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert")這份文件中的其他位置。

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>免責聲明

這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。

本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。 Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。

本技術白皮書僅供參考。 MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。

承諾遵守所有適用的著作權法是使用者的責任。 著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。

本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。 除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。

除非特別註明，否則本文件中所述，用來舉例之公司、組織、產品、網域名稱、電子郵件地址、標誌、人物、場所和事件皆為虛構，沒有意圖或不應該推斷為與任何真實存在的公司、組織、產品、網域名稱、電子郵件地址、標誌、人物、場所或事件有所關聯。

© 2010 Microsoft Corporation。 著作權所有，並保留一切權利。

Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。

本文件中所提實際公司和產品，可能為各所有人所有之商標。
