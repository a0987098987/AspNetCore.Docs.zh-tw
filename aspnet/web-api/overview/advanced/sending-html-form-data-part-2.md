---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: ASP.NET Web API 中傳送 HTML 表單資料： 檔案上傳和多部分 MIME |Microsoft 文件
author: MikeWasson
description: ''
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
ms.locfileid: "28040138"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="3b61c-102">ASP.NET Web API 中傳送 HTML 表單資料： 檔案上傳和 MIME 多組件</span><span class="sxs-lookup"><span data-stu-id="3b61c-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="3b61c-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3b61c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="3b61c-104">第 2 部分： 檔案上傳和多部分 MIME</span><span class="sxs-lookup"><span data-stu-id="3b61c-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="3b61c-105">本教學課程會示範如何將檔案上傳至 web API。</span><span class="sxs-lookup"><span data-stu-id="3b61c-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="3b61c-106">它也會說明如何處理多部分 MIME 資料。</span><span class="sxs-lookup"><span data-stu-id="3b61c-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="3b61c-107">[下載完成的專案](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d)。</span><span class="sxs-lookup"><span data-stu-id="3b61c-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="3b61c-108">HTML 表單的上傳檔案的範例如下：</span><span class="sxs-lookup"><span data-stu-id="3b61c-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="3b61c-109">這個表單包含文字輸入的控制項和檔案的輸入的控制項。</span><span class="sxs-lookup"><span data-stu-id="3b61c-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="3b61c-110">當表單包含檔案的輸入的控制項， **enctype**屬性應永遠為&quot;multipart/表單資料&quot;，以指定格式，會為多部分 MIME 訊息傳送。</span><span class="sxs-lookup"><span data-stu-id="3b61c-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="3b61c-111">多部分 MIME 訊息的格式是最容易了解藉由查看範例要求：</span><span class="sxs-lookup"><span data-stu-id="3b61c-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="3b61c-112">此訊息就會劃分為兩個*部分*，一個用於每個表單控制項。</span><span class="sxs-lookup"><span data-stu-id="3b61c-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="3b61c-113">組件界限被以行開頭為破折號。</span><span class="sxs-lookup"><span data-stu-id="3b61c-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="3b61c-114">組件界限會包含隨機元件 (&quot;41184676334&quot;) 以確保界限字串不小心不顯示訊息部分內。</span><span class="sxs-lookup"><span data-stu-id="3b61c-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="3b61c-115">每個訊息部分包含一或多個標頭，後面接著組件的內容。</span><span class="sxs-lookup"><span data-stu-id="3b61c-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="3b61c-116">內容配置標頭包含控制項的名稱。</span><span class="sxs-lookup"><span data-stu-id="3b61c-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="3b61c-117">對於檔案，它也包含檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="3b61c-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="3b61c-118">Content-type 標頭描述組件中的資料。</span><span class="sxs-lookup"><span data-stu-id="3b61c-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="3b61c-119">如果省略此標頭，則預設為 text/plain。</span><span class="sxs-lookup"><span data-stu-id="3b61c-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="3b61c-120">在上述範例中，使用者上傳的檔案內容類型 image/jpeg; 名為 GrandCanyon.jpg，輸入文字的值是&quot;暑假&quot;。</span><span class="sxs-lookup"><span data-stu-id="3b61c-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="3b61c-121">檔案上傳</span><span class="sxs-lookup"><span data-stu-id="3b61c-121">File Upload</span></span>

