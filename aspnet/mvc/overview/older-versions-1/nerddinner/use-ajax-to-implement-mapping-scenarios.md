---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: 若要實作對應實例中使用 AJAX |Microsoft 文件
author: microsoft
description: 步驟 11 會示範如何將 AJAX 對應支援整合到我們的 NerdDinner 應用程式，讓使用者建立、 編輯或檢視以查看 l dinners...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872711"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a><span data-ttu-id="9efd2-103">用於 AJAX 實作對應實例</span><span class="sxs-lookup"><span data-stu-id="9efd2-103">Use AJAX to Implement Mapping Scenarios</span></span>
====================
<span data-ttu-id="9efd2-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9efd2-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="9efd2-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="9efd2-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="9efd2-106">這是一套免費的步驟 11 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="9efd2-106">This is step 11 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="9efd2-107">步驟 11 示範如何整合到我們 NerdDinner 應用程式，讓使用者建立、 編輯或檢視以圖形方式查看 dinner 位置 dinners AJAX 對應支援。</span><span class="sxs-lookup"><span data-stu-id="9efd2-107">Step 11 shows how to integrate AJAX mapping support into our NerdDinner application, enabling users who are creating, editing or viewing dinners to see the location of the dinner graphically.</span></span>
> 
> <span data-ttu-id="9efd2-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="9efd2-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a><span data-ttu-id="9efd2-109">NerdDinner 步驟 11： 整合的 AJAX 對應</span><span class="sxs-lookup"><span data-stu-id="9efd2-109">NerdDinner Step 11: Integrating an AJAX Map</span></span>

<span data-ttu-id="9efd2-110">我們現在要我們的應用程式一些更視覺化的方式的全新整合 AJAX 對應支援。</span><span class="sxs-lookup"><span data-stu-id="9efd2-110">We'll now make our application a little more visually exciting by integrating AJAX mapping support.</span></span> <span data-ttu-id="9efd2-111">這會讓使用者建立、 編輯或檢視以圖形方式查看 dinner 位置 dinners。</span><span class="sxs-lookup"><span data-stu-id="9efd2-111">This will enable users who are creating, editing or viewing dinners to see the location of the dinner graphically.</span></span>

### <a name="creating-a-map-partial-view"></a><span data-ttu-id="9efd2-112">建立對應的部分檢視</span><span class="sxs-lookup"><span data-stu-id="9efd2-112">Creating a Map Partial View</span></span>

<span data-ttu-id="9efd2-113">我們會使用對應功能，我們的應用程式內的數個位置中。</span><span class="sxs-lookup"><span data-stu-id="9efd2-113">We are going to use mapping functionality in several places within our application.</span></span> <span data-ttu-id="9efd2-114">若要讓我們的程式碼乾我們將會封裝我們可以重複使用跨多個控制器的動作和檢視單一部分範本內的一般對應功能。</span><span class="sxs-lookup"><span data-stu-id="9efd2-114">To keep our code DRY we'll encapsulate the common map functionality within a single partial template that we can re-use across multiple controller actions and views.</span></span> <span data-ttu-id="9efd2-115">我們將"map.ascx 」 的這個部分檢視的名稱，並建立 \Views\Dinners 目錄內。</span><span class="sxs-lookup"><span data-stu-id="9efd2-115">We'll name this partial view "map.ascx" and create it within the \Views\Dinners directory.</span></span>

<span data-ttu-id="9efd2-116">我們可以建立 map.ascx 部分 \Views\Dinners 目錄上按一下滑鼠右鍵，然後選擇 [新增]-&gt;檢視功能表命令。</span><span class="sxs-lookup"><span data-stu-id="9efd2-116">We can create the map.ascx partial by right-clicking on the \Views\Dinners directory and choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="9efd2-117">我們會檢視 「 Map.ascx 」 的名稱、 檢查做為部分檢視，並表示我們會將它傳遞強型別"Dinner 」 模型類別：</span><span class="sxs-lookup"><span data-stu-id="9efd2-117">We'll name the view "Map.ascx", check it as a partial view, and indicate that we are going to pass it a strongly-typed "Dinner" model class:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

<span data-ttu-id="9efd2-118">當我們按一下 [新增] 按鈕將會建立我們部分的範本。</span><span class="sxs-lookup"><span data-stu-id="9efd2-118">When we click the "Add" button our partial template will be created.</span></span> <span data-ttu-id="9efd2-119">然後，我們會將更新 Map.ascx 檔案包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="9efd2-119">We'll then update the Map.ascx file to have the following content:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

