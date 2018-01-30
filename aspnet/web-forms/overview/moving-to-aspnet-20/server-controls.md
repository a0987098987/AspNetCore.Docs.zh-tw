---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: "伺服器控制項 |Microsoft 文件"
author: microsoft
description: "ASP.NET 2.0 增強伺服器控制項，在許多方面。 在此模組中，我們會部分的 ASP.NET 2.0 的方式與 Visual Studio 200 的架構變更..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 72e9cac7cf9a01791c30783fa56ad7ea205a5a11
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="server-controls"></a><span data-ttu-id="a6bde-104">伺服器控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-104">Server Controls</span></span>
====================
<span data-ttu-id="a6bde-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a6bde-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a6bde-106">ASP.NET 2.0 增強伺服器控制項，在許多方面。</span><span class="sxs-lookup"><span data-stu-id="a6bde-106">ASP.NET 2.0 enhances server controls in many ways.</span></span> <span data-ttu-id="a6bde-107">在此模組中，我們將討論的一些架構 ASP.NET 2.0 的方式變更，而 Visual Studio 2005 處理的伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-107">In this module, we'll cover some of the architectural changes to the way ASP.NET 2.0 and Visual Studio 2005 deals with server controls.</span></span>


<span data-ttu-id="a6bde-108">ASP.NET 2.0 增強伺服器控制項，在許多方面。</span><span class="sxs-lookup"><span data-stu-id="a6bde-108">ASP.NET 2.0 enhances server controls in many ways.</span></span> <span data-ttu-id="a6bde-109">在此模組中，我們將討論的一些架構 ASP.NET 2.0 的方式變更，而 Visual Studio 2005 處理的伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-109">In this module, we'll cover some of the architectural changes to the way ASP.NET 2.0 and Visual Studio 2005 deals with server controls.</span></span>

## <a name="view-state"></a><span data-ttu-id="a6bde-110">檢視狀態</span><span class="sxs-lookup"><span data-stu-id="a6bde-110">View state</span></span>

<span data-ttu-id="a6bde-111">在檢視狀態在 ASP.NET 2.0 的主要變更是大幅減少大小。</span><span class="sxs-lookup"><span data-stu-id="a6bde-111">The primary change in view state in ASP.NET 2.0 is a dramatic reduction in size.</span></span> <span data-ttu-id="a6bde-112">請考慮只在其上的行事曆控制項的頁面。</span><span class="sxs-lookup"><span data-stu-id="a6bde-112">Consider a page with only a Calendar control on it.</span></span> <span data-ttu-id="a6bde-113">以下是 ASP.NET 1.1 中的檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-113">Here is the view state in ASP.NET 1.1.</span></span>

[!code-css[Main](server-controls/samples/sample1.css)]

<span data-ttu-id="a6bde-114">現在如下的檢視狀態上完全相同的頁面在 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="a6bde-114">Now here's the view state on an identical page in ASP.NET 2.0.</span></span>

[!code-css[Main](server-controls/samples/sample2.css)]

<span data-ttu-id="a6bde-115">這是很重要的變更，並考慮檢視狀態來回傳送透過網路傳輸，這項變更可以讓開發人員顯著的效能增加。</span><span class="sxs-lookup"><span data-stu-id="a6bde-115">That's a pretty significant change, and considering that view state is carried back and forth over the wire, this change can give developers a significant performance increase.</span></span> <span data-ttu-id="a6bde-116">檢視狀態的大小縮減主要是因為我們在內部處理的方式。</span><span class="sxs-lookup"><span data-stu-id="a6bde-116">The reduction in size of view state is largely due to the way that we handle it internally.</span></span> <span data-ttu-id="a6bde-117">請記住檢視狀態是 Base64 編碼字串。</span><span class="sxs-lookup"><span data-stu-id="a6bde-117">Remember that view state is a Base64 encoded string.</span></span> <span data-ttu-id="a6bde-118">若要進一步了解在 ASP.NET 2.0 在檢視狀態變更，讓我們看看從上述的範例已解碼的值。</span><span class="sxs-lookup"><span data-stu-id="a6bde-118">To better understand the change in view state in ASP.NET 2.0, let's have a look at the decoded values from the examples above.</span></span>

<span data-ttu-id="a6bde-119">以下是解碼 1.1 的檢視狀態：</span><span class="sxs-lookup"><span data-stu-id="a6bde-119">Here is the 1.1 view state decoded:</span></span>

[!code-css[Main](server-controls/samples/sample3.css)]

<span data-ttu-id="a6bde-120">這可能會看起來有點像胡說八道，但有一種模式。</span><span class="sxs-lookup"><span data-stu-id="a6bde-120">This may look a bit like gibberish, but there is a pattern here.</span></span> <span data-ttu-id="a6bde-121">在 ASP.NET 中 1.x，我們用來識別資料類型的單一字元，並分隔值使用&lt;&gt;字元。</span><span class="sxs-lookup"><span data-stu-id="a6bde-121">In ASP.NET 1.x, we used single characters to identify data-types and delimited values using the &lt;&gt; characters.</span></span> <span data-ttu-id="a6bde-122">上面的檢視狀態範例"t"表示數。</span><span class="sxs-lookup"><span data-stu-id="a6bde-122">The "t" in the view state sample above represents a Triplet.</span></span> <span data-ttu-id="a6bde-123">數包含一組 ArrayLists （"l"代表陣列）。其中一個這些 ArrayLists 包含 Int32 ("i") 值是 1，另包含另一個數。</span><span class="sxs-lookup"><span data-stu-id="a6bde-123">The Triplet contains a pair of ArrayLists (the "l" represents an ArrayList.) One of those ArrayLists contains an Int32 ("i") with a value of 1 and the other contains another Triplet.</span></span> <span data-ttu-id="a6bde-124">數包含一組 ArrayLists 等等。要記得的重點是，我們使用 Triplets 包含組、 我們識別字母，透過資料型別和我們使用&lt;和&gt;做為分隔符號的字元。</span><span class="sxs-lookup"><span data-stu-id="a6bde-124">The Triplet contains a pair of ArrayLists, etc. The important thing to remember is that we use Triplets that contain pairs, we identify the data-types via a letter, and we use the &lt; and &gt; characters as delimiters.</span></span>

<span data-ttu-id="a6bde-125">在 ASP.NET 2.0 中，已解碼的檢視狀態看起來稍有不同。</span><span class="sxs-lookup"><span data-stu-id="a6bde-125">In ASP.NET 2.0, the decoded view state looks a bit different.</span></span>

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

