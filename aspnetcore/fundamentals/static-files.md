---
title: "使用 ASP.NET Core 中的靜態檔案"
author: rick-anderson
description: "了解如何做及安全靜態檔案，並設定裝載的 ASP.NET Core web 應用程式中的中介軟體行為的靜態檔案。"
keywords: "ASP.NET Core 靜態檔案、 HTML、 CSS、 JavaScript 靜態資產"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 912923860939a1d1dd91ccc79862e23f9095d161
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>使用 ASP.NET Core 中的靜態檔案

由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Scott Addie](https://twitter.com/Scott_Addie)

靜態檔案，例如 HTML、 CSS、 影像和 JavaScript 中，是 ASP.NET Core 應用程式提供直接給用戶端的資產。 若要讓這些檔案的處理將需要一些設定。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>提供靜態檔案

靜態檔案會儲存在專案的 web 根目錄。 預設目錄是 *\<content_root > / wwwroot*，但是可以透過變更[UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)方法。 請參閱[內容的根](xref:fundamentals/index#content-root)和[Web 根目錄](xref:fundamentals/index#web-root)如需詳細資訊。

應用程式的 web 主機必須察覺之內容的根目錄。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`WebHost.CreateDefaultBuilder`方法設定為目前的目錄內容的根：

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

設定為目前的目錄內容的根，藉由叫用[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)內`Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

靜態檔案會存取透過 web 根目錄的相對路徑。 例如， **Web 應用程式**專案範本包含數個資料夾內*wwwroot*資料夾：

* **wwwroot**
  * **css**
  * **images**
  * **js**

若要存取的檔案中的 URI 格式*映像*子資料夾位於*http://\<server_address > /images/\<image_file_name >*。 例如， *http://localhost:9189/images/banner3.svg*。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果目標.NET Framework，加入[Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)封裝至您的專案。 如果目標.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)包含此套件。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

新增[Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)封裝至您的專案。

---

設定[中介軟體](xref:fundamentals/middleware)讓靜態檔案處理。

### <a name="serve-files-inside-of-web-root"></a>支援在 web 根目錄內的檔案

叫用[UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)方法內`Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

無參數`UseStaticFiles`方法多載會將標示為 servable web 根目錄中的檔案。 下列標記參考*wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>支援 web 根目錄之外的檔案

請考慮靜態檔案提供服務所在的 web 根目錄之外的目錄階層：

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*

要求可以存取*banner1.svg*檔案所設定的靜態檔案中介軟體，如下所示：

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

在上述程式碼， *MyStaticFiles*目錄階層會公開透過*StaticFiles* URI 區段。 若要要求*http://\<server_address > /StaticFiles/images/banner1.svg*做*banner1.svg*檔案。

下列標記參考*MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>設定 HTTP 回應標頭

A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions)物件可以用來設定 HTTP 回應標頭。 除了設定靜態檔案處理，從 web 根目錄下，下列程式碼設定`Cache-Control`標頭：

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append)方法存在於[Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/)封裝。

檔案已公開可快取為 10 分鐘 （600 秒）：

