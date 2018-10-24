---
title: ASP.NET Core 中的靜態檔案
author: rick-anderson
description: 了解如何在 ASP.NET Core Web 應用程式中提供靜態檔案、保護其安全，並設定靜態檔案裝載中介軟體的行為。
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
uid: fundamentals/static-files
ms.openlocfilehash: 52c7916b9fc55c875d56acd49c01f76dd2053817
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2018
ms.locfileid: "47861001"
---
# <a name="static-files-in-aspnet-core"></a>ASP.NET Core 中的靜態檔案

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Scott Addie](https://twitter.com/Scott_Addie) 撰寫

HTML、CSS、影像和 JavaScript 這類靜態檔案都是 ASP.NET Core 應用程式直接提供給用戶端的資產。 您需要進行一些設定，才能提供這些檔案。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>提供靜態檔案

靜態檔案會儲存在專案的 Web 根目錄中。 預設目錄是 *\<content_root>/wwwroot*，但是可以透過 [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) 方法進行變更。 如需詳細資訊，請參閱[內容根目錄](xref:fundamentals/index#content-root)和 [Web 根目錄](xref:fundamentals/index#web-root)。

您必須讓應用程式的 Web 主機記住內容根目錄。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

`WebHost.CreateDefaultBuilder` 方法可將內容根目錄設定為目前的目錄：

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

您可以叫用 `Program.Main` 內的 [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)，以將內容根目錄設定為目前的目錄：

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

您可透過 Web 根目錄的相對路徑來存取靜態檔案。 例如，**Web 應用程式**專案範本的 *wwwroot* 資料夾內包含數個資料夾：

* **wwwroot**
  * **css**
  * **images**
  * **js**

若要存取 *images* 子資料夾的檔案，其 URI 格式為 http://\<伺服器位址>/images/\<影像檔名稱>。 例如，*http://localhost:9189/images/banner3.svg*。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如以 .NET Framework 為目標，請將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) 套件新增至您的專案。 如以 .NET Core 為目標，[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)會包含此套件。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) 套件新增至您的專案。

---

設定[中介軟體](xref:fundamentals/middleware/index)以提供靜態檔案。

### <a name="serve-files-inside-of-web-root"></a>提供 Web 根目錄內的檔案

叫用 `Startup.Configure` 內的 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法：

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

無參數的 `UseStaticFiles` 方法多載會將 Web 根目錄中的檔案標示為可提供。 下列標記參考 *wwwroot/images/banner1.svg*：

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>提供 Web 根目錄外的檔案

請考慮使用目錄階層，以提供不位於 Web 根目錄的靜態檔案：

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*

您可以設定靜態檔案中介軟體，以讓要求存取 *banner1.svg* 檔案，如下所示：

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

上述程式碼會透過 *StaticFiles* URI 區段公開 *MyStaticFiles* 目錄階層。 要求 http://\<伺服器位址>/StaticFiles/images/banner1.svg 時，會提供 *banner1.svg* 檔案。

下列標記參考 *MyStaticFiles/images/banner1.svg*：

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>設定 HTTP 回應標頭

[StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) 物件可以用來設定 HTTP 回應標頭。 除了設定從 Web 根目錄提供靜態檔案，下列程式碼會設定 `Cache-Control` 標頭：

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) 方法存在於 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 套件中。

檔案已在開發環境中設為可公開快取 10 分鐘 (600 秒)：

![已新增顯示「快取控制」標頭的回應標頭](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>靜態檔案授權

靜態檔案中介軟體不提供授權檢查。 其提供的所有檔案，包括在 *wwwroot* 下的檔案，皆可公開存取。 若要依據授權來提供檔案：

* 請將它們儲存在 *wwwroot* 外部和可存取靜態檔案中介軟體的任何目錄，**並**
* 透過動作方法，將它們提供給已套用授權的目標。 傳回 [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) 物件：

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>啟用目錄瀏覽

您的 Web 應用程式使用者可藉由目錄瀏覽功能，查看目錄清單及指定目錄內的檔案。 基於安全性考量，預設為停用目錄瀏覽功能 (請參閱[考量](#considerations))。 您可以叫用 `Startup.Configure` 內的 [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) 方法，以啟用目錄瀏覽功能：

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

您可以從 `Startup.ConfigureServices` 叫用 [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 方法，以新增所需的服務：

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

上述程式碼可讓您使用 URL http://\<伺服器位址>/MyImages 與每個檔案和資料夾的連結，來進行 *wwwroot/images* 資料夾的目錄瀏覽：

![目錄瀏覽](static-files/_static/dir-browse.png)

如需了解啟用瀏覽功能時的安全性風險，請參閱[考量](#considerations)。

請注意下列範例中的兩個 `UseStaticFiles` 呼叫。 第一個呼叫可提供 *wwwroot* 資料夾中的靜態檔案。 第二個呼叫可啟用使用 URL http://\<伺服器位址>/MyImages 的 *wwwroot/images* 資料夾目錄瀏覽功能：

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>提供預設文件

設定預設的歡迎頁面，可讓訪客在瀏覽您的網站時有個合乎邏輯的起點。 若要提供預設頁面 (使用者不需完整限定 URI)，請從 `Startup.Configure` 呼叫 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法：

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> 您必須在 `UseStaticFiles` 之前先呼叫 `UseDefaultFiles`，才能提供預設的檔案。 `UseDefaultFiles` 是 URL 重寫器，並非實際提供檔案。 透過 `UseStaticFiles` 啟用靜態檔案中介軟體以提供檔案。

使用 `UseDefaultFiles` 要求以搜尋資料夾：

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

系統會提供從清單中找到的第一個檔案，就像提出的要求是完整 URI 一樣。 瀏覽器 URL 仍會繼續反應要求的 URI。

下列程式碼會將預設檔案名稱變更為 *mydefault.html*：

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 結合 `UseStaticFiles`、`UseDefaultFiles` 和 `UseDirectoryBrowser` 的功能。

下列程式碼可提供靜態檔案和預設檔案。 未啟用目錄瀏覽功能。

```csharp
app.UseFileServer();
```

下列程式碼會啟用目錄瀏覽功能，以在無參數多載上進行建置：

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

請考慮下列目錄階層：

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*
  * *default.html*

下列程式碼可啟用靜態檔案、預設檔案和 `MyStaticFiles` 的目錄瀏覽功能：

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

當 `EnableDirectoryBrowsing` 屬性值是 `true` 時，必須呼叫 `AddDirectoryBrowser`：

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

使用檔案階層和上述程式碼時，URL 解析方式如下：

| URI            |                             回應  |
| ------- | ------|
| http://\<伺服器位址>/StaticFiles/images/banner1.svg    |      MyStaticFiles/images/banner1.svg |
| http://\<伺服器位址>/StaticFiles             |     MyStaticFiles/default.html |

如果 *MyStaticFiles* 目錄中不存在預設名稱的檔案， http://\<伺服器位址>/StaticFiles 會傳回含有可點按連結的目錄清單：

![靜態檔案清單](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles` 和 `UseDirectoryBrowser` 會使用 URL http://\<伺服器位址>/StaticFiles 不含最後的斜線，以觸發用戶端重新導向至 http://\<伺服器位址>/StaticFiles/。 請注意這裡最後加上的斜線。 文件中的相對 URL 若未含尾端斜線則會被視為無效。

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) 類別包含 `Mappings` 屬性以作為 MIME 內容類型的副檔名對應。 在下列範例中，已有數個副檔名註冊為已知的 MIME 類型。 已取代 *.rtf* 副檔名，並已移除 *.mp4*。

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

請參閱 [MIME 內容類型](http://www.iana.org/assignments/media-types/media-types.xhtml)。

## <a name="non-standard-content-types"></a>非標準的內容類型

靜態檔案中介軟體可理解幾乎 400 種已知的檔案內容類型。 如果使用者要求具有未知檔案類型的檔案，則靜態檔案中介軟體會將該要求傳遞至管線中的下一個中介軟體。 如果沒有中介軟體處理要求，則會傳回「404 找不到」回應。 如果啟用目錄瀏覽功能，則會在目錄清單中顯示檔案的連結。

下列程式碼可讓您提供未知的類型，並會將未知的檔案轉譯為影像：

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

使用上述程式碼時，系統會以影像來回應具有未知內容類型的檔案要求。

> [!WARNING]
> 啟用 [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) 時會造成安全性風險。 預設為停用，亦不建議您使用。 [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) 可提供更安全的替代方法，來提供非標準副檔名的檔案。

### <a name="considerations"></a>考量

> [!WARNING]
> `UseDirectoryBrowser` 和 `UseStaticFiles` 可能會導致洩漏祕密。 強烈建議您在生產環境中停用目錄瀏覽功能。 透過 `UseStaticFiles` 或 `UseDirectoryBrowser`，仔細檢閱要啟用哪些目錄。 因為整個目錄和其子目錄都可供公開存取。 將檔案存放在專門公開提供的目錄，例如 *\<content_root>/wwwroot*。 將這些檔案與 MVC 檢視、Razor 頁面 (僅限 2.x)、設定檔等區隔開來。

* 使用 `UseDirectoryBrowser` 和 `UseStaticFiles` 公開內容的 URL 可能有區分大小寫，並受限於基礎檔案系統的字元限制。 例如，Windows 不區分大小寫&mdash;macOS 和 Linux 則區分大小寫。

* 裝載於 IIS 中的 ASP.NET Core 應用程式會使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)，將所有要求轉送給應用程式 (包括靜態檔案要求)， 而不會使用 IIS 靜態檔案處理常式。 因此，上述處理常式不可能在模組處理要求之前處理要求。

* 請完成 IIS 管理員中的下列步驟，以移除伺服器或網站層級的 IIS 靜態檔案處理常式：
    1. 巡覽至 [模組] 功能。
    1. 選取清單中的 **StaticFileModule**。
    1. 按一下 [動作] 側邊欄中的 [移除]。

> [!WARNING]
> 如果已啟用 IIS 靜態檔案處理常式，**但是**未正確設定 ASP.NET Core 模組，仍可提供靜態檔案。 舉例來說，未部署 *web.config* 檔案時可能會發生上述情況。

* 請將程式碼檔案 (包括 *.cs* 和 *.cshtml*) 放在應用程式專案的 Web 根目錄之外。 如此一來，即會建立應用程式的用戶端內容與伺服器端程式碼之間的邏輯分隔。 這樣可以防止伺服器端程式碼外洩。

## <a name="additional-resources"></a>其他資源

* [中介軟體](xref:fundamentals/middleware/index)
* [ASP.NET Core 簡介](xref:index)
