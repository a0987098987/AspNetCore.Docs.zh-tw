---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: 使用 CascadingDropDown 搭配資料庫 (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 48e52fce4e26ec08403c3d86a4c3f85c103baa82
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824227"
---
<a name="using-cascadingdropdown-with-a-database-vb"></a><span data-ttu-id="55041-103">使用 CascadingDropDown 搭配資料庫 (VB)</span><span class="sxs-lookup"><span data-stu-id="55041-103">Using CascadingDropDown with a Database (VB)</span></span>
====================
<span data-ttu-id="55041-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="55041-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="55041-105">[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="55041-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)</span></span>

> <span data-ttu-id="55041-106">在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="55041-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="55041-107">為了讓此做法能夠運作，必須先建立特殊的 web 服務。</span><span class="sxs-lookup"><span data-stu-id="55041-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="55041-108">總覽</span><span class="sxs-lookup"><span data-stu-id="55041-108">Overview</span></span>

<span data-ttu-id="55041-109">在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="55041-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="55041-110">（比方說，一份清單會提供一份我們狀態，而且下一個清單則填入該狀態中主要城市）。為了讓此做法能夠運作，必須先建立特殊的 web 服務。</span><span class="sxs-lookup"><span data-stu-id="55041-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="55041-111">步驟</span><span class="sxs-lookup"><span data-stu-id="55041-111">Steps</span></span>

<span data-ttu-id="55041-112">首先，資料來源是必要的。</span><span class="sxs-lookup"><span data-stu-id="55041-112">First of all, a data source is required.</span></span> <span data-ttu-id="55041-113">此範例使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。</span><span class="sxs-lookup"><span data-stu-id="55041-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="55041-114">資料庫 （包括 express edition） 的 Visual Studio 安裝的選擇性部分作業，因此也會提供個別下載底下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="55041-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="55041-115">AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (下載網址[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="55041-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="55041-116">若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en))，並將附加`AdventureWorks.mdf`資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="55041-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="55041-117">此範例中，我們假設 SQL Server 2005 Express Edition 的執行個體，會呼叫`SQLEXPRESS`位於與網頁伺服器; 相同的電腦上，這也是預設設定。</span><span class="sxs-lookup"><span data-stu-id="55041-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="55041-118">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="55041-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="55041-119">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但內&lt; `form` &gt;項目):</span><span class="sxs-lookup"><span data-stu-id="55041-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

<span data-ttu-id="55041-120">在下一個步驟中，兩個 DropDownList 控制項則是必要項目。</span><span class="sxs-lookup"><span data-stu-id="55041-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="55041-121">在此範例中，我們會使用 AdventureWorks 的廠商和連絡人資訊，因此我們建立一個清單用於可用的供應商，一個用於可用的連絡人：</span><span class="sxs-lookup"><span data-stu-id="55041-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

<span data-ttu-id="55041-122">然後，兩個 CascadingDropDown extender 必須新增至頁面。</span><span class="sxs-lookup"><span data-stu-id="55041-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="55041-123">其中一個填滿第一個 （廠商） 清單中，和另一個填滿第二個 （連絡人） 清單。</span><span class="sxs-lookup"><span data-stu-id="55041-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="55041-124">必須設定下列屬性：</span><span class="sxs-lookup"><span data-stu-id="55041-124">The following attributes must be set:</span></span>

- <span data-ttu-id="55041-125">`ServicePath`: 提供的清單項目在 web 服務的 URL</span><span class="sxs-lookup"><span data-stu-id="55041-125">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="55041-126">`ServiceMethod`： 提供的清單項目 web 方法</span><span class="sxs-lookup"><span data-stu-id="55041-126">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="55041-127">`TargetControlID`: ID 的下拉式清單</span><span class="sxs-lookup"><span data-stu-id="55041-127">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="55041-128">`Category`： 提交給 web 方法的呼叫時類別目錄資訊</span><span class="sxs-lookup"><span data-stu-id="55041-128">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="55041-129">`PromptText`： 以非同步方式從伺服器載入清單資料時顯示的文字</span><span class="sxs-lookup"><span data-stu-id="55041-129">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>
- <span data-ttu-id="55041-130">`ParentControlID`: (選擇性) 父下拉式清單會列出該觸發程序的載入目前的清單</span><span class="sxs-lookup"><span data-stu-id="55041-130">`ParentControlID`: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="55041-131">根據使用的程式設計語言，有問題的 web 服務的名稱會變更，但所有其他屬性值都相同。</span><span class="sxs-lookup"><span data-stu-id="55041-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="55041-132">以下是第一個下拉式清單中的 CascadingDropDown 項目：</span><span class="sxs-lookup"><span data-stu-id="55041-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

<span data-ttu-id="55041-133">控制項擴充項，如第二個清單需要設定`ParentControlID`屬性，讓載入供應商清單觸發程序中選取一個項目相關的連絡人清單中的項目。</span><span class="sxs-lookup"><span data-stu-id="55041-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

<span data-ttu-id="55041-134">實際的工作則會以下列方式設定 web 服務中完成。</span><span class="sxs-lookup"><span data-stu-id="55041-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="55041-135">請注意，`[ScriptService]`屬性時，ASP.NET AJAX 否則無法建立 JavaScript proxy 來存取 web 方法，從用戶端指令碼的程式碼。</span><span class="sxs-lookup"><span data-stu-id="55041-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

<span data-ttu-id="55041-136">CascadingDropDown 呼叫的 web 方法的簽章如下所示：</span><span class="sxs-lookup"><span data-stu-id="55041-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

<span data-ttu-id="55041-137">因此傳回的值必須是類型的陣列`CascadingDropDownNameValue`定義控制工具組。</span><span class="sxs-lookup"><span data-stu-id="55041-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="55041-138">`GetVendors()`方法是很容易就能實作： 程式碼會連接到 AdventureWorks 資料庫和查詢的前 25 個廠商。</span><span class="sxs-lookup"><span data-stu-id="55041-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="55041-139">中的第一個參數`CascadingDropDownNameValue`建構函式是標題的清單項目，而第二個它的值 (以 HTML 的 value 屬性&lt; `option` &gt;項目)。</span><span class="sxs-lookup"><span data-stu-id="55041-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="55041-140">程式碼如下：</span><span class="sxs-lookup"><span data-stu-id="55041-140">Here is the code:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

<span data-ttu-id="55041-141">取得供應商相關的連絡人 (方法名稱： `GetContactsForVendor()`) 需要一點技巧。</span><span class="sxs-lookup"><span data-stu-id="55041-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="55041-142">首先，您必須決定廠商的第一個下拉式清單中已選取。</span><span class="sxs-lookup"><span data-stu-id="55041-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="55041-143">控制工具組定義該工作的 helper 方法：`ParseKnownCategoryValuesString()`方法會傳回`StringDictionary`下拉式清單中資料的項目：</span><span class="sxs-lookup"><span data-stu-id="55041-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

<span data-ttu-id="55041-144">基於安全性理由，必須先驗證這項資料。</span><span class="sxs-lookup"><span data-stu-id="55041-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="55041-145">因此，如果沒有廠商項目 (因為`Category`的第一個 CascadingDropDown 元素的屬性設定為`"Vendor"`)，可能會擷取所選的廠商識別碼：</span><span class="sxs-lookup"><span data-stu-id="55041-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

<span data-ttu-id="55041-146">其餘的方法就相當簡單易懂。</span><span class="sxs-lookup"><span data-stu-id="55041-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="55041-147">供應商的識別碼可做為參數的 SQL 查詢，該廠商會擷取相關聯的所有連絡人。</span><span class="sxs-lookup"><span data-stu-id="55041-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="55041-148">同樣地，方法會傳回型別的陣列`CascadingDropDownNameValue`。</span><span class="sxs-lookup"><span data-stu-id="55041-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

<span data-ttu-id="55041-149">載入 ASP.NET 頁面中，並在一段時間後，供應商清單會填入 25 項目。</span><span class="sxs-lookup"><span data-stu-id="55041-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="55041-150">挑選一個項目，並注意第二個下拉式清單中填入資料的方式。</span><span class="sxs-lookup"><span data-stu-id="55041-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


<span data-ttu-id="55041-151">[![第一份清單會自動填入](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="55041-151">[![The first list is filled automatically](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)</span></span>

<span data-ttu-id="55041-152">第一份清單會自動填入 ([按一下以檢視完整大小的影像](using-cascadingdropdown-with-a-database-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="55041-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-vb/_static/image3.png))</span></span>


<span data-ttu-id="55041-153">[![第二個清單是根據第一個清單中的選取範圍填滿](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="55041-153">[![The second list is filled according to the selection in the first list](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)</span></span>

<span data-ttu-id="55041-154">第二個清單根據第一個清單中的選取範圍填滿 ([按一下以檢視完整大小的影像](using-cascadingdropdown-with-a-database-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="55041-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="55041-155">[上一頁](filling-a-list-using-cascadingdropdown-vb.md)
> [下一頁](presetting-list-entries-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="55041-155">[Previous](filling-a-list-using-cascadingdropdown-vb.md)
[Next](presetting-list-entries-with-cascadingdropdown-vb.md)</span></span>
