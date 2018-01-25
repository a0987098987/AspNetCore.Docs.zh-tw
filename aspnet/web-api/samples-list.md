---
uid: web-api/samples-list
title: "Web API 範例清單 |Microsoft 文件"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 1e1f43bbeedfc052f0b3a3924f51b544a5a79dca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="web-api-samples-list"></a>Web API 範例清單
====================
## <a name="httpclient-samples"></a>HttpClient 範例

**Bing 翻譯範例** | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

示範如何呼叫[Microsoft Translator 服務](https://msdn.microsoft.com/library/ff512419.aspx)使用**HttpClient**類別。 Microsoft Translator 服務 API 需要應用程式將要求傳送至轉譯器服務每個要求的語彙基元 Azure 伺服器取得 OAuth 語彙基元。 傳送至轉譯服務的要求送入語彙基元 server 的結果。 執行此範例之前，您必須取得[Azure marketplace 的應用程式金鑰](https://msdn.microsoft.com/library/hh454950.aspx)並填入 AccessTokenMessageHandler 範例類別中的資訊。

**Google 地圖範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

使用**HttpClient**下載的 Redmond，WA 從地圖[Google Maps API](https://developers.google.com/maps/)、 將它儲存為本機檔案，並會開啟預設影像檢視器。

**Twitter 用戶端範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

示範如何撰寫簡單的 Twitter 用戶端使用**HttpClient**。 此範例會使用**HttpMessageHandler** OAuth 驗證資訊插入傳出**HttpRequestMessage**。 使用 JSON.NET 讀取 Twitter 的結果。 執行此範例之前，您必須取得[Twitter 應用程式金鑰](https://dev.twitter.com/)，並填入 OAuthMessageHandler 範例類別中的資訊。

**世界銀行範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

示範如何從世界銀行資料網站上，使用 JSON.NET 剖析結果擷取資料。

## <a name="web-api-samples"></a>Web API 範例

**開始使用 ASP.NET Web API** | [VS 2012 來源](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

示範如何建立基本的 web 應用程式開發介面支援 HTTP GET 要求。 教學課程包含原始碼[您的第一個 ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。

**ASP.NET Web API JavaScript 案例 – 註解** | [VS 2012 來源](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

示範如何使用 ASP.NET Web 應用程式開發介面建置的 web Api 的支援瀏覽器用戶端，而且可以輕鬆地呼叫使用 jQuery。

**請連絡管理員** | [VS 2010 來源](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

這個範例會使用 ASP.NET Web API 來建置簡單的連絡人管理員應用程式。 此應用程式包含連絡人的管理員 web 應用程式開發介面，可由 ASP.NET MVC 應用程式和 Windows Phone 應用程式來顯示和管理的連絡人清單。

**批次處理範例** | [詳細描述](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

示範如何實作 asp.net 批次處理 HTTP。 批次放在單一 MIME 多組件的實體內容，然後會傳送至伺服器的 HTTP POST 的多個 HTTP 要求所組成。 處理個別要求和回應會放入另一個的 MIME 多組件的實體內容，傳回至用戶端。

**內容控制器範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

示範如何讀取和寫入要求和回應實體以非同步方式使用資料流。 範例控制器有兩個動作： PUT 動作的非同步讀取要求實體主體，並將它儲存在本機檔案，並取得動作傳回的本機檔案內容。

**自訂組件解析程式範例** | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

示範如何修改 ASP.NET Web API 來支援從以動態方式載入的程式庫的組件的控制站探索。 此範例會實作自訂**IAssembliesResolver**的預設實作會呼叫並將程式庫組件，加入預設結果。

**自訂的媒體類型格式器範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

示範如何建立使用自訂的媒體類型格式器**BufferedMediaTypeFormatter**基底類別。 這個基底類別適用於主要是使用同步讀取和寫入作業格式器。 除了顯示的媒體類型格式器，此範例顯示如何連接所註冊的一部分**HttpConfiguration**應用程式。 請注意，它也可以使用**MediaTypeFormatter**的基底類別直接管理，主要是使用非同步格式器會讀取和寫入作業。

**自訂參數繫結範例** | [詳細描述](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

示範如何自訂參數繫結程序，也就是決定如何從要求的資訊繫結至動作參數的程序。 在此範例中，主控制器會有四個動作：

1. BindPrincipal 示範如何從自訂的一般主體，不會從 HTTP GET 訊息; 繫結 IPrincipal 參數
2. BindCustomComplexTypeFromUriOrBody 示範如何將繫結的複雜型別參數，而可能來自於從訊息內文或在要求 URI 的 HTTP POST 訊息;
3. BindCustomComplexTypeFromUriWithRenamedProperty 示範如何結合來自要求 URI 的 HTTP POST 訊息; 已重新命名屬性的複雜型別參數
4. PostMultipleParametersFromBody 示範如何將多個參數繫結從主體張貼訊息。

**檔案上傳範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

示範如何將檔案上載到**ApiController**使用 MIME 多組件檔案上傳，及如何設定與進度通知**HttpClient**使用**ProgressNotificationHandler**. 控制器的 HTML 檔案上傳內容以非同步方式讀取和寫入本機檔案中的一個或多個本文部分。 回應會包含已上傳檔案 （或檔案） 的相關資訊。

**檔案上的傳至 Azure Blob 存放區範例** | [詳細描述](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

這個範例類似於檔案上傳範例的一部分，但而不是儲存在本機磁碟上傳的檔案，它以非同步方式將檔案上傳至[Azure Blob 存放區](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)使用[Windows Azure SDK for.NET](https://www.windowsazure.com/develop/net/)。 它也提供一種機制，列出目前存在於 blob [Azure Blob 儲存體容器](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。 您可以嘗試執行此範例**Azure 儲存體模擬器**所附的 Azure SDK。 如果您有[Azure 儲存體帳戶](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)，您可以執行的實際儲存體服務。

**Http 訊息處理常式管線範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

示範如何連接**HttpMessageHandler**這兩個用戶端上的執行個體 (**HttpClient**) 和伺服器 (ASP.NET Web API)。 在此範例中，相同的處理常式會使用用戶端和伺服器上。 雖然很少完全相同的處理常式會執行兩個地方，物件模型是在用戶端和伺服器端相同的。

**JSON 上傳範例** | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

示範如何上傳和下載 JSON 的**ApiController**。 此範例會使用最少**ApiController** ，並使用它存取**HttpClient**。

**Mashup 範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

示範如何存取多個遠端站台以非同步方式從**ApiController**動作。 每次叫用動作時，要求會以非同步方式執行，如此會封鎖任何執行緒。

**記憶體追蹤範例** | [詳細描述](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

這個範例專案會建立可將 ASP.NET Web API 應用程式安裝自訂的記憶體中追蹤寫入器的 Nuget 封裝。

**MongoDB 範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

示範如何使用 MongoDB 做為持續性存放區**ApiController**，使用儲存機制模式。

**回應主體的處理器範例** | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

示範如何將回應實體 （也就是 HTTP 回應主體） 複製到本機檔案，再傳輸至用戶端，並執行其他在檔案上以非同步方式處理。 此範例會實作**HttpMessageHandler**包裝回應實體的其中一個，兩者還是會撰寫本身為一般輸出和本機檔案。

**上傳 XDocument 範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

示範如何上傳至 XDocument **ApiController**使用**PushStreamContent**和**HttpClient**。

**驗證範例** | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

顯示如何使用驗證屬性在模型中 ASP.NET WebAPI 驗證 HTTP 要求的內容。 示範如何將標示為必要項、 如何使用這兩架構定義和自訂驗證屬性對您的模型，加上註解的內容，以及如何傳回無效的模型狀態的錯誤回應。

**Web 表單範例** | [詳細描述](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

顯示加入至 Web Form 專案 ApiController。

**[RestBugs 範例](https://github.com/howarddierking/RestBugs)**

RestBugs 是簡單的錯誤，追蹤應用程式，示範如何使用 ASP.NET Web API 和新的 HTTP 用戶端程式庫建立超導向的系統。 此範例包含用戶端和伺服器實作中，使用 ASP.NET Web API。 伺服器會使用自訂的 Razor 格式器產生資源的表示法。 此範例也會提供 node.js 伺服器來說明來自分離用戶端和伺服器使用超媒體設計的優點。

## <a name="web-api-extensions-preview-samples"></a>Web API 擴充功能預覽範例

**OData 查詢範例** | [詳細描述](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

示範如何在導入中使用的 ASP.NET Web API OData 查詢`[Queryable]`屬性，或使用**ODataQueryOptions** action 參數可讓以手動方式檢查查詢之前正在執行的動作。

CustomerController 會示範如何使用 [Queryable] 屬性，並 OrderController 示範如何使用 ODataQueryOptions 參數。 ResponseController 是類似 CustomerController 但而不是傳回 GET 動作`IEnumerable<Customer>`它會傳回**HttpResponseMessage**。 這可讓我們加入額外的標頭欄位，操作狀態碼等時仍在使用查詢功能。 此範例將說明如何使用 $orderby、 $skip、 $top、 any （）、 all （） 和 $filter 的查詢。

**OData 服務範例** | [詳細描述](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 來源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

此範例說明如何建立包含三個實體和三個 Web API 控制器的 OData 服務。 控制站都提供各種層級會公開的 OData 功能方面的功能：

SupplierController 公開功能，包括查詢子集，以取得索引鍵建立，藉由處理這些要求：

- 取得 /Suppliers
- 取得 /Suppliers(key)
- GET /Suppliers？ $filter =...&amp;$orderby =...&amp;$top =...&amp;$skip =...
- POST /Suppliers

ProductsController 公開 GET、 PUT、 POST、 刪除和修補程式藉由直接實作的每項作業的動作。

ProductFamilesController 會利用 EntitySetController 基底類別會公開實作豐富的 OData 服務的有效模式之一。

此外 OData 服務會公開 $metadata 文件可讓取用資料的 WCF 資料服務用戶端和其他用戶端接受 $metadata 格式。
