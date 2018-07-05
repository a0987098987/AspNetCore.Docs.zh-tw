---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: 伺服器控制項 |Microsoft Docs
author: microsoft
description: ASP.NET 2.0 增強伺服器控制項，在許多方面。 在本單元中，我們將討論一些方式 ASP.NET 2.0 和 Visual Studio 200 的架構變更...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 989986c45e76a65582f35172c7d837a78484d09a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374220"
---
<a name="server-controls"></a><span data-ttu-id="229f4-104">伺服器控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-104">Server Controls</span></span>
====================
<span data-ttu-id="229f4-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="229f4-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="229f4-106">ASP.NET 2.0 增強伺服器控制項，在許多方面。</span><span class="sxs-lookup"><span data-stu-id="229f4-106">ASP.NET 2.0 enhances server controls in many ways.</span></span> <span data-ttu-id="229f4-107">在這個模組中，我們將討論一些架構的變更，ASP.NET 2.0 的方式，而 Visual Studio 2005 處理的伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-107">In this module, we'll cover some of the architectural changes to the way ASP.NET 2.0 and Visual Studio 2005 deals with server controls.</span></span>


<span data-ttu-id="229f4-108">ASP.NET 2.0 增強伺服器控制項，在許多方面。</span><span class="sxs-lookup"><span data-stu-id="229f4-108">ASP.NET 2.0 enhances server controls in many ways.</span></span> <span data-ttu-id="229f4-109">在這個模組中，我們將討論一些架構的變更，ASP.NET 2.0 的方式，而 Visual Studio 2005 處理的伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-109">In this module, we'll cover some of the architectural changes to the way ASP.NET 2.0 and Visual Studio 2005 deals with server controls.</span></span>

## <a name="view-state"></a><span data-ttu-id="229f4-110">檢視狀態</span><span class="sxs-lookup"><span data-stu-id="229f4-110">View state</span></span>

<span data-ttu-id="229f4-111">在檢視狀態中的 ASP.NET 2.0 的主要變更是大小可以大幅減少。</span><span class="sxs-lookup"><span data-stu-id="229f4-111">The primary change in view state in ASP.NET 2.0 is a dramatic reduction in size.</span></span> <span data-ttu-id="229f4-112">請考慮只在其上的行事曆控制項的頁面。</span><span class="sxs-lookup"><span data-stu-id="229f4-112">Consider a page with only a Calendar control on it.</span></span> <span data-ttu-id="229f4-113">以下是在 ASP.NET 1.1 中的檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-113">Here is the view state in ASP.NET 1.1.</span></span>

[!code-css[Main](server-controls/samples/sample1.css)]

<span data-ttu-id="229f4-114">現在如下的檢視狀態上完全相同的頁面在 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="229f4-114">Now here's the view state on an identical page in ASP.NET 2.0.</span></span>

[!code-css[Main](server-controls/samples/sample2.css)]

<span data-ttu-id="229f4-115">這是重大的變更，並考慮檢視狀態來回傳送透過網路，這項變更可以讓開發人員大幅提升效能。</span><span class="sxs-lookup"><span data-stu-id="229f4-115">That's a pretty significant change, and considering that view state is carried back and forth over the wire, this change can give developers a significant performance increase.</span></span> <span data-ttu-id="229f4-116">檢視狀態大小縮減主要是因為我們在內部處理的方式。</span><span class="sxs-lookup"><span data-stu-id="229f4-116">The reduction in size of view state is largely due to the way that we handle it internally.</span></span> <span data-ttu-id="229f4-117">請記住，檢視狀態是以 Base64 編碼的字串。</span><span class="sxs-lookup"><span data-stu-id="229f4-117">Remember that view state is a Base64 encoded string.</span></span> <span data-ttu-id="229f4-118">若要進一步了解在檢視狀態中的 ASP.NET 2.0 的變更，讓我們看看從上述範例中的已解碼的值。</span><span class="sxs-lookup"><span data-stu-id="229f4-118">To better understand the change in view state in ASP.NET 2.0, let's have a look at the decoded values from the examples above.</span></span>

<span data-ttu-id="229f4-119">以下是已解碼的 1.1 版的檢視狀態：</span><span class="sxs-lookup"><span data-stu-id="229f4-119">Here is the 1.1 view state decoded:</span></span>

[!code-css[Main](server-controls/samples/sample3.css)]

<span data-ttu-id="229f4-120">這可能會看起來有點像無意義，但沒有在這裡的模式。</span><span class="sxs-lookup"><span data-stu-id="229f4-120">This may look a bit like gibberish, but there is a pattern here.</span></span> <span data-ttu-id="229f4-121">在 ASP.NET 1.x 中，我們用來識別資料類型的單一字元，並分隔使用值&lt;&gt;字元。</span><span class="sxs-lookup"><span data-stu-id="229f4-121">In ASP.NET 1.x, we used single characters to identify data-types and delimited values using the &lt;&gt; characters.</span></span> <span data-ttu-id="229f4-122">在檢視狀態上述範例中的"t"代表 Triplet。</span><span class="sxs-lookup"><span data-stu-id="229f4-122">The "t" in the view state sample above represents a Triplet.</span></span> <span data-ttu-id="229f4-123">三物件組包含一組 ArrayLists （"l"代表 ArrayList）。其中一個這些 ArrayLists 包含 Int32 ("i") 值是 1，而另一個包含另一個 Triplet。</span><span class="sxs-lookup"><span data-stu-id="229f4-123">The Triplet contains a pair of ArrayLists (the "l" represents an ArrayList.) One of those ArrayLists contains an Int32 ("i") with a value of 1 and the other contains another Triplet.</span></span> <span data-ttu-id="229f4-124">三物件組包含一組 ArrayLists 等等。要記住的重點是我們會使用包含配對的 Triplet、 我們識別的資料型別是字母，透過和我們會使用&lt;和&gt;做為分隔符號的字元。</span><span class="sxs-lookup"><span data-stu-id="229f4-124">The Triplet contains a pair of ArrayLists, etc. The important thing to remember is that we use Triplets that contain pairs, we identify the data-types via a letter, and we use the &lt; and &gt; characters as delimiters.</span></span>

<span data-ttu-id="229f4-125">在 ASP.NET 2.0 中，已解碼的檢視狀態看起來會有點不同。</span><span class="sxs-lookup"><span data-stu-id="229f4-125">In ASP.NET 2.0, the decoded view state looks a bit different.</span></span>

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

