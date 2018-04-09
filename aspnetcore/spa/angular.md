---
title: 使用 ASP.NET Core 角度的專案範本
author: SteveSandersonMS
description: 了解如何開始使用 ASP.NET Core 單一頁面應用程式 (SPA) 專案範本，包含 Angular 和 Angular CLI。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: e3956bedbc243578f6dfdc09f5f043327de7c66b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>使用 ASP.NET Core 角度的專案範本

> [!NOTE]
> 這份文件不討論 ASP.NET Core 2.0 中的的 Angular 專案範本。 而是關於您可以手動更新的較新 Angular 範本。 此範本預設包含在 ASP.NET Core 2.1 之中。

已更新的 Angular 專案範本提供方便的起點在 ASP.NET Core 應用程式中使用 Angular 和 Angular CLI 來實作豐富的用戶端使用者介面 (UI)。

範本就相當於建立 ASP.NET Core 專案作為 API 後端，以及建立 Angular CLI 專案做為使用者介面。 範本在單一應用程式專案中提供了這兩種專案類型。 因此，應用程式專案可以建立並發佈成單一單位。

## <a name="create-a-new-app"></a>建立新的應用程式

如果使用 ASP.NET Core 2.0，請確定您已經[安裝更新的 Angular 專案範本](xref:spa/index#installation)。 如果您有 ASP.NET Core 2.1，不需要額外安裝它。

在空目錄中使用命令`dotnet new angular`以從命令提示字元建立新的專案。 例如，下列命令會在*my-new-app*目錄中建立應用程式並切換到該目錄：

```console
dotnet new angular -o my-new-app
cd my-new-app
```

使用 Visual Studio 或 .NET Core CLI 執行應用程式:

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)
開啟產生*.csproj*檔案，並從該處，像平常一樣執行應用程式。

在建置程序還原第一次執行，可能需要幾分鐘的 npm 相依性。 後續建置會更快。

#### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)
請確定您有環境變數呼叫`ASPNETCORE_Environment`值是`Development`。 在 Windows 上 （在非 PowerShell 提示） 中，執行`SET ASPNETCORE_Environment=Development`。 在 Linux 或 macOS 上，執行`export ASPNETCORE_Environment=Development`。

執行[dotnet 組建](/dotnet/core/tools/dotnet-build)以確認應用程式建置正確。 在第一個執行中，在建置程序還原 npm 相依性，這可能要花費幾分鐘的時間。 後續建置會更快。

執行[dotnet 執行](/dotnet/core/tools/dotnet-run)啟動應用程式。 會記錄類似下列訊息：

```console
Now listening on: http://localhost:<port>
```

在瀏覽器中瀏覽此 url。

應用程式在背景中啟動了 Angular CLI 伺服器的執行個體。 會記錄類似下列的訊息： <em>NG 即時程式開發伺服器正在接聽 localhost:&lt;otherport&gt;，開啟瀏覽器上http://localhost: &lt;otherport&gt; /</em>. 忽略此訊息&mdash;它有<strong>不</strong>結合的 ASP.NET Core 和有角度的方向 CLI 應用程式的 URL。

* * *
專案範本會建立 ASP.NET Core 應用程式與 Angular 應用程式。 ASP.NET Core 應用程式應用於資料存取、 授權和其他伺服器端的問題。 位於*ClientApp*子目錄的 Angular 應用程式應用於所有 UI 問題。

## <a name="add-pages-images-styles-modules-etc"></a>新增頁面、 影像、 樣式、 模組、 等等。