<span data-ttu-id="9efd2-120">第一個&lt;指令碼&gt;參考指向 Microsoft 虛擬地球 6.2 對應程式庫。</span><span class="sxs-lookup"><span data-stu-id="9efd2-120">The first &lt;script&gt; reference points to the Microsoft Virtual Earth 6.2 mapping library.</span></span> <span data-ttu-id="9efd2-121">第二個&lt;指令碼&gt;參考指向的 map.js 檔案我們很快就會建立將會封裝我們一般 Javascript 對應邏輯。</span><span class="sxs-lookup"><span data-stu-id="9efd2-121">The second &lt;script&gt; reference points to a map.js file that we will shortly create which will encapsulate our common Javascript mapping logic.</span></span> <span data-ttu-id="9efd2-122">&lt;d i v id ="theMap"&gt;項目是要使用虛擬地球裝載對應的 HTML 容器。</span><span class="sxs-lookup"><span data-stu-id="9efd2-122">The &lt;div id="theMap"&gt; element is the HTML container that Virtual Earth will use to host the map.</span></span>

<span data-ttu-id="9efd2-123">然後我們會內嵌&lt;指令碼&gt;包含兩個此檢視特定的 JavaScript 函式區塊。</span><span class="sxs-lookup"><span data-stu-id="9efd2-123">We then have an embedded &lt;script&gt; block that contains two JavaScript functions specific to this view.</span></span> <span data-ttu-id="9efd2-124">第一個函式會使用來自 jQuery 至 wire 上準備好要執行用戶端指令碼頁面時執行的函式。</span><span class="sxs-lookup"><span data-stu-id="9efd2-124">The first function uses jQuery to wire-up a function that executes when the page is ready to run client-side script.</span></span> <span data-ttu-id="9efd2-125">它會呼叫 LoadMap() helper 函式，我們會定義載入虛擬地球地圖控制項我們 Map.js 指令碼檔案中。</span><span class="sxs-lookup"><span data-stu-id="9efd2-125">It calls a LoadMap() helper function that we'll define within our Map.js script file to load the virtual earth map control.</span></span> <span data-ttu-id="9efd2-126">第二個函式是回呼事件處理常式，以識別位置地圖上加 pin。</span><span class="sxs-lookup"><span data-stu-id="9efd2-126">The second function is a callback event handler that adds a pin to the map that identifies a location.</span></span>

<span data-ttu-id="9efd2-127">請注意我們如何使用伺服器端&lt;%= %&gt;內嵌的緯度與經度的 Dinner 我們想要將對應至 JavaScript 的用戶端指令碼區塊內的區塊。</span><span class="sxs-lookup"><span data-stu-id="9efd2-127">Notice how we are using a server-side &lt;%= %&gt; block within the client-side script block to embed the latitude and longitude of the Dinner we want to map into the JavaScript.</span></span> <span data-ttu-id="9efd2-128">這是一個實用的方法輸出可供用戶端指令碼 （而不需要個別 AJAX 呼叫至伺服器來擷取值 – 使其更快） 的動態值。</span><span class="sxs-lookup"><span data-stu-id="9efd2-128">This is a useful technique to output dynamic values that can be used by client-side script (without requiring a separate AJAX call back to the server to retrieve the values – which makes it faster).</span></span> <span data-ttu-id="9efd2-129">&lt;%= %&gt;檢視在伺服器中 – 上轉譯時，區塊會執行，而且因此 HTML 的輸出只最後會與內嵌的 JavaScript 值 (例如： var 緯度 = 47.64312;)。</span><span class="sxs-lookup"><span data-stu-id="9efd2-129">The &lt;%= %&gt; blocks will execute when the view is rendering on the server – and so the output of the HTML will just end up with embedded JavaScript values (for example: var latitude = 47.64312;).</span></span>

### <a name="creating-a-mapjs-utility-library"></a><span data-ttu-id="9efd2-130">建立 Map.js 公用程式程式庫</span><span class="sxs-lookup"><span data-stu-id="9efd2-130">Creating a Map.js utility library</span></span>

