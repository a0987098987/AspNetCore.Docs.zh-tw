---
title: "使用 ASP.NET Core 中的靜態檔案"
author: rick-anderson
description: "使用 ASP.NET Core 上的靜態檔案"
keywords: "ASP.NET Core 靜態檔案、 HTML、 CSS、 JavaScript 靜態資產"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 69a4542c9b2a0d7091d05d42029e68384b760dd7
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a>使用 ASP.NET Core 中的靜態檔案

<a name=fundamentals-static-files></a>

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

靜態檔案，例如 HTML、 CSS、 影像和 JavaScript 中，是 ASP.NET Core 應用程式可以直接用戶端提供服務的資產。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)

## <a name="serving-static-files"></a>提供靜態檔案

靜態檔案通常位於`web root`(*\<內容根目錄 > / wwwroot*) 資料夾。 請參閱[內容的根](xref:fundamentals/index#content-root)和[Web 根目錄](xref:fundamentals/index#web-root)如需詳細資訊。 您通常會設定為目前的目錄內容的根，讓您的專案`web root`會在開發中找到。

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

靜態檔案可以儲存在底下的任何資料夾`web root`，存取與該根的相對路徑。 例如，當您建立使用 Visual Studio 的預設 Web 應用程式專案時，有數個資料夾內建立*wwwroot*資料夾- *css*，*映像*，和*js*。 可存取中的映像的 URI*映像*子資料夾：

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

為了讓服務靜態檔案，您必須設定[中介軟體](middleware.md)將靜態檔案新增至管線。 您可以設定的靜態檔案中介軟體上新增相依性*Microsoft.AspNetCore.StaticFiles*封裝加入您的專案，並再呼叫`UseStaticFiles`擴充方法，從`Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`中的檔案可讓`web root`(*wwwroot*依預設) servable。 稍後，我將示範如何讓其他目錄的內容與 servable `UseStaticFiles`。

您必須包含 NuGet 封裝 「 Microsoft.AspNetCore.StaticFiles"。

> [!NOTE]
> `web root`預設為*wwwroot*目錄中，但是您可以設定`web root`目錄用於`UseWebRoot`。

假設您有靜態檔案，您想要做的外部專案階層架構`web root`。 例如: 

* wwwroot
  * css
  * 影像
  * ...
* MyStaticFiles
  * test.png

若要存取要求*test.png*，設定靜態檔案中介軟體，如下所示：

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

若要要求`http://<app>/StaticFiles/test.png`做*test.png*檔案。

`StaticFileOptions()`可以設定回應標頭。 例如，下列程式碼會設定靜態檔案提供從*wwwroot*資料夾，然後設定`Cache-Control`標頭，讓它們可公開可快取的 10 分鐘 （600 秒）：

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

![已加入顯示的快取控制標頭的回應標頭](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>靜態檔案授權

提供靜態檔案模組**沒有**授權檢查。 任何檔案由提供服務，包括下*wwwroot*公開使用。 若要提供檔案為基礎的授權：

* 儲存它們的外部*wwwroot*和可存取靜態檔案中介軟體的任何目錄**和**

* 提供控制器動作，傳回透過`FileResult`套用授權的位置

## <a name="enabling-directory-browsing"></a>啟用目錄瀏覽

瀏覽目錄可讓您 web 應用程式的使用者，若要查看目錄和檔案中指定的目錄清單。 瀏覽目錄已停用預設基於安全性考量 (請參閱[考量](#considerations))。 若要啟用瀏覽目錄，請呼叫`UseDirectoryBrowser`擴充方法，從`Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

並加入所需的服務，藉由呼叫`AddDirectoryBrowser`擴充方法，從`Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

上述程式碼讓您瀏覽目錄*wwwroot/影像*資料夾使用 URL http://\<應用程式 > / MyImages，每個檔案和資料夾的連結：

![瀏覽目錄](static-files/_static/dir-browse.png)

請參閱[考量](#considerations)上啟用瀏覽時的安全性風險。

請注意兩個`app.UseStaticFiles`呼叫。 第一個，才能提供 CSS、 影像及中的 JavaScript *wwwroot*資料夾，然後瀏覽目錄的第二個呼叫*wwwroot/影像*資料夾使用 URL http://\<應用程式> / MyImages:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>服務的預設文件

設定預設的首頁提供站台訪客開始瀏覽您的網站時的起點。 為了讓您不必完整限定的 URI，使用者不提供預設頁面的 Web 應用程式，請呼叫`UseDefaultFiles`擴充方法，從`Startup.Configure`，如下所示。

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`之前必須先呼叫`UseStaticFiles`提供預設的檔案。 `UseDefaultFiles`是實際上並不提供此檔案的 URL 重新寫入器。 您必須啟用靜態檔案中介軟體 (`UseStaticFiles`) 來提供此檔案。

與`UseDefaultFiles`，就會搜尋要求的資料夾：

* default.htm
* default.html
* index.htm
* index.html

將服務從清單中找到的第一個檔案，因為如果要求的完整格式的 URI （雖然在瀏覽器 URL 將會繼續顯示要求的 URI）。

下列程式碼示範如何變更預設檔案名稱以*mydefault.html*。

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`結合的功能`UseStaticFiles`， `UseDefaultFiles`，和`UseDirectoryBrowser`。

下列程式碼可讓靜態檔案和預設檔案，以提供，但不允許瀏覽目錄：

```csharp
app.UseFileServer();
   ```

下列程式碼可讓靜態檔案中，預設檔案和目錄瀏覽：

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

請參閱[考量](#considerations)上啟用瀏覽時的安全性風險。 如同`UseStaticFiles`， `UseDefaultFiles`，和`UseDirectoryBrowser`，如果您想要提供檔存在於之外的`web root`，具現化，並設定`FileServerOptions`物件當做參數傳遞`UseFileServer`。 例如，假設下列目錄階層中您 Web 應用程式：

* wwwroot

  * css

  * 影像

  * ...

* MyStaticFiles

  * test.png

  * default.html

使用階層上述範例中，您可能想要啟用靜態檔案、 預設檔案，以及瀏覽`MyStaticFiles`目錄。 在下列程式碼片段中，達成透過單一呼叫`FileServerOptions`。

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

如果`enableDirectoryBrowsing`設`true`您才能呼叫`AddDirectoryBrowser`擴充方法，從`Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

使用的檔案階層和上述程式碼：

| URI            |                             回應  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

如果沒有名為檔案的預設值是在*MyStaticFiles*目錄、 http://\<應用程式 > / StaticFiles 的目錄清單的可點按的連結：

![靜態檔案清單](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles`和`UseDirectoryBrowser`需要 url http://\<應用程式 >，沒有尾端斜線及原因 StaticFiles 用戶端重新導向至 http:// /\<應用程式 > /StaticFiles/ （新增結尾的斜線）。 沒有內文件結尾的斜線相對 Url 會不正確。

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

`FileExtensionContentTypeProvider`類別包含 MIME 內容類型對應檔案副檔名集合。 在下列範例中，數個檔案延伸模組已登錄到已知的 MIME 型別，取代".rtf 」，並 「.mp4"移除。

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

請參閱[MIME 內容類型](http://www.iana.org/assignments/media-types/media-types.xhtml)。

## <a name="non-standard-content-types"></a>非標準的內容類型

ASP.NET 靜態檔案中介軟體可理解幾乎 400 已知的檔案內容類型。 如果使用者要求未知的檔案類型的檔案時，靜態檔案中介軟體會傳回 HTTP 404 （找不到） 回應。 如果已啟用目錄瀏覽，將會顯示檔案的連結，但 URI 會傳回 HTTP 404 錯誤。

下列程式碼可讓處理未知的類型，並會轉譯為影像未知的檔案。

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

上述程式碼，具有未知的內容類型的檔案要求將會傳回做為映像。

>[!WARNING]
> 啟用`ServeUnknownFileTypes`會造成安全性風險，建議使用它。  `FileExtensionContentTypeProvider`（上述說明） 提供了更安全的替代方法，來提供檔案與非標準擴充功能。

### <a name="considerations"></a>考量

>[!WARNING]
> `UseDirectoryBrowser`和`UseStaticFiles`可能會導致洩漏機密資料。 我們建議您**不**啟用目錄瀏覽在生產環境中。 請小心有關哪些目錄啟用與`UseStaticFiles`或`UseDirectoryBrowser`如整個目錄和所有子目錄可存取。 我們建議您在自己的目錄中保持公用內容，例如*\<內容根目錄 > / wwwroot*、 離開應用程式檢視、 組態檔等等。

* 使用公開內容的 Url`UseDirectoryBrowser`和`UseStaticFiles`可能會有區分大小寫，其基礎檔案系統的字元限制。 例如，Windows 不區分大小寫，但 Mac 和 Linux 不是。

* 在 IIS 中裝載的 ASP.NET Core 應用程式使用 ASP.NET 核心模組，將所有要求都轉送到應用程式包含靜態檔案的要求。 不使用 IIS 靜態檔案處理常式，因為它不會有機會處理要求之前會由 ASP.NET 核心模組。

* 若要移除 IIS 靜態檔案處理常式 （在伺服器或網站層級）：

     * 瀏覽至**模組**功能

     * 選取**StaticFileModule**清單中

     * 點選**移除**中**動作**[資訊看板]

>[!WARNING]
> 如果已啟用 IIS 靜態檔案處理常式**和**ASP.NET 核心模組 (ANCM) 的設定不正確 (例如如果*web.config*未部署)，系統將會處理靜態檔案。

* 程式碼檔案 （包括 c# 和 Razor） 應該放在應用程式專案外部`web root`(*wwwroot*依預設)。 這會建立您的應用程式的用戶端端內容和伺服器端原始程式碼，以防止伺服器端程式碼外洩清楚分隔。

## <a name="additional-resources"></a>其他資源

* [中介軟體](middleware.md)

* [ASP.NET Core 簡介](../index.md)