<span data-ttu-id="a6bde-126">您應該注意到巨大的變更，已解碼的檢視狀態的外觀。</span><span class="sxs-lookup"><span data-stu-id="a6bde-126">You should notice a huge change in the appearance of the decoded view state.</span></span> <span data-ttu-id="a6bde-127">這項變更有數個架構 underpinnings。</span><span class="sxs-lookup"><span data-stu-id="a6bde-127">This change has several architectural underpinnings.</span></span> <span data-ttu-id="a6bde-128">檢視狀態中 ASP.NET 1.x 用於 LosFormatter 序列化資料。</span><span class="sxs-lookup"><span data-stu-id="a6bde-128">View state in ASP.NET 1.x used the LosFormatter to serialize data.</span></span> <span data-ttu-id="a6bde-129">在 2.0 中，我們會使用新的 ObjectStateFormatter 類別。</span><span class="sxs-lookup"><span data-stu-id="a6bde-129">In 2.0, we use the new ObjectStateFormatter class.</span></span> <span data-ttu-id="a6bde-130">這個類別已特別設計來協助序列化和還原序列化的檢視狀態，以及控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-130">This class was specifically designed to aid in the serialization and deserialization of view state and control state.</span></span> <span data-ttu-id="a6bde-131">（下一節將討論控制項狀態）。有許多變更之序列化和還原序列化發生的方法取得的優勢。</span><span class="sxs-lookup"><span data-stu-id="a6bde-131">(Control state will be covered in the next section.) There are many benefits gained by changing the method by which serialization and deserialization take place.</span></span> <span data-ttu-id="a6bde-132">不像使用 TextWriter LosFormatter，ObjectStateFormatter 使用 BinaryWriter 事實的最大的其中一個。</span><span class="sxs-lookup"><span data-stu-id="a6bde-132">One of the most dramatic is the fact that unlike the LosFormatter which uses a TextWriter, the ObjectStateFormatter uses a BinaryWriter.</span></span> <span data-ttu-id="a6bde-133">這可讓 ASP.NET 2.0，若要儲存檢視狀態一連串的位元組，而不是字串。</span><span class="sxs-lookup"><span data-stu-id="a6bde-133">This allows ASP.NET 2.0 to store view state a series of bytes instead of strings.</span></span> <span data-ttu-id="a6bde-134">例如，採用整數。</span><span class="sxs-lookup"><span data-stu-id="a6bde-134">Take, for example, an integer.</span></span> <span data-ttu-id="a6bde-135">在 ASP.NET 1.1 中，整數所需的檢視狀態的 4 個位元組。</span><span class="sxs-lookup"><span data-stu-id="a6bde-135">In ASP.NET 1.1, an integer required 4 bytes of view state.</span></span> <span data-ttu-id="a6bde-136">在 ASP.NET 2.0 中，該相同的整數，只需要 1 個位元組。</span><span class="sxs-lookup"><span data-stu-id="a6bde-136">In ASP.NET 2.0, that same integer only requires 1 byte.</span></span> <span data-ttu-id="a6bde-137">進行其他的增強功能減少儲存的檢視狀態的變更。</span><span class="sxs-lookup"><span data-stu-id="a6bde-137">Other enhancements were made to decrease the amount of view state that is stored.</span></span> <span data-ttu-id="a6bde-138">日期時間值，例如，現在儲存 TickCount 而非字串。</span><span class="sxs-lookup"><span data-stu-id="a6bde-138">DateTime values, for example, are now stored using a TickCount instead of a string.</span></span>

<span data-ttu-id="a6bde-139">如同所有作業，都沒有，特別注意支付至其中一個 1.x 中的檢視狀態的最大取用者是資料格和類似之控制項的事實。</span><span class="sxs-lookup"><span data-stu-id="a6bde-139">As if all of that weren't enough, special attention was paid to the fact that one of the greatest consumers of view state in 1.x was the DataGrid and similar controls.</span></span> <span data-ttu-id="a6bde-140">例如而言檢視狀態的 DataGrid 控制項的主要缺點是資訊的它通常包含大量重複。</span><span class="sxs-lookup"><span data-stu-id="a6bde-140">A major drawback of controls such as the DataGrid where view state is concerned is that it often contains large amounts of repeated information.</span></span> <span data-ttu-id="a6bde-141">在 ASP.NET 中 1.x 重複資訊只會儲存和上一次導致過大的檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-141">In ASP.NET 1.x, that repeated information was simply stored over and over again resulting in a bloated view state.</span></span> <span data-ttu-id="a6bde-142">在 ASP.NET 2.0 中，我們可以使用 新 IndexedString 類別來儲存這類資料。</span><span class="sxs-lookup"><span data-stu-id="a6bde-142">In ASP.NET 2.0, we use the new IndexedString class to store such data.</span></span> <span data-ttu-id="a6bde-143">若重複出現的字串，我們只會儲存權杖 IndexedString 和 IndexedString 物件的執行中的表格中的索引。</span><span class="sxs-lookup"><span data-stu-id="a6bde-143">If a string repeats, we just store the token for the IndexedString and the index within a running table of IndexedString objects.</span></span>

## <a name="control-state"></a><span data-ttu-id="a6bde-144">控制項的狀態</span><span class="sxs-lookup"><span data-stu-id="a6bde-144">Control State</span></span>

<span data-ttu-id="a6bde-145">其中一個開發人員就必須檢視狀態主要 gripes 是它加入至 HTTP 內容的大小。</span><span class="sxs-lookup"><span data-stu-id="a6bde-145">One of the major gripes that developers had with view state was the size that it added to the HTTP payload.</span></span> <span data-ttu-id="a6bde-146">如先前所述，檢視狀態的最大取用者的其中一個是 DataGrid 控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-146">As previously mentioned, one of the greatest consumers of view state is the DataGrid control.</span></span> <span data-ttu-id="a6bde-147">若要避免大量的資料格，所產生的檢視狀態，許多開發人員只是停用該控制項的檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-147">To avoid the huge amounts of view state generated by a DataGrid, many developers simply disabled view state for that control.</span></span> <span data-ttu-id="a6bde-148">不幸的是，該方案不一定是一個很好。</span><span class="sxs-lookup"><span data-stu-id="a6bde-148">Unfortunately, that solution wasn't always a good one.</span></span> <span data-ttu-id="a6bde-149">檢視狀態中 ASP.NET 1.x 不只包含資料所需的正確控制項的功能。</span><span class="sxs-lookup"><span data-stu-id="a6bde-149">View state in ASP.NET 1.x contains not only data necessary for the correct functionality of the control.</span></span> <span data-ttu-id="a6bde-150">它也包含有關控制項的 UI 狀態的資訊。</span><span class="sxs-lookup"><span data-stu-id="a6bde-150">It also contains information concerning the state of the control's UI.</span></span> <span data-ttu-id="a6bde-151">這表示如果您想要允許重新編頁上的資料格，您必須啟用檢視狀態，即使您不需要的所有 UI 資訊的檢視狀態包含。</span><span class="sxs-lookup"><span data-stu-id="a6bde-151">This means that if you want to allow for pagination on a DataGrid, you must enable view state even if you don't need all of the UI information that view state contains.</span></span> <span data-ttu-id="a6bde-152">它是想到的案例。</span><span class="sxs-lookup"><span data-stu-id="a6bde-152">It's an all-or-nothing scenario.</span></span>

<span data-ttu-id="a6bde-153">在 ASP.NET 2.0 中，控制項狀態以解決該問題妥善透過引進的控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-153">In ASP.NET 2.0, control state solves that problem nicely via the introduction of control state.</span></span> <span data-ttu-id="a6bde-154">控制項狀態會包含適當的控制項功能的絕對必要的資料。</span><span class="sxs-lookup"><span data-stu-id="a6bde-154">Control state contains the data that is absolutely necessary for the proper functionality of a control.</span></span> <span data-ttu-id="a6bde-155">不同於檢視狀態，無法停用控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-155">Unlike view state, control state cannot be disabled.</span></span> <span data-ttu-id="a6bde-156">因此，很重要的小心控制儲存在控制項狀態的資料。</span><span class="sxs-lookup"><span data-stu-id="a6bde-156">Therefore, it is important that the data being stored in control state is carefully controlled.</span></span>

> [!NOTE]
> <span data-ttu-id="a6bde-157">中的檢視狀態與保存控制項狀態\_ \_VIEWSTATE 隱藏的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="a6bde-157">Control state is persisted along with the view state in the \_\_VIEWSTATE hidden form field.</span></span>


<span data-ttu-id="a6bde-158">這段影片會逐步解說中的檢視狀態，以及控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-158">This video is a walkthrough of view state and control state.</span></span>


![](server-controls/_static/image1.png)


[<span data-ttu-id="a6bde-159">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="a6bde-159">Open Full-Screen Video</span></span>](server-controls/_static/state1.wmv)


<span data-ttu-id="a6bde-160">為了讓伺服器控制項，以讀取和寫入控制狀態，您必須採取三個步驟。</span><span class="sxs-lookup"><span data-stu-id="a6bde-160">In order for a server control to read and write to control state, you must take three steps.</span></span>

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a><span data-ttu-id="a6bde-161">步驟 1： 呼叫 RegisterRequiresControlState 方法</span><span class="sxs-lookup"><span data-stu-id="a6bde-161">Step 1: Call the RegisterRequiresControlState Method</span></span>

<span data-ttu-id="a6bde-162">RegisterRequiresControlState 方法會通知 ASP.NET 控制項需要保存控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-162">The RegisterRequiresControlState method informs ASP.NET that a control needs to persist control state.</span></span> <span data-ttu-id="a6bde-163">它會控制哪些是已註冊的控制項類型的一個引數。</span><span class="sxs-lookup"><span data-stu-id="a6bde-163">It takes one argument of type Control which is the control that is being registered.</span></span>