*ClientApp*目錄包含一般的 Angular CLI 應用程式。 如需詳細資訊請參閱正式的[Angular的文件](https://github.com/angular/angular-cli/wiki)。

使用此 Angular 範本建立的應用程式與使用 Angular CLI (透過`ng new`) 建立的應用程式有些微的不同; 不過，應用程式的功能，維持不變。 範本所建立的應用程式包含[Bootstrap](https://getbootstrap.com/)為基礎的配置和基本的路由範例。

## <a name="run-ng-commands"></a>執行 ng 命令

在命令提示字元切換至*ClientApp*子目錄：

```console
cd ClientApp
```

如果您有安裝全域的`ng`工具，您可以執行任何其命令。 例如，您可以執行`ng lint`， `ng test`，或任何其他[Angular CLI 命令](https://github.com/angular/angular-cli/wiki#additional-commands)。 不需要執行`ng serve`，因為您的 ASP.NET Core 應用程式會處理提供伺服器端和用戶端應用程式部分。 就內部而言，它會使用`ng serve`開發工作。

如果您尚未安裝`ng`工具，請改為執行`npm run ng`。 例如，您可以執行`npm run ng lint`或`npm run ng test`。

## <a name="install-npm-packages"></a>安裝 npm 套件

若要安裝協力廠商 npm 封裝，請使用 命令提示字元中*ClientApp*子目錄。 例如: 

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>發行與部署

在開發中，應用程式以方便開發人員最佳化模式執行。 例如，JavaScript 組合會包含來源對應 （以便偵錯時，您可以看到原始的 TypeScript 程式碼）。 應用程式監控的 TypeScript、 HTML 和 CSS 檔案變更，在磁碟上自動重新編譯並重新載入時看到這些變更的檔案。

在生產環境中，提供您已針對效能最佳化的應用程式的版本的服務。 這會設定為自動進行。 當您發行時，組建組態會發出用戶端程式碼的極簡化預先編譯 (AoT) 組建。 不同於開發組建中，正式組建不需要在伺服器上安裝 Node.js (除非您已啟用[伺服器端預先轉譯](#server-side-rendering))。

您可以使用標準[ASP.NET Core 裝載和部署方法](xref:host-and-deploy/index)。

## <a name="run-ng-serve-independently"></a>獨立執行 "ng serve"

專案已設定為當 ASP.NET Core 應用程式在開發模式中啟動時，會在背景中啟動自己的 Angular CLI 伺服器執行個體。 這是為了方便您不需要手動執行不同的伺服器。

這個預設設定有個缺點。 每當您修改 C# 程式碼且您的 ASP.NET Core 應用程式需要重新啟動時， Angular CLI 伺服器也會重新啟動。 大約 10 秒鐘，才能重新啟動。 如果您頻繁的編輯 C# 程式碼，而不想要等待 Angular CLI 重新啟動，則可以在 ASP.NET Core 程序外獨立執行 Angular CLI 伺服器。 若要這樣做：

1. 在命令提示字元切換至*ClientApp*子目錄，並啟動角度 CLI 程式開發伺服器：

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > 使用`npm start`啟動 Angular CLI 程式開發伺服器，不要用`ng serve`，以便遵守*package.json*中的設定。 若要將其他參數傳遞給 Angular CLI 伺服器，請將它們加入到您 *package.json*檔案中相關的`scripts`設定。

2. 修改您的 ASP.NET Core 應用程式以使用外部的 Angular CLI 執行個體，而不要啟動本身的執行個體。 在您的*Startup*類別中，以下列程式碼來取代 `spa.UseAngularCliServer`叫用：

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

當您啟動 ASP.NET Core 應用程式，它將不會啟動 Angular CLI 伺服器。 而是改為使用您以手動方式啟動的執行個體。 這可讓它更快地啟動與重新啟動。 每次重建您的用戶端應用程式時就不用再等候 Angular CLI。

## <a name="server-side-rendering"></a>伺服器端轉譯

效能的部分，您可以選擇在伺服器上預先轉譯 Angular 應用程式，或是在用戶端上執行應用程式。 這表示瀏覽器中會接收代表應用程式起始 UI 的 HTML 標記，因此甚至在下載及執行 JavaScript 組合前，就能顯示這些標記。 這個實作的大部分來自名為[通用 Angular](https://universal.angular.io/)的 Angular 功能。

> [!TIP]
> 啟用伺服器端轉譯 (SSR) 會使得開發和部署期間更為複雜。 請閱讀[SSR的缺點](#drawbacks-of-ssr)來判斷 SSR 是否適合您的需求。

若要啟用 SSR，您必須先新增一些調整到您的專案中。

在*Startup*類別中，於設定 `spa.Options.SourcePath` 的程式碼行之後，以及呼叫`UseAngularCliServer`或`UseProxyToSpaDevelopmentServer`之前，加入下列程式碼行：

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

在開發模式中，這段程式碼會執行在*ClientApp\package.json*中定義的指令碼`build:ssr`，以嘗試建立 SSR 組合。 這會建置名稱為`ssr` 但未定義的 Angular 的應用程式。 

在結尾`apps`陣列中*ClientApp/.angular-cli.json*，定義額外的應用程式，但名稱`ssr`。 使用下列選項：

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

這個新的 SSR 啟用應用程式設定需要進一步的兩個檔案： *tsconfig.server.json*和*main.server.ts*。 *Tsconfig.server.json*檔案會指定 TypeScript 編譯選項。 *Main.server.ts*期間 SSR 檔將做為程式碼進入點。

加入新的檔案稱為*tsconfig.server.json*內*ClientApp/src* (與現有*tsconfig.app.json*)，其中包含下列：

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

這個檔案會設定要尋找模組呼叫 Angular 的 AoT 編譯器`app.server.module`。 藉由建立新的檔案，在加入此*ClientApp/src/app/app.server.module.ts* (連同現有*app.module.ts*) 包含下列： 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

此模組會繼承您的用戶端`app.module`和定義在 SSR 期間可以使用的額外 Angular 模組。

回憶一下*.angular cli.json*中的新`ssr`項目參考了名為*main.server.ts*的進入點檔案。 您尚未在該檔案中新增任何項目，現在正是時候了。 請在*ClientApp/src/main.server.ts* 中建立新檔案 (連同現有的*main.ts*)，並在其中包含下列程式碼：

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

當 ASP.NET Core 執行您加入*啟動*類別的 `UseSpaPrerendering` 中介軟體時，會針對每個要求來執行此檔案的程式碼。 它會接收來自 .NET 程式碼的`params`(例如所要求的 URL)，以及呼叫 Angular SSR API，以取得產生的 HTML。 

嚴格來說，在開發模式啟用 SSR 已經足夠了。 請務必做最後一個變更，以便發行時，您的應用程式能正常運作。 在您的在您的應用程式主要的 *.csproj*檔案中，設定`BuildServerSideRenderer`屬性值設定為`true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

這會設定建置流程以便在發行期間執行`build:ssr`，以及將 SSR 檔案部署至伺服器。 如果您不要啟用此功能，在生產環境中 SSR 會失敗。

應用程式在開發或實際執行模式中執行時，Angular 程式碼會在伺服器上預先轉譯為 HTML。 用戶端程式碼則正常的執行。

### <a name="pass-data-from-net-code-into-typescript-code"></a>.NET 程式碼中的資料，傳遞到 TypeScript 程式碼

在 SSR 期間，建議將要求的資料從 ASP.NET Core 應用程式傳入 Angular 的應用程式。 比方說，您可以傳遞 Cookie 資訊或從資料庫讀取的資料。 若要這樣做，請編輯您的*Startup*類別。 在`UseSpaPrerendering`的回呼中，將 `options.SupplyData`的值設定如下：

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData`回呼可讓您傳遞任意、 每個要求、 可序列化 JSON 資料 （例如字串、 布林值或數字）。 您*main.server.ts*程式碼會接收為`params.data`。 例如，上述程式碼範例會傳遞做為布林值`params.data.isHttpsRequest`到`createServerRenderer`回呼。 您可以將它傳遞給您的應用程式，以支援 Angular 任何方式的其他組件。 例如，請參閱如何*main.server.ts*傳遞`BASE_URL`接收該宣告的建構函式的任何元件的值。

### <a name="drawbacks-of-ssr"></a>SSR 的缺點

並非所有應用程式都會從 SSR 獲益。 主要優點是效能明顯提升。 透過低速網路連線或較慢行動裝置而來到應用程式的訪客，即使需要花時間來擷取或剖析 JavaScript 組合，也可以快速看到初始 UI。 不過，許多 SPA 主要用在快速內部公司網路的快速電腦上。在這些電腦上，應用程式幾乎會即時出現。

同時，啟用 SSR 會有很大的缺點。 您的開發程序將會更為複雜。 您的程式碼必須在兩個不同環境中執行：用戶端和伺服器端 (在從 ASP.NET Core 叫用的 Node.js 環境中)。 以下是要注意的一些事項：

* SSR 需要安裝在實際執行伺服器上的 Node.js。 這會自動針對某些部署案例，例如 Azure 應用程式服務，但不適用於其他如 Azure Service Fabric 的情況。
* 啟用`BuildServerSideRenderer`erverSideRenderer` 建置旗標會導致您*node_modules*目錄發行。 此資料夾包含 20,000 個以上的檔案，這會增加部署的時間。
* 若要在 Node.js 環境中執行您的程式碼，它不能依賴瀏覽器特定的 JavaScript API，像是 `window`或`localStorage`。 如果您的程式碼 (或您參考的某些協力廠商程式庫) 嘗試使用這些 API，在 SSR 期間將會出現錯誤。 例如，請勿使用 jQuery，因為它在許多地方參考了瀏覽器專用的 API。 若要避免這個錯誤，您必須避免 SSR，或避免瀏覽器特定的 API 或程式庫。 您可以在檢查中包裝這類 API 呼叫，以確保不會在 SSR 期間叫用。 比方說，在 JavaScript 或 TypeScript 程式碼中使用如下的檢查：

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
