---
title: 使用 ASP.NET Core Blazor WebAssembly 建立漸進式 Web 應用程式
author: guardrex
description: 瞭解如何建立以 Blazor為基礎的漸進式 Web 應用程式（Pwa），這是使用新式瀏覽器功能的 Web 應用程式，其行為就像桌面應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: f1c1ce50f20bf579e67e1d4eb02cc7d9d681e354
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083717"
---
# <a name="build-progressive-web-applications-with-aspnet-core-opno-locblazor-webassembly"></a>使用 ASP.NET Core Blazor WebAssembly 建立漸進式 Web 應用程式

作者：[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

漸進式 Web 應用程式（PWA）是以 web 為基礎的應用程式，其使用新式瀏覽器 Api 和功能，其行為就像桌面應用程式。 這些功能可以包括：

* 離線工作並一律立即載入，獨立于網路速度
* 能夠在自己的應用程式視窗中執行，而不只是瀏覽器視窗
* 從主機作業系統（OS） [開始] 功能表、[停駐] 或 [主畫面] 啟動
* 從後端伺服器接收推播通知，即使使用者未使用應用程式
* 在背景中自動更新

使用者可能會先在網頁瀏覽器中探索及使用應用程式，就像任何其他單頁應用程式（SPA），然後在其 OS 中安裝它並啟用推播通知。 這就是我們使用「*漸進*」一詞的原因。

Blazor WebAssembly 是真正以標準為基礎的用戶端 web 應用程式平臺，因此可以使用任何瀏覽器 API，包括以上所列功能所需的 PWA Api。 它可以離線工作，就像任何其他用戶端 web 技術一樣。

## <a name="pwa-template"></a>PWA 範本

建立新的 Blazor WebAssembly 應用程式時，系統會提供您新增 PWA 功能的選項。 在 Visual Studio 中，會在 [專案建立] 對話方塊中將選項指定為核取方塊：

![image](https://user-images.githubusercontent.com/1101362/76207411-a6b54200-61f5-11ea-9dfc-6acd87a91428.png)

如果您要在命令列上建立專案，您可以使用 `--pwa` 旗標。 例如：

```dotnetcli
dotnet new blazorwasm --pwa -o MyNewProject
```

在這兩種情況下，您都可以視需要將它與「ASP.NET Core 裝載」選項結合，但不需要這麼做。 PWA 功能與裝載模型無關。

## <a name="installation-and-app-manifest"></a>安裝和應用程式資訊清單

流覽使用 PWA 範本選項建立的應用程式時，使用者可以選擇將應用程式安裝到其作業系統的 [開始] 功能表、[停駐] 或 [主畫面]。

此選項的呈現方式取決於使用者的瀏覽器。 例如，使用以桌面 Chromium 為基礎的瀏覽器（例如 Edge 或 Chrome）時，URL 列中會出現 [*新增*] 按鈕：

![image](https://user-images.githubusercontent.com/1101362/76208127-f7796a80-61f6-11ea-8aea-7fba704be787.png)

在 iOS 上，訪客可以使用 Safari 的 [*共用*] 按鈕和其 [*新增至 Homescreen* ] 選項來安裝 PWA。 在適用于 Android 的 Chrome 上，使用者應按右上角的 [*功能表*] 按鈕，然後選擇 [*新增到主畫面*]。

安裝之後，應用程式會出現在自己的視窗中，而不會有任何網址列。

![image](https://user-images.githubusercontent.com/1101362/76208588-e2e9a200-61f7-11ea-85e1-8c3fc849b252.png)

若要自訂視窗的標題、色彩配置、圖示或其他詳細資料，請參閱專案的*wwwroot*目錄中的檔案 `manifest.json`。 此檔案的架構是由 web 標準所定義。 如需詳細檔，請參閱 https://developer.mozilla.org/docs/Web/Manifest。

## <a name="offline-support"></a>離線支援

根據預設，使用 PWA 範本選項建立的應用程式支援離線執行。 使用者必須先在上線時造訪應用程式，瀏覽器才會自動下載並快取離線操作所需的所有資源。

> [!IMPORTANT]
> 只有*已發佈*的應用程式才會啟用離線支援。 在開發期間不會啟用此功能。 這是因為它會干擾進行變更和測試的一般開發週期。

> [!WARNING]
> 如果您想要提供已啟用離線的 PWA，必須瞭解[幾個重要的警告和注意事項](#caveats-for-offline-pwas)。 這些是離線 Pwa 的固有特性，而不是 Blazor特定的。 請務必閱讀並瞭解這些注意事項，然後才對啟用離線的應用程式的運作方式進行假設。

若要查看離線支援的運作方式，請先[發佈您的應用程式](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/?view=aspnetcore-3.1&tabs=visual-studio#publish-the-app)，並將它裝載在支援 HTTPS 的伺服器上。 當您造訪應用程式時，您應該能夠開啟瀏覽器的開發工具，並確認已為您的主機註冊*服務背景工作角色*：

![image](https://user-images.githubusercontent.com/1101362/76210294-bd5e9780-61fb-11ea-9869-65c55c62803d.png)

此外，如果您重載頁面，則在 [*網路*] 索引標籤上，您應該會看到載入頁面所需的所有資源都是從*服務背景工作*或*記憶體*快取取得：

![image](https://user-images.githubusercontent.com/1101362/76210472-1d553e00-61fc-11ea-84c6-291644df709e.png)

這會顯示瀏覽器不依賴網路存取來載入您的應用程式。 若要確認這種情況，您可以關閉 web 伺服器，或指示瀏覽器模擬離線模式：

![image](https://user-images.githubusercontent.com/1101362/76210556-47a6fb80-61fc-11ea-9d12-20a8f6528744.png)

現在，即使沒有 web 伺服器的存取權，您也應該能夠重載頁面，並查看您的應用程式仍會載入並執行。 同樣地，即使您模擬非常緩慢的網路連線，您的頁面仍會立即載入，因為它是與網路無關的載入。

### <a name="service-worker"></a>服務背景工作

您可使用服務工作者來完成離線支援。 這是 web 標準，不是 Blazor特定的。 如需服務工作者的相關檔，請參閱 https://developer.mozilla.org/docs/Web/API/Service_Worker_API。 若要深入瞭解服務工作者的常見使用模式，請參閱[服務工作者生命週期](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle)的絕佳文章。

Blazor的 PWA 範本會產生兩個服務工作者檔案：

* *wwwroot/service-worker*，在開發期間使用
* *wwwroot/service-worker. 已發行的 .js*，會在您的應用程式發行後使用

如果您想要在這兩個檔案之間共用邏輯，請考慮新增第三個 JavaScript 檔案來保存通用邏輯，並使用[`self.importScripts`](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts)將該邏輯載入這兩個檔案。

#### <a name="cache-first-fetch-strategy"></a>快取優先提取策略

內建的*service-worker。已發行的 .js*服務工作者會使用快取*優先*策略來解析要求。 這表示不論使用者是否有網路存取權，或伺服器上是否有較新的內容，一律會偏好傳回快取的內容（如果有的話）。

這是很重要的原因有兩個：

* **它可確保可靠性。** 網路存取不是布林值狀態。 使用者不只是「線上」或「離線」。 事實上，使用者的裝置可能會相信它已上線，但網路可能會變得很慢，而無法等待。 或者，網路可能會傳回特定 Url 的無效結果，例如，有一個目前正在封鎖或重新導向特定要求的「正在進行」的驗證 WIFI 入口網站。 這就是為什麼瀏覽器的 `navigator.onLine` API 不可靠，而且不應該依賴。
* **它可確保正確性。** 建立離線資源的快取時，服務工作者會使用內容雜湊來確保它已在單一時刻取得完整且自我一致的資源快照集。 然後，此快取會當做不可部分完成的單位來使用。 在此情況下，並不會要求網路提供較新的資源，因為您想要的是您已快取的版本。 其他任何風險不一致和不相容（例如，嘗試使用未一起編譯的 .NET 元件版本）。

#### <a name="background-updates"></a>背景更新

作為心理模型，您可以將離線優先的 PWA 視為行為，就像可以安裝的行動應用程式。 它一律會立即啟動，而不考慮網路連線，但已安裝的應用程式邏輯來自可能不是最新版本的時間點快照集。

Blazor PWA 範本會產生應用程式，以便在使用者造訪並具有運作中的網路連線時，自動嘗試在背景中自行更新。 其運作方式如下：

* 在編譯期間，您的專案會產生*服務工作者資產資訊清單*。 根據預設，這稱為*service-worker-assets*。 這會列出應用程式離線運作所需的所有靜態資源，例如 .NET 元件、JavaScript 檔案、CSS 等，包括其內容雜湊。 這份清單是由您的服務工作者載入，讓它知道要快取的資源。
* 每次使用者造訪您的應用程式時，瀏覽器會在背景重新要求*service-worker*和*service-worker-assets* 。 如果伺服器傳回其中任一檔案的變更內容（與現有已安裝的服務背景工作角色的位元組比較），服務工作者會嘗試安裝新版的。
* 安裝新版本的本身時，服務背景工作會為離線資源建立新的個別快取，並使用*service-worker-assets*中列出的資源開始擴展。 這個邏輯會在 service-worker 內的 `onInstall` 函式中實作為*已發行的 .js*。
* 如果程式順利完成（亦即，所有資源都已載入而沒有錯誤，且所有內容雜湊都符合），則新的服務工作者會進入「等待啟用」狀態。 一旦使用者關閉您的應用程式（亦即，沒有剩餘的索引標籤或 windows 顯示您的應用程式），新的服務背景工作角色就會變成「作用中」，並用於後續造訪您的應用程式。 系統會刪除舊的服務背景工作角色及其快取。
* 如果進程未順利完成，則會捨棄新的服務背景工作實例。 當使用者下一次造訪時，將會再次嘗試更新程式，希望他們有更好的網路連線可以完成要求。

您可以藉由編輯服務背景工作邏輯來自訂此程式的任何層面。 上述不是 Blazor特有的，但只是 PWA 範本選項所提供的建議。 如需詳細資訊，請參閱[服務工作者檔](https://developer.mozilla.org/docs/Web/API/Service_Worker_API.)。

#### <a name="how-requests-are-resolved"></a>如何解決要求

如上所述，預設的服務工作者會使用快取*優先*策略，這表示它會嘗試提供快取的內容（如果有的話）。 如果沒有針對特定 URL 快取任何內容（例如，從後端 API 要求資料時），服務工作者會回復一般網路要求，只有在可連線到伺服器的情況下才會成功。 這個邏輯會在 service-worker 內的 `onFetch` 中實作為*已發行的 .js*。

如果您的 Blazor 元件依賴從後端 Api 要求資料，而且您想要在這類要求因為網路無法使用而失敗的情況下提供易記的使用者體驗，則您需要在元件內執行邏輯。 例如，使用 `try/catch` 圍繞 `HttpClient` 的要求。

#### <a name="support-server-rendered-pages"></a>支援伺服器呈現的頁面

請考慮當使用者第一次流覽至 URL （例如 `/counter` 或應用程式的任何其他深層連結）時，會發生什麼情況。 在這些情況下，您不想要傳回快取為 `/counter`的內容，而是需要瀏覽器載入快取為 `/index.html` 的內容，以啟動您的 Blazor WebAssembly 應用程式。 這些初始要求稱為「*導覽*要求」（相對於*subresource*對影像/CSS/等的要求，或 API 資料的*fetch/XHR*要求）。

預設的服務背景工作包含導覽要求的特殊案例邏輯。 它會藉由傳回 `/index.html`的快取內容來解決這些問題，而不論要求的 URL 為何。 這個邏輯會在 service-worker 內的 `onFetch` 函式中實作為*已發行的 .js*。

如果您的應用程式有特定 Url 必須傳回伺服器呈現的 HTML （而不是從快取提供 `/index.html`），則您需要編輯服務背景工作中的邏輯。 例如，如果包含 `/Identity/` 的所有 Url 都必須處理為一般僅線上的伺服器要求，則請修改*service-worker。已發行的 .js* `onFetch` 邏輯。 找出下列程式碼：

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

將程式碼變更為下列內容：

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

如果您不這麼做，則不論網路連線為何，服務工作者都會攔截這類 Url 的要求，並使用 `/index.html`加以解析。

#### <a name="control-asset-caching"></a>控制資產快取

如果您的專案定義了名為 `ServiceWorkerAssetsManifest`的 MSBuild 屬性，則 Blazor的組建工具會產生具有指定名稱的服務背景工作資產資訊清單。 預設 PWA 範本會產生包含下列專案的專案檔：

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

檔案會放在*wwwroot*輸出目錄中，因此瀏覽器可以藉由要求 `/service-worker-assets.js`來抓取此檔案。 若要查看內容，請在文字編輯器中開啟*YourProject\bin\Debug\netstandard2.1\wwwroot\service-worker-assets.js* 。 不過，請不要編輯檔案，因為它會在每個組建上重新產生。

根據預設，此資訊清單會列出：

* 任何 Blazor管理的資源，例如 .NET 元件和 .NET WebAssembly 執行時間檔案，需要離線運作
* 將在*wwwroot*目錄中發行的所有資源，例如影像、CSS 檔案和 JavaScript 檔案。 這包括外部專案和 NuGet 套件所提供的靜態 web 資產。

您可以藉由在*service-worker*中編輯 `onInstall` 中的邏輯，來控制服務工作者會提取和快取哪些資源。 根據預設，它會提取和快取符合一般 web 副檔名的檔案，例如 *.html*、 *.css*、 *.js*、 *wasm*和其他檔案，再加上 Blazor WebAssembly （ *.dll*， *.pdb*）特定的檔案類型。

如果您想要包含不存在於*wwwroot*目錄中的其他資源，您可以藉由定義額外的 MSBuild itemgroup 專案來執行此動作。 例如，在您的專案檔中，新增：

```xml
<ItemGroup>
    <ServiceWorkerAssetsManifestItem
        Include="MyDirectory\AnotherFile.json"
        RelativePath="MyDirectory\AnotherFile.json"
        AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

`AssetUrl` 中繼資料會指定瀏覽器在提取要快取的資源時，應使用的基底相對 URL。 這可以獨立于其在磁片上的原始來原始檔案名。

> [!IMPORTANT]
> 新增 `ServiceWorkerAssetsManifestItem` 不會使檔案在您的*wwwroot*目錄中發行。 您必須分別控制發行輸出。 `ServiceWorkerAssetsManifestItem` 只會導致服務工作者資產資訊清單中出現額外的專案。

## <a name="push-notifications"></a>推播通知

就像任何其他 PWA 一樣，Blazor WebAssembly PWA 也可以從後端伺服器接收推播通知。 即使使用者未主動使用您的應用程式（例如，當其他使用者執行可能相關的動作時），您的伺服器也可以隨時傳送這些訊息。

傳送推播通知的機制完全獨立于 Blazor WebAssembly，因為它是由可使用任何技術的後端伺服器所執行。 如果您想要從 ASP.NET Core 伺服器傳送推播通知，請考慮[使用類似于技術比薩研討會中的技巧](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications)。

在用戶端上接收和顯示推播通知的機制也獨立于 Blazor WebAssembly，因為它是在服務背景工作中執行，這是 JavaScript 檔案。 例如，您可以再次看到在[進行中的比薩研討會中使用的方法](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications)。

## <a name="caveats-for-offline-pwas"></a>離線 Pwa 的注意事項

並非所有應用程式都應該嘗試支援離線使用。 它會增加相當大的複雜性，而不一定會相關。

離線支援通常僅與相關：

* 如果您的主要資料存放區是瀏覽器的本機。 例如，針對將資料儲存在 `localStorage` 或[IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API)中的[IOT](https://en.wikipedia.org/wiki/Internet_of_things)裝置建立 UI。

* 如果您執行大量工作來提取和快取與每個使用者相關的後端 API 資料，他們就可以離線流覽它。 如果您支援編輯，您也必須建立一個系統來追蹤變更，並將它們與後端同步處理。

* 如果您的目標是保證應用程式會立即載入，而不論網路狀況為何。 接著，您必須在後端 API 要求中執行適當的使用者體驗，以顯示要求的進度，並在因網路無法使用而失敗時，以正常的方式運作。

此外，具備離線功能的 Pwa 必須處理一系列額外的複雜性。 開發人員應該謹慎熟悉下列注意事項。

### <a name="offline-support-only-when-published"></a>只有在發行時才支援離線

Blazor的 PWA 範本只有在發佈時才會啟用離線支援。 這是因為在開發期間，您通常會想要查看瀏覽器中立即反映的每項變更，而不需要進行背景更新程式。

因此，在建立具備離線功能的應用程式時，在開發模式中測試您的應用程式並不夠。 您必須以其已發佈狀態測試您的應用程式，以瞭解它會如何回應不同的網路狀況。

### <a name="update-completion-after-user-navigation-away-from-app"></a>使用者流覽離開應用程式之後的更新完成

在使用者從所有索引標籤流覽您的應用程式之前，更新都不會完成。 如[背景更新](#background-updates)中所述，在您將更新部署至應用程式之後，瀏覽器將會提取已更新的服務背景工作檔案，並開始進行更新程式。

許多開發人員很驚訝，即使這項更新完成，在使用者離開所有索引標籤之前，都**不**會生效。 重新整理顯示應用程式的索引標籤並**不**足夠，即使它是顯示應用程式的唯一索引標籤也一樣。 在您的應用程式完全關閉之前，新的服務工作者會繼續處於「正在等候啟動」狀態。 **這不是 Blazor特有的，而是標準的 web 平臺行為。**

這通常是麻煩嘗試測試其服務工作者或離線快取資源更新的開發人員。 如果您簽入瀏覽器的開發工具，您可能會看到類似下列的內容：

![image](https://user-images.githubusercontent.com/1101362/76226394-b93f7380-6215-11ea-8572-7d52afee2dd8.png)

只要「用戶端」清單（也就是顯示您應用程式的索引標籤或視窗）不是空的，工作者就會繼續等待。 服務工作者執行此動作的原因是要保證一致性，亦即，所有資源都是從相同的不可部分完成快取中提取。

測試變更時，您可能會發現按一下 [skipWaiting] 連結很方便，如上方螢幕擷取畫面所示，然後重載頁面。 如有需要，您可以將服務工作者編碼以[略過「等待」階段，並在更新時立即啟用](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase)，讓所有使用者都能自動執行此作業。 不過，如果您這樣做，您就能保證資源一律會以一致的方式從相同的快取實例提取。

### <a name="users-may-run-any-historical-version-of-the-app"></a>使用者可以執行應用程式的任何歷程記錄版本

Web 開發人員 habitually 預期使用者只會執行其 web 應用程式的最新部署版本，因為這在傳統 web 散發模型中是正常的。 不過，離線優先的 PWA 更類似于原生行動應用程式，使用者不一定要執行最新版本。

如[背景更新](#background-updates)中所述，在您將更新部署至應用程式之後，**每個現有的使用者都將繼續使用先前的版本，以進行至少一次的流覽**（因為更新是在背景中進行，而且在使用者接著流覽離開之前不會啟用）。 此外，先前使用的版本不一定是您所部署的前一版-它可以是*任何*歷程記錄版本，視使用者上次完成更新的時間而定。

如果應用程式的前端和後端部分需要 API 要求架構的合約，這可能會有問題。 您必須先確定所有使用者都已升級，或至少封鎖使用者使用不相容的繼承應用程式，才能部署回溯不相容的 API 架構變更。 這就像原生行動應用程式一樣。 如果您在伺服器 Api 中部署中斷性變更，用戶端應用程式將會中斷，供尚未更新的人員使用。

可能的話，請勿將中斷性變更部署至您的後端 Api。 但是，如果您必須這麼做，請考慮使用[標準服務背景工作角色 api （例如 `ServiceWorkerRegistration`](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) ）來判斷應用程式是否為最新狀態，如果不是，則避免使用。

### <a name="interference-with-server-rendered-pages"></a>伺服器呈現頁面的干擾

[如上所述，如果](#support-server-rendered-pages)您想要略過服務工作者針對所有導覽要求傳回 `/index.html` 內容的行為，您必須編輯服務背景工作中的邏輯。

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a>預設會快取所有服務工作者資產資訊清單內容

[如上所述，](#control-asset-caching) *service-worker-assets*檔案會在組建期間產生，並列出服務工作者應提取和快取的所有資產。

因為此清單預設包含所有發出至*wwwroot*的內容（包括外部封裝和專案所提供的內容），所以您必須小心不要在該處放入太多內容。 例如，如果您的*wwwroot*目錄包含數百萬個影像，服務工作者會嘗試提取並快取全部，耗用過多的頻寬，而且很可能無法順利完成。

您可以藉由編輯*service-worker*中的 `onInstall` 函式，來執行任意邏輯來控制要提取和快取的資訊清單內容子集。

### <a name="interaction-with-authentication"></a>與驗證互動

您可以使用 [PWA 範本] 選項搭配驗證選項。 具有離線功能的 PWA 也可以在使用者具有網路連線能力時支援驗證。

不過，當使用者沒有網路連線時，他們將無法驗證或取得存取權杖。 嘗試造訪 [登入] 頁面預設會顯示一則訊息，指出「網路錯誤」。

因此，您可以設計 UI 流程，讓使用者在離線時執行有用的工作，而不會嘗試驗證或取得存取權杖，或至少在這些情況下以正常方式失敗。 如果您的應用程式中無法這麼做，您可能不會想要啟用離線支援。