<span data-ttu-id="229f4-126">您應該會注意到顯著的不同，已解碼的檢視狀態的外觀。</span><span class="sxs-lookup"><span data-stu-id="229f4-126">You should notice a huge change in the appearance of the decoded view state.</span></span> <span data-ttu-id="229f4-127">這項變更會有數個結構性的支援。</span><span class="sxs-lookup"><span data-stu-id="229f4-127">This change has several architectural underpinnings.</span></span> <span data-ttu-id="229f4-128">檢視狀態在 ASP.NET 1.x 用於 LosFormatter 序列化資料。</span><span class="sxs-lookup"><span data-stu-id="229f4-128">View state in ASP.NET 1.x used the LosFormatter to serialize data.</span></span> <span data-ttu-id="229f4-129">在 2.0 中，我們會使用新的 ObjectStateFormatter 類別。</span><span class="sxs-lookup"><span data-stu-id="229f4-129">In 2.0, we use the new ObjectStateFormatter class.</span></span> <span data-ttu-id="229f4-130">這個類別被專為協助序列化和還原序列化檢視狀態和控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-130">This class was specifically designed to aid in the serialization and deserialization of view state and control state.</span></span> <span data-ttu-id="229f4-131">（下一節將說明控制項狀態）。有許多優點，獲得變更的序列化和還原序列化發生的方法。</span><span class="sxs-lookup"><span data-stu-id="229f4-131">(Control state will be covered in the next section.) There are many benefits gained by changing the method by which serialization and deserialization take place.</span></span> <span data-ttu-id="229f4-132">最明顯的其中一項是不使用 TextWriter LosFormatter，像 ObjectStateFormatter 使用 BinaryWriter 的事實。</span><span class="sxs-lookup"><span data-stu-id="229f4-132">One of the most dramatic is the fact that unlike the LosFormatter which uses a TextWriter, the ObjectStateFormatter uses a BinaryWriter.</span></span> <span data-ttu-id="229f4-133">這可讓 ASP.NET 2.0，來儲存檢視狀態一系列的位元組，而不是字串。</span><span class="sxs-lookup"><span data-stu-id="229f4-133">This allows ASP.NET 2.0 to store view state a series of bytes instead of strings.</span></span> <span data-ttu-id="229f4-134">例如，需要為整數。</span><span class="sxs-lookup"><span data-stu-id="229f4-134">Take, for example, an integer.</span></span> <span data-ttu-id="229f4-135">在 ASP.NET 1.1 中，整數所需的檢視狀態的 4 個位元組。</span><span class="sxs-lookup"><span data-stu-id="229f4-135">In ASP.NET 1.1, an integer required 4 bytes of view state.</span></span> <span data-ttu-id="229f4-136">在 ASP.NET 2.0 中，該相同的整數，只需要 1 個位元組。</span><span class="sxs-lookup"><span data-stu-id="229f4-136">In ASP.NET 2.0, that same integer only requires 1 byte.</span></span> <span data-ttu-id="229f4-137">其他增強功能對減少儲存的檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-137">Other enhancements were made to decrease the amount of view state that is stored.</span></span> <span data-ttu-id="229f4-138">例如，日期時間值現在儲存使用 TickCount 而非字串。</span><span class="sxs-lookup"><span data-stu-id="229f4-138">DateTime values, for example, are now stored using a TickCount instead of a string.</span></span>

<span data-ttu-id="229f4-139">如果一切都不足夠，特別注意已付費之事實的 1.x 中的檢視狀態的最大取用者的 DataGrid 和類似的控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-139">As if all of that weren't enough, special attention was paid to the fact that one of the greatest consumers of view state in 1.x was the DataGrid and similar controls.</span></span> <span data-ttu-id="229f4-140">控制項，例如 DataGrid 而言檢視狀態的主要缺點是資訊的，它通常會包含大量重複。</span><span class="sxs-lookup"><span data-stu-id="229f4-140">A major drawback of controls such as the DataGrid where view state is concerned is that it often contains large amounts of repeated information.</span></span> <span data-ttu-id="229f4-141">在 ASP.NET 1.x 中，重複資訊直接儲存和上一次而造成過大的檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-141">In ASP.NET 1.x, that repeated information was simply stored over and over again resulting in a bloated view state.</span></span> <span data-ttu-id="229f4-142">在 ASP.NET 2.0 中，我們會使用新 IndexedString 類別來儲存這類資料。</span><span class="sxs-lookup"><span data-stu-id="229f4-142">In ASP.NET 2.0, we use the new IndexedString class to store such data.</span></span> <span data-ttu-id="229f4-143">若重複出現的字串，我們只會儲存權杖 IndexedString 和執行 IndexedString 物件的表格中的索引。</span><span class="sxs-lookup"><span data-stu-id="229f4-143">If a string repeats, we just store the token for the IndexedString and the index within a running table of IndexedString objects.</span></span>

## <a name="control-state"></a><span data-ttu-id="229f4-144">控制項狀態</span><span class="sxs-lookup"><span data-stu-id="229f4-144">Control State</span></span>

<span data-ttu-id="229f4-145">其中一個主要開發人員必須與檢視狀態的牢騷就是它新增至 HTTP 承載的大小。</span><span class="sxs-lookup"><span data-stu-id="229f4-145">One of the major gripes that developers had with view state was the size that it added to the HTTP payload.</span></span> <span data-ttu-id="229f4-146">如先前所述，檢視狀態的最大取用者會在 DataGrid 控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-146">As previously mentioned, one of the greatest consumers of view state is the DataGrid control.</span></span> <span data-ttu-id="229f4-147">若要避免大量資料格所產生的檢視狀態，許多開發人員只要停用該控制項的檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-147">To avoid the huge amounts of view state generated by a DataGrid, many developers simply disabled view state for that control.</span></span> <span data-ttu-id="229f4-148">不幸的是，該方案不一定是一個很好。</span><span class="sxs-lookup"><span data-stu-id="229f4-148">Unfortunately, that solution wasn't always a good one.</span></span> <span data-ttu-id="229f4-149">檢視狀態在 ASP.NET 1.x 不只包含資料之控制項的正確功能所需。</span><span class="sxs-lookup"><span data-stu-id="229f4-149">View state in ASP.NET 1.x contains not only data necessary for the correct functionality of the control.</span></span> <span data-ttu-id="229f4-150">它也包含有關控制項的 UI 狀態的資訊。</span><span class="sxs-lookup"><span data-stu-id="229f4-150">It also contains information concerning the state of the control's UI.</span></span> <span data-ttu-id="229f4-151">這表示，如果您想要允許分頁上的資料格，您必須先啟用檢視狀態，即使您不需要的所有檢視的 UI 資訊狀態就會包含。</span><span class="sxs-lookup"><span data-stu-id="229f4-151">This means that if you want to allow for pagination on a DataGrid, you must enable view state even if you don't need all of the UI information that view state contains.</span></span> <span data-ttu-id="229f4-152">這是孤注一擲的案例。</span><span class="sxs-lookup"><span data-stu-id="229f4-152">It's an all-or-nothing scenario.</span></span>

<span data-ttu-id="229f4-153">在 ASP.NET 2.0 中，控制項狀態會解決該問題，妥善透過控制項狀態的簡介。</span><span class="sxs-lookup"><span data-stu-id="229f4-153">In ASP.NET 2.0, control state solves that problem nicely via the introduction of control state.</span></span> <span data-ttu-id="229f4-154">控制項狀態會包含控制項的適當功能絕對必要的資料。</span><span class="sxs-lookup"><span data-stu-id="229f4-154">Control state contains the data that is absolutely necessary for the proper functionality of a control.</span></span> <span data-ttu-id="229f4-155">不同的檢視狀態，無法停用控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-155">Unlike view state, control state cannot be disabled.</span></span> <span data-ttu-id="229f4-156">因此，務必小心控制 控制項狀態中所儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="229f4-156">Therefore, it is important that the data being stored in control state is carefully controlled.</span></span>

> [!NOTE]
> <span data-ttu-id="229f4-157">中的檢視狀態以及保存控制項狀態\_ \_VIEWSTATE 的隱藏的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="229f4-157">Control state is persisted along with the view state in the \_\_VIEWSTATE hidden form field.</span></span>


<span data-ttu-id="229f4-158">這段影片會逐步解說中的檢視狀態和控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-158">This video is a walkthrough of view state and control state.</span></span>


![](server-controls/_static/image1.png)


[<span data-ttu-id="229f4-159">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="229f4-159">Open Full-Screen Video</span></span>](server-controls/_static/state1.wmv)


<span data-ttu-id="229f4-160">為了讓伺服器控制項，以讀取和寫入至控制狀態，您必須採取三個步驟。</span><span class="sxs-lookup"><span data-stu-id="229f4-160">In order for a server control to read and write to control state, you must take three steps.</span></span>

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a><span data-ttu-id="229f4-161">步驟 1： 呼叫 RegisterRequiresControlState 方法</span><span class="sxs-lookup"><span data-stu-id="229f4-161">Step 1: Call the RegisterRequiresControlState Method</span></span>

<span data-ttu-id="229f4-162">RegisterRequiresControlState 方法會通知 ASP.NET 控制項必須保存控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-162">The RegisterRequiresControlState method informs ASP.NET that a control needs to persist control state.</span></span> <span data-ttu-id="229f4-163">它會採用一個引數的型別即所註冊之控制項的控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-163">It takes one argument of type Control which is the control that is being registered.</span></span>

