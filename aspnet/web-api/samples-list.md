---
uid: web-api/samples-list
title: Web API 範例清單 |Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 09/18/2012
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: d25e0890a1b8d42cc638117f7bef9cf6457f3d75
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824906"
---
<a name="web-api-samples-list"></a>Web API 範例清單
====================
## <a name="httpclient-samples"></a>HttpClient 範例

**Bing 翻譯範例** | [VS 2012 的來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/BingTranslateSample)

示範如何呼叫[Microsoft Translator 服務](https://msdn.microsoft.com/library/ff512419.aspx)使用**HttpClient**類別。 Microsoft Translator 服務 API 需要的 OAuth 權杖，它會將要求傳送至 Azure 的權杖伺服器，以每 translator 服務要求取得的應用程式。 要求傳送至轉譯服務提供給權杖伺服器的結果。 執行此範例之前，您必須取得[從 Azure Marketplace 的應用程式金鑰](https://msdn.microsoft.com/library/hh454950.aspx)並填寫各 AccessTokenMessageHandler 範例類別中的資訊。

**Google 地圖範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012 的來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/GoogleMapsSample)

會使用**HttpClient**若要下載的 Redmond，WA 從地圖[Google Maps API](https://developers.google.com/maps/)、 將它儲存為本機檔案，並會開啟預設映像檢視器。

**Twitter 用戶端範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012 的來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/TwitterSample)

示範如何撰寫使用簡單的 Twitter 用戶端**HttpClient**。 此範例會使用**HttpMessageHandler**將 OAuth 驗證資訊插入傳出**HttpRequestMessage**。 使用 JSON.NET 讀取 Twitter 的結果。 執行此範例之前，您必須取得[應用程式金鑰，從 Twitter](https://dev.twitter.com/)，並填寫各 OAuthMessageHandler 範例類別中的資訊。

**World Bank 範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net40) | [VS 2012 的來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net45)

示範如何從 World Bank 資料網站上，使用 JSON.NET 剖析結果擷取資料。

## <a name="web-api-samples"></a>Web API 範例

