---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "ASP.NET Web API 中傳送 HTML 表單資料： 檔案上傳和多部分 MIME |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>ASP.NET Web API 中傳送 HTML 表單資料： 檔案上傳和 MIME 多組件
====================
由[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>第 2 部分： 檔案上傳和多部分 MIME

本教學課程會示範如何將檔案上傳至 web API。 它也會說明如何處理多部分 MIME 資料。

> [!NOTE]
> [下載完成的專案](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d)。


HTML 表單的上傳檔案的範例如下：

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

這個表單包含文字輸入的控制項和檔案的輸入的控制項。 當表單包含檔案的輸入的控制項， **enctype**屬性應永遠為&quot;multipart/表單資料&quot;，以指定格式，會為多部分 MIME 訊息傳送。

多部分 MIME 訊息的格式是最容易了解藉由查看範例要求：

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

此訊息就會劃分為兩個*部分*，一個用於每個表單控制項。 組件界限被以行開頭為破折號。

> [!NOTE]
> 組件界限會包含隨機元件 (&quot;41184676334&quot;) 以確保界限字串不小心不顯示訊息部分內。


每個訊息部分包含一或多個標頭，後面接著組件的內容。

- 內容配置標頭包含控制項的名稱。 對於檔案，它也包含檔案名稱。
- Content-type 標頭描述組件中的資料。 如果省略此標頭，則預設為 text/plain。

在上述範例中，使用者上傳的檔案內容類型 image/jpeg; 名為 GrandCanyon.jpg，輸入文字的值是&quot;暑假&quot;。

## <a name="file-upload"></a>檔案上傳

現在讓我們看看檔案讀取多部分 MIME 訊息的 Web API 控制器。 控制器會以非同步方式讀取檔案。 Web 應用程式開發介面支援使用的非同步動作[以工作為基礎的程式設計模型](https://msdn.microsoft.com/library/dd460693.aspx)。 如果您的目標.NET Framework 4.5，支援第一次，下列程式碼**非同步**和**await**關鍵字。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

請注意，控制器動作未採用任何參數。 這是因為我們處理要求本文內的動作，而不叫用的媒體類型格式器。

**IsMultipartContent**方法會檢查要求是否包含多部分 MIME 訊息。 如果沒有，控制器會傳回 HTTP 狀態碼 415 （不支援的媒體類型）。

**MultipartFormDataStreamProvider**類別是用來配置檔案資料流上傳的檔案的協助程式物件。 若要讀取多部分 MIME 訊息，呼叫**ReadAsMultipartAsync**方法。 這個方法會擷取所有的訊息部分，並將其寫入所提供的資料流**MultipartFormDataStreamProvider**。

當方法完成時，您可以取得檔案的詳細資訊**FileData**屬性，這是集合的**MultipartFileData**物件。

- **MultipartFileData.FileName**是在伺服器上，儲存檔案的本機檔案名稱。
- **MultipartFileData.Headers**包含部分標頭 (*不*要求標頭)。 這個函數可用來存取內容\_配置和內容類型標頭。

正如其名， **ReadAsMultipartAsync**是非同步的方法。 若要執行工作，在方法完成之後，使用[接續工作](https://msdn.microsoft.com/library/ee372288.aspx)(.NET 4.0) 或**await**關鍵字 (.NET 4.5)。

以下是先前的程式碼的.NET Framework 4.0 版本：

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>讀取表單控制項的資料

先前已顯示的 HTML 表單有文字輸入的控制項。

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

您可以取得的值從控制項**FormData**屬性**MultipartFormDataStreamProvider**。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData**是**NameValueCollection**其中包含表單控制項的名稱/值組。 此集合可以包含重複的索引鍵。 請考慮這種形式：

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

要求主體可能看起來像這樣：

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

在此情況下， **FormData**集合會包含下列索引鍵/值組：

- 路線： 反覆存取
- 選項： nonstop
- 選項： 日期
- 基座： 視窗