<span data-ttu-id="9efd2-131">我們現在來建立 Map.js 檔案，我們可以用來封裝我們地圖的 JavaScript 功能 （和實作 LoadMap 和 LoadPin 上述的方法）。</span><span class="sxs-lookup"><span data-stu-id="9efd2-131">Let's now create the Map.js file that we can use to encapsulate the JavaScript functionality for our map (and implement the LoadMap and LoadPin methods above).</span></span> <span data-ttu-id="9efd2-132">我們可以執行這項操作，在專案中，在 \Scripts 目錄上按一下滑鼠右鍵，然後選擇 「 加入-&gt;新項目 」 的功能表命令選取 JScript 項目，並將它命名為"Map.js"。</span><span class="sxs-lookup"><span data-stu-id="9efd2-132">We can do this by right-clicking on the \Scripts directory within our project, and then choose the "Add-&gt;New Item" menu command, select the JScript item, and name it "Map.js".</span></span>

<span data-ttu-id="9efd2-133">以下是我們會將新增的 JavaScript 程式碼將與虛擬地球，以顯示我們對應並加入至該位置 pin 我們 dinners 互動 Map.js 檔案：</span><span class="sxs-lookup"><span data-stu-id="9efd2-133">Below is the JavaScript code we'll add to the Map.js file that will interact with Virtual Earth to display our map and add locations pins to it for our dinners:</span></span>

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a><span data-ttu-id="9efd2-134">建立和編輯表單與整合對應</span><span class="sxs-lookup"><span data-stu-id="9efd2-134">Integrating the Map with Create and Edit Forms</span></span>

<span data-ttu-id="9efd2-135">我們現在會整合地圖支援與我們現有的建立與編輯案例。</span><span class="sxs-lookup"><span data-stu-id="9efd2-135">We'll now integrate the Map support with our existing Create and Edit scenarios.</span></span> <span data-ttu-id="9efd2-136">這是很簡單的待辦事項，而我們變更控制器的程式碼的任何不需要好消息。</span><span class="sxs-lookup"><span data-stu-id="9efd2-136">The good news is that this is pretty easy to-do, and doesn't require us to change any of our Controller code.</span></span> <span data-ttu-id="9efd2-137">因為我們建立與編輯檢視共用通用"DinnerForm 」 部分檢視，以實作 dinner 表單 UI，所以我們可以將地圖加入在同一個地方，有兩種我們建立與編輯案例並用。</span><span class="sxs-lookup"><span data-stu-id="9efd2-137">Because our Create and Edit views share a common "DinnerForm" partial view to implement the dinner form UI, we can add the map in one place and have both our Create and Edit scenarios use it.</span></span>

<span data-ttu-id="9efd2-138">我們只需要待辦事項是開啟 \Views\Dinners\DinnerForm.ascx 部分檢視並更新它包含我們新的對應部分。</span><span class="sxs-lookup"><span data-stu-id="9efd2-138">All we need to-do is to open the \Views\Dinners\DinnerForm.ascx partial view and update it to include our new map partial.</span></span> <span data-ttu-id="9efd2-139">以下是更新的 DinnerForm 外觀加入地圖之後 (請注意： 為畫面簡潔起見下面程式碼片段中會省略 HTML 表單元素):</span><span class="sxs-lookup"><span data-stu-id="9efd2-139">Below is what the updated DinnerForm will look like once the map is added (note: the HTML form elements are omitted from the code snippet below for brevity):</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

<span data-ttu-id="9efd2-140">部分上方 DinnerForm 會採用"DinnerFormViewModel 」 類型的物件做為模型類型 （因為它需要的 Dinner 物件，以及填入國家/地區 dropdownlist SelectList）。</span><span class="sxs-lookup"><span data-stu-id="9efd2-140">The DinnerForm partial above takes an object of type "DinnerFormViewModel" as its model type (because it needs both a Dinner object, as well as a SelectList to populate the dropdownlist of countries).</span></span> <span data-ttu-id="9efd2-141">我們部分的對應只需要 「 Dinner 」 類型的物件做為模型類型，且因此當我們呈現地圖部分我們傳遞只會將 Dinner 子屬性的 DinnerFormViewModel 它：</span><span class="sxs-lookup"><span data-stu-id="9efd2-141">Our Map partial just needs an object of type "Dinner" as its model type, and so when we render the map partial we are passing just the Dinner sub-property of DinnerFormViewModel to it:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

