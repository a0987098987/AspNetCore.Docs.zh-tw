---
title: 使用 ASP.NET Core Blazor WebAssembly 建立漸進式 Web 應用程式
author: guardrex
description: 瞭解如何建立Blazor以新的瀏覽器功能為基礎的漸進式 Web 應用程式（PWA），其行為類似于桌面應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: cf31c91ddc073498d882b111b597c546e788cc98
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82771555"
---
# <a name="build-progressive-web-applications-with-aspnet-core-blazor-webassembly"></a>使用 ASP.NET Core Blazor WebAssembly 建立漸進式 Web 應用程式

作者：[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

漸進式 Web 應用程式（PWA）是單一頁面應用程式（SPA），其使用現代化瀏覽器 Api 和功能的行為，就像桌面應用程式一樣。 Blazor WebAssembly 是以標準為基礎的用戶端 web 應用程式平臺，因此可以使用任何瀏覽器 API，包括下列功能所需的 PWA Api：

* 離線工作並立即載入，獨立于網路速度。
* 在自己的應用程式視窗中執行，而不只是瀏覽器視窗。
* 從主機的作業系統 [開始] 功能表、[停駐] 或 [主畫面] 啟動。
* 即使使用者未使用應用程式，也會從後端伺服器接收推播通知。
* 自動在背景中更新。

*漸進式*一詞是用來描述這類應用程式，原因如下：

* 使用者可能會先在網頁瀏覽器中探索並使用應用程式，就像任何其他 SPA 一樣。
* 之後，使用者會在其 OS 中進行安裝，並啟用推播通知。

## <a name="create-a-project-from-the-pwa-template"></a>從 PWA 範本建立專案

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [**建立新專案**] 對話方塊中建立新的**Blazor WebAssembly 應用程式**時，請選取 [**進度 Web 應用程式**] 核取方塊：

![[Visual Studio 新增專案] 對話方塊中已選取 [漸進式 Web 應用程式] 核取方塊。](progressive-web-app/_static/image1.png)

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

-->

# <a name="visual-studio-code--net-core-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

使用`--pwa`參數在命令 shell 中建立 PWA 專案：

```dotnetcli
dotnet new blazorwasm -o MyNewProject --pwa
```

---

您可以選擇性地針對從 ASP.NET Core 裝載的範本所建立的應用程式，設定 PWA。 PWA 案例與裝載模型無關。

## <a name="installation-and-app-manifest"></a>安裝和應用程式資訊清單

造訪使用 PWA 範本建立的應用程式時，使用者可以選擇將應用程式安裝到其作業系統的 [開始] 功能表、[停駐] 或 [主畫面]。 此選項的呈現方式取決於使用者的瀏覽器。 使用以桌面 Chromium 為基礎的瀏覽器（例如 Edge 或 Chrome）時，URL 列中會出現 [**新增**] 按鈕。 在使用者選取 [**新增**] 按鈕之後，他們會收到確認對話方塊：

![Google Chrome 中的確認 diaglog 會向使用者顯示「MyBlazorPwa」應用程式的 [安裝] 按鈕按鈕。](progressive-web-app/_static/image2.png)

在 iOS 上，訪客可以使用 Safari 的 [**共用**] 按鈕和其 [**新增至 Homescreen** ] 選項來安裝 PWA。 在適用于 Android 的 Chrome 上，使用者應該選取右上角的 [**功能表**] 按鈕，然後按一下 [**新增到主畫面**]。

安裝之後，應用程式會出現在其本身的視窗中，而不會有位址列：

![' MyBlazorPwa ' 應用程式會在沒有網址列的 Google Chrome 中執行。](progressive-web-app/_static/image3.png)

若要自訂視窗的標題、色彩配置、圖示或其他詳細資料，請參閱專案的*wwwroot*目錄中的*資訊清單. json*檔案。 此檔案的架構是由 web 標準所定義。 如需詳細資訊，請參閱[MDN web 檔： Web 應用程式資訊清單](https://developer.mozilla.org/docs/Web/Manifest)。

## <a name="offline-support"></a>離線支援

根據預設，使用 PWA 範本選項建立的應用程式支援離線執行。 使用者必須先在線上流覽應用程式。 瀏覽器會自動下載並快取離線操作所需的所有資源。

> [!IMPORTANT]
> 開發支援會干擾進行變更和測試的一般開發週期。 因此，只會針對*已發佈*的應用程式啟用離線支援。 

> [!WARNING]
> 如果您想要散發已啟用離線的 PWA，有[幾個重要的警告和注意事項](#caveats-for-offline-pwas)。 這些案例都是離線 Pwa 的固有，而不Blazor是特定的。 請務必閱讀並瞭解這些注意事項，然後才對啟用離線的應用程式的運作方式進行假設。

若要查看離線支援的運作方式：

1. 發行應用程式。 如需詳細資訊，請參閱<xref:host-and-deploy/blazor/index#publish-the-app>。
1. 將應用程式部署至支援 HTTPS 的伺服器，並在瀏覽器中以安全的 HTTPS 位址存取應用程式。
1. 開啟瀏覽器的 [開發工具]，並確認已在 [**應用程式**] 索引標籤上註冊主機的*服務背景工作*：

   ![Google Chrome 開發人員工具的 [應用程式] 索引標籤會顯示已啟動並執行的服務工作者。](progressive-web-app/_static/image4.png)

1. 重載頁面，並檢查 [**網路**] 索引標籤。 [**服務工作者**] 或 [**記憶體**快取] 會列為所有頁面資產的來源：

   ![Google Chrome 開發人員工具 [網路] 索引標籤，顯示所有頁面資產的來源。](progressive-web-app/_static/image5.png)

1. 若要確認瀏覽器不依賴網路存取來載入應用程式，請執行下列其中一個動作：

   * 關閉網頁伺服器，並查看應用程式如何繼續正常運作，包括頁面重載。 同樣地，當網路連線緩慢時，應用程式仍會繼續正常運作。
   * 指示瀏覽器模擬 [**網路**] 索引標籤中的離線模式：

   ![Google Chrome 開發人員工具 [網路] 索引標籤，其中 [瀏覽器模式] 下拉狀態從 [線上] 變更為 [離線]。](progressive-web-app/_static/image6.png)

使用服務工作者的離線支援是 web 標準，而不是特有Blazor的。 如需服務工作者的詳細資訊，請參閱[MDN web 檔：服務工作者 API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API)。 若要深入瞭解服務工作者的常見使用模式，請參閱[Google Web：服務工作者生命週期](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle)。

Blazor的 PWA 範本會產生兩個服務工作者檔案：

* *wwwroot/service-worker*，會在開發期間使用。
* *wwwroot/service-worker. 已發行的 .js*，會在發佈應用程式之後使用。

若要在兩個服務工作者檔案之間共用邏輯，請考慮下列方法：

* 新增第三個 JavaScript 檔案來保存通用邏輯。
* 使用[importScripts](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts)將通用邏輯載入兩個服務工作者檔案中。

### <a name="cache-first-fetch-strategy"></a>快取優先提取策略

內建的*service-worker。已發行的 .js*服務工作者會使用快取*優先*策略來解析要求。 這表示服務工作者偏好傳回快取的內容，不論使用者是否有網路存取權，或伺服器上是否提供較新的內容。

快取優先策略非常重要，因為：

* **它可確保可靠性。** &ndash;網路存取不是布林值狀態。 使用者不只是線上或離線：

  * 使用者的裝置可能會假設它已上線，但網路可能會很慢，而無法等待。
  * 網路可能會針對特定的 Url 傳回不正確結果，例如，當有一個目前正在封鎖或重新導向特定要求的驗證 WIFI 入口網站時。
  
  這就是為什麼瀏覽器`navigator.onLine`的 API 不可靠，且不應依賴的原因。

* **它可確保正確性。** &ndash;建立離線資源的快取時，服務工作者會使用內容雜湊來確保它已在單一時刻取得完整且自我一致的資源快照集。 然後，此快取會當做不可部分完成的單位來使用。 由於只需要已快取的版本，因此不會要求網路提供較新的資源。 其他任何風險不一致和不相容（例如，嘗試使用未一起編譯的 .NET 元件版本）。

### <a name="background-updates"></a>背景更新

作為心理模型，您可以將離線優先的 PWA 視為行為，就像可以安裝的行動應用程式。 無論網路連線為何，應用程式都會立即啟動，但已安裝的應用程式邏輯來自可能不是最新版本的時間點快照集。

Blazor PWA 範本會產生應用程式，以便在使用者造訪並具有正常運作的網路連線時，自動嘗試在背景中自行更新。 其運作方式如下：

* 在編譯期間，專案會產生*服務工作者資產資訊清單*。 根據預設，這稱為*service-worker-assets*。 資訊清單會列出應用程式離線運作所需的所有靜態資源，例如 .NET 元件、JavaScript 檔案和 CSS，包括其內容雜湊。 資源清單是由服務工作者載入，讓它知道要快取的資源。
* 每次使用者造訪應用程式時，瀏覽器會在背景重新要求*service-worker*和*service-worker-assets* 。 這些檔案會與現有已安裝的服務背景工作角色進行逐位元組比較。 如果伺服器傳回上述任一檔案的變更內容，服務工作者會嘗試安裝新版的本身。
* 安裝新版本的本身時，服務背景工作會為離線資源建立新的個別快取，並使用*service-worker-assets*中列出的資源開始擴展快取。 這個邏輯會在 service-worker 內`onInstall`的函式中實作為*已發行的 .js*。
* 載入所有資源時，如果沒有發生錯誤，且所有內容雜湊都相符，此程式就會順利完成。 如果成功，新的服務工作者會進入*等待啟用*狀態。 一旦使用者關閉應用程式（沒有剩餘的應用程式索引標籤或 windows），新的服務背景工作角色就*會變成作用中，並*用於後續的應用程式造訪。 系統會刪除舊的服務背景工作角色及其快取。
* 如果進程未順利完成，則會捨棄新的服務背景工作實例。 使用者下次造訪時，會再次嘗試更新程式，希望用戶端有更好的網路連線可以完成要求。

編輯服務背景工作邏輯以自訂此程式。 上述行為都不是特有的， Blazor但只是 PWA 範本選項所提供的預設體驗。 如需詳細資訊，請參閱[MDN web 檔：服務工作者 API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API)。

### <a name="how-requests-are-resolved"></a>如何解決要求

如快[取優先提取策略](#cache-first-fetch-strategy)一節中所述，預設服務工作者會使用快取優先策略，這表示它會嘗試提供快取*的*內容（如果有的話）。 如果沒有針對特定 URL 快取任何內容（例如，從後端 API 要求資料時），服務工作者會回復一般網路要求。 如果可以連線到伺服器，網路要求就會成功。 這個邏輯會在 service-worker `onFetch`內的函式中實作為*已發行的 .js*。

如果應用程式的Razor元件依賴來自後端 api 的要求資料，而您想要為失敗的要求提供易記的使用者體驗，因為網路無法使用，請在應用程式的元件內執行邏輯。 例如，使用`try/catch` [圍繞`HttpClient`要求]。

### <a name="support-server-rendered-pages"></a>支援伺服器呈現的頁面

請考慮當使用者第一次流覽至 URL （例如） `/counter`或應用程式中的任何其他深層連結時，會發生什麼情況。 在這些情況下，您不想要傳回快取`/counter`的內容，而是需要瀏覽器載入快取的`/index.html`內容，以啟動Blazor您的 WebAssembly 應用程式。 這些初始要求稱為「*導覽*要求」，而不是：

* *subresource*影像、樣式表單或其他檔案的要求。
* *提取/XHR* API 資料的要求。

預設的服務背景工作包含導覽要求的特殊案例邏輯。 無論要求的 URL 為何`/index.html`，服務工作者都會藉由傳回的快取內容來解析要求。 這個邏輯會在 service-worker 內`onFetch`的函式中實作為*已發行的 .js*。

如果您的應用程式有特定 Url 必須傳回伺服器轉譯的 HTML，而不`/index.html`是從快取中提供，則您需要編輯服務背景工作中的邏輯。 如果包含`/Identity/`的所有 url 都必須處理為一般僅線上的伺服器要求，則請修改*service-worker。已發行的 .js* `onFetch`邏輯。 找出下列程式碼：

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

將程式碼變更為下列內容：

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

如果您不這麼做，則不論網路連線為何，服務工作者都會攔截這類 Url 的要求，並使用`/index.html`加以解析。

### <a name="control-asset-caching"></a>控制資產快取

如果您的專案定義`ServiceWorkerAssetsManifest` MSBuild 屬性， Blazor的組建工具會使用指定的名稱來產生服務背景工作資產資訊清單。 預設 PWA 範本會產生包含下列屬性的專案檔：

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

檔案會放在*wwwroot*輸出目錄中，因此瀏覽器可以藉由要求`/service-worker-assets.js`來抓取此檔案。 若要查看此檔案的內容，請在文字編輯器中開啟 */BIN/DEBUG/{TARGET FRAMEWORK}/wwwroot/service-worker-assets.js* 。 不過，請不要編輯檔案，因為它會在每個組建上重新產生。

根據預設，此資訊清單會列出：

* 任何Blazor管理的資源（例如 .net 元件和 .net WebAssembly 執行時間檔案）都必須離線運作。
* 所有用來發行至應用程式*wwwroot*目錄的資源，例如影像、樣式表單和 JavaScript 檔案，包括外部專案和 NuGet 套件所提供的靜態 web 資產。

您可以藉由編輯`onInstall` *service-worker*中的邏輯，來控制服務工作者要提取和快取哪些資源。 根據預設，服務工作者會提取並快取符合一般 web 副檔名的檔案，例如 *.html*、 *.css*、 *.js*和*wasm*，再加上Blazor WebAssembly （*.dll*， *.pdb*）特有的檔案類型。

若要加入不存在於應用程式*wwwroot*目錄中的其他資源，請定義`ItemGroup`額外的 MSBuild 專案，如下列範例所示：

```xml
<ItemGroup>
  <ServiceWorkerAssetsManifestItem Include="MyDirectory\AnotherFile.json"
    RelativePath="MyDirectory\AnotherFile.json" AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

`AssetUrl`中繼資料會指定在提取要快取的資源時，瀏覽器應該使用的基底相對 URL。 這可以獨立于其在磁片上的原始來原始檔案名。

> [!IMPORTANT]
> 新增`ServiceWorkerAssetsManifestItem`不會導致檔案在應用程式的*wwwroot*目錄中發行。 發行輸出必須分開控制。 `ServiceWorkerAssetsManifestItem`只會導致服務工作者資產資訊清單中出現額外的專案。

## <a name="push-notifications"></a>推送通知

就像任何其他 PWA 一樣Blazor ，WEBASSEMBLY 的 pwa 也可以從後端伺服器接收推播通知。 伺服器可以隨時傳送推播通知，即使使用者未主動使用應用程式也一樣。 例如，當其他使用者執行相關動作時，可以傳送推播通知。

傳送推播通知的機制完全獨立于Blazor WebAssembly，因為它是由可使用任何技術的後端伺服器所執行。 如果您想要從 ASP.NET Core 伺服器傳送推播通知，請考慮[使用類似于技術比薩研討會所採用之方法的技巧](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications)。

在用戶端上接收和顯示推播通知的機制也獨立于Blazor WebAssembly，因為它是在服務背景工作 JavaScript 檔案中執行。 如需範例，請參閱在[進行中的比薩研討會中使用的方法](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications)。

## <a name="caveats-for-offline-pwas"></a>離線 Pwa 的注意事項

並非所有應用程式都應該嘗試支援離線使用。 離線支援增加了很大的複雜性，而不一定會與所需的使用案例相關。

離線支援通常僅與相關：

* 如果主要資料存放區是瀏覽器的本機。 例如，此方法與應用程式相關，其適用于[IoT](https://en.wikipedia.org/wiki/Internet_of_things)裝置的 UI，可將資料儲存`localStorage`在或[IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API)中。
* 如果應用程式執行大量的工作來提取和快取與每個使用者相關的後端 API 資料，使其可以離線流覽資料。 如果應用程式必須支援編輯，則必須建立追蹤變更的系統，並將資料與後端同步處理。
* 如果目標是要保證應用程式會立即載入，而不考慮網路狀況。 針對後端 API 要求執行適當的使用者體驗，以顯示要求的進度，並在要求因網路無法使用而失敗時，正常運作。

此外，具備離線功能的 Pwa 必須處理一系列額外的複雜性。 開發人員應該仔細熟悉下列各節中的注意事項。

### <a name="offline-support-only-when-published"></a>只有在發行時才支援離線

在開發期間，您通常會想要查看在瀏覽器中立即反映的每項變更，而不需要進行背景更新程式。 因此， Blazor的 PWA 範本只有在發佈時才會啟用離線支援。

建立具備離線功能的應用程式時，在開發環境中測試應用程式並不夠。 您必須在其 [已發佈] 狀態中測試應用程式，以瞭解它如何回應不同的網路狀況。

### <a name="update-completion-after-user-navigation-away-from-app"></a>使用者流覽離開應用程式之後的更新完成

直到使用者在所有索引標籤中流覽完應用程式之後，更新才會完成。 如 [[背景更新](#background-updates)] 區段中所述，在您將更新部署至應用程式之後，瀏覽器會提取已更新的服務背景工作檔案，以開始進行更新程式。

許多開發人員很驚訝，即使這項更新完成，在使用者離開所有索引標籤之前，都**不**會生效。 重新整理顯示應用程式的索引標籤並**不**足夠，即使它是顯示應用程式的唯一索引標籤也一樣。 在您的應用程式完全關閉之前，新的服務背景工作會維持在*等候啟動*狀態。 **這不是特有的Blazor，而是標準的 web 平臺行為。**

這通常是麻煩嘗試測試其服務工作者或離線快取資源更新的開發人員。 如果您簽入瀏覽器的開發人員工具，您可能會看到類似下列的內容：

![Google Chrome 的 [應用程式] 索引標籤會顯示應用程式的服務背景工作是「正在等候啟動」。](progressive-web-app/_static/image7.png)

只要「用戶端」清單（也就是顯示應用程式的索引標籤或視窗）不是空的，背景工作會繼續等待。 服務工作者執行此動作的原因是要保證一致性。 一致性表示會從相同的不可部分完成快取中提取所有資源。

測試變更時，您可能會發現按一下 [skipWaiting] 連結會很方便，如先前的螢幕擷取畫面所示，然後重載頁面。 您可以藉由撰寫服務背景工作的程式碼來[略過「等待」階段，並在更新時立即啟用，](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase)讓所有使用者都能自動化此作業。 如果您略過等待階段，就會放棄保證一律會從相同的快取實例一致地提取資源。

### <a name="users-may-run-any-historical-version-of-the-app"></a>使用者可以執行應用程式的任何歷程記錄版本

Web 開發人員 habitually 預期使用者只會執行其 web 應用程式的最新部署版本，因為這在傳統 web 散發模型中是正常的。 不過，離線優先的 PWA 更類似于原生行動應用程式，使用者不一定要執行最新版本。

如「[背景更新](#background-updates)」一節中所述，在您將更新部署至應用程式之後，**每個現有的使用者會繼續使用先前的版本至少一個進一步造訪**，因為更新會在背景中進行，而且在使用者之後離開時才會啟用。 此外，先前使用的版本不一定是您所部署的舊版。 先前的版本可以是*任何*歷程記錄版本，視使用者上次完成更新的時間而定。

如果您應用程式的前端和後端部分需要 API 要求的架構相關合約，這可能會產生問題。 您必須先確定所有使用者都已升級，才能部署回溯不相容的 API 架構變更。 或者，禁止使用者使用不相容的繼承應用程式。 此案例的需求與原生行動應用程式相同。 如果您在伺服器 Api 中部署中斷性變更，用戶端應用程式就會中斷，供尚未更新的使用者使用。

可能的話，請勿將中斷性變更部署至您的後端 Api。 如果您必須這麼做，請考慮使用[標準服務背景工作角色 api （例如 ServiceWorkerRegistration](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) ）來判斷應用程式是否為最新狀態，如果不是，則避免使用。

### <a name="interference-with-server-rendered-pages"></a>伺服器呈現頁面的干擾

如[支援伺服器呈現的頁面](#support-server-rendered-pages)一節中所述，如果您想要略過服務工作者針對所有導覽`/index.html`要求傳回內容的行為，請編輯服務背景工作中的邏輯。

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a>預設會快取所有服務工作者資產資訊清單內容

如[控制資產](#control-asset-caching)快取一節中所述， *service-worker-assets*檔案會在組建期間產生，並列出服務工作者應提取和快取的所有資產。

因為此清單預設包含所有發出至*wwwroot*的專案，包括外部封裝和專案所提供的內容，所以您必須小心不要在該處放入太多內容。 如果*wwwroot*目錄包含數百萬個映射，服務工作者會嘗試提取並快取全部，耗用過多的頻寬，而且很可能無法順利完成。

藉由編輯`onInstall` *service-worker*中的函式，來執行任意邏輯以控制應提取和快取的資訊清單內容子集。

### <a name="interaction-with-authentication"></a>與驗證互動

您可以使用 [PWA 範本] 選項搭配驗證選項。 具有離線功能的 PWA 也可以在使用者具有網路連線能力時支援驗證。

當使用者沒有網路連線時，他們無法驗證或取得存取權杖。 根據預設，嘗試在沒有網路存取的情況下流覽登入頁面會產生「網路錯誤」訊息。

您必須設計一個 UI 流程，讓使用者在離線時執行有用的工作，而不會嘗試驗證或取得存取權杖。 或者，您可以設計應用程式在網路無法使用時，以正常方式失敗。 如果您的應用程式中無法這麼做，您可能不會想要啟用離線支援。