<span data-ttu-id="a6bde-164">請務必注意註冊不會保存要求要求。</span><span class="sxs-lookup"><span data-stu-id="a6bde-164">It is important to note that registration does not persist from request to request.</span></span> <span data-ttu-id="a6bde-165">因此，呼叫這個方法必須是每個要求控制項是否保存控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-165">Therefore, this method must be called on every request if a control is to persist control state.</span></span> <span data-ttu-id="a6bde-166">建議的方法呼叫 OnInit 中。</span><span class="sxs-lookup"><span data-stu-id="a6bde-166">It is recommended that the method be called in OnInit.</span></span>

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a><span data-ttu-id="a6bde-167">步驟 2： 覆寫 SaveControlState</span><span class="sxs-lookup"><span data-stu-id="a6bde-167">Step 2: Override SaveControlState</span></span>

<span data-ttu-id="a6bde-168">SaveControlState 方法儲存自上次的回傳控制項的控制項的狀態變更。</span><span class="sxs-lookup"><span data-stu-id="a6bde-168">The SaveControlState method saves control state changes for a control since the last post back.</span></span> <span data-ttu-id="a6bde-169">它會傳回物件，代表控制項的狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-169">It returns an object representing the control's state.</span></span>

## <a name="step-3-override-loadcontrolstate"></a><span data-ttu-id="a6bde-170">步驟 3： 覆寫 LoadControlState</span><span class="sxs-lookup"><span data-stu-id="a6bde-170">Step 3: Override LoadControlState</span></span>

<span data-ttu-id="a6bde-171">LoadControlState 方法會將已儲存的狀態載入控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-171">The LoadControlState method loads saved state into a control.</span></span> <span data-ttu-id="a6bde-172">此方法會採用一個引數的型別物件，包含控制項的儲存的狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-172">The method takes one argument of type Object which holds the saved state for the control.</span></span>

## <a name="full-xhtml-compliance"></a><span data-ttu-id="a6bde-173">完整的 XHTML 相容性</span><span class="sxs-lookup"><span data-stu-id="a6bde-173">Full XHTML Compliance</span></span>

<span data-ttu-id="a6bde-174">任何 Web 開發人員知道標準 Web 應用程式中的重要性。</span><span class="sxs-lookup"><span data-stu-id="a6bde-174">Any Web developer knows the importance of standards in Web applications.</span></span> <span data-ttu-id="a6bde-175">為了維持標準為基礎的開發環境，ASP.NET 2.0 完全是相容的 XHTML。</span><span class="sxs-lookup"><span data-stu-id="a6bde-175">In order to maintain a standards-based development environment, ASP.NET 2.0 is fully XHTML compliant.</span></span> <span data-ttu-id="a6bde-176">因此，所有標記都會轉譯根據 XHTML 標準的瀏覽器支援 HTML 4.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="a6bde-176">Therefore, all tags are rendered according to XHTML standards in browsers that support HTML 4.0 or greater.</span></span>

<span data-ttu-id="a6bde-177">在 ASP.NET 1.1 中的文件類型定義如下所示：</span><span class="sxs-lookup"><span data-stu-id="a6bde-177">The DOCTYPE definition in ASP.NET 1.1 was as follows:</span></span>

[!code-html[Main](server-controls/samples/sample6.html)]

<span data-ttu-id="a6bde-178">在 ASP.NET 2.0 的預設文件類型定義如下所示：</span><span class="sxs-lookup"><span data-stu-id="a6bde-178">In ASP.NET 2.0, the default DOCTYPE definition is as follows:</span></span>

[!code-html[Main](server-controls/samples/sample7.html)]

<span data-ttu-id="a6bde-179">如果您選擇，您可以變更透過組態檔中的 xhtmlConformance 節點的預設 XHML 相容性。</span><span class="sxs-lookup"><span data-stu-id="a6bde-179">If you choose, you can alter the default XHML compliance via the xhtmlConformance node in the configuration file.</span></span> <span data-ttu-id="a6bde-180">例如，在 web.config 檔案中的下列節點會變成 XHTML 相容性 XHTML 1.0 Strict:</span><span class="sxs-lookup"><span data-stu-id="a6bde-180">For example, the following node in the web.config file will change XHTML compliance to XHTML 1.0 Strict:</span></span>

[!code-xml[Main](server-controls/samples/sample8.xml)]

<span data-ttu-id="a6bde-181">如果您選擇，您也可以設定要使用舊版 ASP.NET 中使用的設定 ASP.NET 1.x，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a6bde-181">If you choose, you can also configure ASP.NET to use the legacy configuration used in ASP.NET 1.x as follows:</span></span>

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a><span data-ttu-id="a6bde-182">自動調整轉譯時使用配接器</span><span class="sxs-lookup"><span data-stu-id="a6bde-182">Adaptive Rendering Using Adapters</span></span>

<span data-ttu-id="a6bde-183">在 ASP.NET 中 1.x，組態檔包含&lt;browserCaps&gt;填入 HttpBrowserCapabilities 物件的區段。</span><span class="sxs-lookup"><span data-stu-id="a6bde-183">In ASP.NET 1.x, the configuration file contained a &lt;browserCaps&gt; section that populated a HttpBrowserCapabilities object.</span></span> <span data-ttu-id="a6bde-184">這個物件允許開發人員以判斷哪些裝置正在進行特定的要求，並適當地轉譯程式碼。</span><span class="sxs-lookup"><span data-stu-id="a6bde-184">This object allowed a developer to determine what device is making a particular request and render code appropriately.</span></span> <span data-ttu-id="a6bde-185">在 ASP.NET 2.0 中，模型已經改良，現在會使用新的 ControlAdapter 類別。</span><span class="sxs-lookup"><span data-stu-id="a6bde-185">In ASP.NET 2.0, the model has improved and now uses the new ControlAdapter class.</span></span> <span data-ttu-id="a6bde-186">ControlAdapter 類別覆寫控制項的生命週期中的事件，並控制使用者代理程式的功能為基礎的控制項的呈現方式。</span><span class="sxs-lookup"><span data-stu-id="a6bde-186">The ControlAdapter class overrides events in the control's lifecycle and controls the rendering of controls based upon the user agent's capabilities.</span></span> <span data-ttu-id="a6bde-187">瀏覽器定義檔案 （副檔名為.browser 檔案的檔案） 儲存在 c:\windows\microsoft.net\framework\v2.0 定義特定使用者代理程式的功能。\* \* \* \*\CONFIG\Browsers 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a6bde-187">The capabilities of a specific user agent are defined by a browser definition file (a file with a .browser file extension) stored in the c:\windows\microsoft.net\framework\v2.0.\*\*\*\*\CONFIG\Browsers folder.</span></span>

> [!NOTE]
> <span data-ttu-id="a6bde-188">ControlAdapter 類別是抽象類別。</span><span class="sxs-lookup"><span data-stu-id="a6bde-188">The ControlAdapter class is an abstract class.</span></span>


<span data-ttu-id="a6bde-189">就像是&lt;browserCaps&gt;區段 1.x 瀏覽器定義檔中的使用規則運算式剖析以找出要求的瀏覽器使用者代理字串。</span><span class="sxs-lookup"><span data-stu-id="a6bde-189">Much like the &lt;browserCaps&gt; section in 1.x, the browser definition file uses a Regular Expression to parse the user agent string in order to identify the requesting browser.</span></span> <span data-ttu-id="a6bde-190">它它們定義該使用者代理程式的特定功能。</span><span class="sxs-lookup"><span data-stu-id="a6bde-190">It them defines particular capabilities for that user agent.</span></span> <span data-ttu-id="a6bde-191">ControlAdapter 呈現控制項透過轉譯方法。</span><span class="sxs-lookup"><span data-stu-id="a6bde-191">The ControlAdapter renders the control via the Render method.</span></span> <span data-ttu-id="a6bde-192">因此，如果您覆寫轉譯方法，您不應該呼叫轉譯基底類別。</span><span class="sxs-lookup"><span data-stu-id="a6bde-192">Therefore, if you override the Render method, you should not call Render on the base class.</span></span> <span data-ttu-id="a6bde-193">如此一來，可能會導致發生兩次，一次的配接器，一次在控制項本身呈現。</span><span class="sxs-lookup"><span data-stu-id="a6bde-193">Doing so may cause rendering to occur twice, once for the adapter and once for the control itself.</span></span>