<span data-ttu-id="9efd2-142">JavaScript 函式我們已加入至部分會使用 jQuery 附加至 「 位址 」 HTML 文字方塊中的 「 模糊 」 事件。</span><span class="sxs-lookup"><span data-stu-id="9efd2-142">The JavaScript function we've added to the partial uses jQuery to attach a "blur" event to the "Address" HTML textbox.</span></span> <span data-ttu-id="9efd2-143">您可能聽過的 「 焦點 」 引發事件，當使用者按一下或索引標籤的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="9efd2-143">You've probably heard of "focus" events that fire when a user clicks or tabs into a textbox.</span></span> <span data-ttu-id="9efd2-144">相反的操作是使用者結束文字方塊時所引發的 「 模糊 」 事件。</span><span class="sxs-lookup"><span data-stu-id="9efd2-144">The opposite is a "blur" event that fires when a user exits a textbox.</span></span> <span data-ttu-id="9efd2-145">當這種情況，，然後繪製我們地圖上的新位址位置時，上述事件處理常式便會清除緯度和經度的文字方塊值。</span><span class="sxs-lookup"><span data-stu-id="9efd2-145">The above event handler clears the latitude and longitude textbox values when this happens, and then plots the new address location on our map.</span></span> <span data-ttu-id="9efd2-146">我們 map.js 檔所定義的回呼事件處理常式會更新經度和緯度的文字方塊，我們使用虛擬地球根據我們會給它的位址傳回值的表單上。</span><span class="sxs-lookup"><span data-stu-id="9efd2-146">A callback event handler that we defined within the map.js file will then update the longitude and latitude textboxes on our form using values returned by virtual earth based on the address we gave it.</span></span>

<span data-ttu-id="9efd2-147">和現在當我們我們再次執行應用程式，並按一下 「 主機 Dinner 」 索引標籤，我們會看到對應的預設值連同我們標準 Dinner 表單項目一起顯示：</span><span class="sxs-lookup"><span data-stu-id="9efd2-147">And now when we run our application again and click the "Host Dinner" tab we'll see a default map displayed along with our standard Dinner form elements:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

<span data-ttu-id="9efd2-148">當我們輸入位址，並立即索引標籤時，對應會動態更新以顯示位置，和事件處理常式將會填入經緯度文字方塊的位置值：</span><span class="sxs-lookup"><span data-stu-id="9efd2-148">When we type in an address, and then tab away, the map will dynamically update to display the location, and our event handler will populate the latitude/longitude textboxes with the location values:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

<span data-ttu-id="9efd2-149">如果儲存新 dinner 後再重新開啟進行編輯，我們會發現，在載入頁面時，顯示地圖位置：</span><span class="sxs-lookup"><span data-stu-id="9efd2-149">If we save the new dinner and then open it again for editing, we'll find that the map location is displayed when the page loads:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

<span data-ttu-id="9efd2-150">每次變更 [位址] 欄位時，將會更新對應和經緯度座標。</span><span class="sxs-lookup"><span data-stu-id="9efd2-150">Every time the address field is changed, the map and the latitude/longitude coordinates will update.</span></span>

<span data-ttu-id="9efd2-151">既然地圖會顯示 Dinner 位置，我們也可以變更為緯度和經度的表單欄位修改為隱藏的項目 （因為對應自動更新每次輸入的地址） 所顯示的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="9efd2-151">Now that the map displays the Dinner location, we can also change the Latitude and Longitude form fields from being visible textboxes to instead be hidden elements (since the map is automatically updating them each time an address is entered).</span></span> <span data-ttu-id="9efd2-152">待辦事項這我們將切換使用 Html.TextBox() HTML helper 來使用 Html.Hidden() helper 方法：</span><span class="sxs-lookup"><span data-stu-id="9efd2-152">To-do this we'll switch from using the Html.TextBox() HTML helper to using the Html.Hidden() helper method:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

<span data-ttu-id="9efd2-153">和我們 form 現在有點更加易懂易記，並避免顯示未經處理的經緯度 （同時仍儲存這些項目資料庫中的每個 Dinner）：</span><span class="sxs-lookup"><span data-stu-id="9efd2-153">And now our forms are a little more user-friendly and avoid displaying the raw latitude/longitude (while still storing them with each Dinner in the database):</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a><span data-ttu-id="9efd2-154">整合地圖與詳細資料檢視</span><span class="sxs-lookup"><span data-stu-id="9efd2-154">Integrating the Map with the Details View</span></span>