<span data-ttu-id="229f4-164">請務必注意註冊不會保存在要求。</span><span class="sxs-lookup"><span data-stu-id="229f4-164">It is important to note that registration does not persist from request to request.</span></span> <span data-ttu-id="229f4-165">因此，這個方法必須呼叫每個要求的控制項是否保存控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-165">Therefore, this method must be called on every request if a control is to persist control state.</span></span> <span data-ttu-id="229f4-166">建議在 OnInit 會呼叫此方法。</span><span class="sxs-lookup"><span data-stu-id="229f4-166">It is recommended that the method be called in OnInit.</span></span>

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a><span data-ttu-id="229f4-167">步驟 2： 覆寫 SaveControlState</span><span class="sxs-lookup"><span data-stu-id="229f4-167">Step 2: Override SaveControlState</span></span>

<span data-ttu-id="229f4-168">SaveControlState 方法儲存自上次的回傳控制項的控制項狀態變更。</span><span class="sxs-lookup"><span data-stu-id="229f4-168">The SaveControlState method saves control state changes for a control since the last post back.</span></span> <span data-ttu-id="229f4-169">它會傳回物件，代表控制項的狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-169">It returns an object representing the control's state.</span></span>

## <a name="step-3-override-loadcontrolstate"></a><span data-ttu-id="229f4-170">步驟 3： 覆寫 LoadControlState</span><span class="sxs-lookup"><span data-stu-id="229f4-170">Step 3: Override LoadControlState</span></span>

<span data-ttu-id="229f4-171">LoadControlState 方法會載入控制項中的已儲存的狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-171">The LoadControlState method loads saved state into a control.</span></span> <span data-ttu-id="229f4-172">此方法會採用一個引數的型別物件，其中包含控制項的已儲存的狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-172">The method takes one argument of type Object which holds the saved state for the control.</span></span>

## <a name="full-xhtml-compliance"></a><span data-ttu-id="229f4-173">完全符合 XHTML 的規範</span><span class="sxs-lookup"><span data-stu-id="229f4-173">Full XHTML Compliance</span></span>

<span data-ttu-id="229f4-174">任何 Web 開發人員知道 Web 應用程式中的標準的重要性。</span><span class="sxs-lookup"><span data-stu-id="229f4-174">Any Web developer knows the importance of standards in Web applications.</span></span> <span data-ttu-id="229f4-175">若要維護標準為基礎的開發環境，ASP.NET 2.0 是完全符合 XHTML。</span><span class="sxs-lookup"><span data-stu-id="229f4-175">In order to maintain a standards-based development environment, ASP.NET 2.0 is fully XHTML compliant.</span></span> <span data-ttu-id="229f4-176">因此，所有標記都都呈現根據符合 XHTML 標準，在瀏覽器支援 HTML 4.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="229f4-176">Therefore, all tags are rendered according to XHTML standards in browsers that support HTML 4.0 or greater.</span></span>

<span data-ttu-id="229f4-177">在 ASP.NET 1.1 中的文件類型定義如下所示：</span><span class="sxs-lookup"><span data-stu-id="229f4-177">The DOCTYPE definition in ASP.NET 1.1 was as follows:</span></span>

[!code-html[Main](server-controls/samples/sample6.html)]

<span data-ttu-id="229f4-178">在 ASP.NET 2.0 中，預設文件類型定義如下所示：</span><span class="sxs-lookup"><span data-stu-id="229f4-178">In ASP.NET 2.0, the default DOCTYPE definition is as follows:</span></span>

[!code-html[Main](server-controls/samples/sample7.html)]

<span data-ttu-id="229f4-179">如果您選擇，您可以改變預設 XHML 合規性，透過組態檔中的 [xhtmlConformance] 節點。</span><span class="sxs-lookup"><span data-stu-id="229f4-179">If you choose, you can alter the default XHML compliance via the xhtmlConformance node in the configuration file.</span></span> <span data-ttu-id="229f4-180">比方說，在 web.config 檔案中的下列節點會將符合 XHTML 的規範變更為 XHTML 1.0 Strict:</span><span class="sxs-lookup"><span data-stu-id="229f4-180">For example, the following node in the web.config file will change XHTML compliance to XHTML 1.0 Strict:</span></span>

[!code-xml[Main](server-controls/samples/sample8.xml)]

<span data-ttu-id="229f4-181">如果您選擇，您也可以設定要使用舊版 ASP.NET 中使用的設定的 ASP.NET 1.x，如下所示：</span><span class="sxs-lookup"><span data-stu-id="229f4-181">If you choose, you can also configure ASP.NET to use the legacy configuration used in ASP.NET 1.x as follows:</span></span>

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a><span data-ttu-id="229f4-182">自適性轉譯時使用配接器</span><span class="sxs-lookup"><span data-stu-id="229f4-182">Adaptive Rendering Using Adapters</span></span>

<span data-ttu-id="229f4-183">在 ASP.NET 1.x 中，所包含的組態檔案&lt;browserCaps&gt;填入 HttpBrowserCapabilities 物件的區段。</span><span class="sxs-lookup"><span data-stu-id="229f4-183">In ASP.NET 1.x, the configuration file contained a &lt;browserCaps&gt; section that populated a HttpBrowserCapabilities object.</span></span> <span data-ttu-id="229f4-184">這個物件允許開發人員判斷哪些裝置提出特定要求，並適當地轉譯程式碼。</span><span class="sxs-lookup"><span data-stu-id="229f4-184">This object allowed a developer to determine what device is making a particular request and render code appropriately.</span></span> <span data-ttu-id="229f4-185">在 ASP.NET 2.0 中，模型已經改良，現在會使用新的 ControlAdapter 類別</span><span class="sxs-lookup"><span data-stu-id="229f4-185">In ASP.NET 2.0, the model has improved and now uses the new ControlAdapter class.</span></span> <span data-ttu-id="229f4-186">ControlAdapter 類別會覆寫控制項的生命週期中的事件，並控制使用者代理程式的功能為基礎的控制項的呈現。</span><span class="sxs-lookup"><span data-stu-id="229f4-186">The ControlAdapter class overrides events in the control's lifecycle and controls the rendering of controls based upon the user agent's capabilities.</span></span> <span data-ttu-id="229f4-187">儲存在 c:\windows\microsoft.net\framework\v2.0 瀏覽器定義檔 （.browser 檔案副檔名的檔案） 會定義特定的使用者代理程式的功能。\* \* \* \*\CONFIG\Browsers 資料夾。</span><span class="sxs-lookup"><span data-stu-id="229f4-187">The capabilities of a specific user agent are defined by a browser definition file (a file with a .browser file extension) stored in the c:\windows\microsoft.net\framework\v2.0.\*\*\*\*\CONFIG\Browsers folder.</span></span>

> [!NOTE]
> <span data-ttu-id="229f4-188">ControlAdapter 類別是抽象類別。</span><span class="sxs-lookup"><span data-stu-id="229f4-188">The ControlAdapter class is an abstract class.</span></span>


<span data-ttu-id="229f4-189">就像是&lt;browserCaps&gt; 1.x 中，瀏覽器定義檔中的一節會使用規則運算式剖析使用者代理字串來識別要求的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="229f4-189">Much like the &lt;browserCaps&gt; section in 1.x, the browser definition file uses a Regular Expression to parse the user agent string in order to identify the requesting browser.</span></span> <span data-ttu-id="229f4-190">它它們定義該使用者代理程式的特定功能。</span><span class="sxs-lookup"><span data-stu-id="229f4-190">It them defines particular capabilities for that user agent.</span></span> <span data-ttu-id="229f4-191">ControlAdapter 呈現 Render 方法透過控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-191">The ControlAdapter renders the control via the Render method.</span></span> <span data-ttu-id="229f4-192">因此，如果您覆寫 Render 方法，您不應該呼叫轉譯的基底類別。</span><span class="sxs-lookup"><span data-stu-id="229f4-192">Therefore, if you override the Render method, you should not call Render on the base class.</span></span> <span data-ttu-id="229f4-193">如此一來，可能會造成轉譯發生兩次，一次的配接器，一次是控制項本身。</span><span class="sxs-lookup"><span data-stu-id="229f4-193">Doing so may cause rendering to occur twice, once for the adapter and once for the control itself.</span></span>

