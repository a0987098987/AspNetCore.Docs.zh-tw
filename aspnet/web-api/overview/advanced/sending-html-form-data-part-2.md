---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: ASP.NET Web API 中傳送 HTML 表單資料： 檔案上傳和多部分 MIME |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c1c85f462141daf747e23aa4215d47f2d263140
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386704"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="77a12-102">ASP.NET Web API 中傳送 HTML 表單資料： 檔案上傳和多部分 MIME</span><span class="sxs-lookup"><span data-stu-id="77a12-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="77a12-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="77a12-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="77a12-104">第 2 部分： 檔案上傳和多個 MIME</span><span class="sxs-lookup"><span data-stu-id="77a12-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="77a12-105">本教學課程會示範如何將檔案上傳至 web API。</span><span class="sxs-lookup"><span data-stu-id="77a12-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="77a12-106">它也會說明如何處理多部分 MIME 資料。</span><span class="sxs-lookup"><span data-stu-id="77a12-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="77a12-107">[下載已完成的專案](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d)。</span><span class="sxs-lookup"><span data-stu-id="77a12-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="77a12-108">將檔案上傳之 HTML 表單的範例如下：</span><span class="sxs-lookup"><span data-stu-id="77a12-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="77a12-109">這個表單包含文字輸入的控制項和檔案的輸入的控制項。</span><span class="sxs-lookup"><span data-stu-id="77a12-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="77a12-110">當表單包含檔案的輸入的控制項， **enctype**屬性應永遠為&quot;multipart/form-data&quot;，指定將會傳送為多部分 MIME 訊息的格式。</span><span class="sxs-lookup"><span data-stu-id="77a12-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="77a12-111">多部分 MIME 訊息的格式是最容易了解藉由查看要求範例：</span><span class="sxs-lookup"><span data-stu-id="77a12-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="77a12-112">此訊息會分割成兩個*組件*，一個用於每個表單控制項。</span><span class="sxs-lookup"><span data-stu-id="77a12-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="77a12-113">組件界限會以虛線開始的行。</span><span class="sxs-lookup"><span data-stu-id="77a12-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="77a12-114">組件界限會包含隨機的元件 (&quot;41184676334&quot;) 以確保界限字串不小心不顯示訊息部分內。</span><span class="sxs-lookup"><span data-stu-id="77a12-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="77a12-115">每個訊息組件包含一或多個標頭，後面接著組件的內容。</span><span class="sxs-lookup"><span data-stu-id="77a12-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="77a12-116">內容配置標頭包含控制項的名稱。</span><span class="sxs-lookup"><span data-stu-id="77a12-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="77a12-117">對於檔案，它也會包含檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="77a12-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="77a12-118">Content-type 標頭描述組件中的資料。</span><span class="sxs-lookup"><span data-stu-id="77a12-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="77a12-119">如果省略此標頭，則預設值為 text/plain。</span><span class="sxs-lookup"><span data-stu-id="77a12-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="77a12-120">在上述範例中，使用者上傳檔案，名為 GrandCanyon.jpg，內容類型 image/jpeg;文字輸入的值是&quot;暑假&quot;。</span><span class="sxs-lookup"><span data-stu-id="77a12-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="77a12-121">檔案上傳</span><span class="sxs-lookup"><span data-stu-id="77a12-121">File Upload</span></span>