<span data-ttu-id="9efd2-155">現在，我們已經對應整合與我們建立與編輯的情況下，我們也將它整合到我們的案例中的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="9efd2-155">Now that we have the map integrated with our Create and Edit scenarios, let's also integrate it with our Details scenario.</span></span> <span data-ttu-id="9efd2-156">我們只需要待辦事項就是呼叫&lt;%html.renderpartial("map"); %&gt;詳細資料檢視中。</span><span class="sxs-lookup"><span data-stu-id="9efd2-156">All we need to-do is to call &lt;% Html.RenderPartial("map"); %&gt; within the Details view.</span></span>

<span data-ttu-id="9efd2-157">以下是完整詳細資料檢視 （與對應整合） 的原始碼看起來像：</span><span class="sxs-lookup"><span data-stu-id="9efd2-157">Below is what the source code to the complete Details view (with map integration) looks like:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

<span data-ttu-id="9efd2-158">現在當使用者巡覽至 /Dinners/詳細資料 / [id] URL 它們將會看到有關 dinner，在地圖上 dinner 位置的詳細資料 （完整的圖釘，當停留顯示 dinner 的標題和它的位址），而有 RSVP fo AJAX 連結r 它：</span><span class="sxs-lookup"><span data-stu-id="9efd2-158">And now when a user navigates to a /Dinners/Details/[id] URL they'll see details about the dinner, the location of the dinner on the map (complete with a push-pin that when hovered over displays the title of the dinner and the address of it), and have an AJAX link to RSVP for it:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a><span data-ttu-id="9efd2-159">在我們的資料庫和儲存機制中實作位置搜尋</span><span class="sxs-lookup"><span data-stu-id="9efd2-159">Implementing Location Search in our Database and Repository</span></span>

<span data-ttu-id="9efd2-160">[完成] 關閉我們 AJAX 的實作，讓我們將地圖加入至應用程式，可讓使用者以圖形方式搜尋 dinners 接近它們的首頁。</span><span class="sxs-lookup"><span data-stu-id="9efd2-160">To finish off our AJAX implementation, let's add a Map to the home page of the application that allows users to graphically search for dinners near them.</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

<span data-ttu-id="9efd2-161">我們一開始會藉由實作有效率地執行 Dinners 位置為主的 radius 搜尋我們資料庫和資料儲存機制層內的支援。</span><span class="sxs-lookup"><span data-stu-id="9efd2-161">We'll begin by implementing support within our database and data repository layer to efficiently perform a location-based radius search for Dinners.</span></span> <span data-ttu-id="9efd2-162">我們可以使用新[地理空間功能的 SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx)來實作，或者我們可以使用以下文章中討論的 Gary Dryden SQL 函式方法： [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx)和 Rob Conery這裡使用 linq to SQL 相關龐大的網友： [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)</span><span class="sxs-lookup"><span data-stu-id="9efd2-162">We could use the new [geospatial features of SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) to implement this, or alternatively we can use a SQL function approach that Gary Dryden discussed in article here: [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) and Rob Conery blogged about using with LINQ to SQL here: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)</span></span>

<span data-ttu-id="9efd2-163">若要實作這項技術，我們會開啟 「 伺服器總管 」 在 Visual Studio 中選取 NerdDinner 資料庫，並以滑鼠右鍵按一下其下的 < 函數 > 子節點上，選擇建立新 「 純量值的函式 」:</span><span class="sxs-lookup"><span data-stu-id="9efd2-163">To implement this technique, we will open the "Server Explorer" within Visual Studio, select the NerdDinner database, and then right-click on the "functions" sub-node under it and choose to create a new "Scalar-valued function":</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

<span data-ttu-id="9efd2-164">然後，我們將貼上下列 DistanceBetween 函式中：</span><span class="sxs-lookup"><span data-stu-id="9efd2-164">We'll then paste in the following DistanceBetween function:</span></span>

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

<span data-ttu-id="9efd2-165">我們再將 SQL Server，我們將稱之為 「 NearestDinners 」 中建立新的資料表值函式：</span><span class="sxs-lookup"><span data-stu-id="9efd2-165">We'll then create a new table-valued function in SQL Server that we'll call "NearestDinners":</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