## <a name="developing-a-custom-adapter"></a><span data-ttu-id="229f4-194">開發自訂配接器</span><span class="sxs-lookup"><span data-stu-id="229f4-194">Developing a Custom Adapter</span></span>

<span data-ttu-id="229f4-195">您可以藉由繼承自 ControlAdapter 開發您自己自訂的配接器。</span><span class="sxs-lookup"><span data-stu-id="229f4-195">You can develop your own custom adapter by inheriting from ControlAdapter.</span></span> <span data-ttu-id="229f4-196">此外，您可以繼承自抽象類別 PageAdapter 萬一頁面需要配接器的位置。</span><span class="sxs-lookup"><span data-stu-id="229f4-196">Additionally, you can inherit from the abstract class PageAdapter in cases where an adapter is needed for a page.</span></span> <span data-ttu-id="229f4-197">透過即可完成對應的自訂配接器的控制項&lt;的 controlAdapters&gt;瀏覽器定義檔案中的項目。</span><span class="sxs-lookup"><span data-stu-id="229f4-197">Mapping of controls to your custom adapter is accomplished via the &lt;controlAdapters&gt; element in the browser definition file.</span></span> <span data-ttu-id="229f4-198">例如，下列 XML 程式碼從瀏覽器定義檔案對應功能表控制項 MenuAdapter 類別：</span><span class="sxs-lookup"><span data-stu-id="229f4-198">For example, the following XML from a browser definition file maps the Menu control to the MenuAdapter class:</span></span>

[!code-html[Main](server-controls/samples/sample10.html)]

<span data-ttu-id="229f4-199">使用此模型時，會變得很容易就可以針對特定裝置或瀏覽器控制項開發人員。</span><span class="sxs-lookup"><span data-stu-id="229f4-199">Using this model, it becomes quite easy for a control developer to target a particular device or browser.</span></span> <span data-ttu-id="229f4-200">它也是開發人員能夠完全控制每個裝置上呈現頁面的方式相當簡單。</span><span class="sxs-lookup"><span data-stu-id="229f4-200">It's also quite simple for a developer to have complete control over how pages render on every device.</span></span>

## <a name="per-device-rendering"></a><span data-ttu-id="229f4-201">每一裝置轉譯</span><span class="sxs-lookup"><span data-stu-id="229f4-201">Per-Device Rendering</span></span>

<span data-ttu-id="229f4-202">在 ASP.NET 2.0 中的伺服器控制項屬性可以指定每個裝置使用的瀏覽器特定前置詞。</span><span class="sxs-lookup"><span data-stu-id="229f4-202">Server control properties in ASP.NET 2.0 can be specified per-device using a browser-specific prefix.</span></span> <span data-ttu-id="229f4-203">例如，下列程式碼會變更標籤，視哪一種裝置用來瀏覽網頁的文字。</span><span class="sxs-lookup"><span data-stu-id="229f4-203">For example, the code below will change the Text of a label depending upon which device is being used to browse the page.</span></span>

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

<span data-ttu-id="229f4-204">當從 Internet Explorer，瀏覽包含此標籤的頁面時，標籤會顯示文字指出 「 您正在瀏覽從 Internet Explorer。 」</span><span class="sxs-lookup"><span data-stu-id="229f4-204">When the page containing this label is browsed from Internet Explorer, the label will display text saying "You are browsing from Internet Explorer."</span></span> <span data-ttu-id="229f4-205">當從 Firefox 瀏覽網頁時，標籤會顯示文字 「 瀏覽從 Firefox。 」</span><span class="sxs-lookup"><span data-stu-id="229f4-205">When the page is browsed from Firefox, the label will display the text "You are browsing from Firefox."</span></span> <span data-ttu-id="229f4-206">當從任何其他裝置，瀏覽網頁時，它會顯示 「 您正在瀏覽從未知裝置。 」</span><span class="sxs-lookup"><span data-stu-id="229f4-206">When the page is browsed from any other device, it will display "You are browsing from an unknown device."</span></span> <span data-ttu-id="229f4-207">可以使用這個特殊的語法來指定任何屬性。</span><span class="sxs-lookup"><span data-stu-id="229f4-207">Any property can be specified using this special syntax.</span></span>

## <a name="setting-focus"></a><span data-ttu-id="229f4-208">設定焦點</span><span class="sxs-lookup"><span data-stu-id="229f4-208">Setting Focus</span></span>

<span data-ttu-id="229f4-209">有關如何在特定的控制項上設定初始焦點的 ASP.NET 1.x 開發人員常見問題集。</span><span class="sxs-lookup"><span data-stu-id="229f4-209">ASP.NET 1.x developers frequently asked about how to set initial focus on a particular control.</span></span> <span data-ttu-id="229f4-210">比方說，在登入頁面上，最好將 [使用者識別碼] 文字方塊中第一次載入頁面時，取得焦點。</span><span class="sxs-lookup"><span data-stu-id="229f4-210">For example, on a login page, it's useful to have the User ID textbox get the focus when the page first loads.</span></span> <span data-ttu-id="229f4-211">在 ASP.NET 1.x 中，執行此動作會需要撰寫一些用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="229f4-211">In ASP.NET 1.x, doing this required writing some client-side script.</span></span> <span data-ttu-id="229f4-212">即使這類指令碼是簡單的工作，就不會再感謝 SetFocus 方法必須在 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="229f4-212">Even though such a script is a trivial task, it's no longer necessary in ASP.NET 2.0 thanks to the SetFocus method.</span></span> <span data-ttu-id="229f4-213">SetFocus 方法會採用一個引數指出應該接收焦點的控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-213">The SetFocus method takes one argument indicating the control that should receive focus.</span></span> <span data-ttu-id="229f4-214">這個引數可以是用戶端控制項的 ID 做為字串或伺服器控制項，以控制物件的名稱。</span><span class="sxs-lookup"><span data-stu-id="229f4-214">This argument can either be the client ID of the control as a string or the name of the Server control as a Control object.</span></span> <span data-ttu-id="229f4-215">比方說，若要設定初始焦點至第一次載入頁面時，呼叫 txtUserID 文字方塊控制項，將下列程式碼新增至頁面\_負載：</span><span class="sxs-lookup"><span data-stu-id="229f4-215">For example, to set the initial focus to a TextBox control called txtUserID when the page first loads, add the following code to Page\_Load:</span></span>

[!code-csharp[Main](server-controls/samples/sample12.cs)]

<span data-ttu-id="229f4-216">-或</span><span class="sxs-lookup"><span data-stu-id="229f4-216">-- or</span></span>

[!code-csharp[Main](server-controls/samples/sample13.cs)]

<span data-ttu-id="229f4-217">ASP.NET 2.0 使用 Webresource.axd 處理常式 （先前所述），來呈現設定焦點的用戶端功能。</span><span class="sxs-lookup"><span data-stu-id="229f4-217">ASP.NET 2.0 uses the Webresource.axd handler (discussed previously) to render a client-side function that sets the focus.</span></span> <span data-ttu-id="229f4-218">用戶端函式的名稱是 WebForm\_AutoFocus 如下所示：</span><span class="sxs-lookup"><span data-stu-id="229f4-218">The name of the client-side function is WebForm\_AutoFocus as shown here:</span></span>

[!code-html[Main](server-controls/samples/sample14.html)]

<span data-ttu-id="229f4-219">或者，您可以使用控制項的 Focus 方法，若要設定初始焦點的控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-219">Alternatively, you can use the Focus method for a control to set the initial focus to that control.</span></span> <span data-ttu-id="229f4-220">焦點方法衍生自 Control 類別，並可用於所有的 ASP.NET 2.0 控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-220">The Focus method derives from the Control class and is available to all ASP.NET 2.0 controls.</span></span> <span data-ttu-id="229f4-221">它也可將焦點設定至特定控制項發生驗證錯誤時。</span><span class="sxs-lookup"><span data-stu-id="229f4-221">It is also possible to set focus to a particular control when a validation error occurs.</span></span> <span data-ttu-id="229f4-222">將會在更新版本的模組說明。</span><span class="sxs-lookup"><span data-stu-id="229f4-222">That will be covered in a later module.</span></span>