<span data-ttu-id="3b61c-122">現在讓我們看看檔案讀取多部分 MIME 訊息的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="3b61c-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="3b61c-123">控制器會以非同步方式讀取檔案。</span><span class="sxs-lookup"><span data-stu-id="3b61c-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="3b61c-124">Web 應用程式開發介面支援使用的非同步動作[以工作為基礎的程式設計模型](https://msdn.microsoft.com/library/dd460693.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3b61c-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="3b61c-125">如果您的目標.NET Framework 4.5，支援第一次，下列程式碼**非同步**和**await**關鍵字。</span><span class="sxs-lookup"><span data-stu-id="3b61c-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="3b61c-126">請注意，控制器動作未採用任何參數。</span><span class="sxs-lookup"><span data-stu-id="3b61c-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="3b61c-127">這是因為我們處理要求本文內的動作，而不叫用的媒體類型格式器。</span><span class="sxs-lookup"><span data-stu-id="3b61c-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="3b61c-128">**IsMultipartContent**方法會檢查要求是否包含多部分 MIME 訊息。</span><span class="sxs-lookup"><span data-stu-id="3b61c-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="3b61c-129">如果沒有，控制器會傳回 HTTP 狀態碼 415 （不支援的媒體類型）。</span><span class="sxs-lookup"><span data-stu-id="3b61c-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="3b61c-130">**MultipartFormDataStreamProvider**類別是用來配置檔案資料流上傳的檔案的協助程式物件。</span><span class="sxs-lookup"><span data-stu-id="3b61c-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="3b61c-131">若要讀取多部分 MIME 訊息，呼叫**ReadAsMultipartAsync**方法。</span><span class="sxs-lookup"><span data-stu-id="3b61c-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="3b61c-132">這個方法會擷取所有的訊息部分，並將其寫入所提供的資料流**MultipartFormDataStreamProvider**。</span><span class="sxs-lookup"><span data-stu-id="3b61c-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="3b61c-133">當方法完成時，您可以取得檔案的詳細資訊**FileData**屬性，這是集合的**MultipartFileData**物件。</span><span class="sxs-lookup"><span data-stu-id="3b61c-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="3b61c-134">**MultipartFileData.FileName**是在伺服器上，儲存檔案的本機檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="3b61c-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="3b61c-135">**MultipartFileData.Headers**包含部分標頭 (*不*要求標頭)。</span><span class="sxs-lookup"><span data-stu-id="3b61c-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="3b61c-136">這個函數可用來存取內容\_配置和內容類型標頭。</span><span class="sxs-lookup"><span data-stu-id="3b61c-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="3b61c-137">正如其名， **ReadAsMultipartAsync**是非同步的方法。</span><span class="sxs-lookup"><span data-stu-id="3b61c-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="3b61c-138">若要執行工作，在方法完成之後，使用[接續工作](https://msdn.microsoft.com/library/ee372288.aspx)(.NET 4.0) 或**await**關鍵字 (.NET 4.5)。</span><span class="sxs-lookup"><span data-stu-id="3b61c-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="3b61c-139">以下是先前的程式碼的.NET Framework 4.0 版本：</span><span class="sxs-lookup"><span data-stu-id="3b61c-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="3b61c-140">讀取表單控制項的資料</span><span class="sxs-lookup"><span data-stu-id="3b61c-140">Reading Form Control Data</span></span>

<span data-ttu-id="3b61c-141">先前已顯示的 HTML 表單有文字輸入的控制項。</span><span class="sxs-lookup"><span data-stu-id="3b61c-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="3b61c-142">您可以取得的值從控制項**FormData**屬性**MultipartFormDataStreamProvider**。</span><span class="sxs-lookup"><span data-stu-id="3b61c-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="3b61c-143">**FormData**是**NameValueCollection**其中包含表單控制項的名稱/值組。</span><span class="sxs-lookup"><span data-stu-id="3b61c-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="3b61c-144">此集合可以包含重複的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="3b61c-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="3b61c-145">請考慮這種形式：</span><span class="sxs-lookup"><span data-stu-id="3b61c-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="3b61c-146">要求主體可能看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="3b61c-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="3b61c-147">在此情況下， **FormData**集合會包含下列索引鍵/值組：</span><span class="sxs-lookup"><span data-stu-id="3b61c-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="3b61c-148">路線： 反覆存取</span><span class="sxs-lookup"><span data-stu-id="3b61c-148">trip: round-trip</span></span>
- <span data-ttu-id="3b61c-149">選項： nonstop</span><span class="sxs-lookup"><span data-stu-id="3b61c-149">options: nonstop</span></span>
- <span data-ttu-id="3b61c-150">選項： 日期</span><span class="sxs-lookup"><span data-stu-id="3b61c-150">options: dates</span></span>
- <span data-ttu-id="3b61c-151">基座： 視窗</span><span class="sxs-lookup"><span data-stu-id="3b61c-151">seat: window</span></span>