<span data-ttu-id="9efd2-166">此"NearestDinners 」 資料表函數會使用 DistanceBetween helper 函式來傳回所有 Dinners 內 100 英哩的緯度和經度我們提供：</span><span class="sxs-lookup"><span data-stu-id="9efd2-166">This "NearestDinners" table function uses the DistanceBetween helper function to return all Dinners within 100 miles of the latitude and longitude we supply it:</span></span>

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

<span data-ttu-id="9efd2-167">若要呼叫此函式，我們會先開啟 LINQ to SQL 設計工具按兩下 NerdDinner.dbml 檔案，我們 \Models 目錄中：</span><span class="sxs-lookup"><span data-stu-id="9efd2-167">To call this function, we'll first open up the LINQ to SQL designer by double-clicking on the NerdDinner.dbml file within our \Models directory:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

<span data-ttu-id="9efd2-168">我們會再將 NearestDinners 和 DistanceBetween 函數拖曳至 LINQ 到 SQL 設計工具，會使其以我們的 LINQ 方法加入至 SQL NerdDinnerDataContext 類別：</span><span class="sxs-lookup"><span data-stu-id="9efd2-168">We'll then drag the NearestDinners and DistanceBetween functions onto the LINQ to SQL designer, which will cause them to be added as methods on our LINQ to SQL NerdDinnerDataContext class:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

<span data-ttu-id="9efd2-169">我們可以接著公開"FindByLocation"查詢方法會使用 NearestDinner 函式傳回即將我們 DinnerRepository 類別上的指定位置的 100 英哩內 Dinners:</span><span class="sxs-lookup"><span data-stu-id="9efd2-169">We can then expose a "FindByLocation" query method on our DinnerRepository class that uses the NearestDinner function to return upcoming Dinners that are within 100 miles of the specified location:</span></span>

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a><span data-ttu-id="9efd2-170">JSON 型 AJAX 搜尋動作方法的實作</span><span class="sxs-lookup"><span data-stu-id="9efd2-170">Implementing a JSON-based AJAX Search Action Method</span></span>

<span data-ttu-id="9efd2-171">我們現在將實作控制器動作方法採用新 FindByLocation() 儲存機制的方法傳回到 Dinner 資料可以用來填入對應的清單。</span><span class="sxs-lookup"><span data-stu-id="9efd2-171">We'll now implement a controller action method that takes advantage of the new FindByLocation() repository method to return back a list of Dinner data that can be used to populate a map.</span></span> <span data-ttu-id="9efd2-172">我們必須傳回到 Dinner 資料 JSON （JavaScript 物件標記法） 格式，讓它可以輕鬆管理使用 JavaScript 用戶端上此動作方法。</span><span class="sxs-lookup"><span data-stu-id="9efd2-172">We'll have this action method return back the Dinner data in a JSON (JavaScript Object Notation) format so that it can be easily manipulated using JavaScript on the client.</span></span>

<span data-ttu-id="9efd2-173">若要實作這種情況，我們將建立新的 「 SearchController 」 類別 \Controllers 目錄上按一下滑鼠右鍵，然後選擇 [新增]-&gt;控制器功能表命令。</span><span class="sxs-lookup"><span data-stu-id="9efd2-173">To implement this, we'll create a new "SearchController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span> <span data-ttu-id="9efd2-174">然後，我們會實作下面類似新 SearchController 類別內的"SearchByLocation 」 動作方法：</span><span class="sxs-lookup"><span data-stu-id="9efd2-174">We'll then implement a "SearchByLocation" action method within the new SearchController class like below:</span></span>

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

<span data-ttu-id="9efd2-175">SearchController SearchByLocation 動作方法內部 FindByLocation 上呼叫方法來取得一份附近 dinners DinnerRespository。</span><span class="sxs-lookup"><span data-stu-id="9efd2-175">The SearchController's SearchByLocation action method internally calls the FindByLocation method on DinnerRespository to get a list of nearby dinners.</span></span> <span data-ttu-id="9efd2-176">而非直接給用戶端傳回 Dinner 物件，不過，它改為傳回 JsonDinner 物件。</span><span class="sxs-lookup"><span data-stu-id="9efd2-176">Rather than return the Dinner objects directly to the client, though, it instead returns JsonDinner objects.</span></span> <span data-ttu-id="9efd2-177">JsonDinner 類別會公開 Dinner 屬性的子集 (例如： 基於安全理由，它不會揭露的晚餐具有 rsvp 的活動的人的名稱)。</span><span class="sxs-lookup"><span data-stu-id="9efd2-177">The JsonDinner class exposes a subset of Dinner properties (for example: for security reasons it doesn't disclose the names of the people who have RSVP'd for a dinner).</span></span> <span data-ttu-id="9efd2-178">它也包含 RSVPCount Dinner – 上不存在和屬性的動態計算方式是計算特定 dinner 相關聯的 RSVP 物件數目。</span><span class="sxs-lookup"><span data-stu-id="9efd2-178">It also includes an RSVPCount property that doesn't exist on Dinner– and which is dynamically calculated by counting the number of RSVP objects associated with a particular dinner.</span></span>