![已加入顯示的快取控制標頭的回應標頭](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>靜態檔案授權

靜態檔案中介軟體並不提供授權檢查。 任何檔案由提供服務，包括下*wwwroot*，是可公開存取。 若要提供檔案為基礎的授權：

* 儲存它們的外部*wwwroot*和可存取靜態檔案中介軟體的任何目錄**和**
* 提供透過授權套用到動作方法。 傳回[FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult)物件：

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>啟用目錄瀏覽

瀏覽目錄可讓您 web 應用程式的使用者看見的目錄清單，而且檔案內指定的目錄。 瀏覽目錄已停用預設基於安全性考量 (請參閱[考量](#considerations))。 啟用目錄瀏覽叫用[UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_)方法中的`Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

加入所需的服務所叫用[AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_)方法從`Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

上述程式碼讓您瀏覽目錄*wwwroot/影像*資料夾使用的 URL *http://\<server_address > / MyImages*，每個檔案和資料夾的連結：

![瀏覽目錄](static-files/_static/dir-browse.png)

請參閱[考量](#considerations)上啟用瀏覽時的安全性風險。

請注意兩個`UseStaticFiles`在下列範例會呼叫。 第一次呼叫可讓靜態檔案中的服務*wwwroot*資料夾。 第二次呼叫可讓您啟用目錄瀏覽的*wwwroot/影像*資料夾使用的 URL *http://\<server_address > / MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>預設文件

設定預設的首頁提供訪客的邏輯起點造訪您的網站時。 若要完整限定的 URI，使用者不提供預設的網頁，呼叫[UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)方法從`Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles`之前必須先呼叫`UseStaticFiles`提供預設的檔案。 `UseDefaultFiles`是實際上並不提供此檔案的 URL 重寫器。 啟用透過靜態檔案中的介軟體`UseStaticFiles`來提供此檔案。

與`UseDefaultFiles`，資料夾中搜尋的要求：

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

從清單中找到的第一個檔案是提供服務，即使該要求的完整格式的 URI。 在瀏覽器 URL 會繼續反映出要求的 URI。

下列程式碼變更的預設檔案名稱*mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_)結合的功能`UseStaticFiles`， `UseDefaultFiles`，和`UseDirectoryBrowser`。

下列程式碼可讓靜態檔案和預設的檔案服務。 瀏覽目錄未啟用。

```csharp
app.UseFileServer();
```

下列程式碼會根據透過啟用目錄瀏覽的無參數多載：

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

請考慮下列的目錄階層：

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*
  * *default.html*

下列程式碼可啟用靜態檔案、 預設檔案和目錄瀏覽的`MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser`時，必須呼叫`EnableDirectoryBrowsing`屬性值是`true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

Url 使用的檔案階層和之前程式碼，解決，如下所示：

| URI            |                             回應  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

如果沒有預設值命名的檔案存在於*MyStaticFiles*目錄， *http://\<server_address > / StaticFiles*傳回目錄的可點按的連結清單：

![靜態檔案清單](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles`和`UseDirectoryBrowser`使用 URL *http://\<server_address > / StaticFiles*沒有尾端斜線，以觸發用戶端重新導向至*http://\<server_address > /StaticFiles /*。 請注意結尾的斜線的加法。 文件中的相對 Url 會被視為無效，沒有尾端斜線。

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider)類別包含`Mappings`屬性做為副檔名的 MIME 內容類型的對應。 在下列範例中，數個檔案延伸模組已登錄到已知的 MIME 類型。 *.Rtf*取代擴充功能，和*.mp4*已移除。

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

請參閱[MIME 內容類型](http://www.iana.org/assignments/media-types/media-types.xhtml)。

## <a name="non-standard-content-types"></a>非標準的內容類型

靜態檔案中介軟體可理解幾乎 400 已知的檔案內容類型。 如果使用者要求未知的檔案類型的檔案時，靜態檔案中介軟體會傳回 HTTP 404 （找不到） 回應。 如果啟用目錄瀏覽時，會顯示檔案的連結。 URI 會傳回 HTTP 404 錯誤。

下列程式碼可讓處理未知的類型，並轉譯為影像未知的檔案：

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

使用上述的程式碼，具有未知的內容類型的檔案的要求會傳回做為映像。

> [!WARNING]
> 啟用[ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes)會造成安全性風險。 根據預設，已停用，不建議使用它。 [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider)提供更安全的替代方法，來提供檔案與非標準擴充功能。

### <a name="considerations"></a>考量

> [!WARNING]
> `UseDirectoryBrowser`和`UseStaticFiles`可能會導致洩漏機密資料。 強烈建議在生產環境中瀏覽的停用目錄。 請仔細檢閱 哪個目錄中已啟用透過`UseStaticFiles`或`UseDirectoryBrowser`。 整個目錄以及目錄及其子目錄會變成可公開存取。 存放區中的檔案適用於公開服務專用的目錄，例如 *\<content_root > / wwwroot*。 這些檔案分開 MVC 檢視，Razor 頁面 (2.x 僅)，設定檔、 等等。

* 使用公開內容的 Url`UseDirectoryBrowser`和`UseStaticFiles`可能會有區分大小寫，基礎檔案系統的字元限制。 例如，Windows 不區分&mdash;Mac 和 Linux 不是。

* ASP.NET Core 應用程式裝載於 IIS 使用[ASP.NET 核心模組 (ANCM)](xref:fundamentals/servers/aspnet-core-module)轉送至應用程式，包括靜態檔案要求的所有要求。 不會使用 IIS 靜態檔案處理常式。 它有沒有機會之前由 ANCM 處理要求。

* 完成下列步驟在 IIS 管理員移除 IIS 靜態檔案處理常式，在伺服器或網站層級：
    1. 瀏覽至**模組**功能。
    1. 選取**StaticFileModule**清單中。
    1. 按一下**移除**中**動作**[資訊看板]。

> [!WARNING]
> 如果已啟用 IIS 靜態檔案處理常式**和**ANCM 設定不正確、 靜態檔案都有受到處理。 發生這種情況，例如，如果*web.config*檔案未部署。

* 程式碼檔案 (包括*.cs*和*.cshtml*) 應用程式專案的 web 根目錄之外。 邏輯分離，因此會建立應用程式的用戶端的內容與伺服器端程式碼之間。 這可防止伺服器端程式碼外洩。

## <a name="additional-resources"></a>其他資源

* [中介軟體](xref:fundamentals/middleware)

* [ASP.NET Core 簡介](xref:index)