## <a name="developing-a-custom-adapter"></a><span data-ttu-id="a6bde-194">開發自訂配接器</span><span class="sxs-lookup"><span data-stu-id="a6bde-194">Developing a Custom Adapter</span></span>

<span data-ttu-id="a6bde-195">您可以開發自訂配接器，繼承自 ControlAdapter。</span><span class="sxs-lookup"><span data-stu-id="a6bde-195">You can develop your own custom adapter by inheriting from ControlAdapter.</span></span> <span data-ttu-id="a6bde-196">此外，您可以繼承自抽象類別 PageAdapter 萬一頁面需要配接器的位置。</span><span class="sxs-lookup"><span data-stu-id="a6bde-196">Additionally, you can inherit from the abstract class PageAdapter in cases where an adapter is needed for a page.</span></span> <span data-ttu-id="a6bde-197">對應的控制項，以自訂配接器透過完成&lt;controlAdapters&gt;瀏覽器定義檔案中的項目。</span><span class="sxs-lookup"><span data-stu-id="a6bde-197">Mapping of controls to your custom adapter is accomplished via the &lt;controlAdapters&gt; element in the browser definition file.</span></span> <span data-ttu-id="a6bde-198">例如，下列 XML 程式碼從瀏覽器定義檔案對應功能表控制項 MenuAdapter 類別：</span><span class="sxs-lookup"><span data-stu-id="a6bde-198">For example, the following XML from a browser definition file maps the Menu control to the MenuAdapter class:</span></span>

[!code-html[Main](server-controls/samples/sample10.html)]

<span data-ttu-id="a6bde-199">使用此模型時，會變得很容易讓控制項開發人員為目標的特定裝置或瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="a6bde-199">Using this model, it becomes quite easy for a control developer to target a particular device or browser.</span></span> <span data-ttu-id="a6bde-200">它也是開發人員將擁有完整控制權頁面每個裝置上轉譯的方式相當簡單。</span><span class="sxs-lookup"><span data-stu-id="a6bde-200">It's also quite simple for a developer to have complete control over how pages render on every device.</span></span>

## <a name="per-device-rendering"></a><span data-ttu-id="a6bde-201">每一裝置轉譯</span><span class="sxs-lookup"><span data-stu-id="a6bde-201">Per-Device Rendering</span></span>

<span data-ttu-id="a6bde-202">在 ASP.NET 2.0 的伺服器控制項屬性可以指定每個裝置使用瀏覽器專屬的前置詞。</span><span class="sxs-lookup"><span data-stu-id="a6bde-202">Server control properties in ASP.NET 2.0 can be specified per-device using a browser-specific prefix.</span></span> <span data-ttu-id="a6bde-203">例如，下列程式碼會變更標籤，視哪一種裝置用來瀏覽網頁的文字。</span><span class="sxs-lookup"><span data-stu-id="a6bde-203">For example, the code below will change the Text of a label depending upon which device is being used to browse the page.</span></span>

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

<span data-ttu-id="a6bde-204">當從 Internet Explorer 瀏覽包含此標籤的頁面時，標籤會顯示文字說 「 瀏覽從 Internet Explorer。 」</span><span class="sxs-lookup"><span data-stu-id="a6bde-204">When the page containing this label is browsed from Internet Explorer, the label will display text saying "You are browsing from Internet Explorer."</span></span> <span data-ttu-id="a6bde-205">當從 Firefox 瀏覽網頁時，標籤會顯示文字 「 瀏覽從 Firefox。 」</span><span class="sxs-lookup"><span data-stu-id="a6bde-205">When the page is browsed from Firefox, the label will display the text "You are browsing from Firefox."</span></span> <span data-ttu-id="a6bde-206">當從其他任何裝置瀏覽網頁時，它會顯示 「 您瀏覽從未知的裝置。 」</span><span class="sxs-lookup"><span data-stu-id="a6bde-206">When the page is browsed from any other device, it will display "You are browsing from an unknown device."</span></span> <span data-ttu-id="a6bde-207">可以使用這個特殊的語法來指定任何屬性。</span><span class="sxs-lookup"><span data-stu-id="a6bde-207">Any property can be specified using this special syntax.</span></span>

## <a name="setting-focus"></a><span data-ttu-id="a6bde-208">設定焦點</span><span class="sxs-lookup"><span data-stu-id="a6bde-208">Setting Focus</span></span>

<span data-ttu-id="a6bde-209">有關如何在特定的控制項上設定初始焦點常見問題集 ASP.NET 1.x 開發人員。</span><span class="sxs-lookup"><span data-stu-id="a6bde-209">ASP.NET 1.x developers frequently asked about how to set initial focus on a particular control.</span></span> <span data-ttu-id="a6bde-210">比方說，在登入頁面上，最好擁有第一次載入頁面時，取得焦點的 [使用者識別碼] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="a6bde-210">For example, on a login page, it's useful to have the User ID textbox get the focus when the page first loads.</span></span> <span data-ttu-id="a6bde-211">在 ASP.NET 1.x，如此一來會需要寫入一些用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="a6bde-211">In ASP.NET 1.x, doing this required writing some client-side script.</span></span> <span data-ttu-id="a6bde-212">即使這類指令碼是簡單的工作，它不再需要 ASP.NET 2.0 中這點受惠 SetFocus 方法。</span><span class="sxs-lookup"><span data-stu-id="a6bde-212">Even though such a script is a trivial task, it's no longer necessary in ASP.NET 2.0 thanks to the SetFocus method.</span></span> <span data-ttu-id="a6bde-213">SetFocus 方法會採用一個引數指出應該接收焦點的控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-213">The SetFocus method takes one argument indicating the control that should receive focus.</span></span> <span data-ttu-id="a6bde-214">這個引數可以是用戶端控制項的 ID 做為字串或伺服器控制項，以控制物件的名稱。</span><span class="sxs-lookup"><span data-stu-id="a6bde-214">This argument can either be the client ID of the control as a string or the name of the Server control as a Control object.</span></span> <span data-ttu-id="a6bde-215">比方說，若要設定文字方塊控制項第一次載入頁面時，呼叫 txtUserID 初始焦點，將下列程式碼加入至頁面\_負載：</span><span class="sxs-lookup"><span data-stu-id="a6bde-215">For example, to set the initial focus to a TextBox control called txtUserID when the page first loads, add the following code to Page\_Load:</span></span>

[!code-csharp[Main](server-controls/samples/sample12.cs)]

<span data-ttu-id="a6bde-216">-或</span><span class="sxs-lookup"><span data-stu-id="a6bde-216">-- or</span></span>

[!code-csharp[Main](server-controls/samples/sample13.cs)]

<span data-ttu-id="a6bde-217">ASP.NET 2.0 使用呈現的用戶端函式，將焦點設定 Webresource.axd 處理常式 （先前所討論）。</span><span class="sxs-lookup"><span data-stu-id="a6bde-217">ASP.NET 2.0 uses the Webresource.axd handler (discussed previously) to render a client-side function that sets the focus.</span></span> <span data-ttu-id="a6bde-218">用戶端函式的名稱是 WebForm\_AutoFocus 如下所示：</span><span class="sxs-lookup"><span data-stu-id="a6bde-218">The name of the client-side function is WebForm\_AutoFocus as shown here:</span></span>

[!code-html[Main](server-controls/samples/sample14.html)]