<span data-ttu-id="77a12-122">現在讓我們看看檔案讀取多部分 MIME 訊息的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="77a12-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="77a12-123">控制器會以非同步方式讀取檔案。</span><span class="sxs-lookup"><span data-stu-id="77a12-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="77a12-124">Web API 支援使用非同步動作[以工作為基礎的程式設計模型](https://msdn.microsoft.com/library/dd460693.aspx)。</span><span class="sxs-lookup"><span data-stu-id="77a12-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="77a12-125">如果您的目標.NET Framework 4.5 中，可支援第一，以下是程式碼**非同步**並**await**關鍵字。</span><span class="sxs-lookup"><span data-stu-id="77a12-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="77a12-126">請注意，控制器動作未採用任何參數。</span><span class="sxs-lookup"><span data-stu-id="77a12-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="77a12-127">這是因為我們處理要求本文內的動作，而不叫用的媒體類型格式器。</span><span class="sxs-lookup"><span data-stu-id="77a12-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="77a12-128">**IsMultipartContent**方法會檢查要求是否包含多部分 MIME 訊息。</span><span class="sxs-lookup"><span data-stu-id="77a12-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="77a12-129">如果沒有，則控制器會傳回 HTTP 狀態碼 415 （不支援的媒體類型）。</span><span class="sxs-lookup"><span data-stu-id="77a12-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="77a12-130">**MultipartFormDataStreamProvider**類別是一個 helper 物件，配置檔案資料流已上傳檔案。</span><span class="sxs-lookup"><span data-stu-id="77a12-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="77a12-131">若要讀取多部分 MIME 訊息，呼叫**ReadAsMultipartAsync**方法。</span><span class="sxs-lookup"><span data-stu-id="77a12-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="77a12-132">這個方法會擷取所有的訊息組件，並將它們寫入至所提供的資料流**MultipartFormDataStreamProvider**。</span><span class="sxs-lookup"><span data-stu-id="77a12-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="77a12-133">方法完成時，您可以取得檔案的詳細資訊**FileData**屬性，這是集合的**MultipartFileData**物件。</span><span class="sxs-lookup"><span data-stu-id="77a12-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="77a12-134">**MultipartFileData.FileName**是在伺服器上，檔案儲存位置的本機檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="77a12-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="77a12-135">**MultipartFileData.Headers**包含組件的標頭 (*不*要求標頭)。</span><span class="sxs-lookup"><span data-stu-id="77a12-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="77a12-136">您可以使用此選項來存取內容\_配置和內容類型標頭。</span><span class="sxs-lookup"><span data-stu-id="77a12-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="77a12-137">恰如其名**ReadAsMultipartAsync**是非同步方法。</span><span class="sxs-lookup"><span data-stu-id="77a12-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="77a12-138">若要執行的工作，在方法完成之後，使用[接續工作](https://msdn.microsoft.com/library/ee372288.aspx)(.NET 4.0) 或**await**關鍵字 (.NET 4.5)。</span><span class="sxs-lookup"><span data-stu-id="77a12-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="77a12-139">以下是先前的程式碼的.NET Framework 4.0 版本：</span><span class="sxs-lookup"><span data-stu-id="77a12-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="77a12-140">讀取表單控制項的資料</span><span class="sxs-lookup"><span data-stu-id="77a12-140">Reading Form Control Data</span></span>

<span data-ttu-id="77a12-141">我先前所示範的 HTML 表單有文字輸入的控制項。</span><span class="sxs-lookup"><span data-stu-id="77a12-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="77a12-142">您可以取得從控制項的值**FormData**屬性**MultipartFormDataStreamProvider**。</span><span class="sxs-lookup"><span data-stu-id="77a12-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="77a12-143">**FormData**已**NameValueCollection**包含表單控制項的名稱/值組。</span><span class="sxs-lookup"><span data-stu-id="77a12-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="77a12-144">集合可以包含重複的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="77a12-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="77a12-145">請考慮這種形式：</span><span class="sxs-lookup"><span data-stu-id="77a12-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="77a12-146">要求主體可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="77a12-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="77a12-147">在此情況下， **FormData**集合會包含下列索引鍵/值組：</span><span class="sxs-lookup"><span data-stu-id="77a12-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="77a12-148">車程： 反覆存取</span><span class="sxs-lookup"><span data-stu-id="77a12-148">trip: round-trip</span></span>
- <span data-ttu-id="77a12-149">選項： nonstop</span><span class="sxs-lookup"><span data-stu-id="77a12-149">options: nonstop</span></span>
- <span data-ttu-id="77a12-150">選項： 日期</span><span class="sxs-lookup"><span data-stu-id="77a12-150">options: dates</span></span>
- <span data-ttu-id="77a12-151">基座： 視窗</span><span class="sxs-lookup"><span data-stu-id="77a12-151">seat: window</span></span>
