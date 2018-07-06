---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: 如何使用下拉式方塊控制項？ (VB) | Microsoft Docs
author: microsoft
description: 下拉式方塊會是結合使用者可以從中選擇的選項清單中的文字方塊中的彈性以 ASP.NET AJAX 控制項。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3241641b3e136b24c8cff75026e496ddf8eb04ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371900"
---
<a name="how-do-i-use-the-combobox-control-vb"></a>如何使用下拉式方塊控制項？ (VB)
====================
by [Microsoft](https://github.com/microsoft)

> 下拉式方塊會是結合使用者可以從中選擇的選項清單中的文字方塊中的彈性以 ASP.NET AJAX 控制項。


本教學課程的目標在於說明 AJAX 控制項工具組下拉式方塊控制項。 下拉式方塊的運作方式類似標準的 ASP.NET DropDownList 控制項和文字方塊控制項的結合。 您可以從預先存在的項目清單中選取，或輸入新的項目。

下拉式方塊類似於 AutoComplete 控制項擴充項，但不同的案例中使用的控制項。 AutoComplete 擴充項會查詢 web 服務，以取得相符的項目。 下拉式方塊控制項，相較之下，會使用一組項目初始化。 使用自動完成擴充項會適合您正在使用大量 （數百萬計的汽車組件） 的資料時使用下拉式方塊控制項有意義時使用較少的資料部分 （幾十個汽車部分）。

## <a name="selecting-from-a-static-list-of-items"></a>從靜態項目清單中選取

可讓 s 使用下拉式方塊控制項的簡單範例開始。 假設您想要顯示在下拉式清單中的靜態項目清單。 不過，您會想要保持開啟的清單不完整的可能性。 您想要允許使用者清單中輸入自訂值。

我們將建立新的 ASP.NET Web Form 頁面和頁面中，使用下拉式方塊控制項。 專案中加入新的 ASP.NET 頁面，並切換至 [設計] 檢視中。

如果您想要使用 ComboBox 控制項頁面中您必須將 ScriptManager 控制項加入至頁面。 將 ScriptManager 控制項來 [AJAX 延伸模組] 索引標籤拖曳至設計工具介面。 您應該在頁面的頂端加入 ScriptManager 控制項您可以將它立即下方開啟伺服器端&lt;表單&gt;標記。

接下來，拖曳到頁面的下拉式方塊控制項。 您可以在 [工具箱] 中與其他 AJAX Control Toolkit 控制項及控制項擴充項 （請參閱 [圖 1]） 中找到下拉式方塊控制項。


[![簡單的表單建立名片](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**圖 01**： 從 [工具箱] 中選取下拉式方塊控制項 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image2.png))


我們將使用下拉式方塊控制項來顯示靜態選項清單。 使用者可以從三種選項的清單中選取食物的 spiciness 的特定層級： 輕微、 中型和經常性存取 （請參閱 圖 2）。


[![從靜態項目清單中選取](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**圖 02**： 選取靜態清單的項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image4.png))


有兩種方式，您可以加入這些選項的下拉式方塊控制項。 首先，您將滑鼠停留在設計檢視中的控制項時，請選取 [編輯選項] 工作選項，並開啟項目編輯器 （請參閱 [圖 3]）。


[![編輯下拉式方塊項目](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**圖 03**： 編輯下拉式方塊項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image6.png))


第二個選項是新增的開頭和結尾之間的項目清單&lt;asp: ComboBox&gt;來源檢視中的標籤。 [列表 1] 頁面包含 [更新] 下拉式方塊具有項目清單。

**列表 1-Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

當您在 列表 1 中開啟頁面時，您可以從下拉式方塊中選取其中一個預先存在的選項。 換句話說，下拉式方塊的運作方式一樣 DropDownList 控制項。

不過，您也可以選擇輸入新選擇 （例如，超級辣味） 不在現有的清單中。 因此，下拉式方塊也的運作方式類似文字方塊控制項。

不論您是否選取既存的項目，或輸入自訂的項目中，當您提交表單時，您的選擇會顯示在標籤控制項。 當您提交表單，btnSubmit\_Click 處理常式會執行，並更新標籤 （請參閱 圖 4）。


[![顯示選取的項目](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**圖 04**： 顯示選取的項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image8.png))


下拉式方塊來擷取選取的項目表單送出之後，支援 DropDownList 控制項相同的屬性：

- SelectedItem.Text-顯示選取項目的 Text 屬性的值。
- SelectedItem.Value-顯示選取的項目的 Value 屬性的值，或顯示下拉式方塊中輸入的文字。
- SelectedValue-相同 SelectedItem.Value 不同之處在於此屬性可讓您指定預設值 （初始） 選取的項目。

如果您輸入到下拉式方塊然後自訂的選擇是自訂的選擇會指派給 SelectedItem.Text] 和 [SelectedItem.Value 屬性中。

## <a name="selecting-the-list-of-items-from-the-database"></a>從資料庫中選取項目的清單

您可以從資料庫擷取下拉式方塊會顯示的項目的清單。 例如，您可以將下拉式方塊 SqlDataSource 控制項、 ObjectDataSource 控制項、 LinqDataSource，或 EntityDataSource 繫結。

假設您想要顯示在下拉式方塊的電影清單。 您想要從電影資料庫資料表中擷取的電影清單。 請依照下列步驟：

1. 建立名為 Movies.aspx 的頁面
2. 在拖曳到頁面的 [工具箱] 拖曳 [AJAX 延伸模組] 索引標籤底下 ScriptManager 加入網頁 ScriptManager 控制項。
3. 下拉式方塊拖曳到頁面，頁面將下拉式方塊控制項。
4. 在 [設計] 檢視中，將滑鼠移下拉式方塊控制項，然後選取**選擇資料來源**工作選項 （請參閱 [圖 5]）。 資料來源組態精靈會啟動。
5. 在 **選擇資料來源**步驟中，選取&lt;新的資料來源&gt;選項。
6. 在 **選擇資料來源類型**步驟中，選取資料庫。
7. 在 **選擇資料連接**步驟中，選取您的資料庫 (例如 MoviesDB.mdf)。
8. 在 **儲存連接字串儲存到應用程式組態檔**步驟中，選取要儲存您的連接字串的選項。
9. 在 **設定 Select 陳述式**步驟中，選取的電影資料庫資料表，再選取所有資料行。
10. 在 **測試查詢**步驟中，按一下 完成 按鈕。
11. 回到**選擇資料來源**步驟中，選取要顯示之欄位的標題資料行和資料的識別碼資料行欄位 （請參閱圖）。
12. 按一下 [確定] 按鈕，即可關閉精靈。


[![選擇資料來源](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**圖 05**： 選擇資料來源 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image10.png))


[![選擇資料的文字和值欄位](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**圖 06**： 選擇資料的文字和值欄位 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image12.png))


完成上述步驟之後，下拉式方塊繫結代表影片的電影資料庫資料表從 SqlDataSource 控制項。 頁面的來源是由 （我已清除的格式有點） 的列表 2 所示。

**列表 2-Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

請注意下拉式方塊控制項都有指向 SqlDataSource 控制項 DataSourceID 屬性。 當您開啟網頁瀏覽器中時，會顯示的資料庫中的電影清單 （請參閱 圖 7）。 您可以挑選其中一個影片，以從清單或下拉式方塊中輸入電影中輸入新的電影。


[![顯示電影的清單](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**圖 07**： 顯示的電影清單 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>設定 DropDownStyle

您可以使用下拉式方塊 DropDownStyle 屬性來變更下拉式方塊的行為。 這個屬性會接受那里可能的值：

- 下拉式清單-（預設值） 的下拉式方塊會顯示在下拉式清單中列出當您按一下箭號，您可以輸入自訂值。
- 簡單-下拉式方塊會自動顯示下拉式清單中，您可以輸入自訂值。
- DropDownList-下拉式方塊的運作方式類似 DropDownList 控制項。

不同之間下拉式清單中，並且會簡單時，會顯示的項目清單。 在簡單的情況下，請立即將焦點移至 ComboBox，會顯示清單。 在下拉式清單中，您必須按一下箭號，若要查看的項目清單。

DropDownList 值會造成下拉式方塊控制項，就像標準的 DropDownList 控制項一樣運作。 不過，還有一個重要的差異。 舊版 Internet Explorer 會顯示無限的 z 軸索引的 DropDownList 控制項，控制項便會出現在其前面放置任何控制項之前。 因為下拉式方塊會呈現 HTML &lt;div&gt;而不是 HTML 標記&lt;選取&gt;標記，下拉式方塊正確尊重疊置順序。

## <a name="setting-the-autocompletemode"></a>設定 AutoCompleteMode

您可以使用下拉式方塊 AutoCompleteMode 屬性來指定當有人下拉式方塊中輸入文字時，會發生什麼事。 這個屬性接受下列可能的值：

- 無-（預設值） 下拉式方塊不會提供任何自動完成行為。
- 建議-下拉式方塊會顯示清單，它會反白顯示清單中的符合項目 （請參閱 圖 8）。
- 附加-下拉式方塊不會顯示清單和附加相符的項目，從清單中拖曳至您輸入的內容 （請參閱 圖 9）。
- 同時 SuggestAppend-下拉式方塊會顯示的清單，並將附加相符的項目，從清單中拖曳至您輸入的內容 （請參閱 圖 10）。


[![下拉式方塊可讓建議](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**圖 08**: 下拉式方塊會建議 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image16.png))


[![下拉式方塊將相符的文字附加](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**圖 09**： 下拉式方塊將相符的文字附加 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image18.png))


[![下拉式方塊的建議，並將附加](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**圖 10**： 建議的下拉式方塊，並附加 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image20.png))


## <a name="summary"></a>總結

在本教學課程中，您已了解如何使用下拉式方塊控制項來顯示一組固定的項目。 我們繫結下拉式方塊控制項都設為靜態設定的項目和資料庫資料表。 最後，您已了解如何藉由設定其 DropDownStyle 和 AutoCompleteMode 屬性修改下拉式方塊的行為。

> [!div class="step-by-step"]
> [上一步](how-do-i-use-the-combobox-control-cs.md)