<span data-ttu-id="a6bde-219">或者，您可以使用控制項的焦點方法，將初始焦點設定該控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-219">Alternatively, you can use the Focus method for a control to set the initial focus to that control.</span></span> <span data-ttu-id="a6bde-220">焦點方法衍生自 Control 類別，並使用所有 ASP.NET 2.0 控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-220">The Focus method derives from the Control class and is available to all ASP.NET 2.0 controls.</span></span> <span data-ttu-id="a6bde-221">它也可將焦點設定至特定控制項，當發生驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="a6bde-221">It is also possible to set focus to a particular control when a validation error occurs.</span></span> <span data-ttu-id="a6bde-222">將涵蓋的更新版本的模組中。</span><span class="sxs-lookup"><span data-stu-id="a6bde-222">That will be covered in a later module.</span></span>

## <a name="new-server-controls-in-aspnet-20"></a><span data-ttu-id="a6bde-223">在 ASP.NET 2.0 的新伺服器控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-223">New Server Controls in ASP.NET 2.0</span></span>

<span data-ttu-id="a6bde-224">以下是在 ASP.NET 2.0 的新伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-224">The following are new server controls in ASP.NET 2.0.</span></span> <span data-ttu-id="a6bde-225">我們將在這些更新版本的模組中的某些移到更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a6bde-225">We will go into more detail on some of them in later modules.</span></span>

## <a name="imagemap-control"></a><span data-ttu-id="a6bde-226">ImageMap 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-226">ImageMap Control</span></span>

<span data-ttu-id="a6bde-227">ImageMap 控制項可讓您加入的映像，可以起始回傳或巡覽至 URL 的作用點。</span><span class="sxs-lookup"><span data-stu-id="a6bde-227">The ImageMap control allows you to add hotspots to an image that can initiate a post back or navigate to a URL.</span></span> <span data-ttu-id="a6bde-228">有可用的三種類型的作用點。CircleHotSpot、 RectangleHotSpot 和 PolygonHotSpot。</span><span class="sxs-lookup"><span data-stu-id="a6bde-228">There are three types of hotspots available; CircleHotSpot, RectangleHotSpot, and PolygonHotSpot.</span></span> <span data-ttu-id="a6bde-229">透過 Visual Studio 中或以程式設計方式在程式碼中的集合編輯器加入熱點。</span><span class="sxs-lookup"><span data-stu-id="a6bde-229">Hotspots are added via a collection editor in Visual Studio or programmatically in code.</span></span> <span data-ttu-id="a6bde-230">沒有適用於繪製在影像上的作用點的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="a6bde-230">There is no user-interface available for drawing hotspots on an image.</span></span> <span data-ttu-id="a6bde-231">必須以宣告方式指定的座標和大小或作用點的半徑。</span><span class="sxs-lookup"><span data-stu-id="a6bde-231">The coordinates and size or radius of the hotspot must be specified declaratively.</span></span> <span data-ttu-id="a6bde-232">也是沒有作用區設計工具中的視覺表示。</span><span class="sxs-lookup"><span data-stu-id="a6bde-232">There is also no visual representation of a hotspot in the designer.</span></span> <span data-ttu-id="a6bde-233">如果作用區設定為瀏覽至 URL，URL 會指定透過 NavigateUrl 屬性的作用點。</span><span class="sxs-lookup"><span data-stu-id="a6bde-233">If a hotspot is configured to navigate to a URL, the URL is specified via the NavigateUrl property of the hotspot.</span></span> <span data-ttu-id="a6bde-234">在 post 回作用點，PostBackValue 屬性可讓您在伺服器端程式碼中傳遞回傳可擷取的字串。</span><span class="sxs-lookup"><span data-stu-id="a6bde-234">In the case of a post back hotspot, the PostBackValue property allows you to pass a string in the post back that can be retrieved in server-side code.</span></span>


![在 Visual Studio 中的作用點集合編輯器](server-controls/_static/image1.jpg)

<span data-ttu-id="a6bde-236">**圖 1**： 在 Visual Studio 中的作用點集合編輯器</span><span class="sxs-lookup"><span data-stu-id="a6bde-236">**Figure 1**: HotSpot Collection Editor in Visual Studio</span></span>


## <a name="bulletedlist-control"></a><span data-ttu-id="a6bde-237">BulletedList 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-237">BulletedList Control</span></span>

<span data-ttu-id="a6bde-238">BulletedList 控制項是可以輕鬆地進行資料繫結項目符號清單。</span><span class="sxs-lookup"><span data-stu-id="a6bde-238">The BulletedList control is a bulleted list that can easily be data bound.</span></span> <span data-ttu-id="a6bde-239">清單可以排序 （編號的） 或未透過 BulletStyle 屬性排序。</span><span class="sxs-lookup"><span data-stu-id="a6bde-239">The list can be ordered (numbered) or unordered via the BulletStyle property.</span></span> <span data-ttu-id="a6bde-240">在清單中的每個項目會以清單項目物件。</span><span class="sxs-lookup"><span data-stu-id="a6bde-240">Each item in the list is represented by a ListItem object.</span></span>


![Visual Studio 中設定 BulletedList 控制項](server-controls/_static/image1.gif)

<span data-ttu-id="a6bde-242">**圖 2**: Visual Studio 中設定 BulletedList 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-242">**Figure 2**: BulletedList Control in Visual Studio</span></span>


## <a name="hiddenfield-control"></a><span data-ttu-id="a6bde-243">Hiddenfield</span><span class="sxs-lookup"><span data-stu-id="a6bde-243">HiddenField Control</span></span>

<span data-ttu-id="a6bde-244">HiddenField 控制頁面上，其值可用於伺服器端程式碼中加入隱藏的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="a6bde-244">The HiddenField control adds a hidden form field to your page, the value of which is available in server-side code.</span></span> <span data-ttu-id="a6bde-245">通常應該隱藏的表單欄位的值會維持不變回傳之間。</span><span class="sxs-lookup"><span data-stu-id="a6bde-245">The value of a hidden form field is generally expected to remain unchanged between post backs.</span></span> <span data-ttu-id="a6bde-246">但是，它可能是惡意使用者可以變更為值先前的回傳。</span><span class="sxs-lookup"><span data-stu-id="a6bde-246">However, it is possible for a malicious user to change the value prior to post back.</span></span> <span data-ttu-id="a6bde-247">如果發生這種情況，HiddenField 控制項將會引發 ValueChanged 事件。</span><span class="sxs-lookup"><span data-stu-id="a6bde-247">If this happens, the HiddenField control will raise the ValueChanged event.</span></span> <span data-ttu-id="a6bde-248">如果您有 HiddenField 控制項中的機密資訊，而且您想要確保維持不變，您應該在程式碼中處理 ValueChanged 事件。</span><span class="sxs-lookup"><span data-stu-id="a6bde-248">If you have sensitive information in the HiddenField control and you want to ensure that it remains unchanged, you should handle the ValueChanged event in your code.</span></span>

## <a name="fileupload-control"></a><span data-ttu-id="a6bde-249">檔案上傳控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-249">FileUpload Control</span></span>

<span data-ttu-id="a6bde-250">在 ASP.NET 2.0 中的檔案上傳控制項讓您可以將檔案上傳至 Web 伺服器，透過 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="a6bde-250">The FileUpload control in ASP.NET 2.0 makes it possible to upload files to a Web server via an ASP.NET page.</span></span> <span data-ttu-id="a6bde-251">此控制項是相當類似於 ASP.NET 1.x HtmlInputFile 類別有一些例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a6bde-251">This control is quite similar to the ASP.NET 1.x HtmlInputFile class with a few exceptions.</span></span> <span data-ttu-id="a6bde-252">在 ASP.NET 中 1.x 曾經建議 null 檢查 PostedFile 屬性，以判斷您是否有正確的檔案。</span><span class="sxs-lookup"><span data-stu-id="a6bde-252">In ASP.NET 1.x, it was recommended that the PostedFile property be checked for null in order to determine if you had a good file.</span></span> <span data-ttu-id="a6bde-253">在 ASP.NET 2.0 中的檔案上傳控制項，將新的 HasFile 屬性達成相同目的，您可以使用而且更有效率。</span><span class="sxs-lookup"><span data-stu-id="a6bde-253">The FileUpload control in ASP.NET 2.0 adds a new HasFile property that you can use for the same purpose and it's a bit more efficient.</span></span>

