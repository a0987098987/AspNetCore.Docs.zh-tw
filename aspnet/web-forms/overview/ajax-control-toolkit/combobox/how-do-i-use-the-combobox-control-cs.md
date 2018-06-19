---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: 如何使用下拉式方塊控制項？ (C#) | Microsoft Docs
author: microsoft
description: 下拉式方塊是一種 ASP.NET AJAX 控制項，結合的使用者可以從中選擇的選項清單中的文字方塊中的彈性。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 20afd7334437a021f6f68216f84406eef5ea65c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875223"
---
<a name="how-do-i-use-the-combobox-control-c"></a>如何使用下拉式方塊控制項？ (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 下拉式方塊是一種 ASP.NET AJAX 控制項，結合的使用者可以從中選擇的選項清單中的文字方塊中的彈性。


本教學課程的目標是來說明 AJAX 控制項 Toolkit 下拉式方塊控制項。 下拉式方塊的運作方式類似標準 ASP.NET DropDownList 控制項和文字方塊控制項之間的組合。 您可以從預先存在的項目清單中選取或輸入新的項目。

下拉式方塊類似自動完成控制項擴充項，但會在不同案例中使用的控制項。 自動完成 extender 查詢 web 服務以取得相符的項目。 下拉式方塊控制項，相較之下，以初始化一組項目。 使用自動完成 extender 使意義上，當您正在使用的大量資料 （包含數百萬個汽車組件） 時使用下拉式方塊控制項合理使用較少的資料時 （數十個汽車組件）。

## <a name="selecting-from-a-static-list-of-items"></a>從靜態項目清單中選取

可讓 s 開頭使用下拉式方塊控制項的簡單範例。 假設您想要顯示在下拉式清單中的靜態清單的項目。 不過，您會想要保持開啟的清單不完整的可能性。 您想要讓使用者輸入自訂值的清單中。

我們 ll 建立新的 ASP.NET Web Form 頁面和頁面中使用下拉式方塊控制項。 專案中加入新的 ASP.NET 網頁，並切換至 [設計] 檢視中。

如果您想要使用下拉式方塊控制項頁面中您必須將 ScriptManager 控制項加入至頁面。 將 [AJAX 擴充功能] 索引標籤 from beneath ScriptManager 控制項拖曳至設計工具介面。 您應該在頁面頂端的; 加入 ScriptManager 控制項您可以將它立即開啟伺服器端下方&lt;表單&gt;標記。

下一步，拖曳到頁面的下拉式方塊控制項。 您可以使用其他 AJAX Control Toolkit 控制項和控制項擴充項 （請參閱圖 1） 的 [工具箱] 中找到 ComboBox 控制項。


[![簡單的表單建立名片](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**圖 01**： 從 [工具箱] 中選取 ComboBox 控制項 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image2.png))


我們 ll 用於 ComboBox 控制項顯示靜態選項清單。 使用者可以從三個選項的清單中選取食物的 spiciness 特定層級： 不雅、 中型和作用 （請參閱圖 2）。


[![從靜態項目清單中選取](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**圖 02**： 從靜態項目清單中選取 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image4.png))


有兩種方式，您可以將這些選擇加入的下拉式方塊控制項。 首先，當滑鼠停留在 [設計] 檢視中的控制項選取編輯選項工作選項，並開啟項目編輯器 （請參閱圖 3）。


[![編輯下拉式方塊項目](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**圖 03**： 編輯下拉式方塊項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image6.png))


第二個選項是加入在開頭和結尾之間的項目清單&lt;asp: ComboBox&gt;來源檢視中的標籤。 程式碼範例 1 中的頁面包含更新具有項目清單的下拉式方塊。

**Listing 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

當您開啟的頁面清單 1 中時，您可以從下拉式方塊選取其中一個預先存在的選項。 換句話說，下拉式方塊的運作方式類似 DropDownList 控制項。

不過，您也可以輸入選項 （例如超級辣味） 不在現有的清單中的選擇。 因此，下拉式方塊也運作方式類似的 TextBox 控制項。

不論您是否選擇既存的項目，或輸入自訂的項目，當您送出表單，您的選擇會出現在 label 控制項。 當您提交表單，btnSubmit\_按一下 執行處理常式，並更新標籤 （請參閱圖 4）。


[![顯示選取的項目](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**圖 04**： 顯示選取的項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image8.png))


下拉式方塊支援 DropDownList 控制項相同的內容之後提交表單時，擷取選取的項目：

- SelectedItem.Text-顯示選取之項目的文字屬性的值。
- SelectedItem.Value-顯示選取之項目的值屬性的值，或顯示下拉式方塊中輸入的文字。
- SelectedValue-相同 SelectedItem.Value 不同之處在於此屬性可讓您指定預設值 （初始） 選取的項目。

如果您輸入至下拉式方塊然後自訂選項的自訂選項會指派給 SelectedItem.Text 和 SelectedItem.Value 屬性。

## <a name="selecting-the-list-of-items-from-the-database"></a>從資料庫中選取項目的清單

您可以從資料庫擷取下拉式方塊會顯示的項目的清單。 例如，您可以繫結下拉式方塊 SqlDataSource 控制項、 ObjectDataSource 控制項、 LinqDataSource 或 EntityDataSource。

假設您想要在下拉式方塊中顯示的電影清單。 您想要從電影資料庫資料表中擷取的電影清單。 請依照下列步驟：

1. 建立名為 Movies.aspx 的頁面
2. 加入至頁面 ScriptManager 控制項拖曳到頁面的 [工具箱] 中拖曳 [AJAX 擴充功能] 索引標籤底下 ScriptManager。
3. 加入頁面的下拉式方塊控制項拖曳到頁面的下拉式方塊。
4. 在設計檢視中，滑鼠停留下拉式方塊控制項，然後選取**選擇資料來源**工作選項 （請參閱圖 5）。 資料來源組態精靈會啟動。
5. 在**選擇資料來源**步驟中，選取&lt;新的資料來源&gt;選項。
6. 在**選擇資料來源類型**步驟中，選取資料庫。
7. 在**選擇資料連接**步驟中，選取您的資料庫 (例如，MoviesDB.mdf)。
8. 在**連接字串儲存到應用程式組態檔**步驟中，選取要儲存您的連接字串選項。
9. 在**設定 Select 陳述式**步驟、 選取影片資料庫資料表，再選取所有資料行。
10. 在**測試查詢**步驟中，按一下 [完成] 按鈕。
11. 回到**選擇資料來源**步驟中，選取要顯示之欄位的標題資料行和資料的識別碼資料行欄位 （請參閱圖）。
12. 按一下 [確定] 按鈕以關閉精靈。


[![選擇資料來源](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**圖 05**： 選擇資料來源 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![選擇資料的文字和值欄位](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**圖 06**： 選擇資料的文字和值欄位 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image12.png))


完成上述步驟之後，下拉式方塊都代表電影電影資料庫資料表中的 SqlDataSource 控制項繫結。 來源頁面看起來像列出 2 （我清除有點格式）。

**列出 2-Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

請注意下拉式方塊控制項有指向 SqlDataSource 控制項 DataSourceID 屬性。 當您開啟的網頁瀏覽器中時，會顯示的影片，從資料庫清單 （請參閱圖 7）。 您可以挑選其中一個電影從清單或下拉式方塊中輸入電影中輸入新的電影。


[![顯示電影的清單](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**圖 07**： 顯示的電影清單 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>設定 DropDownStyle

您可以使用下拉式方塊 DropDownStyle 屬性，變更下拉式方塊的行為。 這個屬性可以接受那里可能的值：

- 在下拉式清單中的 （預設值） 的下拉式方塊會顯示下拉式清單，當您按一下箭號，而且您可以輸入自訂值。
- 簡單-下拉式方塊會自動顯示下拉式清單中，您可以輸入自訂值。
- DropDownList-下拉式方塊的運作方式類似 DropDownList 控制項。

不同之間下拉式清單中且簡單時，會顯示的項目清單。 在簡單的情況下立即將焦點移到下拉式方塊會顯示清單。 在下拉式清單中，您必須按一下箭頭來查看的項目清單。

DropDownList 值導致 ComboBox 控制項就像標準的 DropDownList 控制項一樣運作。 不過，是一個重要的差異。 舊版 Internet Explorer 會顯示具有無限 z-索引 DropDownList 控制項，控制項就會出現任何控制項放置在它前面的前面。 因為下拉式方塊可呈現 HTML &lt;div&gt;標記，而不是 HTML&lt;選取&gt;標記，下拉式方塊正確尊重疊置順序。

## <a name="setting-the-autocompletemode"></a>設定 AutoCompleteMode

您可以使用下拉式方塊 AutoCompleteMode 屬性來指定有人類型到下拉式方塊的文字時，會發生什麼事。 這個屬性可以接受下列可能的值：

- 無-（預設值） 下拉式方塊不會提供任何自動完成行為。
- 建議的下拉式方塊會顯示清單，它會反白顯示清單中的符合項目 （請參閱圖 8）。
- 附加-下拉式方塊不會顯示清單並附加到您輸入的內容 （請參閱圖 9） 清單中的符合項目。
- SuggestAppend-下拉式方塊會顯示清單並將附加到您輸入的內容 （請參閱圖 10） 清單中的符合項目。


[![下拉式方塊可讓提供的建議](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**圖 08**: 下拉式方塊會建議 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![下拉式方塊將相符的文字附加](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**圖 09**： 下拉式方塊將相符的文字附加 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![下拉式方塊的建議，並將附加](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**圖 10**: ComboBox 建議，並將附加 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>總結

在本教學課程中，您學會如何使用下拉式方塊控制項來顯示一組固定的項目。 我們繫結下拉式方塊控制項，同時為靜態設定的項目與資料庫資料表。 最後，您會學到如何藉由設定其 DropDownStyle 而且 AutoCompleteMode 屬性修改下拉式方塊的行為。

> [!div class="step-by-step"]
> [下一步](how-do-i-use-the-combobox-control-vb.md)
