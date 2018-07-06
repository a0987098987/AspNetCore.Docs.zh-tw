---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: ASP.NET Web API 中傳送 HTML 表單資料： 檔案上傳和多部分 MIME |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: adaa59b47cb16def5b423181ca06729d43cd77de
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804341"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>ASP.NET Web API 中傳送 HTML 表單資料： 檔案上傳和多部分 MIME
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>第 2 部分： 檔案上傳和多個 MIME

本教學課程會示範如何將檔案上傳至 web API。 它也會說明如何處理多部分 MIME 資料。

> [!NOTE]
> [下載已完成的專案](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d)。


將檔案上傳之 HTML 表單的範例如下：

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

這個表單包含文字輸入的控制項和檔案的輸入的控制項。 當表單包含檔案的輸入的控制項， **enctype**屬性應永遠為&quot;multipart/form-data&quot;，指定將會傳送為多部分 MIME 訊息的格式。

多部分 MIME 訊息的格式是最容易了解藉由查看要求範例：

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

此訊息會分割成兩個*組件*，一個用於每個表單控制項。 組件界限會以虛線開始的行。

> [!NOTE]
> 組件界限會包含隨機的元件 (&quot;41184676334&quot;) 以確保界限字串不小心不顯示訊息部分內。


每個訊息組件包含一或多個標頭，後面接著組件的內容。

- 內容配置標頭包含控制項的名稱。 對於檔案，它也會包含檔案名稱。
- Content-type 標頭描述組件中的資料。 如果省略此標頭，則預設值為 text/plain。

在上述範例中，使用者上傳檔案，名為 GrandCanyon.jpg，內容類型 image/jpeg;文字輸入的值是&quot;暑假&quot;。

## <a name="file-upload"></a>檔案上傳

現在讓我們看看檔案讀取多部分 MIME 訊息的 Web API 控制器。 控制器會以非同步方式讀取檔案。 Web API 支援使用非同步動作[以工作為基礎的程式設計模型](https://msdn.microsoft.com/library/dd460693.aspx)。 如果您的目標.NET Framework 4.5 中，可支援第一，以下是程式碼**非同步**並**await**關鍵字。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

請注意，控制器動作未採用任何參數。 這是因為我們處理要求本文內的動作，而不叫用的媒體類型格式器。

**IsMultipartContent**方法會檢查要求是否包含多部分 MIME 訊息。 如果沒有，則控制器會傳回 HTTP 狀態碼 415 （不支援的媒體類型）。

**MultipartFormDataStreamProvider**類別是一個 helper 物件，配置檔案資料流已上傳檔案。 若要讀取多部分 MIME 訊息，呼叫**ReadAsMultipartAsync**方法。 這個方法會擷取所有的訊息組件，並將它們寫入至所提供的資料流**MultipartFormDataStreamProvider**。

方法完成時，您可以取得檔案的詳細資訊**FileData**屬性，這是集合的**MultipartFileData**物件。

- **MultipartFileData.FileName**是在伺服器上，檔案儲存位置的本機檔案名稱。
- **MultipartFileData.Headers**包含組件的標頭 (*不*要求標頭)。 您可以使用此選項來存取內容\_配置和內容類型標頭。

恰如其名**ReadAsMultipartAsync**是非同步方法。 若要執行的工作，在方法完成之後，使用[接續工作](https://msdn.microsoft.com/library/ee372288.aspx)(.NET 4.0) 或**await**關鍵字 (.NET 4.5)。

以下是先前的程式碼的.NET Framework 4.0 版本：

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>讀取表單控制項的資料

我先前所示範的 HTML 表單有文字輸入的控制項。

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

您可以取得從控制項的值**FormData**屬性**MultipartFormDataStreamProvider**。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData**已**NameValueCollection**包含表單控制項的名稱/值組。 集合可以包含重複的索引鍵。 請考慮這種形式：

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

要求主體可能如下所示：

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

在此情況下， **FormData**集合會包含下列索引鍵/值組：

- 車程： 反覆存取
- 選項： nonstop
- 選項： 日期
- 基座： 視窗