<span data-ttu-id="a6bde-254">PostedFile 屬性仍可供存取 HttpPostedFile 物件，但某些 HttpPostedFile 功能就可在本質上與檔案上傳控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-254">The PostedFile property is still available for access to an HttpPostedFile object, but some of the functionality of the HttpPostedFile is now available intrinsically with the FileUpload control.</span></span> <span data-ttu-id="a6bde-255">例如，若要將上傳的檔案儲存在 ASP.NET 1.x 您另存新檔上呼叫方法 HttpPostedFile 物件。</span><span class="sxs-lookup"><span data-stu-id="a6bde-255">For example, to save an uploaded file in ASP.NET 1.x, you call the SaveAs method on the HttpPostedFile object.</span></span> <span data-ttu-id="a6bde-256">使用 ASP.NET 2.0 中的檔案上傳控制項，您可以在檔案上傳控制項本身呼叫 SaveAs 方法。</span><span class="sxs-lookup"><span data-stu-id="a6bde-256">Using the FileUpload control in ASP.NET 2.0, you would call the SaveAs method on the FileUpload control itself.</span></span>

<span data-ttu-id="a6bde-257">另一個 2.0 行為 （與可能是最重要的變更） 中的重大變更是它已不再需要儲存之前，先將整個上傳的檔案載入記憶體。</span><span class="sxs-lookup"><span data-stu-id="a6bde-257">Another significant change in the 2.0 behavior (and likely the most significant change) is that it is no longer necessary to load an entire uploaded file into memory before saving it.</span></span> <span data-ttu-id="a6bde-258">1.x，在已上傳任何檔案會儲存完全讀入記憶體之前寫入至磁碟。</span><span class="sxs-lookup"><span data-stu-id="a6bde-258">In 1.x, any file that was uploaded is saved entirely into memory prior to being written to disk.</span></span> <span data-ttu-id="a6bde-259">這個架構可預防大型檔案上的傳。</span><span class="sxs-lookup"><span data-stu-id="a6bde-259">This architecture prevents the upload of large files.</span></span>

<span data-ttu-id="a6bde-260">在 ASP.NET 2.0 httpRuntime 元素 requestLengthDiskThreshold 屬性可讓您設定多少 Kb 會保留在記憶體之前寫入的緩衝區寫入磁碟。</span><span class="sxs-lookup"><span data-stu-id="a6bde-260">In ASP.NET 2.0, the requestLengthDiskThreshold attribute of the httpRuntime element allows you to configure how many Kilobytes are held in a buffer in memory prior to being written to disk.</span></span>

<span data-ttu-id="a6bde-261">**重要**: MSDN 文件 （及其他位置的文件） 指定這個值是以位元組為單位 （千位元組） 且預設值是 256。</span><span class="sxs-lookup"><span data-stu-id="a6bde-261">**IMPORTANT**: MSDN documentation (and documentation elsewhere) specifies that this value is in bytes (not Kilobytes) and that the default is 256.</span></span> <span data-ttu-id="a6bde-262">實際上以 kb 為單位指定的值，預設值為 80。</span><span class="sxs-lookup"><span data-stu-id="a6bde-262">The value is actually specified in Kilobytes and the default value is 80.</span></span> <span data-ttu-id="a6bde-263">有預設值是 80 K，我們會確保在大型物件堆積不 estado intermedio 緩衝區。</span><span class="sxs-lookup"><span data-stu-id="a6bde-263">By having a default value of 80K, we ensure that the buffer does not end up on the large object heap.</span></span>

## <a name="wizard-control"></a><span data-ttu-id="a6bde-264">精靈控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-264">Wizard Control</span></span>

<span data-ttu-id="a6bde-265">它是會遇到 ASP.NET 開發人員掙扎與嘗試收集資訊數列中的 「 頁面 」 使用面板或傳輸 頁面，以相當常見。</span><span class="sxs-lookup"><span data-stu-id="a6bde-265">It is fairly common to encounter ASP.NET developers struggling with attempting to gather information in a series of "pages" using panels or by transferring from page to page.</span></span> <span data-ttu-id="a6bde-266">常常會，工作是令人沮喪，會耗用的時間。</span><span class="sxs-lookup"><span data-stu-id="a6bde-266">More often than not, the endeavor is a frustrating one and is time consuming.</span></span> <span data-ttu-id="a6bde-267">新的精靈控制項藉由使用中的精靈介面，使用者熟悉的線性和非線性步驟解決的問題。</span><span class="sxs-lookup"><span data-stu-id="a6bde-267">The new Wizard control solves the problems by allowing for linear and non-linear steps in a wizard interface that users are familiar with.</span></span> <span data-ttu-id="a6bde-268">精靈控制項提供輸入的表單，以一系列的步驟。</span><span class="sxs-lookup"><span data-stu-id="a6bde-268">The Wizard control presents input forms in a series of steps.</span></span> <span data-ttu-id="a6bde-269">每個步驟是控制項的特定類型 StepType 屬性所指定。</span><span class="sxs-lookup"><span data-stu-id="a6bde-269">Each step is of a particular type specified by the StepType property of the control.</span></span> <span data-ttu-id="a6bde-270">可用的步驟類型如下所示：</span><span class="sxs-lookup"><span data-stu-id="a6bde-270">The available step types are as follows:</span></span>

| <span data-ttu-id="a6bde-271">**步驟類型**</span><span class="sxs-lookup"><span data-stu-id="a6bde-271">**Step Type**</span></span> | <span data-ttu-id="a6bde-272">**Explanation**</span><span class="sxs-lookup"><span data-stu-id="a6bde-272">**Explanation**</span></span> |
| --- | --- |
| <span data-ttu-id="a6bde-273">自動</span><span class="sxs-lookup"><span data-stu-id="a6bde-273">Auto</span></span> | <span data-ttu-id="a6bde-274">精靈會自動決定它步驟階層內的位置為基礎的步驟的類型。</span><span class="sxs-lookup"><span data-stu-id="a6bde-274">The wizard automatically determines the type of step based upon its position within the step hierarchy.</span></span> |
| <span data-ttu-id="a6bde-275">啟動</span><span class="sxs-lookup"><span data-stu-id="a6bde-275">Start</span></span> | <span data-ttu-id="a6bde-276">第一個步驟中，通常用來呈現簡介的陳述式。</span><span class="sxs-lookup"><span data-stu-id="a6bde-276">The first step, often used to present an introductory statement.</span></span> |
| <span data-ttu-id="a6bde-277">步驟</span><span class="sxs-lookup"><span data-stu-id="a6bde-277">Step</span></span> | <span data-ttu-id="a6bde-278">一般的步驟。</span><span class="sxs-lookup"><span data-stu-id="a6bde-278">A normal step.</span></span> |
| <span data-ttu-id="a6bde-279">完成</span><span class="sxs-lookup"><span data-stu-id="a6bde-279">Finish</span></span> | <span data-ttu-id="a6bde-280">最後一個步驟，通常用來呈現按鈕以完成精靈。</span><span class="sxs-lookup"><span data-stu-id="a6bde-280">The final step, usually used to present a button to finish the wizard.</span></span> |
| <span data-ttu-id="a6bde-281">完成</span><span class="sxs-lookup"><span data-stu-id="a6bde-281">Complete</span></span> | <span data-ttu-id="a6bde-282">顯示通訊成功或失敗的訊息。</span><span class="sxs-lookup"><span data-stu-id="a6bde-282">Presents a message communicating success or failure.</span></span> |

> [!NOTE]
> <span data-ttu-id="a6bde-283">精靈控制項會追蹤的狀態使用 ASP.NET 控制項狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-283">The Wizard control keeps track of its state using ASP.NET control state.</span></span> <span data-ttu-id="a6bde-284">因此，EnableViewState 屬性可以設定為 false，不含任何而妨礙。</span><span class="sxs-lookup"><span data-stu-id="a6bde-284">Therefore, the EnableViewState property can be set to false without any detriment.</span></span>


<span data-ttu-id="a6bde-285">這段影片是精靈控制項的逐步解說。</span><span class="sxs-lookup"><span data-stu-id="a6bde-285">This video is a walkthrough of the Wizard control.</span></span>