## <a name="new-server-controls-in-aspnet-20"></a><span data-ttu-id="229f4-223">ASP.NET 2.0 中的新伺服器控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-223">New Server Controls in ASP.NET 2.0</span></span>

<span data-ttu-id="229f4-224">以下是在 ASP.NET 2.0 的新伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-224">The following are new server controls in ASP.NET 2.0.</span></span> <span data-ttu-id="229f4-225">我們將會進入更多詳細資料，在其中更新版本的模組中一些。</span><span class="sxs-lookup"><span data-stu-id="229f4-225">We will go into more detail on some of them in later modules.</span></span>

## <a name="imagemap-control"></a><span data-ttu-id="229f4-226">ImageMap 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-226">ImageMap Control</span></span>

<span data-ttu-id="229f4-227">ImageMap 控制項可讓您將加入的映像，可初始化回傳，或瀏覽至 URL 中的熱點。</span><span class="sxs-lookup"><span data-stu-id="229f4-227">The ImageMap control allows you to add hotspots to an image that can initiate a post back or navigate to a URL.</span></span> <span data-ttu-id="229f4-228">有三種類型的作用點;CircleHotSpot、 RectangleHotSpot 和 PolygonHotSpot。</span><span class="sxs-lookup"><span data-stu-id="229f4-228">There are three types of hotspots available; CircleHotSpot, RectangleHotSpot, and PolygonHotSpot.</span></span> <span data-ttu-id="229f4-229">透過 Visual Studio 中，或以程式設計方式在程式碼中的集合編輯器新增作用點。</span><span class="sxs-lookup"><span data-stu-id="229f4-229">Hotspots are added via a collection editor in Visual Studio or programmatically in code.</span></span> <span data-ttu-id="229f4-230">不沒有可用於繪製的映像上的作用點的任何使用者介面。</span><span class="sxs-lookup"><span data-stu-id="229f4-230">There is no user-interface available for drawing hotspots on an image.</span></span> <span data-ttu-id="229f4-231">必須以宣告方式指定的座標和大小或作用點的半徑。</span><span class="sxs-lookup"><span data-stu-id="229f4-231">The coordinates and size or radius of the hotspot must be specified declaratively.</span></span> <span data-ttu-id="229f4-232">另外還有一個作用區設計工具中沒有視覺表示法。</span><span class="sxs-lookup"><span data-stu-id="229f4-232">There is also no visual representation of a hotspot in the designer.</span></span> <span data-ttu-id="229f4-233">如果作用點設定為巡覽至 URL，透過 NavigateUrl 屬性的作用被指定的 URL。</span><span class="sxs-lookup"><span data-stu-id="229f4-233">If a hotspot is configured to navigate to a URL, the URL is specified via the NavigateUrl property of the hotspot.</span></span> <span data-ttu-id="229f4-234">如果是 post 回熱點，PostBackValue 屬性可讓您在伺服器端程式碼中傳遞回文章中可擷取的字串。</span><span class="sxs-lookup"><span data-stu-id="229f4-234">In the case of a post back hotspot, the PostBackValue property allows you to pass a string in the post back that can be retrieved in server-side code.</span></span>


![在 Visual Studio 中的作用點集合編輯器](server-controls/_static/image1.jpg)

<span data-ttu-id="229f4-236">**圖 1**： 在 Visual Studio 中的作用點集合編輯器</span><span class="sxs-lookup"><span data-stu-id="229f4-236">**Figure 1**: HotSpot Collection Editor in Visual Studio</span></span>


## <a name="bulletedlist-control"></a><span data-ttu-id="229f4-237">BulletedList 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-237">BulletedList Control</span></span>

<span data-ttu-id="229f4-238">BulletedList 控制項是可以輕鬆地進行資料繫結項目符號清單。</span><span class="sxs-lookup"><span data-stu-id="229f4-238">The BulletedList control is a bulleted list that can easily be data bound.</span></span> <span data-ttu-id="229f4-239">（編號的） 排序清單或未按順序透過 BulletStyle 屬性。</span><span class="sxs-lookup"><span data-stu-id="229f4-239">The list can be ordered (numbered) or unordered via the BulletStyle property.</span></span> <span data-ttu-id="229f4-240">在清單中的每個項目被以清單項目物件。</span><span class="sxs-lookup"><span data-stu-id="229f4-240">Each item in the list is represented by a ListItem object.</span></span>


![Visual Studio 中的 BulletedList 控制項](server-controls/_static/image1.gif)

<span data-ttu-id="229f4-242">**圖 2**: BulletedList Visual Studio 中的控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-242">**Figure 2**: BulletedList Control in Visual Studio</span></span>


## <a name="hiddenfield-control"></a><span data-ttu-id="229f4-243">Hiddenfield</span><span class="sxs-lookup"><span data-stu-id="229f4-243">HiddenField Control</span></span>

<span data-ttu-id="229f4-244">HiddenField 控制隱藏的表單將欄位加入至您的頁面上，其值可用於伺服器端程式碼。</span><span class="sxs-lookup"><span data-stu-id="229f4-244">The HiddenField control adds a hidden form field to your page, the value of which is available in server-side code.</span></span> <span data-ttu-id="229f4-245">隱藏的表單欄位的值通常預期回傳之間保持不變。</span><span class="sxs-lookup"><span data-stu-id="229f4-245">The value of a hidden form field is generally expected to remain unchanged between post backs.</span></span> <span data-ttu-id="229f4-246">但是，它可能是惡意使用者變更值之前，要回傳。</span><span class="sxs-lookup"><span data-stu-id="229f4-246">However, it is possible for a malicious user to change the value prior to post back.</span></span> <span data-ttu-id="229f4-247">如果發生這種情況，HiddenField 控制項就會引發 ValueChanged 事件。</span><span class="sxs-lookup"><span data-stu-id="229f4-247">If this happens, the HiddenField control will raise the ValueChanged event.</span></span> <span data-ttu-id="229f4-248">如果您有 HiddenField 控制項中的機密資訊，您想要確保它會維持不變，您應該處理 ValueChanged 事件，在程式碼中。</span><span class="sxs-lookup"><span data-stu-id="229f4-248">If you have sensitive information in the HiddenField control and you want to ensure that it remains unchanged, you should handle the ValueChanged event in your code.</span></span>

## <a name="fileupload-control"></a><span data-ttu-id="229f4-249">FileUpload 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-249">FileUpload Control</span></span>

<span data-ttu-id="229f4-250">在 ASP.NET 2.0 FileUpload 控制項可讓您能夠將檔案上傳至 Web 伺服器透過 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="229f4-250">The FileUpload control in ASP.NET 2.0 makes it possible to upload files to a Web server via an ASP.NET page.</span></span> <span data-ttu-id="229f4-251">此控制項是相當類似於 ASP.NET 1.x HtmlInputFile 類別有一些例外狀況。</span><span class="sxs-lookup"><span data-stu-id="229f4-251">This control is quite similar to the ASP.NET 1.x HtmlInputFile class with a few exceptions.</span></span> <span data-ttu-id="229f4-252">在 ASP.NET 1.x 中，會建議 null 檢查 PostedFile 屬性，以判斷您是否有正確的檔案。</span><span class="sxs-lookup"><span data-stu-id="229f4-252">In ASP.NET 1.x, it was recommended that the PostedFile property be checked for null in order to determine if you had a good file.</span></span> <span data-ttu-id="229f4-253">FileUpload 控制項在 ASP.NET 2.0 中的，將新的 HasFile 屬性可用於相同的目的，並會更有效率。</span><span class="sxs-lookup"><span data-stu-id="229f4-253">The FileUpload control in ASP.NET 2.0 adds a new HasFile property that you can use for the same purpose and it's a bit more efficient.</span></span>

