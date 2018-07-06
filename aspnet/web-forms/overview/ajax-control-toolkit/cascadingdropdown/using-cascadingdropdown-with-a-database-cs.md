---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: 使用 CascadingDropDown 搭配資料庫 (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 9930319d2f9443ef3b50a87c7dd3d42b879168c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818744"
---
<a name="using-cascadingdropdown-with-a-database-c"></a>使用 CascadingDropDown 搭配資料庫 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> 在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 為了讓此做法能夠運作，必須先建立特殊的 web 服務。


## <a name="overview"></a>總覽

在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 （比方說，一份清單會提供一份我們狀態，而且下一個清單則填入該狀態中主要城市）。為了讓此做法能夠運作，必須先建立特殊的 web 服務。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 此範例使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫 （包括 express edition） 的 Visual Studio 安裝的選擇性部分作業，因此也會提供個別下載底下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (下載網址[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en))，並將附加`AdventureWorks.mdf`資料庫檔案。

此範例中，我們假設 SQL Server 2005 Express Edition 的執行個體，會呼叫`SQLEXPRESS`位於與網頁伺服器; 相同的電腦上，這也是預設設定。 如果您的設定不同，您必須調整資料庫的連接資訊。

若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但內&lt; `form` &gt;項目):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

在下一個步驟中，兩個 DropDownList 控制項則是必要項目。 在此範例中，我們會使用 AdventureWorks 的廠商和連絡人資訊，因此我們建立一個清單用於可用的供應商，一個用於可用的連絡人：

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

然後，兩個 CascadingDropDown extender 必須新增至頁面。 其中一個填滿第一個 （廠商） 清單中，和另一個填滿第二個 （連絡人） 清單。 必須設定下列屬性：

- `ServicePath`: 提供的清單項目在 web 服務的 URL
- `ServiceMethod`： 提供的清單項目 web 方法
- `TargetControlID`: ID 的下拉式清單
- `Category`： 提交給 web 方法的呼叫時類別目錄資訊
- `PromptText`： 以非同步方式從伺服器載入清單資料時顯示的文字
- `ParentControlID`: (選擇性) 父下拉式清單會列出該觸發程序的載入目前的清單

根據使用的程式設計語言，有問題的 web 服務的名稱會變更，但所有其他屬性值都相同。 以下是第一個下拉式清單中的 CascadingDropDown 項目：

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

控制項擴充項，如第二個清單需要設定`ParentControlID`屬性，讓載入供應商清單觸發程序中選取一個項目相關的連絡人清單中的項目。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

實際的工作則會以下列方式設定 web 服務中完成。 請注意，`[ScriptService]`屬性時，ASP.NET AJAX 否則無法建立 JavaScript proxy 來存取 web 方法，從用戶端指令碼的程式碼。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

CascadingDropDown 呼叫的 web 方法的簽章如下所示：

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

因此傳回的值必須是類型的陣列`CascadingDropDownNameValue`定義控制工具組。 `GetVendors()`方法是很容易就能實作： 程式碼會連接到 AdventureWorks 資料庫和查詢的前 25 個廠商。 中的第一個參數`CascadingDropDownNameValue`建構函式是標題的清單項目，而第二個它的值 (以 HTML 的 value 屬性&lt; `option` &gt;項目)。 程式碼如下：

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

取得供應商相關的連絡人 (方法名稱： `GetContactsForVendor()`) 需要一點技巧。 首先，您必須決定廠商的第一個下拉式清單中已選取。 控制工具組定義該工作的 helper 方法：`ParseKnownCategoryValuesString()`方法會傳回`StringDictionary`下拉式清單中資料的項目：

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

基於安全性理由，必須先驗證這項資料。 因此，如果沒有廠商項目 (因為`Category`的第一個 CascadingDropDown 元素的屬性設定為`"Vendor"`)，可能會擷取所選的廠商識別碼：

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

其餘的方法就相當簡單易懂。 供應商的識別碼可做為參數的 SQL 查詢，該廠商會擷取相關聯的所有連絡人。 同樣地，方法會傳回型別的陣列`CascadingDropDownNameValue`。

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

載入 ASP.NET 頁面中，並在一段時間後，供應商清單會填入 25 項目。 挑選一個項目，並注意第二個下拉式清單中填入資料的方式。


[![第一份清單會自動填入](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

第一份清單會自動填入 ([按一下以檢視完整大小的影像](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![第二個清單是根據第一個清單中的選取範圍填滿](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

第二個清單根據第一個清單中的選取範圍填滿 ([按一下以檢視完整大小的影像](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

> [!div class="step-by-step"]
> [上一頁](filling-a-list-using-cascadingdropdown-cs.md)
> [下一頁](presetting-list-entries-with-cascadingdropdown-cs.md)