![](server-controls/_static/image2.png)


[<span data-ttu-id="a6bde-286">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="a6bde-286">Open Full-Screen Video</span></span>](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a><span data-ttu-id="a6bde-287">當地語系化控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-287">Localize Control</span></span>

<span data-ttu-id="a6bde-288">與常值的控制項相似 Localize 控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-288">The Localize control is similar to a Literal control.</span></span> <span data-ttu-id="a6bde-289">不過，有 Localize 控制項**模式**控制項加入至它的標記的呈現方式的屬性。</span><span class="sxs-lookup"><span data-stu-id="a6bde-289">However, the Localize control has a **Mode** property that controls how markup that is added to it is rendered.</span></span> <span data-ttu-id="a6bde-290">Mode 屬性支援下列值：</span><span class="sxs-lookup"><span data-stu-id="a6bde-290">The Mode property supports the following values:</span></span>

| <span data-ttu-id="a6bde-291">**模式**</span><span class="sxs-lookup"><span data-stu-id="a6bde-291">**Mode**</span></span> | <span data-ttu-id="a6bde-292">**Explanation**</span><span class="sxs-lookup"><span data-stu-id="a6bde-292">**Explanation**</span></span> |
| --- | --- |
| <span data-ttu-id="a6bde-293">Transform</span><span class="sxs-lookup"><span data-stu-id="a6bde-293">Transform</span></span> | <span data-ttu-id="a6bde-294">標記是根據提出要求之瀏覽器的通訊協定轉換。</span><span class="sxs-lookup"><span data-stu-id="a6bde-294">Markup is transformed according to the protocol of the browser making the request.</span></span> |
| <span data-ttu-id="a6bde-295">通過</span><span class="sxs-lookup"><span data-stu-id="a6bde-295">PassThrough</span></span> | <span data-ttu-id="a6bde-296">標記會轉譯為位是。</span><span class="sxs-lookup"><span data-stu-id="a6bde-296">Markup is rendered as-is.</span></span> |
| <span data-ttu-id="a6bde-297">編碼</span><span class="sxs-lookup"><span data-stu-id="a6bde-297">Encode</span></span> | <span data-ttu-id="a6bde-298">使用 HtmlEncode 編碼加入至控制項的標記。</span><span class="sxs-lookup"><span data-stu-id="a6bde-298">Markup that is added to the control is encoded using HtmlEncode.</span></span> |

## <a name="multiview-and-view-controls"></a><span data-ttu-id="a6bde-299">MultiView 和檢視控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-299">MultiView and View Controls</span></span>

<span data-ttu-id="a6bde-300">MultiView 控制項做為容器的檢視控制項，並檢視控制項做為其他控制項的容器 （非常類似面板控制項）。</span><span class="sxs-lookup"><span data-stu-id="a6bde-300">The MultiView control acts as a container for View controls, and the View control acts as a container (much like a Panel control) for other controls.</span></span> <span data-ttu-id="a6bde-301">MultiView 控制項中的每個檢視是由單一的檢視控制項表示。</span><span class="sxs-lookup"><span data-stu-id="a6bde-301">Each view in a MultiView control is represented by a single View control.</span></span> <span data-ttu-id="a6bde-302">MultiView 之第一個檢視控制項是檢視 0，第二個是 1、 檢視等等。您可以藉由指定 MultiView 控制項 ActiveViewIndex 切換檢視。</span><span class="sxs-lookup"><span data-stu-id="a6bde-302">The first View control in the MultiView is view 0, the second is view 1, etc. You can switch views by specifying the ActiveViewIndex of the MultiView control.</span></span>

## <a name="substitution-control"></a><span data-ttu-id="a6bde-303">替代控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-303">Substitution Control</span></span>

<span data-ttu-id="a6bde-304">替代控制項用於搭配 ASP.NET 快取。</span><span class="sxs-lookup"><span data-stu-id="a6bde-304">The Substitution control is used in conjunction with ASP.NET caching.</span></span> <span data-ttu-id="a6bde-305">在您想要充分利用快取，但您有必須更新每個要求 （亦即，頁面上的部分豁免於快取） 的頁面上的部分情況下，替代元件提供絕佳的解決方案。</span><span class="sxs-lookup"><span data-stu-id="a6bde-305">In cases where you want to take advantage of caching, but you have portions of a page that must be updated on each request (in other words, portions of a page that are exempt from caching), the Substitution component provides a great solution.</span></span> <span data-ttu-id="a6bde-306">控制項不會實際呈現其本身的任何輸出。</span><span class="sxs-lookup"><span data-stu-id="a6bde-306">The control doesn't actually render any output on its own.</span></span> <span data-ttu-id="a6bde-307">相反地，它會繫結中伺服器端程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="a6bde-307">Instead, it is bound to a method in server-side code.</span></span> <span data-ttu-id="a6bde-308">要求頁面時，會呼叫的方法，傳回的標記代替替代控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-308">When the page is requested, the method is called and the returned markup is rendered in place of the substitution control.</span></span>

<span data-ttu-id="a6bde-309">替代控制項所繫結的方法透過指定**MethodName**屬性。</span><span class="sxs-lookup"><span data-stu-id="a6bde-309">The method to which the Substitution control is bound is specified via the **MethodName** property.</span></span> <span data-ttu-id="a6bde-310">該方法必須符合下列準則：</span><span class="sxs-lookup"><span data-stu-id="a6bde-310">That method must meet the following criteria:</span></span>

- <span data-ttu-id="a6bde-311">它必須是靜態 (共用中的 VB) 方法。</span><span class="sxs-lookup"><span data-stu-id="a6bde-311">It must be a static (shared in VB) method.</span></span>
- <span data-ttu-id="a6bde-312">它會接受一個參數的型別 HttpContext。</span><span class="sxs-lookup"><span data-stu-id="a6bde-312">It accepts one parameter of type HttpContext.</span></span>
- <span data-ttu-id="a6bde-313">它會傳回字串，表示應該取代頁面上的控制項的標記。</span><span class="sxs-lookup"><span data-stu-id="a6bde-313">It returns a string representing the markup that should replace the control on the page.</span></span>

<span data-ttu-id="a6bde-314">替代控制項沒有能夠修改任何其他控制項在頁面上，但它並沒有存取目前的 HttpContext 透過其參數。</span><span class="sxs-lookup"><span data-stu-id="a6bde-314">The Substitution control does not have the ability to modify any other control on the page, but it does have access to the current HttpContext via its parameter.</span></span>

## <a name="gridview-control"></a><span data-ttu-id="a6bde-315">GridView 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-315">GridView Control</span></span>

<span data-ttu-id="a6bde-316">GridView 控制項是 DataGrid 控制項取代。</span><span class="sxs-lookup"><span data-stu-id="a6bde-316">The GridView control is the replacement for the DataGrid control.</span></span> <span data-ttu-id="a6bde-317">此控制項將涵蓋的更新版本的模組中詳細說明。</span><span class="sxs-lookup"><span data-stu-id="a6bde-317">This control will be covered in more detail in a later module.</span></span>

## <a name="detailsview-control"></a><span data-ttu-id="a6bde-318">在 DetailsView 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-318">DetailsView Control</span></span>

<span data-ttu-id="a6bde-319">在 DetailsView 控制項可讓您顯示資料來源的單一記錄，以及編輯或刪除它。</span><span class="sxs-lookup"><span data-stu-id="a6bde-319">The DetailsView control allows you to display a single record from a data source and to edit or delete it.</span></span> <span data-ttu-id="a6bde-320">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-320">It is covered in more detail in a later module.</span></span>

## <a name="formview-control"></a><span data-ttu-id="a6bde-321">在 FormView 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-321">FormView Control</span></span>

<span data-ttu-id="a6bde-322">在 FormView 控制項來顯示資料來源的單一記錄的可設定的介面。</span><span class="sxs-lookup"><span data-stu-id="a6bde-322">The FormView control is used to display a single record from a datasource in a configurable interface.</span></span> <span data-ttu-id="a6bde-323">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-323">It is covered in more detail in a later module.</span></span>

## <a name="accessdatasource-control"></a><span data-ttu-id="a6bde-324">AccessDataSource Control</span><span class="sxs-lookup"><span data-stu-id="a6bde-324">AccessDataSource Control</span></span>

<span data-ttu-id="a6bde-325">AccessDataSource 控制項是用來存取資料庫的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="a6bde-325">The AccessDataSource control is used to data bind an Access database.</span></span> <span data-ttu-id="a6bde-326">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-326">It is covered in more detail in a later module.</span></span>

## <a name="objectdatasource-control"></a><span data-ttu-id="a6bde-327">ObjectDataSource 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-327">ObjectDataSource Control</span></span>

<span data-ttu-id="a6bde-328">ObjectDataSource 控制項用來支援三層式架構，使控制項可以是資料繫結至中介層商務物件，而不是兩層的模型會直接與資料來源繫結控制項。</span><span class="sxs-lookup"><span data-stu-id="a6bde-328">The ObjectDataSource control is used to support a three-tier architecture so that controls can be data-bound to a middle-tier business object as opposed to a two-tiered model where controls are bound directly to the data source.</span></span> <span data-ttu-id="a6bde-329">將更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-329">It will be discussed in more detail in a later module.</span></span>

## <a name="xmldatasource-control"></a><span data-ttu-id="a6bde-330">XmlDataSource 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-330">XmlDataSource Control</span></span>

<span data-ttu-id="a6bde-331">XmlDataSource 控制項用來資料繫結到 XML 資料來源。</span><span class="sxs-lookup"><span data-stu-id="a6bde-331">The XmlDataSource control is used to data bind to an XML data source.</span></span> <span data-ttu-id="a6bde-332">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-332">It is covered in more detail in a later module.</span></span>

## <a name="sitemapdatasource-control"></a><span data-ttu-id="a6bde-333">Treeview 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-333">SiteMapDataSource Control</span></span>

<span data-ttu-id="a6bde-334">Treeview 控制項提供站台地圖為基礎的站台瀏覽控制項的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="a6bde-334">The SiteMapDataSource control provides data binding for site navigation controls based on a site map.</span></span> <span data-ttu-id="a6bde-335">將更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-335">It will be discussed in more detail in a later module.</span></span>

## <a name="sitemappath-control"></a><span data-ttu-id="a6bde-336">SiteMapPath 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-336">SiteMapPath Control</span></span>

<span data-ttu-id="a6bde-337">SiteMapPath 控制項顯示一系列統稱為階層連結列瀏覽連結。</span><span class="sxs-lookup"><span data-stu-id="a6bde-337">The SiteMapPath control displays a series of navigation links commonly referred to as breadcrumbs.</span></span> <span data-ttu-id="a6bde-338">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-338">It is covered in more detail in a later module.</span></span>

## <a name="menu-control"></a><span data-ttu-id="a6bde-339">功能表控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-339">Menu Control</span></span>

<span data-ttu-id="a6bde-340">此功能表控制項顯示使用 DHTML 的動態功能表。</span><span class="sxs-lookup"><span data-stu-id="a6bde-340">The Menu control displays dynamic menus using DHTML.</span></span> <span data-ttu-id="a6bde-341">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-341">It is covered in more detail in a later module.</span></span>

## <a name="treeview-control"></a><span data-ttu-id="a6bde-342">TreeView 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-342">TreeView Control</span></span>

<span data-ttu-id="a6bde-343">TreeView 控制項用來顯示資料的階層式樹狀檢視。</span><span class="sxs-lookup"><span data-stu-id="a6bde-343">The TreeView control is used to display a hierarchical tree view of data.</span></span> <span data-ttu-id="a6bde-344">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-344">It is covered in more detail in a later module.</span></span>

## <a name="login-control"></a><span data-ttu-id="a6bde-345">登入控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-345">Login Control</span></span>

<span data-ttu-id="a6bde-346">登入控制項提供一個機制來登入網站。</span><span class="sxs-lookup"><span data-stu-id="a6bde-346">The Login control provides for a mechanism to log into a Web site.</span></span> <span data-ttu-id="a6bde-347">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-347">It is covered in more detail in a later module.</span></span>

## <a name="loginview-control"></a><span data-ttu-id="a6bde-348">LoginView 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-348">LoginView Control</span></span>

<span data-ttu-id="a6bde-349">LoginView 控制項可讓顯示的不同使用者的登入狀態為基礎的範本。</span><span class="sxs-lookup"><span data-stu-id="a6bde-349">The LoginView control allows for the display of different templates based upon a user's login status.</span></span> <span data-ttu-id="a6bde-350">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-350">It is covered in more detail in a later module.</span></span>

## <a name="passwordrecovery-control"></a><span data-ttu-id="a6bde-351">Provider 控制項</span><span class="sxs-lookup"><span data-stu-id="a6bde-351">PasswordRecovery Control</span></span>

<span data-ttu-id="a6bde-352">Provider 控制項用來擷取忘記密碼，ASP.NET 應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="a6bde-352">The PasswordRecovery control is used to retrieve forgotten passwords by users of an ASP.NET application.</span></span> <span data-ttu-id="a6bde-353">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-353">It is covered in more detail in a later module.</span></span>

## <a name="loginstatus"></a><span data-ttu-id="a6bde-354">LoginStatus</span><span class="sxs-lookup"><span data-stu-id="a6bde-354">LoginStatus</span></span>

<span data-ttu-id="a6bde-355">LoginStatus 控制項顯示使用者的登入狀態。</span><span class="sxs-lookup"><span data-stu-id="a6bde-355">The LoginStatus control displays a user's login status.</span></span> <span data-ttu-id="a6bde-356">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-356">It is covered in more detail in a later module.</span></span>

## <a name="loginname"></a><span data-ttu-id="a6bde-357">LoginName</span><span class="sxs-lookup"><span data-stu-id="a6bde-357">LoginName</span></span>

<span data-ttu-id="a6bde-358">LoginName 控制項之後正在登入的 ASP.NET 應用程式顯示使用者的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="a6bde-358">The LoginName control displays a user's username after being logged into an ASP.NET application.</span></span> <span data-ttu-id="a6bde-359">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-359">It is covered in more detail in a later module.</span></span>

## <a name="createuserwizard"></a><span data-ttu-id="a6bde-360">CreateUserWizard</span><span class="sxs-lookup"><span data-stu-id="a6bde-360">CreateUserWizard</span></span>

<span data-ttu-id="a6bde-361">適用於 CreateUserWizard 是一個可設定的精靈，讓使用者能夠建立 ASP.NET 應用程式中使用的 ASP.NET 成員資格帳戶。</span><span class="sxs-lookup"><span data-stu-id="a6bde-361">The CreateUserWizard is a configurable wizard which gives users the ability to create an ASP.NET Membership account for use in an ASP.NET application.</span></span> <span data-ttu-id="a6bde-362">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-362">It is covered in more detail in a later module.</span></span>

## <a name="changepassword"></a><span data-ttu-id="a6bde-363">ChangePassword</span><span class="sxs-lookup"><span data-stu-id="a6bde-363">ChangePassword</span></span>

<span data-ttu-id="a6bde-364">ChangePassword 控制項可讓使用者變更其密碼，ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6bde-364">The ChangePassword control allows users to change their password for an ASP.NET application.</span></span> <span data-ttu-id="a6bde-365">它會在更新版本的模組中的更詳細地討論。</span><span class="sxs-lookup"><span data-stu-id="a6bde-365">It is covered in more detail in a later module.</span></span>

## <a name="various-webparts"></a><span data-ttu-id="a6bde-366">各種 web 組件</span><span class="sxs-lookup"><span data-stu-id="a6bde-366">Various WebParts</span></span>

<span data-ttu-id="a6bde-367">ASP.NET 2.0 提供了各種網頁組件。</span><span class="sxs-lookup"><span data-stu-id="a6bde-367">ASP.NET 2.0 ships with various Web Parts.</span></span> <span data-ttu-id="a6bde-368">這些都將涵蓋於更新版本的模組中的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a6bde-368">These will be covered in detail in a later module.</span></span>