<span data-ttu-id="229f4-254">PostedFile 屬性仍可供存取 HttpPostedFile 物件，但某些 HttpPostedFile 功能現已本質上與 FileUpload 控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-254">The PostedFile property is still available for access to an HttpPostedFile object, but some of the functionality of the HttpPostedFile is now available intrinsically with the FileUpload control.</span></span> <span data-ttu-id="229f4-255">例如，若要將上傳的檔案儲存在 ASP.NET 1.x 中，您另存新檔上呼叫方法 HttpPostedFile 物件。</span><span class="sxs-lookup"><span data-stu-id="229f4-255">For example, to save an uploaded file in ASP.NET 1.x, you call the SaveAs method on the HttpPostedFile object.</span></span> <span data-ttu-id="229f4-256">使用 FileUpload 控制項在 ASP.NET 2.0 中，您另存新檔上呼叫方法一樣 FileUpload 控制項本身。</span><span class="sxs-lookup"><span data-stu-id="229f4-256">Using the FileUpload control in ASP.NET 2.0, you would call the SaveAs method on the FileUpload control itself.</span></span>

<span data-ttu-id="229f4-257">2.0 的行為 （與可能是最重要的變更） 中的另一個重大變更是，它不再需要載入記憶體中的整個上傳的檔案，然後再將它儲存。</span><span class="sxs-lookup"><span data-stu-id="229f4-257">Another significant change in the 2.0 behavior (and likely the most significant change) is that it is no longer necessary to load an entire uploaded file into memory before saving it.</span></span> <span data-ttu-id="229f4-258">在 1.x 中，已上傳任何檔案會儲存完全讀入記憶體之前寫入至磁碟。</span><span class="sxs-lookup"><span data-stu-id="229f4-258">In 1.x, any file that was uploaded is saved entirely into memory prior to being written to disk.</span></span> <span data-ttu-id="229f4-259">此架構可避免大型檔案上的傳。</span><span class="sxs-lookup"><span data-stu-id="229f4-259">This architecture prevents the upload of large files.</span></span>

<span data-ttu-id="229f4-260">在 ASP.NET 2.0 中，httpRuntime 元素 requestLengthDiskThreshold 屬性可讓您設定多少 Kb 會保留在記憶體中之前寫入緩衝區中到磁碟。</span><span class="sxs-lookup"><span data-stu-id="229f4-260">In ASP.NET 2.0, the requestLengthDiskThreshold attribute of the httpRuntime element allows you to configure how many Kilobytes are held in a buffer in memory prior to being written to disk.</span></span>

<span data-ttu-id="229f4-261">**重要**: MSDN 文件 （及其他位置的文件） 指定這個值是以位元組為單位 （而不是千位元組），且預設值是 256。</span><span class="sxs-lookup"><span data-stu-id="229f4-261">**IMPORTANT**: MSDN documentation (and documentation elsewhere) specifies that this value is in bytes (not Kilobytes) and that the default is 256.</span></span> <span data-ttu-id="229f4-262">值實際上以 kb 為單位指定，預設值為 80。</span><span class="sxs-lookup"><span data-stu-id="229f4-262">The value is actually specified in Kilobytes and the default value is 80.</span></span> <span data-ttu-id="229f4-263">擁有預設值是 80 K，我們會確保大型物件堆積不未抵達緩衝區。</span><span class="sxs-lookup"><span data-stu-id="229f4-263">By having a default value of 80K, we ensure that the buffer does not end up on the large object heap.</span></span>

## <a name="wizard-control"></a><span data-ttu-id="229f4-264">精靈控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-264">Wizard Control</span></span>

<span data-ttu-id="229f4-265">它會致力於嘗試收集一系列中的資訊的 「 頁面 」 使用面板，或藉由傳送 頁面的 ASP.NET 開發人員相當常見。</span><span class="sxs-lookup"><span data-stu-id="229f4-265">It is fairly common to encounter ASP.NET developers struggling with attempting to gather information in a series of "pages" using panels or by transferring from page to page.</span></span> <span data-ttu-id="229f4-266">多半的努力是令人沮喪，相當耗時。</span><span class="sxs-lookup"><span data-stu-id="229f4-266">More often than not, the endeavor is a frustrating one and is time consuming.</span></span> <span data-ttu-id="229f4-267">新的精靈控制項讓使用者熟悉的精靈介面中的線性和非線性步驟解決的問題。</span><span class="sxs-lookup"><span data-stu-id="229f4-267">The new Wizard control solves the problems by allowing for linear and non-linear steps in a wizard interface that users are familiar with.</span></span> <span data-ttu-id="229f4-268">精靈控制項提供輸入的表單中一系列的步驟。</span><span class="sxs-lookup"><span data-stu-id="229f4-268">The Wizard control presents input forms in a series of steps.</span></span> <span data-ttu-id="229f4-269">每個步驟是控制項的特定類型; vlastnost StepType 屬性所指定。</span><span class="sxs-lookup"><span data-stu-id="229f4-269">Each step is of a particular type specified by the StepType property of the control.</span></span> <span data-ttu-id="229f4-270">可用的步驟類型如下所示：</span><span class="sxs-lookup"><span data-stu-id="229f4-270">The available step types are as follows:</span></span>

| <span data-ttu-id="229f4-271">**步驟類型**</span><span class="sxs-lookup"><span data-stu-id="229f4-271">**Step Type**</span></span> | <span data-ttu-id="229f4-272">**說明**</span><span class="sxs-lookup"><span data-stu-id="229f4-272">**Explanation**</span></span> |
| --- | --- |
| <span data-ttu-id="229f4-273">自動</span><span class="sxs-lookup"><span data-stu-id="229f4-273">Auto</span></span> | <span data-ttu-id="229f4-274">精靈會自動決定根據其位置步驟階層中的步驟的類型。</span><span class="sxs-lookup"><span data-stu-id="229f4-274">The wizard automatically determines the type of step based upon its position within the step hierarchy.</span></span> |
| <span data-ttu-id="229f4-275">啟動</span><span class="sxs-lookup"><span data-stu-id="229f4-275">Start</span></span> | <span data-ttu-id="229f4-276">第一個步驟，通常用來呈現簡介的陳述式。</span><span class="sxs-lookup"><span data-stu-id="229f4-276">The first step, often used to present an introductory statement.</span></span> |
| <span data-ttu-id="229f4-277">步驟</span><span class="sxs-lookup"><span data-stu-id="229f4-277">Step</span></span> | <span data-ttu-id="229f4-278">一般的步驟。</span><span class="sxs-lookup"><span data-stu-id="229f4-278">A normal step.</span></span> |
| <span data-ttu-id="229f4-279">完成</span><span class="sxs-lookup"><span data-stu-id="229f4-279">Finish</span></span> | <span data-ttu-id="229f4-280">最後一個步驟，通常用來呈現按鈕以完成精靈。</span><span class="sxs-lookup"><span data-stu-id="229f4-280">The final step, usually used to present a button to finish the wizard.</span></span> |
| <span data-ttu-id="229f4-281">完成</span><span class="sxs-lookup"><span data-stu-id="229f4-281">Complete</span></span> | <span data-ttu-id="229f4-282">顯示訊息，通訊成功或失敗。</span><span class="sxs-lookup"><span data-stu-id="229f4-282">Presents a message communicating success or failure.</span></span> |

> [!NOTE]
> <span data-ttu-id="229f4-283">它使用 ASP.NET 控制項狀態的狀態記錄的精靈控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-283">The Wizard control keeps track of its state using ASP.NET control state.</span></span> <span data-ttu-id="229f4-284">因此，EnableViewState 屬性可以設定為 false，而不需要任何有利有弊。</span><span class="sxs-lookup"><span data-stu-id="229f4-284">Therefore, the EnableViewState property can be set to false without any detriment.</span></span>


<span data-ttu-id="229f4-285">這段影片會 Wizard 控制項逐步解說。</span><span class="sxs-lookup"><span data-stu-id="229f4-285">This video is a walkthrough of the Wizard control.</span></span>


