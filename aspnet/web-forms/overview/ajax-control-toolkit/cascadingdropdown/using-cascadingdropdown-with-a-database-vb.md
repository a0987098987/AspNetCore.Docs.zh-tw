---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: "資料庫 (VB) 搭配使用 CascadingDropDown |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯 anoth 中的值..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 65b9a499dd9b500338ccdb90e56b742ff50a1024
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>CascadingDropDown 資料庫使用的 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 為了讓此工作，您必須建立特殊的 web 服務。


## <a name="overview"></a>概觀

AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 （比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。為了讓此工作，您必須建立特殊的 web 服務。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。

此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。 如果您的設定不同，您必須調整資料庫的連接資訊。

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部&lt; `form` &gt;項目):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

在下一個步驟中，兩個 DropDownList 控制項都必須的。 在此範例中，我們使用 AdventureWorks 的廠商和連絡人資訊，因此我們建立一個清單可用的供應商，另一個可用的連絡人：

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

然後，兩個 CascadingDropDown extender 必須加入至頁面。 其中一個填滿第一個 （廠商） 清單中，和另一個填滿第二個 （連絡人） 清單。 必須設定下列屬性：

- `ServicePath`： 提供的清單項目 web 服務 URL
- `ServiceMethod`: Web 方法傳遞之清單項目
- `TargetControlID`: ID 的下拉式清單
- `Category`： 提交給 web 方法的呼叫時類別目錄資訊
- `PromptText`： 當以非同步方式從伺服器載入清單資料時顯示的文字
- `ParentControlID`: (選擇性) 父下拉式清單目前清單的觸發程序載入

根據使用的程式設計語言，web 服務有問題的名稱變更，但是所有其他屬性值都相同。 以下是第一個下拉式清單中的 CascadingDropDown 項目：

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

第二個清單控制項擴充項需要設定`ParentControlID`，以便在載入的供應商清單觸發程序中選取一個項目相關聯的連絡人清單中的項目。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

實際工作，然後是在 web 服務設定，如下所示。 請注意，`[ScriptService]`屬性時，ASP.NET AJAX 否則無法建立要從用戶端指令碼的程式碼存取的 web 方法的 JavaScript proxy。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

呼叫 CascadingDropDown 的 web 方法的簽章如下所示：

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

因此傳回的值必須是類型的陣列`CascadingDropDownNameValue`定義控制項的工具組。 `GetVendors()`方法是實作相當簡單： 程式碼會連接到 AdventureWorks 資料庫和查詢的前 25 個廠商。 中的第一個參數`CascadingDropDownNameValue`建構函式是標題的清單項目，第二個它的值 (以 HTML 的 value 屬性&lt; `option` &gt;項目)。 下列程式碼：

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

供應商取得相關的連絡人 (方法名稱： `GetContactsForVendor()`) 棘手。 首先，您必須決定廠商的第一個下拉式清單中已選取。 Control Toolkit 定義該工作的 helper 方法：`ParseKnownCategoryValuesString()`方法會傳回`StringDictionary`下拉式清單中資料的項目：

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

基於安全性理由，必須先驗證這項資料。 因此，如果廠商項目 (因為`Category`CascadingDropDown，第一個元素的屬性設定為`"Vendor"`)，可擷取所選的廠商識別碼：

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

方法的其餘部分是相當簡單，則。 廠商識別碼用於做為參數的 SQL 查詢，該廠商會擷取所有相關聯的連絡人。 同樣地，方法會傳回型別的陣列`CascadingDropDownNameValue`。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

載入 ASP.NET 頁面中，並在短時間之後廠商清單填滿 25 的項目。 選取一個項目，並注意第二個下拉式清單填入資料的方式。


[![自動填滿第一個清單中](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

自動填滿第一個清單中 ([按一下以檢視完整大小的影像](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![第二個清單是根據第一個清單中的選取範圍填滿](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

第二個清單根據第一個清單中的選取範圍填滿 ([按一下以檢視完整大小的影像](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

>[!div class="step-by-step"]
[上一頁](filling-a-list-using-cascadingdropdown-vb.md)
[下一頁](presetting-list-entries-with-cascadingdropdown-vb.md)