<span data-ttu-id="9efd2-179">我們再返回 dinners 使用 JSON 基礎 wire 格式的順序控制站的基底類別上使用 Json() helper 方法。</span><span class="sxs-lookup"><span data-stu-id="9efd2-179">We are then using the Json() helper method on the Controller base class to return the sequence of dinners using a JSON-based wire format.</span></span> <span data-ttu-id="9efd2-180">JSON 是代表簡單的資料結構的標準文字格式。</span><span class="sxs-lookup"><span data-stu-id="9efd2-180">JSON is a standard text format for representing simple data-structures.</span></span> <span data-ttu-id="9efd2-181">以下是範例 JSON 格式的兩個 JsonDinner 物件清單看起來像我們的動作方法傳回時：</span><span class="sxs-lookup"><span data-stu-id="9efd2-181">Below is an example of what a JSON-formatted list of two JsonDinner objects looks like when returned from our action method:</span></span>

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a><span data-ttu-id="9efd2-182">呼叫使用 jQuery JSON 型 AJAX 方法</span><span class="sxs-lookup"><span data-stu-id="9efd2-182">Calling the JSON-based AJAX method using jQuery</span></span>

<span data-ttu-id="9efd2-183">我們現在已準備好更新 NerdDinner 在使用 SearchController SearchByLocation 動作方法的應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="9efd2-183">We are now ready to update the home page of the NerdDinner application to use the SearchController's SearchByLocation action method.</span></span> <span data-ttu-id="9efd2-184">待辦事項，我們將會開啟 /Views/Home/Index.aspx 檢視範本，並更新如文字方塊、 [搜尋] 按鈕、 地圖中，讓它和&lt;div&gt;名為 dinnerList 項目：</span><span class="sxs-lookup"><span data-stu-id="9efd2-184">To-do this, we'll open the /Views/Home/Index.aspx view template and update it to have a textbox, search button, our map, and a &lt;div&gt; element named dinnerList:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

<span data-ttu-id="9efd2-185">我們就可以加入至頁面的兩個 JavaScript 函式：</span><span class="sxs-lookup"><span data-stu-id="9efd2-185">We can then add two JavaScript functions to the page:</span></span>

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

<span data-ttu-id="9efd2-186">第一次載入頁面時，第一個 JavaScript 函式會載入對應。</span><span class="sxs-lookup"><span data-stu-id="9efd2-186">The first JavaScript function loads the map when the page first loads.</span></span> <span data-ttu-id="9efd2-187">JavaScript 函式第二個線路向上 JavaScript 按一下搜尋按鈕的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="9efd2-187">The second JavaScript function wires up a JavaScript click event handler on the search button.</span></span> <span data-ttu-id="9efd2-188">按下按鈕時呼叫 FindDinnersGivenLocation() JavaScript 函式，我們會將其加入我們 Map.js 檔案：</span><span class="sxs-lookup"><span data-stu-id="9efd2-188">When the button is pressed it calls the FindDinnersGivenLocation() JavaScript function which we'll add to our Map.js file:</span></span>

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

<span data-ttu-id="9efd2-189">此 FindDinnersGivenLocation() 函式呼叫的對應。Find() 虛擬地球控制項置上輸入的位置。</span><span class="sxs-lookup"><span data-stu-id="9efd2-189">This FindDinnersGivenLocation() function calls map.Find() on the Virtual Earth Control to center it on the entered location.</span></span> <span data-ttu-id="9efd2-190">當虛擬地球對應服務傳回時，對應。Find() 方法會叫用我們當做最後的引數傳遞給 callbackUpdateMapDinners 回呼方法。</span><span class="sxs-lookup"><span data-stu-id="9efd2-190">When the virtual earth map service returns, the map.Find() method invokes the callbackUpdateMapDinners callback method we passed it as the final argument.</span></span>

