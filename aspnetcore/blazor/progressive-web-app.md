---
title: 使用ASP.NET核心BlazorWeb 組裝建構漸進式 Web 應用程式
author: guardrex
description: 瞭解如何建構Blazor基於 漸進式 Web 應用程式 (PWA),該應用程式使用現代瀏覽器功能來像桌面應用一樣運行。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: fe69e51aefae9c80e5bb4b78151d384ce25d41a7
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218943"
---
# <a name="build-progressive-web-applications-with-aspnet-core-blazor-webassembly"></a>使用ASP.NET核心 Blazor Web 組裝建構漸進式 Web 應用程式

作者：[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

漸進式 Web 應用程式 (PWA) 是單頁應用程式 (SPA),它使用現代瀏覽器 API 和功能來像桌面應用程式一樣執行。 Blazor WebAssembly 是一個基於標準的用戶端 Web 應用平臺,因此可以使用任何瀏覽器 API,包括以下功能所需的 PWA API:

* 離線工作並立即載入,與網路速度無關。
* 在其自己的應用視窗中運行,而不僅僅是瀏覽器視窗。
* 從主機的操作系統啟動功能表、銜接站或主螢幕啟動。
* 從後端伺服器接收推送通知,即使使用者不使用應用也是如此。
* 在後台自動更新。

"*累進"* 一詞用於描述此類應用,因為:

* 使用者可能首先發現並使用其 Web 瀏覽器中的應用,就像任何其他 SPA 一樣。
* 稍後,用戶繼續將其安裝到其操作系統中並啟用推送通知。

## <a name="create-a-project-from-the-pwa-template"></a>從 PWA 樣本建立項目

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 **「建立新項目**」對話框中建立新的**Blazor WebAssembly 應用程式**時,請選擇 **「進度 Web 應用程式**」 選單方塊:

!["逐步 Web 應用程式"複選框在可視化工作室新項目對話方塊中被選中。](progressive-web-app/_static/image1.png)

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

-->

# <a name="visual-studio-code--net-core-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

使用`--pwa`交換機在指令 shell 中建立 PWA 專案:

```dotnetcli
dotnet new blazorwasm -o MyNewProject --pwa
```

---

或者,PWA 可以配置為從 ASP.NET核心託管範本創建的應用。 PWA 方案獨立於託管模型。

## <a name="installation-and-app-manifest"></a>安裝和應用程式清單

訪問使用 PWA 樣本建立的應用時,用戶可以選擇將應用安裝到其作業系統的啟動功能表、擴充站或主螢幕中。 顯示此選項的方式取決於用戶的瀏覽器。 使用基於鉻的桌面瀏覽器(如邊緣瀏覽器或 Chrome 瀏覽器)時,網址欄中將顯示 **「添加**」按鈕。 使用者選擇「**新增**」按鈕後,他們將收到確認對話框:

![Google Chrome 中的確認記錄向"MyBlazorPwa"應用的用戶顯示"安裝"按鈕。](progressive-web-app/_static/image2.png)

在 iOS 上,訪問者可以使用 Safari 的 **「共用」** 按鈕及其 **「添加到主螢幕」** 選項安裝 PWA。 在 Android 的 Chrome 上,用戶應選擇右上角的 **「功能表」** 按鈕,然後選擇 **「添加到主螢幕**」。。

安裝後,應用將顯示在自己的視窗中,沒有位址列:

!["MyBlazorPwa"應用程式在谷歌Chrome中運行,沒有位址欄。](progressive-web-app/_static/image3.png)

要自定義視窗的標題、色彩配置、圖示或其他詳細資訊,請參閱專案的*wwwroot*目錄中的*清單.json*檔。 此文件的架構由 Web 標準定義。 有關詳細資訊,請參閱[MDN Web 文件:Web 應用清單](https://developer.mozilla.org/docs/Web/Manifest)。

## <a name="offline-support"></a>離線支援

預設情況下,使用 PWA 樣本選項創建的應用支援離線運行。 用戶必須首次訪問應用,而他們處於連線狀態。 瀏覽器會自動下載並緩存離線操作所需的所有資源。

> [!IMPORTANT]
> 發展支援將干擾通常的變革和測試週期。 因此,僅為*已發佈的*應用啟用脫機支援。 

> [!WARNING]
> 如果要分發啟用離線的 PWA,有幾個[重要的警告和注意事項](#caveats-for-offline-pwas)。 這些方案是離線 PWA 固有的,不Blazor特定於 。 在假設啟用脫機的應用如何工作之前,請務必閱讀並理解這些注意事項。

要查看離線支援的工作原理:

1. 發行應用程式。 如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/index#publish-the-app>。
1. 將應用部署到支援 HTTPS 的伺服器,並在瀏覽器中在其安全的 HTTPS 位址存取應用。
1. 開啟瀏覽器的開發人員工具,並驗證 **「應用程式**」選項卡上的主機是否註冊*了服務輔助角色*:

   ![Google Chrome 開發者工具"應用程式"選項卡顯示服務工作人員啟動並運行。](progressive-web-app/_static/image4.png)

1. 重新載入頁面並檢查 **「網路**」選項卡。**服務輔助角色**或**記憶體快取**的檔案產生式設定 :

   ![Google Chrome 開發者工具"網络"選項卡,顯示網頁所有資產的來源。](progressive-web-app/_static/image5.png)

1. 要驗證瀏覽器是否依賴於網路存取來載入應用,請:

   * 關閉 Web 伺服器,查看應用如何繼續正常運行,其中包括頁面重新載入。 同樣,當網路連接速度較慢時,應用程式將繼續正常運行。
   * 指示瀏覽器在 **「網路」** 選項卡中模擬離線模式:

   ![Google Chrome 開發者工具"網络"選項卡,瀏覽器模式下拉即從"在線"更改為"離線"。](progressive-web-app/_static/image6.png)

使用服務輔助角色進行離線支援是 Web 標準Blazor,不特定於 。 有關服務輔助角色的詳細資訊,請參閱[MDN Web 文件:服務輔助角色 API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API)。 要瞭解有關服務工作人員常見使用模式的更多資訊,請參閱[Google Web:服務工作人員生命週期](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle)。

BlazorPWA 樣本產生兩個服務輔助檔案:

* *wwwroot/服務工作者.js,* 在開發過程中使用。
* *wwwroot/服務工作者.published.js,* 在發佈應用程式後使用。

要在兩個服務輔助檔案之間共用邏輯,請考慮以下方法:

* 添加第三個 JavaScript 檔案以保存通用邏輯。
* 使用[self.importScripts](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts)將通用邏輯載入到兩個服務輔助檔中。

### <a name="cache-first-fetch-strategy"></a>快取優先提取原則

內置*服務輔助人員.published.js*服務工作線程使用*緩存優先*策略解析請求。 這意味著服務工作人員更喜歡返回緩存的內容,而不管使用者是否具有網路訪問許可權或伺服器上是否有較新的內容。

快取優先策略非常有價值,因為:

* **它確保可靠性。** &ndash;網路訪問不是布爾狀態。 使用者不只是連線或離線:

  * 使用者的設備可能假定它是連線的,但網路可能太慢,以至於無法等待。
  * 網路可能會返回某些 URL 無效的結果,例如,當前存在阻止或重定向某些請求的強制 WIFI 門戶。
  
  這就是為什麼瀏覽器的`navigator.onLine`API 不可靠,不應依賴。

* **它確保正確性。** &ndash;構建離線資源的緩存時,服務工作人員使用內容哈希來保證它在一個瞬間獲取了完整且自我一致的資源快照。 然後,此緩存用作原子單元。 向網路詢問更新資源是沒有意義的,因為唯一需要的版本是已緩存的版本。 任何其他內容都可能導致不一致和不相容(例如,嘗試使用未一起編譯的 .NET 程式集的版本)。

### <a name="background-updates"></a>背景更新

作為一種心理模型,您可以將離線優先 PWA 視為可以安裝的移動應用。 無論網路連接如何,應用都會立即啟動,但已安裝的應用邏輯來自可能不是最新版本的時間點快照。

Blazor PWA 樣本產生的應用程式,當使用者訪問並且具有正常工作的網路連接時,這些應用會自動嘗試在後台更新自己。 其工作方式如下:

* 在編譯過程中,專案產生*服務工人資產清單*。 默認情況下,這稱為*服務輔助資產。* 清單列出了應用離線工作所需的所有靜態資源,如 .NET 程式集、JAvaScript 檔和 CSS,包括其內容哈希。 服務工作人員載入資源清單,以便知道要緩存的資源。
* 每次使用者訪問應用時,瀏覽器都會在後台重新請求*服務輔助人員.js* *和服務輔助資產.js。* 將檔按位元組與現有已安裝的服務輔助角色進行比較。 如果伺服器返回其中任一檔更改的內容,則服務輔助角色將嘗試安裝其自身的新版本。
* 安裝其自身的新版本時,服務輔助角色會為脫機資源創建新的單獨緩存,並開始將緩存與*服務輔助資源*中列出的資源填充。 此邏輯在`onInstall`*服務輔助人員*內部的函數中實現。
* 當載入所有資源時,該過程將成功完成,並且所有內容哈希匹配。 如果成功,新服務工作人員將進入*等待激活*狀態。 使用者關閉應用(沒有剩餘的應用選項卡或視窗)后,新服務輔助角色將*變為活動*狀態,並用於後續應用訪問。 將刪除舊服務輔助角色及其緩存。
* 如果行程未成功完成,則丟棄新的服務輔助角色實例。 在使用者下次訪問時,如果希望用戶端具有更好的網路連接,可以完成請求,則再次嘗試更新過程。

通過編輯服務輔助角色邏輯自定義此過程。 上述行為都不是特定於的,Blazor而只是 PWA 樣本選項提供的默認體驗。 有關詳細資訊,請參閱[MDN Web 文件:服務輔助角色 API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API)。

### <a name="how-requests-are-resolved"></a>如何解決要求

如[快取優先提取策略](#cache-first-fetch-strategy)部分所述,預設服務輔助角色使用*快取優先*策略,這意味著它嘗試在可用時提供緩存的內容。 如果特定 URL 沒有快取的內容(例如,從後端 API 請求數據時)服務輔助角色會返回常規網路請求。 如果伺服器可以訪問,則網路請求將成功。 此邏輯在`onFetch`*服務輔助角色*內部實現。

如果應用的 Razor 元件依賴於從後端 API 請求數據,並且希望為由於網路不可用而導致的失敗請求提供友好的使用者體驗,請在應用的元件中實現邏輯。 例如,圍繞`HttpClient`請求`try/catch`使用。

### <a name="support-server-rendered-pages"></a>支援伺服器呈現的頁面

考慮當使用者首次導航到 URL(如應用中的任何其他深層連結`/counter`) 時會發生什麼情況。 在這些情況下,您不希望返回緩存為`/counter`的內容,而是需要瀏覽器載入緩存的內容`/index.html`以Blazor啟動 WebAssembly 應用。 這些初始請求稱為*導航*請求,而不是:

* 對圖像、樣式表或其他檔的*子資源*請求。
* *獲取/XHR* API 資料請求。

默認服務輔助角色包含導航請求的特殊情況邏輯。 服務工作人員通過返回的`/index.html`緩存內容來解決請求,而不考慮請求的 URL。 此邏輯在`onFetch`*服務輔助人員*內部的函數中實現。

如果應用的某些 URL 必須返回伺服器呈現的 HTML,而不是從快取`/index.html`中提供服務 ,則需要編輯服務輔助角色中的邏輯。 如果`/Identity/`包含的所有 URL 都需要作為定期僅連線請求處理到伺服器,則修改*服務輔助人員.published.js*`onFetch`邏輯。 找出下列程式碼：

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

將代碼變更為以下內容:

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

如果不執行此操作,則無論網路連接如何,服務工作人員都會攔截此類 URL 的請求,並使用`/index.html`解析這些 URL 的請求。

### <a name="control-asset-caching"></a>控制資產快取

如果專案定義了`ServiceWorkerAssetsManifest`MSBuildBlazor屬性, 則生成工具將生成具有指定名稱的服務輔助角色資產清單。 預設 PWA 樣本產生包含以下屬性的項目檔:

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

該檔被放置在*wwwroot*輸出目錄中,因此瀏覽器`/service-worker-assets.js`可以通過請求 來檢索此檔。 要查看此文件的內容,請打開 */bin/除錯/[目標框架]/wwwroot/服務-輔助資源.js*在文字編輯器中。 但是,不要編輯文件,因為它在每個生成上重新生成。

預設情況下,此清單列出:

* 任何Blazor託管資源,如 .NET 程式集和 .NET WebAssembly 運行時檔,都需要離線運行。
* 用於發佈到應用*的 wwwroot*目錄的所有資源,如影像、樣式表和 JAvaScript 檔,包括外部專案和 NuGet 套件提供的靜態 Web 資源。

您可以通過`onInstall`在*服務輔助角色*中編輯邏輯來控制服務工作人員提取和緩存這些資源中的哪些。 預設情況下,服務輔助角色獲取和緩存與典型 Web 檔Blazor名副檔名(如 .html、.css、.js 和 *.wasm)* 匹配的檔,以及特定於 *.js**.css**.html*WebAssembly 的檔案類型 *(.dll.pdb)。* *.pdb*

要包括應用*的 wwwroot*目錄中不存在的其他資源,請定義額外的`ItemGroup`MSBuild 條目,如以下範例所示:

```xml
<ItemGroup>
  <ServiceWorkerAssetsManifestItem Include="MyDirectory\AnotherFile.json"
    RelativePath="MyDirectory\AnotherFile.json" AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

中繼`AssetUrl`資料指定瀏覽器在提取要快取的資源時應使用的與基礎相關的網址。 這可以獨立於磁碟上的原始源檔名。

> [!IMPORTANT]
> 添加`ServiceWorkerAssetsManifestItem`不會導致檔在應用的*wwwroot*目錄中發布。 發佈輸出必須單獨控制。 唯`ServiceWorkerAssetsManifestItem`一會導致服務輔助角色資產清單中顯示的其他條目。

## <a name="push-notifications"></a>推播通知

與任何其他 PWABlazor一樣,WebAssembly PWA 可以從後端伺服器接收推送通知。 即使使用者未主動使用應用,伺服器也可以隨時發送推送通知。 例如,當其他使用者執行相關操作時,可以發送推送通知。

發送推送通知的機制完全獨立於BlazorWebAssembly,因為它由可以使用任何技術的後端伺服器實現。 如果要從ASP.NET核心伺服器傳送推送通知,請考慮[使用類似於在燃極比薩車間中採用的方法的技術](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications)。

在用戶端上接收和顯示推送通知的機制也獨立於BlazorWebAssembly,因為它在服務輔助元件 JavaScript 檔中實現。 例如,請參閱[「燃燒比薩」車間中使用的方法](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications)。

## <a name="caveats-for-offline-pwas"></a>離線 PWA 的注意事項

並非所有應用都應嘗試支援脫機使用。 脫機支援增加了顯著的複雜性,但並不總是與所需的用例相關。

離線支援通常僅相關:

* 如果主資料儲存是瀏覽器的本地。 例如,該方法在具有 UI 的應用中相關,該應用用於在或索引`localStorage`[DB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API)中儲存數據的[IoT](https://en.wikipedia.org/wiki/Internet_of_things)設備。
* 如果應用執行大量工作來提取和緩存與每個用戶相關的後端 API 數據,以便他們可以離線流覽數據。 如果應用必須支援編輯,則必須構建用於跟蹤更改和將資料與後端同步的系統。
* 如果目標是保證應用立即載入,而不考慮網路條件。 圍繞後端 API 請求實現適當的使用者體驗,以顯示請求的進度,並在請求因網路不可用而失敗時正常執行。

此外,支援離線的 PWA 必須處理一系列額外的併發症。 開發人員應仔細熟悉以下各節中的注意事項。

### <a name="offline-support-only-when-published"></a>僅在發佈時提供離線支援

在開發過程中,您通常希望看到每個更改立即反映在瀏覽器中,而無需經歷後台更新過程。 因此,PWABlazor範本僅在發佈時啟用脫機支援。

構建支援離線的應用時,在開發環境中測試應用是不夠的。 您必須以已發佈狀態測試應用,以瞭解它如何回應不同的網路條件。

### <a name="update-completion-after-user-navigation-away-from-app"></a>使用者導覽離開應用程式後更新完成

在使用者從所有選項卡中導航遠離應用之前,更新不會完成。 如["後台更新](#background-updates)"部分所述,在將更新部署到應用後,瀏覽器將提取更新的服務輔助檔以開始更新過程。

令許多開發人員驚訝的是,即使此更新完成,它**也不會**生效,直到使用者在所有選項卡中導航離開。 刷新顯示應用的選項卡**是不夠的**,即使它是唯一顯示應用的選項卡。 在應用完全關閉之前,新的服務工作人員將保持*等待激活*狀態。 **這不是特定於的Blazor,而是標準 Web 平台行為。**

這通常會困擾嘗試測試其服務輔助角色或脫機緩存資源的更新的開發人員。 如果簽入瀏覽器的開發人員工具,您可能會看到以下內容:

![Google Chrome"應用程式"選項卡顯示,應用的服務工作人員正在"等待啟動"。](progressive-web-app/_static/image7.png)

只要顯示應用的選項卡或視窗的「用戶端」清單為非空,工作人員將繼續等待。 服務人員這樣做的原因是為了保證一致性。 一致性意味著所有資源都從同一原子緩存獲取。

測試更改時,您可能會發現按一下「跳過等待」連結(如上圖所示)以方便,然後重新載入頁面。 您可以通過編碼服務工作人員跳過[「等待」階段並立即在更新時啟動](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase),為所有使用者自動執行此功能。 如果跳過等待階段,則放棄始終從同一緩存實例獲取資源的保證。

### <a name="users-may-run-any-historical-version-of-the-app"></a>使用者可以執行應用程式的任何歷史版本

Web 開發人員習慣性地希望使用者只運行其 Web 應用的最新部署版本,因為這是傳統 Web 分發模型中的常態。 但是,離線優先 PWA 更類似於本機行動應用,使用者不一定運行最新版本。

如[「背景更新」](#background-updates)部分所述,在將更新部署到應用後,**每個現有使用者將繼續使用以前的版本進行至少一次進一次訪問**,因為更新發生在後台,並且在使用者之後導航離開之前不會啟動。 此外,正在使用的早期版本不一定是您部署的前一個版本。 以前的版本可以是*任何*歷史版本,具體取決於使用者上次完成更新的次。

如果應用的前端和後端部分需要就 API 請求的架構達成一致,則這可能是一個問題。 在確保所有使用者都已升級之前,不得部署向後不相容的 API 架構更改。 或者,阻止使用者使用不相容的舊版本的應用。 此方案要求與本機移動應用相同。 如果部署伺服器 API 中的重大更改,則尚未更新的使用者的用戶端應用將中斷。

如果可能,不要將重大更改部署到後端 API。 如果必須這樣做,請考慮使用[標準服務輔助角色 API(如服務輔助角色註冊](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration))來確定應用是否是最新的應用,如果沒有,則防止使用。

### <a name="interference-with-server-rendered-pages"></a>干擾伺服器呈現的頁面

如[支援伺服器呈現的頁面](#support-server-rendered-pages)部分所述,如果要繞過服務工作人員為所有導航`/index.html`請求返回 內容的行為,請編輯服務輔助角色中的邏輯。

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a>預設情況下快取所有服務輔助角色資產清單內容

如[控制資產緩存](#control-asset-caching)「部分所述,檔*服務-輔助角色資產.js*是在生成期間生成的,並列出服務工作人員應提取和緩存的所有資產。

由於此清單預設包含發送到*wwwroot*的所有內容,包括外部包和專案提供的內容,因此必須小心不要將太多內容放在其中。 如果*wwwroot*目錄包含數百萬個圖像,則服務工作人員將嘗試獲取和緩存它們,消耗過多的頻寬,並且很可能無法成功完成。

實現任意邏輯,通過編輯`onInstall`*服務輔助角色*中的函數來控制應提取和緩存清單內容的哪個子集。

### <a name="interaction-with-authentication"></a>與認證的互動

可以結合身份驗證選項使用 PWA 樣本選項。 當使用者具有網路連接時,支援離線的 PWA 也可以支援身份驗證。

當用戶沒有網路連接時,他們無法驗證或獲取訪問權杖。 默認情況下,嘗試在沒有網路訪問的情況下訪問登錄頁會導致"網路錯誤"消息。

您必須設計一個 UI 流,允許使用者在離線時執行有用操作,而無需嘗試驗證或獲取存取權杖。 或者,您可以將應用設計為在網路不可用時以正常方式失敗。 如果應用中無法啟用此功能,則可能不希望啟用脫機支援。
