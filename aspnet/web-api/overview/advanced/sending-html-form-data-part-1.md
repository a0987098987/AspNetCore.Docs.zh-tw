---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 傳送 HTML 表單資料的 ASP.NET Web API： 表單 urlencoded 資料 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506937"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="991b8-102">傳送 HTML 表單資料的 ASP.NET Web API： 表單 urlencoded 資料</span><span class="sxs-lookup"><span data-stu-id="991b8-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="991b8-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="991b8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="991b8-104">第 1 部分： 表單 urlencoded 資料</span><span class="sxs-lookup"><span data-stu-id="991b8-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="991b8-105">本文示範如何將表單 urlencoded 資料發佈到 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="991b8-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="991b8-106">HTML 表單的概觀</span><span class="sxs-lookup"><span data-stu-id="991b8-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="991b8-107">傳送複雜型別</span><span class="sxs-lookup"><span data-stu-id="991b8-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="991b8-108">透過 AJAX 傳送表單資料</span><span class="sxs-lookup"><span data-stu-id="991b8-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="991b8-109">傳送的簡單類型</span><span class="sxs-lookup"><span data-stu-id="991b8-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="991b8-110">[下載完成的專案](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007)。</span><span class="sxs-lookup"><span data-stu-id="991b8-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="991b8-111">HTML 表單的概觀</span><span class="sxs-lookup"><span data-stu-id="991b8-111">Overview of HTML Forms</span></span>

<span data-ttu-id="991b8-112">HTML 表單使用取得，或是張貼到將資料傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="991b8-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="991b8-113">**方法**屬性**表單**項目提供的 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="991b8-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="991b8-114">預設的方法為 GET。</span><span class="sxs-lookup"><span data-stu-id="991b8-114">The default method is GET.</span></span> <span data-ttu-id="991b8-115">如果此表單會使用 GET，資料會被編碼為查詢字串 URI 中的表單。</span><span class="sxs-lookup"><span data-stu-id="991b8-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="991b8-116">如果此表單會使用 POST，表單資料會在要求主體中。</span><span class="sxs-lookup"><span data-stu-id="991b8-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="991b8-117">已張貼資料**enctype**屬性指定的要求主體格式：</span><span class="sxs-lookup"><span data-stu-id="991b8-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="991b8-118">enctype</span><span class="sxs-lookup"><span data-stu-id="991b8-118">enctype</span></span> | <span data-ttu-id="991b8-119">說明</span><span class="sxs-lookup"><span data-stu-id="991b8-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="991b8-120">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="991b8-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="991b8-121">表單資料編碼為名稱/值組，類似於 URI 查詢字串。</span><span class="sxs-lookup"><span data-stu-id="991b8-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="991b8-122">這是文章的預設格式。</span><span class="sxs-lookup"><span data-stu-id="991b8-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="991b8-123">multipart/表單資料</span><span class="sxs-lookup"><span data-stu-id="991b8-123">multipart/form-data</span></span> | <span data-ttu-id="991b8-124">表單資料編碼為多部分 MIME 訊息。</span><span class="sxs-lookup"><span data-stu-id="991b8-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="991b8-125">如果您要將檔案上傳到伺服器，請使用此格式。</span><span class="sxs-lookup"><span data-stu-id="991b8-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="991b8-126">第 1 部分，這篇文章探討 x-www-表單-urlencoded 格式。</span><span class="sxs-lookup"><span data-stu-id="991b8-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="991b8-127">[第 2 部分](sending-html-form-data-part-2.md)描述 MIME 多組件。</span><span class="sxs-lookup"><span data-stu-id="991b8-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="991b8-128">傳送複雜型別</span><span class="sxs-lookup"><span data-stu-id="991b8-128">Sending Complex Types</span></span>