<span data-ttu-id="9efd2-191">這是 callbackUpdateMapDinners() 方法是實際工作。</span><span class="sxs-lookup"><span data-stu-id="9efd2-191">The callbackUpdateMapDinners() method is where the real work is done.</span></span> <span data-ttu-id="9efd2-192">它使用 jQuery 的 $.post() helper 方法以執行我們 SearchController SearchByLocation() 動作方法 – 將傳遞給它的緯度與經度新置中對齊對應的 AJAX 呼叫。</span><span class="sxs-lookup"><span data-stu-id="9efd2-192">It uses jQuery's $.post() helper method to perform an AJAX call to our SearchController's SearchByLocation() action method – passing it the latitude and longitude of the newly centered map.</span></span> <span data-ttu-id="9efd2-193">它會定義內嵌函式 $.post() helper 方法完成時，會呼叫方法以及從動作方法會將傳遞給使用名為"dinners"SearchByLocation() 傳回 JSON 格式 dinner 結果。</span><span class="sxs-lookup"><span data-stu-id="9efd2-193">It defines an inline function that will be called when the $.post() helper method completes, and the JSON-formatted dinner results returned from the SearchByLocation() action method will be passed it using a variable called "dinners".</span></span> <span data-ttu-id="9efd2-194">然後 foreach 未透過每個傳回的 dinner 並在地圖上加入新的 pin 碼中使用 dinner 緯度和經度和其他屬性。</span><span class="sxs-lookup"><span data-stu-id="9efd2-194">It then does a foreach over each returned dinner, and uses the dinner's latitude and longitude and other properties to add a new pin on the map.</span></span> <span data-ttu-id="9efd2-195">也會增加 dinner 項目右邊的對應 dinners HTML 清單。</span><span class="sxs-lookup"><span data-stu-id="9efd2-195">It also adds a dinner entry to the HTML list of dinners to the right of the map.</span></span> <span data-ttu-id="9efd2-196">它然後線路總圖釘和 HTML 清單在暫留事件使使用者停留時顯示 dinner 有關的詳細資料：</span><span class="sxs-lookup"><span data-stu-id="9efd2-196">It then wires-up a hover event for both the pushpins and the HTML list so that details about the dinner are displayed when a user hovers over them:</span></span>

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

<span data-ttu-id="9efd2-197">現在當我們執行應用程式，並瀏覽我們將會看到對應的主頁面。</span><span class="sxs-lookup"><span data-stu-id="9efd2-197">And now when we run the application and visit the home-page we'll be presented with a map.</span></span> <span data-ttu-id="9efd2-198">當我們輸入城市名稱的對應會顯示其附近即將 dinners:</span><span class="sxs-lookup"><span data-stu-id="9efd2-198">When we enter the name of a city the map will display the upcoming dinners near it:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

<span data-ttu-id="9efd2-199">停留 dinner 會顯示其相關的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="9efd2-199">Hovering over a dinner will display details about it.</span></span>

<span data-ttu-id="9efd2-200">按一下 Dinner 標題在泡泡，或在 [HTML] 清單右邊將瀏覽至我們的 dinner – 我們可以再選擇性地 RSVP 的：</span><span class="sxs-lookup"><span data-stu-id="9efd2-200">Clicking the Dinner title either in the bubble or on the right-hand side in the HTML list will navigate us to the dinner – which we can then optionally RSVP for:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a><span data-ttu-id="9efd2-201">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="9efd2-201">Next Step</span></span>

<span data-ttu-id="9efd2-202">我們現在已實作 NerdDinner 應用程式的所有應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="9efd2-202">We've now implemented all the application functionality of our NerdDinner application.</span></span> <span data-ttu-id="9efd2-203">讓我們看看我們如何啟用自動化單元現在它的測試。</span><span class="sxs-lookup"><span data-stu-id="9efd2-203">Let's now look at how we can enable automated unit testing of it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9efd2-204">[上一頁](use-ajax-to-deliver-dynamic-updates.md)
> [下一頁](enable-automated-unit-testing.md)</span><span class="sxs-lookup"><span data-stu-id="9efd2-204">[Previous](use-ajax-to-deliver-dynamic-updates.md)
[Next](enable-automated-unit-testing.md)</span></span>
