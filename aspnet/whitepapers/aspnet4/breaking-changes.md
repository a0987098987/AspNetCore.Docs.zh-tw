---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 重大變更 |Microsoft Docs
author: rick-anderson
description: 本文件說明已針對.NET Framework 版本可能會影響使用所建立的應用程式的月 4 日發行的變更...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 112483abdd920649fb530959a538b1d5ed6064d7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833796"
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 重大變更
====================
> 本文件說明已針對.NET Framework 版本可能會影響使用較早的版本，包括 ASP.NET 4 Beta 1 和 Beta 2 版本所建立的應用程式的月 4 日發行的變更。
> 
> [下載此白皮書](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>內容

[在 Web.config 檔案中的 ControlRenderingCompatibilityVersion 設定](#0.1__Toc256770141 "_Toc256770141")  
[變更 ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode 和 UrlEncode 現在編碼單引號](#0.1__Toc256770143 "_Toc256770143")  
[ASP.NET 網頁 (.aspx) 剖析器會 Stricter](#0.1__Toc256770144 "_Toc256770144")  
[瀏覽器定義檔案，更新](#0.1__Toc256770145 "_Toc256770145")  
[從根 Web 組態檔中移除 System.Web.Mobile.dll](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET 要求驗證](#0.1__Toc256770147 "_Toc256770147")  
[雜湊演算法的預設值為 Now HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[新的 ASP.NET 4 根組態相關組態錯誤](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 子應用程式無法啟動時以 ASP.NET 2.0 或 ASP.NET 3.5 應用程式](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 網站無法在已安裝 SharePoint 的電腦上啟動](#0.1__Toc256770151 "_Toc256770151")  
[HttpRequest.FilePath 屬性不再包含 PathInfo 值](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 應用程式可能會產生參考 reference eurl.axd 的 HttpException 錯誤](#0.1__Toc256770153 "_Toc256770153")  
[事件處理常式可能不會不引發中的預設文件，在 IIS 7 或 IIS 7.5 整合模式](#0.1__Toc256770154 "_Toc256770154")  
[變更為 ASP.NET 程式碼存取安全性 (CAS) 實作](#0.1__Toc256770155 "_Toc256770155")  
[在移動 MembershipUser 和 System.Web.Security 命名空間中的其他型別](#0.1__Toc256770156 "_Toc256770156")  
[輸出快取的變更，更改\*HTTP 標頭](#0.1__Toc256770157 "_Toc256770157")  
[Passport System.Web.Security 類型為已淘汰](#0.1__Toc256770158 "_Toc256770158")  
[MenuItem.PopOutImageUrl 屬性無法轉譯 ASP.NET 4 中的映像](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 失敗，當要呈現的影像路徑包含反斜線](#0.1__Toc256770160 "_Toc256770160")  
[Disclaimer](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>在 Web.config 檔案中的 ControlRenderingCompatibilityVersion 設定

ASP.NET 控制項以可讓您指定如何更精確地說他們會呈現在標記已修改.NET Framework 第 4 版中。 在舊版的.NET Framework 中，某些控制項已發出有沒有辦法停用的標記。 根據預設，不會再產生 ASP.NET 4 這種類型的標記。

如果您使用 Visual Studio 2010 從 ASP.NET 2.0 或 ASP.NET 3.5 升級應用程式時，此工具會自動加入電腦的設定`Web.config`檔案，以保留舊版轉譯。 不過，如果您將 IIS 中的應用程式集區變更成以.NET Framework 4 為目標來升級應用程式，則 ASP.NET 預設會使用新轉譯模式。 若要停用新的轉譯模式，請新增 齌欞眭飶`Web.config`檔案：

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

新的行為會將主要的轉譯變更如下所示：

- **映像**並**ImageButton**控制項不再轉譯`border="0"`屬性。
- **BaseValidator**從它衍生的類別和驗證控制項預設不再轉譯紅色文字。
- **HtmlForm**控制項不會呈現**名稱**屬性。
- **表格**控制項不再轉譯`border="0"`屬性。
- 不設計供使用者輸入的控制項 (例如**標籤**控制項) 不再轉譯`disabled="disabled"`屬性如果其**已啟用**屬性設定為**false**（或如果它們是從容器控制項繼承此設定）。

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode 變更

**ClientIDMode** ASP.NET 4 中的設定可讓您指定 ASP.NET 如何產生**識別碼**屬性的 HTML 項目。 在舊版 ASP.NET 中，預設行為是相當於**AutoID**設定**ClientIDMode**。 不過，預設值是現在**Predictable**。

如果您使用 Visual Studio 2010 從 ASP.NET 2.0 或 ASP.NET 3.5 升級應用程式時，此工具會自動加入電腦的設定`Web.config`檔案，以保留舊版的.NET Framework 的行為。 不過，如果您將 IIS 中的應用程式集區變更成以.NET Framework 4 為目標來升級應用程式，則 ASP.NET 預設會使用新模式。 若要停用新的用戶端識別碼模式，請新增 齌欞眭飶`Web.config`檔案：

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode 和 UrlEncode 現在編碼單引號

在 ASP.NET 4 中， **HtmlEncode**並**UrlEncode**方法**HttpUtility**並**HttpServerUtility**類別已更新為編碼單引號字元 （'），如下所示：

- **HtmlEncode**方法會將編碼單引號的執行個體 」。
- **UrlEncode**方法會將單引號的執行個體編碼為 %27。

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>ASP.NET 網頁 (.aspx) 剖析器會 Stricter

ASP.NET 網頁的頁面剖析器 (`.aspx`檔案) 和使用者控制項 (`.ascx`檔案) 是在 ASP.NET 4 中更嚴格，並將會拒絕無效的標記更多執行個體。 例如，下列兩個程式碼片段會成功剖析在舊版 ASP.NET 中，但現在將會引發在 ASP.NET 4 中的剖析器錯誤。

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

請注意結尾的分號無效**HiddenField**標記。

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

請注意封閉**樣式**屬性，會遇到**CssClass**屬性。

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>更新的瀏覽器定義檔案

瀏覽器定義檔已更新成包含新增和已更新瀏覽器及裝置的資訊。 已移除 Netscape Navigator 這類較舊的瀏覽器和裝置，並已新增 Google Chrome 和 Apple iPhone 這類較新的瀏覽器和裝置。

如果您的應用程式包含繼承自其中一個已移除瀏覽器定義的自訂瀏覽器定義，則您會看到錯誤。 例如，如果`App_Browsers`資料夾包含繼承自 IE2 瀏覽器定義的瀏覽器定義，您會收到下列的組態錯誤訊息：

- 找不到識別碼為 'IE2' 的瀏覽器或閘道的項目。

> [!NOTE]
> **HttpBrowserCapabilities**物件 (這由頁面的**Request.Browser**屬性) 由瀏覽器定義檔案所驅動。 因此，存取此物件在 ASP.NET 4 中的屬性所傳回的資訊可能會不同於在舊版 ASP.NET 中傳回的資訊。


您可以複製下列資料夾中的瀏覽器定義檔案，就可還原為舊的瀏覽器定義檔案：

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

將檔案複製到對應`\CONFIG\Browsers`針對 ASP.NET 4 的資料夾。 將檔案複製後，執行 Aspnet\_regbrowsers.exe 命令列工具。

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>從根 Web 組態檔中移除的 System.Web.Mobile.dll

在舊版 ASP.NET 中，System.Web.Mobile.dll 組件的參考包含在根目錄`Web.config`檔案中**組件**區段底下。 若要改善效能，已移除此組件的參考。

System.Web.Mobile.dll 組件包含在 ASP.NET 4 中，但它已被取代。 如果您想要使用來自 System.Web.Mobile.dll 組件的型別，則將此組件的參考加入任一個根`Web.config`檔案或應用程式`Web.config`檔案。 例如，如果您想要使用的任何 （已過時） 的 ASP.NET 行動控制項，您必須加入 System.Web.Mobile.dll 組件，以參考`Web.config`檔案。

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>ASP.NET 要求驗證

在 ASP.NET 要求驗證功能提供特定層級的預設保護來抵禦跨網站指令碼 (XSS) 攻擊。 在舊版 ASP.NET 中，依預設會啟用要求驗證。 不過，它只套用至 ASP.NET 網頁 (`.aspx`檔和其類別檔案) 和僅執行這些頁面。

在 ASP.NET 4 中，根據預設，已啟用要求驗證所有要求，因為它已啟用，才能**BeginRequest**的 HTTP 要求階段。 如此一來，要求驗證會套用至所有的 ASP.NET 資源，不只是.aspx 頁面要求的要求。 這包括 Web 服務呼叫等自訂 HTTP 處理常式的要求。 自訂 HTTP 模組會讀取 HTTP 要求的內容時，也是作用中要求驗證。

如此一來，先前未不會觸發錯誤的要求可能現在會要求驗證錯誤。 若要還原到 ASP.NET 2.0 要求驗證功能的行為，請新增 齌欞眭飶`Web.config`檔案：

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

不過，我們建議您分析以判斷是否有現有的處理常式、 模組或其他自訂程式碼可能會存取不安全 HTTP 輸入可能為 XSS 攻擊向量的任何要求驗證錯誤。

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>雜湊演算法的預設值為 Now HMACSHA256

ASP.NET 使用加密和雜湊演算法來協助保護資料，例如表單驗證 Cookie 和檢視狀態。 根據預設，ASP.NET 4 現在會使用 HMACSHA256 演算法對 cookie 和檢視狀態的雜湊作業。 舊版 ASP.NET 使用較舊的 HMACSHA1 演算法。

如果您執行混合的 ASP.NET 2.0/ASP.NET 4，可能會影響您的應用程式資料，例如表單驗證 cookie 必須在其中運作 across.NET Framework 版本的環境。 若要設定 ASP.NET 4 Web 應用程式使用較舊的 HMACSHA1 演算法，新增 齌欞眭飶`Web.config`檔案：

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>新的 ASP.NET 4 根組態相關的組態錯誤

根組態檔 (`machine.config`檔案和根`Web.config`檔案) 的.NET Framework 4 （以及 ASP.NET 4） 已更新為包含大部分的未定案組態資訊中找不到，在 ASP.NET 3.5應用程式`Web.config`檔案。 受管理 IIS 7 和 IIS 7.5 組態系統的複雜度，因為執行 ASP.NET 3.5 應用程式在 ASP.NET 4 以及 IIS 7 和 IIS 7.5 下可能會導致 ASP.NET 或 IIS 組態錯誤。

我們建議您在 Visual Studio 2010 中，使用專案升級工具升級至 ASP.NET 4 的 ASP.NET 3.5 應用程式，如果可行的話。 Visual Studio 2010 會自動修改 ASP.NET 3.5 應用程式的`Web.config`檔案，以包含適當的設定，針對 ASP.NET 4。

不過，它是支援的案例，以執行使用.NET Framework 4，而不必重新編譯的 ASP.NET 3.5 應用程式。 在此情況下，您可能必須手動修改應用程式的`Web.config`檔案然後再執行應用程式在.NET Framework 4 以及 IIS 7 或 IIS 7.5 下。

下面兩節說明您可能需要不同組合的軟體進行的變更。

**安裝既不 hotfix KB958854 或 SP2 的 Windows Vista SP1 或 Windows Server 2008 SP1。** 在此組態中，IIS 7 組態系統正確合併應用程式的受管理的設定藉由比較應用程式層級`Web.config`檔案，以 ASP.NET 2.0`machine.config`檔案。 因為此，應用程式層級`Web.config`從.NET Framework 3.5 或更新版本的檔案必須有**system.web.extensions**為了避免會導致 IIS 7 驗證失敗的組態區段定義 （項目）。

不過，以手動方式修改應用程式層級`Web.config`不精確地符合原始未定案組態區段定義使用 Visual Studio 2008 所導入的檔案項目會導致 ASP.NET 組態錯誤。 （Visual Studio 2008 所產生的預設組態項目正常運作。）常見的問題是，以手動方式修改`Web.config`檔案已省略**allowDefinition**並**requirePermission**在各種不同的 [設定] 區段找到的組態屬性定義。 這會導致應用程式層級中的縮寫的組態區段不符`Web.config`檔案和 ASP.NET 4 中的完整定義`machine.config`檔案。 如此一來，在執行階段，ASP.NET 4 組態系統，會擲回的組態錯誤。

**Windows Vista SP2，Windows Server 2008 SP2、 Windows 7、 Windows Server 2008 R2，和也 Windows Vista SP1 和 Windows Server 2008 SP1 已安裝 hotfix KB958854。**

在此案例中，IIS 7 和 IIS 7.5 原生組態系統傳回組態錯誤，因為它在執行文字比較**型別**定義為受管理的組態區段處理常式的屬性。 因為所有`Web.config`Visual Studio 2008 和 Visual Studio 2008 SP1 所產生的檔案有"3.5"中的型別字串**system.web.extensions** （及相關） 的組態區段處理常式，而且因為 ASP.NET4`machine.config`檔案有"4.0"**型別**屬性 (attribute) 相同的組態區段的處理常式，一律會產生 Visual Studio 2008 或 Visual Studio 2008 SP1 中的應用程式無法在 IIS 7 中的設定驗證和IIS 7.5。

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>解決這些問題

第一個案例的因應措施是，更新應用程式層級`Web.config`檔案包含未定案組態文字從`Web.config`Visual Studio 2008 所自動產生的檔案。

第一個案例的一種解決方法是在電腦上安裝適用於 Vista 或 Windows Server 2008 Service Pack 2，或安裝 hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) 若要修正的不正確的組態合併行為IIS 組態系統。 不過，當您執行上述其中一個動作之後，您的應用程式可能會遇到的組態錯誤，因為第二個案例所述的問題。

第二個案例的因應措施是刪除或取消註解所有**system.web.extensions**組態區段定義和組態區段群組的應用程式層級定義`Web.config`檔案。 這些定義通常是在應用程式層級的頂端`Web.config`檔案，並可以藉由識別**configSections**元素和其子系。

這兩種情況下，建議您手動刪除**system.codedom**區段中，雖然這並非必要。

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 子應用程式無法啟動時以 ASP.NET 2.0 或 ASP.NET 3.5 應用程式

因為發生組態或編譯錯誤，所以可能無法啟動設定為執行舊版 ASP.NET 之應用程式子系的 ASP.NET 4 應用程式。 下列範例會顯示受影響的應用程式的目錄結構。

`/parentwebapp` （設定為使用 ASP.NET 2.0 或 ASP.NET 3.5）  
`/childwebapp` （已設定為使用 ASP.NET 4）

中的應用程式`childwebapp`資料夾將無法啟動在 IIS 7 或 IIS 7.5 上，並會回報組態錯誤。 錯誤文字將包含類似下面的訊息：

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

在 IIS 6 中的應用程式上`childwebapp`資料夾也將無法啟動，但是它會報告不同的錯誤。 比方說，您可能狀態下列的錯誤文字：

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

這些情況下發生，因為父應用程式中的組態資訊`parentwebapp`資料夾是決定最終合併的組態設定所使用的子網路的組態資訊的階層架構的一部分應用程式`childwebapp`資料夾。 根據 ASP.NET 4 Web 應用程式是否執行 IIS 7 （或 IIS 7.5） 或 IIS 6 上，IIS 組態系統或 ASP.NET 4 編譯系統會傳回錯誤。

若要解決此問題並取得子系 ASP.NET 4 應用程式，以使用，您必須遵循的步驟取決於 ASP.NET 4 應用程式是否執行 IIS 6 上，或在 IIS 7 （或 IIS 7.5）。

### <a name="step-1-iis-7-or-iis-75-only"></a>步驟 1 （IIS 7 或 IIS 7.5 只）

這是必要步驟只能在執行 IIS 7 的作業系統或 IIS 7.5，其中包括 Windows Vista、 Windows Server 2008、 Windows 7 和 Windows Server 2008 R2 上。

移動**configSections**中的定義`Web.config`上層應用程式 （執行 ASP.NET 2.0 或 ASP.NET 3.5 應用程式） 的檔案到根目錄`Web.config`.net Framework 2.0 的檔案。 IIS 7 和 IIS 7.5 的原生組態系統會掃描**configSections**時它會合併組態檔的階層項目。 移動**configSections**從上層 Web 應用程式定義`Web.config`根目錄的檔案`Web.config`檔案實際上會隱藏子系 ASP.NET 4，就會發生這種組態合併處理的項目應用程式。

在 32 位元作業系統上，或 32 位元應用程式集區，根`Web.config`ASP.NET 2.0 和 ASP.NET 3.5 中的檔案通常位於下列資料夾：

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

在 64 位元作業系統上或 64 位元應用程式集區，根`Web.config`ASP.NET 2.0 和 ASP.NET 3.5 中的檔案通常位於下列資料夾：

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

如果您在 64 位元電腦上執行 32 位元和 64 位元的 Web 應用程式，您必須移動**configSections**項目上移到根目錄`Web.config`32 位元和 64 位元系統檔案。

當您將放**configSections**根目錄中的項目`Web.config`檔案中，貼上一節之後立即**configuration**項目。 下列範例顯示哪些的上半部根目錄`Web.config`檔案應該看起來像當您完成移動項目。

> [!NOTE]
> 在下列範例中，已包裝行以提高可讀性。


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>步驟 2 （所有版本的 IIS）

這個步驟是必要的 ASP.NET 4 子系 Web 應用程式是否正在執行 IIS 6 上或在 IIS 7 （或 IIS 7.5）。

在`Web.config`父系 ASP.NET 2 或 ASP.NET 3.5 中，執行的 Web 應用程式的檔案並新增**位置**明確指定 （適用於 IIS 和 ASP.NET 組態系統） 的標記，只能的組態項目套用至父代的 Web 應用程式。 下列範例示範的語法**位置**来加入項目：

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

下列範例示範如何**位置**標記用來包裝所有的組態區段開頭**appSettings**一節，以結束**system.webServer**一節。

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

當您完成步驟 1 和 2 時，子系 ASP.NET 4 Web 應用程式會開始正確無誤。

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 網站無法在已安裝 SharePoint 的電腦上啟動

執行 SharePoint 的網頁伺服器則有`Web.config`部署的 SharePoint 網站根目錄的檔案 (例如`c:\inetpub\wwwroot\web.config`預設的網站)。 在此`Web.config`檔案，SharePoint 設定自訂的部分信任層級名為 WSS\_最少。

如果您嘗試執行這種類型的 SharePoint 網站的子系的 ASP.NET 4 網站部署，您會看到下列錯誤：

`Could not find permission set named 'ASP.NET'.`

因為 ASP.NET 4 程式碼存取安全性 (CAS) 基礎結構會尋找一個名為 ASP.NET 的權限集合，就會發生此錯誤。 不過，部分信任 WSS 所參考的組態檔\_最少不包含具有該名稱的任何權限集。

目前沒有可用的 SharePoint 與 ASP.NET 相容的版本。 如此一來，您應該嘗試執行 ASP.NET 4 網站作為子站台底下 SharePoint Web 站台。

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>HttpRequest.FilePath 屬性不再包含 PathInfo 值

舊版 ASP.NET 隨附**PathInfo**各種檔案路徑相關的屬性，包括從傳回的值中的值**HttpRequest.FilePath**， **HttpRequest.AppRelativeCurrentExecutionFilePath**，並**HttpRequest.CurrentExecutionFilePath**。 ASP.NET 4 不再包含**PathInfo**中這些屬性的傳回值的值。 相反地， **PathInfo**中會提供資訊**HttpRequest.PathInfo**。 例如，假設有下列 URL 片段：

`/testapp/Action.mvc/SomeAction`

在舊版 ASP.NET 中， **HttpRequest**屬性具有下列值：

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: （空白）

在 ASP.NET 4 中， **HttpRequest**屬性改為具有下列值：

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 應用程式可能會產生參考 reference eurl.axd 的 HttpException 錯誤

ASP.NET 4 啟用 IIS 6 上之後，（在 Windows Server 2003 或 Windows Server 2003 R2） 的 IIS 6 執行的 ASP.NET 2.0 應用程式可能會產生錯誤，如下所示：

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

當 ASP.NET 偵測到網站已設定為使用 ASP.NET 4，ASP.NET 4 的原生元件傳送的無副檔名 URL 至 ASP.NET 的 managed 部分進行進一步處理，就會發生此錯誤。 不過，如果 ASP.NET 4 網站之下的虛擬目錄設定為使用 ASP.NET 2.0，處理在修改後的 URL，其中包含字串"reference eurl.axd 」 此方法會產生無副檔名的 URL。 這個修改過的 URL 則會傳送至 ASP.NET 2.0 應用程式中。 ASP.NET 2.0 無法辨識"reference eurl.axd 」 格式。 因此，ASP.NET 2.0 嘗試尋找名為`eurl.axd`並執行它。 因為沒有這類檔案存在，要求會失敗並**HttpException**例外狀況。

您可以解決此問題，使用下列選項之一。

### <a name="option-1"></a>選項 1

如果 ASP.NET 4 時不需要執行的網站，重新對應站台改為使用 ASP.NET 2.0。

### <a name="option-2"></a>選項 2

如果 ASP.NET 4 才能執行網站，請將任何子 ASP.NET 2.0 虛擬目錄移至不同的網站對應至 ASP.NET 2.0。

### <a name="option-3"></a>選項 3

如果不是實際重新對應至 ASP.NET 2.0 網站，或變更虛擬目錄的位置，明確停用處理 ASP.NET 4 中的無副檔名 URL。 使用下列程序：

1. 在 Windows 登錄中，開啟下列節點：

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. 建立新**DWORD**值並命名為**EnableExtensionlessUrls**。
2. 設定**EnableExtensionlessUrls**設為 0。 這會停用無副檔名 URL 行為。
3. 儲存登錄值並關閉登錄編輯程式。
4. 執行**iisreset**命令列工具，這會導致 IIS 以讀取新的登錄值。

> [!NOTE]
> 設定**EnableExtensionlessUrls**為 1 啟用無副檔名 URL 行為。 這是未不指定任何值時的預設值。


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>事件處理常式可能不會不引發中的預設文件，在 IIS 7 或 IIS 7.5 整合模式

ASP.NET 4 包含變更的修改方式**動作**屬性的 html**表單**項目會呈現時的無副檔名 URL 解析為預設文件。 會解析為預設文件的無副檔名 URL 的範例[ http://contoso.com/ ](http://contoso.com/)，產生的要求[ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx)。

ASP.NET 4 現在會呈現 HTML**表單**項目的**動作**已對應到它的預設文件的無副檔名 url 提出要求時，屬性值為空字串。 例如，在舊版 ASP.NET 中，要求[ http://contoso.com ](http://contoso.com)的要求會導致`Default.aspx`。 在文件中開啟**表單**會呈現標記，如下列範例所示：

`<form action="Default.aspx" />`

在 ASP.NET 4 中，要求[ http://contoso.com ](http://contoso.com)也會導致要求`Default.aspx`。 不過，ASP.NET 現在會轉譯 HTML 開啟**表單**標記，如下列範例所示：

`<form action="" />`

這項差異在於如何**動作**屬性轉譯可能造成表單 post 的 IIS 和 ASP.NET 的處理方式稍有變更。 當**動作**屬性是空字串，IIS **DefaultDocumentModule**物件會建立的子要求`Default.aspx`。 在大部分的情況下此子要求是透明的應用程式程式碼和`Default.aspx`網頁會正常執行。

不過，Managed 程式碼與 IIS 7 或 IIS 7.5 整合模式之間的可能互動可能會在子要求期間讓受管理 .aspx 頁面適當地停止運作。 如果發生下列狀況，子要求`Default.aspx`文件會導致錯誤或非預期的行為：

1. .Aspx 頁面傳送至瀏覽器**表單**項目的**動作**屬性設為""。
2. 表單回傳至 ASP.NET。
3. 受管理的 HTTP 模組會讀取實體主體的某個部分。 例如，模組會讀取**Request.Form**或是**Request.Params**。 這會將 POST 要求的實體主體讀入受管理記憶體中。 因此，任何以 IIS 7 或 IIS 7.5 整合模式執行的機器碼模組都無法再使用實體主體。
4. IIS **DefaultDocumentModule**物件最後會執行，並建立的子要求`Default.aspx`文件。 不過，因為 Managed 程式碼的某個部分已經讀取實體主體，所以沒有實體主體可用來傳送至子要求。
5. 當針對子要求的處理常式執行 HTTP 管線`.aspx`執行階段期間執行的檔案。
6. 因為沒有實體主體，有沒有表單變數和檢視狀態，所以沒有資訊.aspx 頁面處理常式可用來判斷應該引發事件 （如果有的話）。 因此，未執行受影響 .aspx 頁面的回傳事件處理常式。

您可以透過下列方式來解決這種行為：

- 識別在預設文件要求期間存取要求的實體主體的 HTTP 模組，並判斷是否可以設定只會針對受管理的要求執行。 在 IIS 7 和 IIS 7.5 整合模式下，HTTP 模組可以執行只會針對受管理的要求，將下列屬性新增至模組的標示**system.webServer/modules**項目：

- `precondition="managedHandler"`

- 這個設定會停用的模組可讓您要求 IIS 7 和 IIS 7.5 判斷為不受管理的要求。 對於預設文件的要求，第一個要求是無副檔名的 URL。 因此，IIS 不會標示任何受管理的模組會以執行受管理的處理常式的前置條件在初始要求處理期間。 如此一來，受管理的模組將不會意外讀取實體主體，因此實體仍然可用，並傳遞至子要求和預設文件。

- 如果有問題的 HTTP 模組都必須執行的所有要求 (無副檔名的 url 可解析成的靜態檔案的**DefaultDocumentModule**物件，針對受管理的要求等)，明確修改受影響的.aspx 頁面設定**動作**屬性頁面**System.Web.UI.HtmlControls.HtmlForm**控制項為非空白字串。 例如，如果預設文件就`Default.aspx`，修改網頁的程式碼，來明確設定**HtmlForm**控制項的**動作**「 Default.aspx 」 的屬性。

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>ASP.NET 程式碼存取安全性 (CAS) 實作的變更

ASP.NET 2.0 中，並由延伸模組在 3.5 中，已新增的 ASP.NET 功能會使用.NET Framework 1.1 和 2.0 的程式碼存取安全性 (CAS) 模型。 不過，已大幅全面檢查 ASP.NET 4 中 CAS 的實作。 如此一來，在全域組件快取 (GAC) 中執行的受信任程式碼所依賴的部分信任 ASP.NET 應用程式可能會失敗，各種安全性例外狀況。 依賴對電腦 CAS 原則進行大量修改的部分信任應用程式也可能會失敗的安全性例外狀況。

您可以還原部分信任 ASP.NET 4 應用程式行為的 ASP.NET 1.1 和 2.0 版使用新**legacyCasModel**屬性中**信任**組態項目，如下列範例所示:

`<trust level= "Medium" legacyCasModel="true" />`

當您還原舊版的 CAS 模型時，會啟用下列舊 CA 行為：

- 會接受機器 CAS 原則。
- 允許在單一應用程式定義域中的多個不同的權限集。
- 不需要的組件在 GAC 中，在堆疊上只有 ASP.NET 或其他.NET Framework 程式碼時，會叫用明確的權限判斷提示。

無法還原其中一個案例，在.NET Framework 4： 非 Web 部分信任應用程式不會再呼叫 System.Web.dll 和 System.Web.Extensions.dll 中的某些 Api。 在舊版的.NET Framework 中，已被明確授與非 Web 部分信任應用程式<strong>AspNetHostingPermission</strong>權限。 接著可以使用這些應用程式<strong>System.Web.HttpUtility</strong>中的型別<strong>System.Web.ClientServices。\</ s t > * 命名空間和類型與成員資格、 角色和設定檔。 在.NET Framework 4 中不再支援從非 Web 部分信任應用程式呼叫這些類型。

> [!NOTE]
> **HtmlEncode**並**HtmlDecode**的功能**System.Web.HttpUtility**類別已移至新的.NET Framework 4 **System.Net.WebUtility**類別。 如果這是所使用的只有 ASP.NET 功能，修改應用程式的程式碼，以使用新**WebUtility**類別。


在 ASP.NET 4 中的預設 CA 實作變更的高階摘要如下：

- ASP.NET 應用程式定義域已同質性的應用程式定義域。 只有部分信任和完全信任授與集合可用於應用程式定義域。
- ASP.NET 部分信任的授權集是獨立於任何企業層級、 電腦層級或使用者層級的 CAS 原則。
- ASP.NET 3.5 和 3.5 SP1 隨附的組件已轉換成使用.NET Framework 4 的透明度模型。
- 使用 ASP.NET **AspNetHostingPermission**屬性已大幅降低。 從公用 ASP.NET Api 已移除大部分的執行個體，這個屬性。
- 以明確標記為透明的組件已更新 ASP.NET 組建提供者所建立的動態編譯組件。
- ASP.NET 的所有組件現在會標示為 APTCA 屬性會接受只能在 Web 裝載環境中的方式。 部分信任的非 Web 裝載環境等 ClickOnce 無法呼叫到 ASP.NET 的組件。

如需有關新的 ASP.NET 4 程式碼存取安全性模型的詳細資訊，請參閱 < [ASP.NET 應用程式中使用程式碼存取安全性](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx)MSDN 網站上。

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>在移動 MembershipUser 和 System.Web.Security 命名空間中的其他類型

使用 ASP.NET 成員資格中某些類型已從移動`System.Web.dll`新 System.Web.ApplicationServices.dll 組件。 已移動類型，以解析用戶端和擴充 .NET Framework SKU 中類型之間的架構層相依性。

網站專案沒有問題，因為將這些類型，因為 System.Web.ApplicationServices.dll 已新增至參考的組件的清單，顯示依預設會使用 ASP.NET 編譯系統。 如果您升級使用較早版本的 ASP.NET 至 ASP.NET 4 開啟 Visual Studio 2010 中建立的網站專案，專案會編譯無誤。

同樣地，如果您升級舊版的 ASP.NET 至 ASP.NET 4 中，開啟 Visual Studio 2010 中建立的 Web 應用程式專案時，升級程序會加入至 System.Web.ApplicationServices.dll 的參考加入專案。 因此，升級的 Web 應用程式專案也會編譯無誤。

無誤編譯使用舊版 ASP.NET 所建立的 （二進位） 檔案也在 ASP.NET 4 中，執行，即使成員資格類型移到不同的組件。 類型轉送資訊已新增至 ASP.NET 4 版本`System.Web.dll`，會自動將這些類型的執行階段參考路由至新的位置類型。

不過，類別庫，使用特定的成員資格類型，而且已從舊版 ASP.NET 升級，將無法編譯的 ASP.NET 4 專案中使用時。 例如，類別庫專案可能會無法編譯，並回報錯誤，如下所示：

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

您可以將您的類別庫專案中的參考加入至 System.Web.ApplicationServices.dll 來解決這個問題。

下列清單顯示*System.Web.Security*從其中移動的類型`System.Web.dll`至 System.Web.ApplicationServices.dll:

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

## <a name="output-caching-changes-to-vary--http-header"></a>輸出快取的變更，更改\*HTTP 標頭

在 ASP.NET 1.0 中，bug 會讓快取指定的頁面`Location="ServerAndClient"`做為輸出 – 快取設定，以發出`Vary:*`HTTP 標頭回應中的。 這會影響如何告知用戶端瀏覽器永遠不要在本機快取頁面。

在 ASP.NET 1.1 **System.Web.HttpCachePolicy.SetOmitVaryStar**方法加入，您可以呼叫它來隱藏`Vary:*`標頭。 這個方法已選擇，因為變更發出的 HTTP 標頭，被視為潛在重大的改變。 不過，開發人員都搞在 ASP.NET 中，行為和錯誤報告建議開發人員不知道現有**SetOmitVaryStar**行為。

在 ASP.NET 4 中，決定已修正根本問題。 `Vary:*` HTTP 標頭不會再發出從指定的下列指示詞的回應：

`<%@OutputCache Location="ServerAndClient" %>`

如此一來， **SetOmitVaryStar**不再需要為了隱藏`Vary:*`標頭。

在指定的應用程式中`Location="ServerAndClient"`中 **@ OutputCache**指示詞會在頁面上，您現在會看到行為的名稱所暗示**位置**屬性的值 – 也就是說，頁面會是在您呼叫而不需要瀏覽器快取**SetOmitVaryStar**方法。

如果您的應用程式中的頁面必須發出`Vary:*`，呼叫**AppendHeader**方法，如下列範例所示：

`HttpResponse.AppendHeader("Vary","*");`

或者，您可以變更輸出快取的值**位置**屬性設定為 「 伺服器 」。

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Passport System.Web.Security 類型為過時

ASP.NET 2.0 內建的 Passport 支援已過時，不支援幾年，因為 Passport (現在 LiveID) 中的變更。 如此一來，五種類型中與 Passport 有關**System.Web.Security**現在會以標示**ObsoleteAttribute**屬性。

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>MenuItem.PopOutImageUrl 屬性無法轉譯 ASP.NET 4 中的映像

在 ASP.NET 3.5 中， *MenuItem.PopOutImageUrl*屬性可讓您指定的功能表項目，表示功能表項目具有動態子功能表中顯示影像的 URL。 下列範例示範如何在 ASP.NET 3.5 中的標記中指定這個屬性。

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

因 ASP.NET 4 中的設計變更，而沒有輸出轉譯*PopOutImageUrl*如果屬性設定為*MenuItem*類別。 相反地，您必須指定影像 URL 直接在* 功能表*控制使用*StaticPopOutImageUrl*屬性或有*DynamicPopOutImageUrl*屬性。 當您使用靜態功能表中， *Menu.StaticPopOutImageUrl*屬性指定的 URL，該影像顯示以表示靜態功能表項目具有子功能表，如下列範例所示：

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

如果您正在使用動態功能表中，您會使用*Menu.DynamicPopOutImageUrl*屬性來指定表示動態功能表項目具有子功能表之影像的 URL。 下列範例類似於前一個位置，但會示範如何設定*DynamicPopOutImageUrl*針對動態功能表的屬性。

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

如果*Menu.DynamicPopOutImageUrl*未設定屬性和*Menu.DynamicEnableDefaultPopOutImage*屬性設定為*false*，不會顯示影像。 同樣地，如果*StaticPopOutImageUrl*未設定屬性和*StaticEnableDefaultPopOutImage*屬性設定為*false*，不會顯示影像。

當您設定這些屬性的路徑時，使用正斜線 （/），而不是反斜線 (\)。 如需詳細資訊，請參閱 < [Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 無法轉譯映像時路徑包含反斜線](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu。") 在其他地方在這份文件。

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 無法呈現影像路徑包含反斜線時

在 ASP.NET 4 中，使用指定的映像*Menu.StaticPopOutImageUrl*並*Menu.DynamicPopOutImageUrl*屬性不會呈現，如果路徑包含 backlashes (\)。 這是從舊版 ASP.NET 的變更。

下列範例* 功能表*控制項會顯示標記*StaticPopOutImageUrl*屬性設定使用的路徑包含反斜線。 在 ASP.NET 4 中，將不會呈現在屬性中指定的映像。

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

若要修正此問題，請變更 中所指定的路徑值*StaticPopOutImageUrl*並*DynamicPopOutImageUrl*使用正斜線 （/） 的屬性。 下列範例會示範這項變更：

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

請注意，從早期版本的 ASP.NET 至 ASP.NET 4 已移轉的應用程式可能也會受到影響，因為*MenuItem.PopOutImageUrl*屬性已變更。 如需詳細資訊，請參閱 < [MenuItem.PopOutImageUrl 屬性無法轉譯 ASP.NET 4 中的映像](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert")這份文件中的其他位置。

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>免責聲明

這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。

本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。 Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。

本技術白皮書僅供參考。 MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。

承諾遵守所有適用的著作權法是使用者的責任。 著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。

本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。 除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。

除非另有說明，範例公司、 組織、 產品、 網域名稱、 電子郵件地址、 標誌、 人物、 地點及事件本文所述之屬虛構，以及與任何真實公司、 組織、 產品、 網域名稱、 電子郵件沒有關聯地址、 標誌、 人員、 位置或事件是純屬巧合。

© 2010 Microsoft Corporation. 著作權所有，並保留一切權利。

Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。

本文件中所提實際公司和產品，可能為各所有人所有之商標。