![](server-controls/_static/image2.png)


[<span data-ttu-id="229f4-286">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="229f4-286">Open Full-Screen Video</span></span>](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a><span data-ttu-id="229f4-287">當地語系化控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-287">Localize Control</span></span>

<span data-ttu-id="229f4-288">Localize 控制項很類似常值的控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-288">The Localize control is similar to a Literal control.</span></span> <span data-ttu-id="229f4-289">不過，Localize 控制項具有**模式**控制項加入至它的標記的呈現方式的屬性。</span><span class="sxs-lookup"><span data-stu-id="229f4-289">However, the Localize control has a **Mode** property that controls how markup that is added to it is rendered.</span></span> <span data-ttu-id="229f4-290">Mode 屬性支援下列值：</span><span class="sxs-lookup"><span data-stu-id="229f4-290">The Mode property supports the following values:</span></span>

| <span data-ttu-id="229f4-291">**模式**</span><span class="sxs-lookup"><span data-stu-id="229f4-291">**Mode**</span></span> | <span data-ttu-id="229f4-292">**說明**</span><span class="sxs-lookup"><span data-stu-id="229f4-292">**Explanation**</span></span> |
| --- | --- |
| <span data-ttu-id="229f4-293">Transform</span><span class="sxs-lookup"><span data-stu-id="229f4-293">Transform</span></span> | <span data-ttu-id="229f4-294">標記是根據提出要求的瀏覽器的通訊協定轉換。</span><span class="sxs-lookup"><span data-stu-id="229f4-294">Markup is transformed according to the protocol of the browser making the request.</span></span> |
| <span data-ttu-id="229f4-295">通過</span><span class="sxs-lookup"><span data-stu-id="229f4-295">PassThrough</span></span> | <span data-ttu-id="229f4-296">標記會轉譯成-是。</span><span class="sxs-lookup"><span data-stu-id="229f4-296">Markup is rendered as-is.</span></span> |
| <span data-ttu-id="229f4-297">編碼</span><span class="sxs-lookup"><span data-stu-id="229f4-297">Encode</span></span> | <span data-ttu-id="229f4-298">使用 HtmlEncode 編碼新增至控制項的標記。</span><span class="sxs-lookup"><span data-stu-id="229f4-298">Markup that is added to the control is encoded using HtmlEncode.</span></span> |

## <a name="multiview-and-view-controls"></a><span data-ttu-id="229f4-299">MultiView 和 View 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-299">MultiView and View Controls</span></span>

<span data-ttu-id="229f4-300">MultiView 控制項做為檢視控制項的容器，並檢視控制項做為其他控制項 （如同面板控制項） 的容器。</span><span class="sxs-lookup"><span data-stu-id="229f4-300">The MultiView control acts as a container for View controls, and the View control acts as a container (much like a Panel control) for other controls.</span></span> <span data-ttu-id="229f4-301">MultiView 控制項中的每個檢視被以單一的檢視控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-301">Each view in a MultiView control is represented by a single View control.</span></span> <span data-ttu-id="229f4-302">第一個檢視中的控制項 MultiView 是檢視 0，第二個是檢視 1，等等。您可以藉由指定 MultiView 控制項的 ActiveViewIndex 切換檢視。</span><span class="sxs-lookup"><span data-stu-id="229f4-302">The first View control in the MultiView is view 0, the second is view 1, etc. You can switch views by specifying the ActiveViewIndex of the MultiView control.</span></span>

## <a name="substitution-control"></a><span data-ttu-id="229f4-303">替代控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-303">Substitution Control</span></span>

<span data-ttu-id="229f4-304">替代控制項用於搭配 ASP.NET 快取。</span><span class="sxs-lookup"><span data-stu-id="229f4-304">The Substitution control is used in conjunction with ASP.NET caching.</span></span> <span data-ttu-id="229f4-305">當您想要利用的快取，但您有必須更新每個要求 （亦即，頁面上的部分豁免於快取） 的頁面上的部分，替代元件會提供絕佳的解決方案。</span><span class="sxs-lookup"><span data-stu-id="229f4-305">In cases where you want to take advantage of caching, but you have portions of a page that must be updated on each request (in other words, portions of a page that are exempt from caching), the Substitution component provides a great solution.</span></span> <span data-ttu-id="229f4-306">控制項未實際呈現本身的任何輸出。</span><span class="sxs-lookup"><span data-stu-id="229f4-306">The control doesn't actually render any output on its own.</span></span> <span data-ttu-id="229f4-307">相反地，它會繫結至伺服器端程式碼中的方法。</span><span class="sxs-lookup"><span data-stu-id="229f4-307">Instead, it is bound to a method in server-side code.</span></span> <span data-ttu-id="229f4-308">要求頁面時，會呼叫的方法，並傳回的標記來取代替代控制項呈現。</span><span class="sxs-lookup"><span data-stu-id="229f4-308">When the page is requested, the method is called and the returned markup is rendered in place of the substitution control.</span></span>

<span data-ttu-id="229f4-309">替代控制項所繫結的方法，並透過指定**MethodName**屬性。</span><span class="sxs-lookup"><span data-stu-id="229f4-309">The method to which the Substitution control is bound is specified via the **MethodName** property.</span></span> <span data-ttu-id="229f4-310">該方法必須符合下列準則：</span><span class="sxs-lookup"><span data-stu-id="229f4-310">That method must meet the following criteria:</span></span>

- <span data-ttu-id="229f4-311">它必須是靜態 (在中共用 VB) 方法。</span><span class="sxs-lookup"><span data-stu-id="229f4-311">It must be a static (shared in VB) method.</span></span>
- <span data-ttu-id="229f4-312">它會接受一個類型參數的 HttpContext。</span><span class="sxs-lookup"><span data-stu-id="229f4-312">It accepts one parameter of type HttpContext.</span></span>
- <span data-ttu-id="229f4-313">它會傳回字串，表示應該取代頁面上的控制項的標記。</span><span class="sxs-lookup"><span data-stu-id="229f4-313">It returns a string representing the markup that should replace the control on the page.</span></span>

<span data-ttu-id="229f4-314">替代控制項沒有能夠修改任何其他控制項在頁面上，但它並沒有存取目前的 HttpContext 透過其參數。</span><span class="sxs-lookup"><span data-stu-id="229f4-314">The Substitution control does not have the ability to modify any other control on the page, but it does have access to the current HttpContext via its parameter.</span></span>

## <a name="gridview-control"></a><span data-ttu-id="229f4-315">GridView 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-315">GridView Control</span></span>

<span data-ttu-id="229f4-316">GridView 控制項是 DataGrid 控制項取代。</span><span class="sxs-lookup"><span data-stu-id="229f4-316">The GridView control is the replacement for the DataGrid control.</span></span> <span data-ttu-id="229f4-317">這個控制項將會涵蓋在更新版本的模組中的更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="229f4-317">This control will be covered in more detail in a later module.</span></span>

## <a name="detailsview-control"></a><span data-ttu-id="229f4-318">在 DetailsView 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-318">DetailsView Control</span></span>

<span data-ttu-id="229f4-319">在 DetailsView 控制項可讓您顯示資料來源的單一記錄，以及編輯或刪除它。</span><span class="sxs-lookup"><span data-stu-id="229f4-319">The DetailsView control allows you to display a single record from a data source and to edit or delete it.</span></span> <span data-ttu-id="229f4-320">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-320">It is covered in more detail in a later module.</span></span>

## <a name="formview-control"></a><span data-ttu-id="229f4-321">FormView 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-321">FormView Control</span></span>

<span data-ttu-id="229f4-322">FormView 控制項來顯示資料來源中的單一記錄中的可設定的介面。</span><span class="sxs-lookup"><span data-stu-id="229f4-322">The FormView control is used to display a single record from a datasource in a configurable interface.</span></span> <span data-ttu-id="229f4-323">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-323">It is covered in more detail in a later module.</span></span>

## <a name="accessdatasource-control"></a><span data-ttu-id="229f4-324">AccessDataSource 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-324">AccessDataSource Control</span></span>

<span data-ttu-id="229f4-325">AccessDataSource 控制項是用來將資料繫結的 Access 資料庫。</span><span class="sxs-lookup"><span data-stu-id="229f4-325">The AccessDataSource control is used to data bind an Access database.</span></span> <span data-ttu-id="229f4-326">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-326">It is covered in more detail in a later module.</span></span>

## <a name="objectdatasource-control"></a><span data-ttu-id="229f4-327">ObjectDataSource 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-327">ObjectDataSource Control</span></span>

<span data-ttu-id="229f4-328">ObjectDataSource 控制項用來支援三層式架構，使控制項可以是資料繫結至中介層商務物件而不是兩層的模型會直接與資料來源繫結控制項。</span><span class="sxs-lookup"><span data-stu-id="229f4-328">The ObjectDataSource control is used to support a three-tier architecture so that controls can be data-bound to a middle-tier business object as opposed to a two-tiered model where controls are bound directly to the data source.</span></span> <span data-ttu-id="229f4-329">它會在更新版本的模組的詳細討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-329">It will be discussed in more detail in a later module.</span></span>

## <a name="xmldatasource-control"></a><span data-ttu-id="229f4-330">XmlDataSource 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-330">XmlDataSource Control</span></span>

<span data-ttu-id="229f4-331">XmlDataSource 控制項用來資料繫結至 XML 資料來源。</span><span class="sxs-lookup"><span data-stu-id="229f4-331">The XmlDataSource control is used to data bind to an XML data source.</span></span> <span data-ttu-id="229f4-332">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-332">It is covered in more detail in a later module.</span></span>

## <a name="sitemapdatasource-control"></a><span data-ttu-id="229f4-333">SiteMapDataSource 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-333">SiteMapDataSource Control</span></span>

<span data-ttu-id="229f4-334">SiteMapDataSource 控制項提供站台地圖為基礎的網站巡覽控制項的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="229f4-334">The SiteMapDataSource control provides data binding for site navigation controls based on a site map.</span></span> <span data-ttu-id="229f4-335">它會在更新版本的模組的詳細討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-335">It will be discussed in more detail in a later module.</span></span>

## <a name="sitemappath-control"></a><span data-ttu-id="229f4-336">SiteMapPath 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-336">SiteMapPath Control</span></span>

<span data-ttu-id="229f4-337">SiteMapPath 控制項顯示一系列通常稱為階層連結的導覽連結。</span><span class="sxs-lookup"><span data-stu-id="229f4-337">The SiteMapPath control displays a series of navigation links commonly referred to as breadcrumbs.</span></span> <span data-ttu-id="229f4-338">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-338">It is covered in more detail in a later module.</span></span>

## <a name="menu-control"></a><span data-ttu-id="229f4-339">功能表控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-339">Menu Control</span></span>

<span data-ttu-id="229f4-340">功能表控制項顯示使用 DHTML 的動態功能表。</span><span class="sxs-lookup"><span data-stu-id="229f4-340">The Menu control displays dynamic menus using DHTML.</span></span> <span data-ttu-id="229f4-341">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-341">It is covered in more detail in a later module.</span></span>

## <a name="treeview-control"></a><span data-ttu-id="229f4-342">TreeView 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-342">TreeView Control</span></span>

<span data-ttu-id="229f4-343">TreeView 控制項來顯示資料的階層式樹狀結構檢視。</span><span class="sxs-lookup"><span data-stu-id="229f4-343">The TreeView control is used to display a hierarchical tree view of data.</span></span> <span data-ttu-id="229f4-344">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-344">It is covered in more detail in a later module.</span></span>

## <a name="login-control"></a><span data-ttu-id="229f4-345">Login 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-345">Login Control</span></span>

<span data-ttu-id="229f4-346">Login 控制項提供一個機制來登入網站。</span><span class="sxs-lookup"><span data-stu-id="229f4-346">The Login control provides for a mechanism to log into a Web site.</span></span> <span data-ttu-id="229f4-347">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-347">It is covered in more detail in a later module.</span></span>

## <a name="loginview-control"></a><span data-ttu-id="229f4-348">LoginView 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-348">LoginView Control</span></span>

<span data-ttu-id="229f4-349">LoginView 控制項可讓不同的範本，根據使用者的登入狀態的顯示。</span><span class="sxs-lookup"><span data-stu-id="229f4-349">The LoginView control allows for the display of different templates based upon a user's login status.</span></span> <span data-ttu-id="229f4-350">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-350">It is covered in more detail in a later module.</span></span>

## <a name="passwordrecovery-control"></a><span data-ttu-id="229f4-351">Provider 控制項</span><span class="sxs-lookup"><span data-stu-id="229f4-351">PasswordRecovery Control</span></span>

<span data-ttu-id="229f4-352">在 ASP.NET 應用程式的使用者，Provider 控制項用來擷取遺忘的密碼。</span><span class="sxs-lookup"><span data-stu-id="229f4-352">The PasswordRecovery control is used to retrieve forgotten passwords by users of an ASP.NET application.</span></span> <span data-ttu-id="229f4-353">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-353">It is covered in more detail in a later module.</span></span>

## <a name="loginstatus"></a><span data-ttu-id="229f4-354">LoginStatus</span><span class="sxs-lookup"><span data-stu-id="229f4-354">LoginStatus</span></span>

<span data-ttu-id="229f4-355">LoginStatus 控制項顯示使用者的登入狀態。</span><span class="sxs-lookup"><span data-stu-id="229f4-355">The LoginStatus control displays a user's login status.</span></span> <span data-ttu-id="229f4-356">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-356">It is covered in more detail in a later module.</span></span>

## <a name="loginname"></a><span data-ttu-id="229f4-357">LoginName</span><span class="sxs-lookup"><span data-stu-id="229f4-357">LoginName</span></span>

<span data-ttu-id="229f4-358">LoginName 控制項之後都記錄在 ASP.NET 應用程式顯示使用者的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="229f4-358">The LoginName control displays a user's username after being logged into an ASP.NET application.</span></span> <span data-ttu-id="229f4-359">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-359">It is covered in more detail in a later module.</span></span>

## <a name="createuserwizard"></a><span data-ttu-id="229f4-360">CreateUserWizard</span><span class="sxs-lookup"><span data-stu-id="229f4-360">CreateUserWizard</span></span>

<span data-ttu-id="229f4-361">CreateUserWizard 是一個可設定的精靈，讓使用者能夠在 ASP.NET 應用程式建立使用 ASP.NET 成員資格帳戶。</span><span class="sxs-lookup"><span data-stu-id="229f4-361">The CreateUserWizard is a configurable wizard which gives users the ability to create an ASP.NET Membership account for use in an ASP.NET application.</span></span> <span data-ttu-id="229f4-362">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-362">It is covered in more detail in a later module.</span></span>

## <a name="changepassword"></a><span data-ttu-id="229f4-363">ChangePassword</span><span class="sxs-lookup"><span data-stu-id="229f4-363">ChangePassword</span></span>

<span data-ttu-id="229f4-364">ChangePassword 控制項可讓使用者變更其密碼為 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="229f4-364">The ChangePassword control allows users to change their password for an ASP.NET application.</span></span> <span data-ttu-id="229f4-365">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="229f4-365">It is covered in more detail in a later module.</span></span>

## <a name="various-webparts"></a><span data-ttu-id="229f4-366">各種網頁組件</span><span class="sxs-lookup"><span data-stu-id="229f4-366">Various WebParts</span></span>

<span data-ttu-id="229f4-367">ASP.NET 2.0 隨附於各種網頁組件。</span><span class="sxs-lookup"><span data-stu-id="229f4-367">ASP.NET 2.0 ships with various Web Parts.</span></span> <span data-ttu-id="229f4-368">這些將會在更新版本的模組中有詳細說明。</span><span class="sxs-lookup"><span data-stu-id="229f4-368">These will be covered in detail in a later module.</span></span>
