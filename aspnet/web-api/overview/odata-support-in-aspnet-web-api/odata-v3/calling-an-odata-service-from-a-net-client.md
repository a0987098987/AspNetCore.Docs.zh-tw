---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: 呼叫 OData 服務的.NET 用戶端 (C#) |Microsoft Docs
author: MikeWasson
description: 本教學課程會示範如何從 C# 用戶端應用程式呼叫 OData 服務。 軟體版本，用於教學課程的 Visual Studio 2013 （適用於 Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 45d4d1304d2c628d2ab0d09925d2050bf6f58c60
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824894"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="ddc84-104">從.NET 用戶端 (C#) 呼叫 OData 服務</span><span class="sxs-lookup"><span data-stu-id="ddc84-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="ddc84-105">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ddc84-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ddc84-106">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="ddc84-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="ddc84-107">本教學課程會示範如何從 C# 用戶端應用程式呼叫 OData 服務。</span><span class="sxs-lookup"><span data-stu-id="ddc84-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ddc84-108">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="ddc84-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ddc84-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) （適用於 Visual Studio 2012）</span><span class="sxs-lookup"><span data-stu-id="ddc84-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="ddc84-110">WCF Data Services 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="ddc84-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="ddc84-111">Web API 2。</span><span class="sxs-lookup"><span data-stu-id="ddc84-111">Web API 2.</span></span> <span data-ttu-id="ddc84-112">（使用 Web API 2 中，所建置 OData 服務的範例，但用戶端應用程式不相依於 Web API）。</span><span class="sxs-lookup"><span data-stu-id="ddc84-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="ddc84-113">在本教學課程中，我將逐步建立呼叫 OData 服務的用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="ddc84-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="ddc84-114">OData 服務會公開下列實體：</span><span class="sxs-lookup"><span data-stu-id="ddc84-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="ddc84-115">下列文章說明如何在 Web API 中實作 OData 服務。</span><span class="sxs-lookup"><span data-stu-id="ddc84-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="ddc84-116">（您不需要讀取他們了解本教學課程中，不過）。</span><span class="sxs-lookup"><span data-stu-id="ddc84-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="ddc84-117">建立 Web API 2 中的 OData 端點</span><span class="sxs-lookup"><span data-stu-id="ddc84-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="ddc84-118">Web API 2 中的 OData 實體關聯性</span><span class="sxs-lookup"><span data-stu-id="ddc84-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="ddc84-119">Web API 2 中的 OData 動作</span><span class="sxs-lookup"><span data-stu-id="ddc84-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="ddc84-120">產生服務 Proxy</span><span class="sxs-lookup"><span data-stu-id="ddc84-120">Generate the Service Proxy</span></span>

<span data-ttu-id="ddc84-121">第一個步驟是產生服務 proxy。</span><span class="sxs-lookup"><span data-stu-id="ddc84-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="ddc84-122">服務 proxy 是一個.NET 類別來定義存取 OData 服務的方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="ddc84-123">Proxy 會轉譯成 HTTP 要求的方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="ddc84-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="ddc84-124">首先開啟 Visual Studio 中的 OData 服務專案。</span><span class="sxs-lookup"><span data-stu-id="ddc84-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="ddc84-125">按下 CTRL + F5，以在 IIS Express 在本機執行服務。</span><span class="sxs-lookup"><span data-stu-id="ddc84-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="ddc84-126">請注意本機位址，包括 Visual Studio 會將指派的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="ddc84-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="ddc84-127">當您建立的 proxy 時，您將需要此位址。</span><span class="sxs-lookup"><span data-stu-id="ddc84-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="ddc84-128">接下來，開啟 Visual studio 的另一個執行個體，並建立主控台應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="ddc84-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="ddc84-129">主控台應用程式將會是我們的 OData 用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="ddc84-129">The console application will be our OData client application.</span></span> <span data-ttu-id="ddc84-130">（您也可以加入專案與服務相同的方案。）</span><span class="sxs-lookup"><span data-stu-id="ddc84-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="ddc84-131">其餘的步驟，請參閱主控台專案。</span><span class="sxs-lookup"><span data-stu-id="ddc84-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="ddc84-132">在 [方案總管] 中，以滑鼠右鍵按一下**參考**，然後選取**加入服務參考**。</span><span class="sxs-lookup"><span data-stu-id="ddc84-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="ddc84-133">在 [**加入服務參考**] 對話方塊中，輸入的 OData 服務的位址：</span><span class="sxs-lookup"><span data-stu-id="ddc84-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="ddc84-134">何處*連接埠*的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="ddc84-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="ddc84-135">針對**命名空間**，輸入 「 ProductService"。</span><span class="sxs-lookup"><span data-stu-id="ddc84-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="ddc84-136">此選項會定義 proxy 類別的命名空間。</span><span class="sxs-lookup"><span data-stu-id="ddc84-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="ddc84-137">按一下 [ **Go**]。</span><span class="sxs-lookup"><span data-stu-id="ddc84-137">Click **Go**.</span></span> <span data-ttu-id="ddc84-138">Visual Studio 會讀取 OData 中繼資料文件，來探索服務中的實體。</span><span class="sxs-lookup"><span data-stu-id="ddc84-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="ddc84-139">按一下 **確定**將 proxy 類別新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="ddc84-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="ddc84-140">建立服務 Proxy 類別的執行個體</span><span class="sxs-lookup"><span data-stu-id="ddc84-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="ddc84-141">在您`Main`方法，建立 proxy 類別的新執行個體，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ddc84-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="ddc84-142">同樣地，使用實際的連接埠號碼正在執行您的服務。</span><span class="sxs-lookup"><span data-stu-id="ddc84-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="ddc84-143">當您部署您的服務時，您將使用即時服務的 URI。</span><span class="sxs-lookup"><span data-stu-id="ddc84-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="ddc84-144">您不需要更新的 proxy。</span><span class="sxs-lookup"><span data-stu-id="ddc84-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="ddc84-145">下列程式碼加入事件處理常式會列印至主控台視窗的要求 Uri。</span><span class="sxs-lookup"><span data-stu-id="ddc84-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="ddc84-146">此步驟並非必要，但很值得查看每個查詢的 Uri。</span><span class="sxs-lookup"><span data-stu-id="ddc84-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="ddc84-147">查詢服務</span><span class="sxs-lookup"><span data-stu-id="ddc84-147">Query the Service</span></span>

<span data-ttu-id="ddc84-148">下列程式碼會從 OData 服務取得產品的清單。</span><span class="sxs-lookup"><span data-stu-id="ddc84-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="ddc84-149">請注意，您不需要撰寫任何程式碼來傳送 HTTP 要求，或剖析回應。</span><span class="sxs-lookup"><span data-stu-id="ddc84-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="ddc84-150">Proxy 類別會自動從事此當您列舉`Container.Products`中的集合**foreach**迴圈。</span><span class="sxs-lookup"><span data-stu-id="ddc84-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="ddc84-151">當您執行應用程式時，輸出看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="ddc84-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="ddc84-152">若要取得的識別碼實體，請使用`where`子句。</span><span class="sxs-lookup"><span data-stu-id="ddc84-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="ddc84-153">本主題的其餘部分，我不會顯示整個`Main`函式，就呼叫服務所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ddc84-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="ddc84-154">套用查詢選項</span><span class="sxs-lookup"><span data-stu-id="ddc84-154">Apply Query Options</span></span>

<span data-ttu-id="ddc84-155">OData 會定義[查詢選項](../supporting-odata-query-options.md)，可用來篩選、 排序、 分頁資料等等。</span><span class="sxs-lookup"><span data-stu-id="ddc84-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="ddc84-156">在服務 proxy，您可以使用各種 LINQ 運算式，以套用這些選項。</span><span class="sxs-lookup"><span data-stu-id="ddc84-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="ddc84-157">在本節中，我會說明簡單的範例。</span><span class="sxs-lookup"><span data-stu-id="ddc84-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="ddc84-158">如需詳細資訊，請參閱主題[LINQ 考量 (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="ddc84-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="ddc84-159">篩選 ($filter)</span><span class="sxs-lookup"><span data-stu-id="ddc84-159">Filtering ($filter)</span></span>

<span data-ttu-id="ddc84-160">若要篩選，請使用`where`子句。</span><span class="sxs-lookup"><span data-stu-id="ddc84-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="ddc84-161">下列範例會篩選依產品類別目錄。</span><span class="sxs-lookup"><span data-stu-id="ddc84-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="ddc84-162">此程式碼會對應至下列 OData 查詢。</span><span class="sxs-lookup"><span data-stu-id="ddc84-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="ddc84-163">請注意，proxy 會將轉換`where`OData 子句`$filter`運算式。</span><span class="sxs-lookup"><span data-stu-id="ddc84-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="ddc84-164">排序 ($orderby)</span><span class="sxs-lookup"><span data-stu-id="ddc84-164">Sorting ($orderby)</span></span>

<span data-ttu-id="ddc84-165">若要排序，請使用`orderby`子句。</span><span class="sxs-lookup"><span data-stu-id="ddc84-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="ddc84-166">下列範例會依價格，從最高到最低。</span><span class="sxs-lookup"><span data-stu-id="ddc84-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="ddc84-167">以下是對應的 OData 要求。</span><span class="sxs-lookup"><span data-stu-id="ddc84-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="ddc84-168">用戶端的分頁 （$skip 和 $top）</span><span class="sxs-lookup"><span data-stu-id="ddc84-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="ddc84-169">為大型實體集，用戶端可能會想要限制結果數目。</span><span class="sxs-lookup"><span data-stu-id="ddc84-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="ddc84-170">例如，用戶端可能會顯示 10 個項目一次。</span><span class="sxs-lookup"><span data-stu-id="ddc84-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="ddc84-171">這就叫做*用戶端分頁*。</span><span class="sxs-lookup"><span data-stu-id="ddc84-171">This is called *client-side paging*.</span></span> <span data-ttu-id="ddc84-172">(另外還有[伺服器端分頁](../supporting-odata-query-options.md#server-paging)，其中伺服器限制的結果數目。)若要執行用戶端分頁，請使用 LINQ**略過**並**採取**方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="ddc84-173">下列範例會略過前 40 的結果，並採用 10 的下一步。</span><span class="sxs-lookup"><span data-stu-id="ddc84-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="ddc84-174">以下是對應的 OData 要求：</span><span class="sxs-lookup"><span data-stu-id="ddc84-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="ddc84-175">選取 ($select)，以及展開 ($expand)</span><span class="sxs-lookup"><span data-stu-id="ddc84-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="ddc84-176">若要加入相關的實體，使用`DataServiceQuery<t>.Expand`方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="ddc84-177">例如，若要納入`Supplier`針對每個`Product`:</span><span class="sxs-lookup"><span data-stu-id="ddc84-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="ddc84-178">以下是對應的 OData 要求：</span><span class="sxs-lookup"><span data-stu-id="ddc84-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="ddc84-179">若要變更回應的圖形，請使用 LINQ**選取**子句。</span><span class="sxs-lookup"><span data-stu-id="ddc84-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="ddc84-180">下列範例會取得每項產品，與任何其他屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="ddc84-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="ddc84-181">以下是對應的 OData 要求：</span><span class="sxs-lookup"><span data-stu-id="ddc84-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="ddc84-182">Select 子句可以包含相關的實體。</span><span class="sxs-lookup"><span data-stu-id="ddc84-182">A select clause can include related entities.</span></span> <span data-ttu-id="ddc84-183">在此情況下，請勿呼叫**展開**; proxy 會自動包含擴充在此情況下。</span><span class="sxs-lookup"><span data-stu-id="ddc84-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="ddc84-184">下列範例會取得每個產品的供應商與名稱。</span><span class="sxs-lookup"><span data-stu-id="ddc84-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="ddc84-185">以下是對應的 OData 要求。</span><span class="sxs-lookup"><span data-stu-id="ddc84-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="ddc84-186">請注意，它包含 **$expand**選項。</span><span class="sxs-lookup"><span data-stu-id="ddc84-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="ddc84-187">如需有關 $select 和 $展開，請參閱 <<c0> [ 使用 $select，$expand、 和 Web API 2 中的 $value](../using-select-expand-and-value.md)。</span><span class="sxs-lookup"><span data-stu-id="ddc84-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="ddc84-188">加入新的實體</span><span class="sxs-lookup"><span data-stu-id="ddc84-188">Add a New Entity</span></span>

<span data-ttu-id="ddc84-189">若要新增新實體至實體集，請呼叫`AddToEntitySet`，其中*EntitySet*是實體集的名稱。</span><span class="sxs-lookup"><span data-stu-id="ddc84-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="ddc84-190">例如，`AddToProducts`將新`Product`到`Products`實體集。</span><span class="sxs-lookup"><span data-stu-id="ddc84-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="ddc84-191">當您產生 proxy 時，WCF Data Services 會自動建立這些強型別**AddTo**方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="ddc84-192">若要新增兩個實體之間的連結，請使用**AddLink**並**SetLink**方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="ddc84-193">下列程式碼加入新的供應商和新的產品，並接著會建立它們之間的連結。</span><span class="sxs-lookup"><span data-stu-id="ddc84-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="ddc84-194">使用**AddLink**導覽屬性時的集合。</span><span class="sxs-lookup"><span data-stu-id="ddc84-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="ddc84-195">在此範例中，我們會新增至產品`Products`上供應商的集合。</span><span class="sxs-lookup"><span data-stu-id="ddc84-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="ddc84-196">使用**SetLink**導覽屬性時的單一實體。</span><span class="sxs-lookup"><span data-stu-id="ddc84-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="ddc84-197">在此範例中，我們要設定`Supplier`產品上的屬性。</span><span class="sxs-lookup"><span data-stu-id="ddc84-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="ddc84-198">更新/修補程式</span><span class="sxs-lookup"><span data-stu-id="ddc84-198">Update / Patch</span></span>

<span data-ttu-id="ddc84-199">若要更新實體，請呼叫**UpdateObject**方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="ddc84-200">當您呼叫時，執行更新**SaveChanges**。</span><span class="sxs-lookup"><span data-stu-id="ddc84-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="ddc84-201">根據預設，WCF 會傳送 HTTP MERGE 要求。</span><span class="sxs-lookup"><span data-stu-id="ddc84-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="ddc84-202">**PatchOnUpdate**選項會告訴 WCF 要改為傳送的 HTTP PATCH。</span><span class="sxs-lookup"><span data-stu-id="ddc84-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="ddc84-203">為什麼修補程式與合併？</span><span class="sxs-lookup"><span data-stu-id="ddc84-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="ddc84-204">原始的 HTTP 1.1 規格 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) 未定義任何 HTTP 方法，以 「 部分更新 」 語意。</span><span class="sxs-lookup"><span data-stu-id="ddc84-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="ddc84-205">若要支援部分更新，OData 規格會定義合併式方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="ddc84-206">在 2010年[RFC 5789](http://tools.ietf.org/html/rfc5789)定義部分更新的 PATCH 方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="ddc84-207">您可以讀取此歷程記錄的一些[部落格文章](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx)WCF Data Services 部落格。</span><span class="sxs-lookup"><span data-stu-id="ddc84-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="ddc84-208">現在，修補程式，最好透過合併。</span><span class="sxs-lookup"><span data-stu-id="ddc84-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="ddc84-209">Web API scaffolding 所建立的 OData 控制器支援這兩種方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="ddc84-210">如果您想要取代整個實體 （PUT 語意），指定**ReplaceOnUpdate**選項。</span><span class="sxs-lookup"><span data-stu-id="ddc84-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="ddc84-211">這會導致傳送 HTTP PUT 要求的 WCF。</span><span class="sxs-lookup"><span data-stu-id="ddc84-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="ddc84-212">刪除實體</span><span class="sxs-lookup"><span data-stu-id="ddc84-212">Delete an Entity</span></span>

<span data-ttu-id="ddc84-213">若要刪除實體，請呼叫**DeleteObject**。</span><span class="sxs-lookup"><span data-stu-id="ddc84-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="ddc84-214">叫用 OData 動作</span><span class="sxs-lookup"><span data-stu-id="ddc84-214">Invoke an OData Action</span></span>

<span data-ttu-id="ddc84-215">在 OData 中，[動作](odata-actions.md)來新增伺服器端行為，不會輕易地定義為實體上的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="ddc84-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="ddc84-216">雖然 OData 中繼資料文件描述的動作，proxy 類別並不會為其建立任何強型別方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="ddc84-217">您可以使用泛型仍叫用 OData 動作**Execute**方法。</span><span class="sxs-lookup"><span data-stu-id="ddc84-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="ddc84-218">不過，您必須知道資料類型的參數和傳回值。</span><span class="sxs-lookup"><span data-stu-id="ddc84-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="ddc84-219">例如，`RateProduct`動作會將名為"Rating"類型的參數`Int32`，並傳回`double`。</span><span class="sxs-lookup"><span data-stu-id="ddc84-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="ddc84-220">下列程式碼示範如何叫用此動作。</span><span class="sxs-lookup"><span data-stu-id="ddc84-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="ddc84-221">如需詳細資訊，請參閱 <<c0> [ 呼叫服務作業和動作](https://msdn.microsoft.com/library/hh230677.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ddc84-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="ddc84-222">其中一個選項是擴充**容器**類別，以提供強型別的方法叫用的動作：</span><span class="sxs-lookup"><span data-stu-id="ddc84-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