<span data-ttu-id="991b8-129">一般而言，您會將傳送複雜類型，從多個表單控制項的值所組成。</span><span class="sxs-lookup"><span data-stu-id="991b8-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="991b8-130">請考慮下列代表的狀態更新的模型：</span><span class="sxs-lookup"><span data-stu-id="991b8-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="991b8-131">以下是可接受的 Web API 控制器`Update`透過 POST 的物件。</span><span class="sxs-lookup"><span data-stu-id="991b8-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="991b8-132">此控制器會使用[動作為基礎的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name)，因此路由範本是&quot;api / {controller} / {action} / {id}&quot;。</span><span class="sxs-lookup"><span data-stu-id="991b8-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="991b8-133">用戶端會將資料公佈至&quot;/api/updates/complex&quot;。</span><span class="sxs-lookup"><span data-stu-id="991b8-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="991b8-134">現在讓我們寫 HTML 表單送出狀態更新的使用者。</span><span class="sxs-lookup"><span data-stu-id="991b8-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="991b8-135">請注意，**動作**表單上的屬性是我們的控制器動作的 URI。</span><span class="sxs-lookup"><span data-stu-id="991b8-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="991b8-136">以下是具有某些輸入值的表單：</span><span class="sxs-lookup"><span data-stu-id="991b8-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="991b8-137">當使用者按一下時送出時，瀏覽器會傳送 HTTP 要求與下列類似：</span><span class="sxs-lookup"><span data-stu-id="991b8-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="991b8-138">請注意要求主體包含表單資料，格式為名稱/值組。</span><span class="sxs-lookup"><span data-stu-id="991b8-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="991b8-139">Web 應用程式開發介面會自動將轉換的名稱/值組的執行個體`Update`類別。</span><span class="sxs-lookup"><span data-stu-id="991b8-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="991b8-140">透過 AJAX 傳送表單資料</span><span class="sxs-lookup"><span data-stu-id="991b8-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="991b8-141">當使用者提交表單時，瀏覽器巡覽離開目前的頁面，並呈現回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="991b8-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="991b8-142">回應是 HTML 網頁，這會是 [確定]。</span><span class="sxs-lookup"><span data-stu-id="991b8-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="991b8-143">與 web 應用程式開發介面，不過，回應主體是通常是空的或包含結構化的資料，例如 JSON。</span><span class="sxs-lookup"><span data-stu-id="991b8-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="991b8-144">在此情況下，它可讓更多的意義上，將傳送的表單資料，使用 AJAX 要求，讓網頁可以處理回應。</span><span class="sxs-lookup"><span data-stu-id="991b8-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="991b8-145">下列程式碼會示範如何發佈使用 jQuery 表單資料。</span><span class="sxs-lookup"><span data-stu-id="991b8-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="991b8-146">JQuery**提交**函式取代新的函式中的表單動作。</span><span class="sxs-lookup"><span data-stu-id="991b8-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="991b8-147">這會覆寫 [提交] 按鈕的預設行為。</span><span class="sxs-lookup"><span data-stu-id="991b8-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="991b8-148">**序列化**函式會將表單資料序列化成名稱/值組。</span><span class="sxs-lookup"><span data-stu-id="991b8-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="991b8-149">若要將表單資料傳送至伺服器，呼叫`$.post()`。</span><span class="sxs-lookup"><span data-stu-id="991b8-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="991b8-150">當要求完成時，`.success()`或`.error()`處理常式會向使用者顯示適當的訊息。</span><span class="sxs-lookup"><span data-stu-id="991b8-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="991b8-151">傳送的簡單類型</span><span class="sxs-lookup"><span data-stu-id="991b8-151">Sending Simple Types</span></span>

<span data-ttu-id="991b8-152">上一節中，我們會傳送 Web API 模型類別的執行個體還原序列化的複雜類型。</span><span class="sxs-lookup"><span data-stu-id="991b8-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="991b8-153">您也可以傳送簡單類型，例如字串。</span><span class="sxs-lookup"><span data-stu-id="991b8-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="991b8-154">在傳送之前的簡單類型，請考慮改為包裝複雜類型中的值。</span><span class="sxs-lookup"><span data-stu-id="991b8-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="991b8-155">這可讓您在伺服器端的模型驗證的優點，並可讓您更輕鬆地擴充您的模型，如有需要。</span><span class="sxs-lookup"><span data-stu-id="991b8-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="991b8-156">傳送簡單類型的基本步驟都相同，但有兩個只有些微的差異。</span><span class="sxs-lookup"><span data-stu-id="991b8-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="991b8-157">首先，控制站，您必須裝飾的參數名稱與**FromBody**屬性。</span><span class="sxs-lookup"><span data-stu-id="991b8-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="991b8-158">根據預設，Web API 會嘗試取得要求 URI 中的簡單類型。</span><span class="sxs-lookup"><span data-stu-id="991b8-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="991b8-159">**FromBody**屬性會告知 Web API 來讀取的要求主體中的值。</span><span class="sxs-lookup"><span data-stu-id="991b8-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="991b8-160">Web API 會讀取回應主體最多一次，因此只有一個動作的參數可能來自要求主體。</span><span class="sxs-lookup"><span data-stu-id="991b8-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="991b8-161">如果您需要從要求主體中取得多個值，定義複雜型別。</span><span class="sxs-lookup"><span data-stu-id="991b8-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="991b8-162">第二，用戶端必須傳送的值，以下列格式：</span><span class="sxs-lookup"><span data-stu-id="991b8-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="991b8-163">具體而言，名稱/值組的名稱部分必須是空的簡單類型。</span><span class="sxs-lookup"><span data-stu-id="991b8-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="991b8-164">並非所有瀏覽器支援 HTML 表單，但您建立此格式在指令碼，如下所示：</span><span class="sxs-lookup"><span data-stu-id="991b8-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="991b8-165">以下是範例格式：</span><span class="sxs-lookup"><span data-stu-id="991b8-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="991b8-166">此外，這裡有要送出表單值的指令碼。</span><span class="sxs-lookup"><span data-stu-id="991b8-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="991b8-167">從先前的指令碼的唯一差異是傳入的引數**張貼**函式。</span><span class="sxs-lookup"><span data-stu-id="991b8-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="991b8-168">您可以使用相同的方法來傳送簡單類型的陣列：</span><span class="sxs-lookup"><span data-stu-id="991b8-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="991b8-169">其他資源</span><span class="sxs-lookup"><span data-stu-id="991b8-169">Additional Resources</span></span>

[<span data-ttu-id="991b8-170">第 2 部分： 檔案上傳和多部分 MIME</span><span class="sxs-lookup"><span data-stu-id="991b8-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