**開始使用 ASP.NET Web API** | [VS 2012 的來源](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

示範如何建立基本的 web API 支援 HTTP GET 要求。 本教學課程中包含的原始程式碼[您的第一個 ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。

**ASP.NET Web API JavaScript 案例-註解** | [VS 2012 的來源](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

示範如何使用 ASP.NET Web API 來建置 web Api 的支援瀏覽器用戶端，而且可以輕鬆地呼叫使用 jQuery。

**請連絡管理員** | [VS 2010 來源](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

此範例會使用 ASP.NET Web API 來建置簡單的連絡人管理員應用程式。 在應用程式包含可由 ASP.NET MVC 應用程式和 Windows Phone 應用程式來顯示與管理連絡人清單中的連絡人管理員 web API。

**批次範例** | [詳細描述](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012 的來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/BatchSample)

示範如何實作 ASP.NET 內的 HTTP 批次處理。 批次包含將放在單一 MIME 多組件的實體主體，然後傳送至伺服器的 HTTP POST 的多個 HTTP 要求。 個別處理要求和回應會放入另一個的 MIME 多組件的實體內容，會傳回給用戶端。

**內容控制器範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net40) | [VS 2012 的來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net45)

示範如何讀取和寫入要求和回應的實體，以非同步方式使用資料流。 範例控制器有兩個動作： PUT 動作，以非同步方式讀取要求實體內容，並將它儲存在本機檔案，並傳回本機檔案的內容取得動作。

**自訂組件解析程式範例** | [VS 2012 的來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomAssemblyResolverSample)

示範如何修改 ASP.NET Web API，以支援來自動態載入的程式庫組件的控制站的探索。 此範例會實作自訂**IAssembliesResolver**會呼叫預設的實作，並再將程式庫組件加入至預設結果。

**自訂媒體類型格式器範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [VS 2010 來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomMediaTypeFormatterSample)

示範如何建立使用自訂的媒體類型格式器**BufferedMediaTypeFormatter**基底類別。 這個基底類別適用於這主要是使用同步讀取和寫入作業格式器。 除了顯示的媒體類型格式器，此範例說明如何連結來註冊它的一部分**HttpConfiguration**應用程式。 請注意，您也可以使用**MediaTypeFormatter**的基底類別直接的格式器主要是使用非同步讀取和寫入作業。

**自訂參數繫結範例** | [詳細描述](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [VS 2010 來源](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomParameterBinding)

示範如何自訂參數繫結程序，也就是決定如何從要求的資訊繫結至動作參數的程序。 在此範例中，主控制器會有四個動作：

1. BindPrincipal 示範如何從自訂的一般主體，不會從 HTTP GET 訊息; 繫結 IPrincipal 參數
2. BindCustomComplexTypeFromUriOrBody 示範如何繫結至複雜型別參數，而可能來自於從訊息主體，或從要求 URI 的 HTTP POST 訊息;
3. BindCustomComplexTypeFromUriWithRenamedProperty 示範如何繫結的複雜型別參數，與來自要求 URI 的 HTTP POST 訊息; 已重新命名屬性
4. PostMultipleParametersFromBody 示範如何將多個參數繫結從主體的 POST 訊息;

**檔案上傳範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012 的來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/FileUploadSample)

示範如何將檔案上載**ApiController**使用 MIME 多組件檔案上傳，以及如何設定使用的進度通知**HttpClient**使用**ProgressNotificationHandler**. 控制站以非同步方式讀取 HTML 檔案上傳的內容，並寫入本機檔案中的一或多個主體組件。 回應會包含已上傳檔案 （或檔案） 的相關資訊。

**檔案上的傳至 Azure Blob 存放區範例** | [詳細描述](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012 的來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/AzureBlobsFileUploadSample)

此範例類似於檔案上傳範例中，但而不是儲存在本機的磁碟上傳的檔案，它以非同步方式將檔案上傳至[Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)使用[Windows Azure SDK for.NET](https://www.windowsazure.com/develop/net/)。 它也提供一種機制，列出目前存在於 blob [Azure Blob 儲存體容器](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。 您可以嘗試執行此範例**Azure 儲存體模擬器**隨附於 Azure SDK。 如果您有[Azure 儲存體帳戶](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)，您可以執行的實際的儲存體服務。

**Http 訊息處理常式管線範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [VS 2010 來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/HttpMessageHandlerPipelineSample)

示範如何接通**HttpMessageHandler**這兩個用戶端上的執行個體 (**HttpClient**) 和伺服器 (ASP.NET Web API)。 在此範例中，相同的處理常式會使用用戶端和伺服器上。 雖然很少完全相同的處理常式會執行兩個位置中，物件模型是在用戶端和伺服器端上相同。

**JSON 上傳範例** | [VS 2012 的來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/JsonUploadSample)

示範如何上傳和下載 JSON 來回**ApiController**。 此範例會使用最小**ApiController** ，並使用它存取**HttpClient**。

**混搭範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012 的來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MashupSample)

示範如何從以非同步方式存取多個遠端站台**ApiController**動作。 叫用動作時，每次執行的要求以非同步的方式，如此會封鎖任何執行緒。

**記憶體追蹤範例** | [詳細描述](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [VS 2010 來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MemoryTracingSample)

這個範例專案會建立 Nuget 套件會安裝到 ASP.NET Web API 應用程式的自訂記憶體中的追蹤寫入器。

**MongoDB 範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012 的來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MongoSample)

示範如何使用 MongoDB 做為持續性存放區**ApiController**，使用儲存機制模式。

**回應主體的處理器範例** | [VS 2012 的來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ResponseEntityProcessorSample)

示範如何將回應實體 （也就是 HTTP 回應主體） 複製到本機檔案，再傳輸到用戶端，並執行其他處理序對該檔案以非同步的方式。 此範例會實作**HttpMessageHandler**包裝回應實體與其中一個，同時將本身移至標準輸出和本機檔案。

**上傳範例 XDocument** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012 的來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/UploadXDocumentSample)

示範如何上傳至 XDocument **ApiController**使用**PushStreamContent**並**HttpClient**。

**驗證範例** | [VS 2010 來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ValidationSample)

示範如何使用驗證屬性在 ASP.NET WebAPI 中的模型來驗證 HTTP 要求的內容。 示範如何將標示為必要項、 如何使用這兩個架構定義並加上註解您的模型、 自訂驗證屬性的屬性，以及如何傳回的無效模型狀態錯誤回應。

**Web 表單範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [VS 2010 來源](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/WebFormSample)

顯示 ApiController 新增至 Web Form 專案。

**[RestBugs 範例](https://github.com/howarddierking/RestBugs)**

RestBugs 是簡單的 bug，追蹤應用程式，示範如何使用 ASP.NET Web API 和新的 HTTP 用戶端程式庫來建立一個超媒體驅動系統。 此範例包含用戶端和伺服器實作中，使用 ASP.NET Web API。 伺服器會使用自訂的 Razor 格式器，產生資源的表示法。 此範例也會提供 node.js 伺服器來說明來自減少用戶端和伺服器使用超媒體設計的優點。
