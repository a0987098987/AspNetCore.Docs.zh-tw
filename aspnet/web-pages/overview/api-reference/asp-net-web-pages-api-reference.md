---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.NET Web Pages (Razor) API 的快速參考 |Microsoft 文件
author: tfitzmac
description: 此頁面包含的最常用的物件、 屬性和方法，程式設計含有 Razor 語法的 ASP.NET Web Pages 簡短範例的清單。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 5f9d84f4d453583d7d4eae12e4fc510275255616
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897580"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.NET Web Pages (Razor) API 的快速參考
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 此頁面包含的最常用的物件、 屬性和方法，程式設計含有 Razor 語法的 ASP.NET Web Pages 簡短範例的清單。
> 
> ASP.NET Web Pages 中第 2 版導入 」 (v2) 」 以標記的描述。
> 
> API 參考文件，請參閱[ASP.NET Web Pages 參考文件](https://go.microsoft.com/fwlink/?LinkId=208659)MSDN 上。
> 
> ## <a name="software-versions"></a>軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2 和 ASP.NET Web Pages 1.0 （除了標示 v2 的功能）。


此頁面包含下列參考資訊：

- [類別](#Classes)
- [Data](#Data)
- [協助程式](#Helpers)
- [驗證](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>類別

### `AppState[key], AppState[index],App`

包含可在應用程式中任何頁面所共用的資料。 您可以使用動態`App`屬性來存取相同的資料，如下列範例所示：

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

將字串值轉換成布林值 (true/false)。 傳回 false 或指定的值，如果字串不是 true/false。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

將轉換成日期/時間的字串值。 傳回`DateTime.MinValue`或指定的值，如果字串不是日期/時間。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

將字串值轉換成十進位值。 傳回 0.0 或指定的值，如果字串不是十進位值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

將字串值轉換成浮點數。 傳回 0.0 或指定的值，如果字串不是十進位值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

將字串值轉換為整數。 傳回 0 或指定的值，如果字串不是整數。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

從本機檔案路徑，選擇性的其他路徑的組件中建立相容的瀏覽器 URL。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

呈現*值*為 HTML 標記，而不是轉譯為 HTML 編碼的輸出。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

如果此值可以從字串轉換成指定的類型，則傳回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

如果物件或變數沒有任何值，則傳回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

如果要求為 POST，則傳回 true。 （初始要求通常是 GET）。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

指定要套用至這個頁面版面配置頁的路徑。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

包含目前要求中的頁面、 版面配置頁及部分頁面之間共用資料。 您可以使用動態`Page`屬性來存取相同的資料，如下列範例所示：

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

（版面配置頁）呈現不是任何具名區段的內容頁面的內容。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

呈現內容頁面，使用指定的路徑和選用的額外資料。 您可以取得的額外參數的值`PageData`依位置 （例如，1） 或索引鍵 （例如，2）。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

（版面配置頁）呈現內容的區段名稱。 設定*必要*為 false，讓選擇性區段。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

取得或設定 HTTP cookie 的值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

取得目前要求中上傳的檔案。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

取得表單中回傳 （做為字串） 的資料。 `Request[key]` 檢查同時`Request.Form`和`Request.QueryString`集合。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

取得指定之 URL 查詢字串中的資料。 `Request[key]` 檢查同時`Request.Form`和`Request.QueryString`集合。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

選擇性地停用要求驗證表單項目、 查詢字串值、 cookie 或標頭值。 要求驗證預設會啟用，並防止使用者張貼標記或其他潛在危險的內容。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

將 HTTP 伺服器標頭加入回應。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

會對網頁輸出快取指定的時間。 選擇性地設定*滑動*重設存取每個頁面上的逾時和*varyByParams*快取不同版本之頁面的頁面要求中每個不同的查詢字串。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

將瀏覽器要求重新導向至新位置。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

設定傳送到瀏覽器的 HTTP 狀態碼。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

內容寫入*資料*至含有選擇性 MIME 類型的回應。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

將檔案的內容寫入至回應。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

（版面配置頁）定義內容區段名稱。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

將解碼的 HTML 編碼字串。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

編碼字串中的 HTML 標記的呈現。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

傳回指定的虛擬路徑的伺服器實體路徑。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

將解碼的 URL 中的文字。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

將編碼將放在 URL 中的文字。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

取得或設定值，這個值存在於在使用者關閉瀏覽器。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

顯示物件的值的字串表示。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

從 URL 取得額外的資料 (例如， */MyPage/ExtraData*)。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

變更指定之使用者的密碼。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

確認使用的帳戶確認語彙基元的帳戶。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

使用指定的使用者名稱和密碼建立新的使用者帳戶。 如果需要確認語彙基元，請傳遞 true *requireConfirmationToken。*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

取得目前登入之使用者的整數識別碼。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

取得目前登入的使用者名稱。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

產生密碼重設語彙基元可以傳送電子郵件給使用者，讓使用者可以重設密碼。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

傳回使用者名稱中的使用者識別碼。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

如果目前的使用者登入，則傳回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

如果已確認使用者 （例如，透過確認電子郵件），則傳回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

如果目前的使用者名稱符合指定的使用者名稱，則傳回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

記錄中的使用者藉由在 cookie 中設定驗證語彙基元。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

藉由移除驗證語彙基元 cookie 逾時記錄的使用者。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

如果使用者未經過驗證，請將 HTTP 狀態設定為 401 （未經授權）。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

如果目前的使用者不是其中一個指定的角色的成員，請將 HTTP 狀態設定為 401 （未經授權）。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

如果目前的使用者不是所指定的使用者*username*，將 HTTP 狀態設定為 401 （未經授權）。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

如果密碼重設語彙基元有效，請為新的密碼變更使用者的密碼。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>資料

### `Database.Execute(SQLstatement [,parameters]`

執行*SQLstatement* （內含選擇性參數） 例如 INSERT、 DELETE 或 UPDATE，並傳回受影響記錄的計數。

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

傳回最近插入的資料列識別資料行。

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

開啟指定的資料庫檔案或資料庫使用的具名的連接字串指定*Web.config*檔案。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

開啟資料庫使用的連接字串。 (這與相反`Database.Open`，它會使用連接字串名稱。)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

查詢資料庫使用*SQLstatement* （可選擇傳遞參數），並傳回結果做為集合。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

執行*SQLstatement* （內含選擇性參數），並傳回單一記錄。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

執行*SQLstatement* （內含選擇性參數），並傳回單一值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>協助程式

### `Analytics.GetGoogleHtml(webPropertyId)`

呈現 Google Analytics JavaScript 程式碼針對指定的識別碼。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

呈現指定的專案 StatCounter 分析 JavaScript 程式碼。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

呈現指定的帳戶的 Yahoo 分析 JavaScript 程式碼。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

傳遞到 Bing 的搜尋。 若要指定搜尋的搜尋方塊的標題的站台，您可以設定`Bing.SiteUrl`和`Bing.SiteTitle`屬性。 在設定這些屬性通常 *\_AppStart*頁面。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

初始化圖表。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

將圖例加入至圖表。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

將一系列的值加入至圖表。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

傳回指定之資料的雜湊。 預設的演算法是`sha256`。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

可讓連線到網頁的 Facebook 使用者。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

上傳的檔案中呈現 UI。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

呈現指定的 Xbox 玩家的標記。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

呈現指定的電子郵件地址的 Gravatar 映像。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

將資料物件轉換成 JavaScript Object Notation (JSON) 格式的字串。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

將 JSON 編碼的輸入的字串轉換成您可以反覆查看，或資料庫中插入的資料物件。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

呈現社交網路的連結，使用指定的標題和選擇性的 URL。

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

將錯誤訊息與表單欄位相關聯。 使用`ModelState`helper 來存取這個成員。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

關聯表單中的錯誤訊息。 使用`ModelState`helper 來存取這個成員。

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

如果沒有任何驗證錯誤，則傳回 true。 使用`ModelState`helper 來存取這個成員。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

呈現的屬性和值的物件和任何子物件。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

呈現 reCAPTCHA 驗證測試。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

設定公用和私用的索引鍵 reCAPTCHA 服務。 在設定這些屬性通常 *\_AppStart*頁面。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

傳回 reCAPTCHA 測試的結果。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

呈現有關 ASP.NET Web 網頁的狀態資訊。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

呈現 Twitter 資料流以供指定的使用者。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

呈現指定的搜尋文字的 Twitter 資料流。

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

會轉譯為選擇性的寬度和高度指定的檔案快閃影片播放器。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

呈現 Windows Media player 為選擇性的寬度和高度指定的檔案。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

呈現指定 Silverlight 播放程式 *.xap*檔案所需的寬度和高度。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

傳回所指定的物件*金鑰*，或如果找不到物件則為 null。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

移除所指定的物件*金鑰*從快取。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

將*值*到所指定的名稱下快取*金鑰*。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

建立新`WebGrid`物件使用中查詢的資料。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

呈現標記，以 HTML 表格中顯示資料。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

呈現頁面巡覽區`WebGrid`物件。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

從指定的路徑載入影像。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

將指定的映像加入做為浮水印。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

將指定的文字加入至映像。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

水平或垂直翻轉影像。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

映像檔案上傳期間張貼到網頁時，請載入影像。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

調整大小的影像。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

旋轉影像到左邊或右邊。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

將影像儲存至指定的路徑。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

SMTP 伺服器設定的密碼。 在設定此屬性通常 *\_AppStart*頁面。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

傳送電子郵件訊息。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

設定 SMTP 伺服器名稱。 在設定此屬性通常<em>\_AppStart</em>頁面。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

設定 SMTP 伺服器的使用者名稱。 您通常應該設定這個屬性 *\_AppStart*頁面。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>驗證

### `Html.ValidationMessage(field)`

(v2)呈現指定欄位的驗證錯誤訊息。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2)顯示所有驗證錯誤的清單。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2)註冊指定類型的驗證使用者輸入項目。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2)動態呈現 CSS 類別屬性，用戶端驗證，如此您可以格式化驗證錯誤訊息。 （需要您參考適當的用戶端指令碼程式庫和您定義 CSS 類別）。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2)可讓用戶端驗證的使用者輸入欄位。 （需要您參考適當的用戶端指令碼程式庫）。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2)如果使用者輸入的所有項目進行驗證的 registred 包含有效的值，則傳回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2)指定使用者必須提供使用者輸入項目的值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2)指定使用者必須提供每個使用者輸入項目的值。 這個方法不會讓您指定的自訂錯誤訊息。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2)指定當您使用的驗證測試`Validation.Add`方法。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
