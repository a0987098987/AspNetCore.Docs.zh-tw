---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: 驗證與 IDataErrorInfo 介面 (C#) |Microsoft Docs
author: StephenWalther
description: Stephen Walther 會示範如何藉由在模型類別中實作的 IDataErrorInfo 介面中顯示自訂的驗證錯誤訊息。
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: d3d3e379e5b2cfdd1385d724c0d9bf2a83ce339a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836023"
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>驗證與 IDataErrorInfo 介面 (C#)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther 會示範如何藉由在模型類別中實作的 IDataErrorInfo 介面中顯示自訂的驗證錯誤訊息。


本教學課程的目標是說明其中一個方法來執行驗證的 ASP.NET MVC 應用程式中。 您了解如何防止有人提出 HTML 表單，而不提供所需的表單欄位的值。 在本教學課程中，您將了解如何使用 IErrorDataInfo 介面來執行驗證。

## <a name="assumptions"></a>假設

在本教學課程中，我將使用 MoviesDB 資料庫及電影資料庫資料表。 此資料表具有下列資料行：

<a id="0.5_table01"></a>


| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | Int | False |
| 標題 | Nvarchar(100) | False |
| 總監 | Nvarchar(100) | False |
| DateReleased | DateTime | False |


在本教學課程中，我可以使用 Microsoft Entity Framework 產生我的資料庫模型類別。 Entity Framework 所產生的電影類別會顯示在 圖 1。


[![電影實體](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**圖 01**: 電影實體 ([按一下以檢視完整大小的影像](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> 若要深入瞭解如何使用 Entity Framework 來產生您的資料庫模型類別，請參閱我的教學課程中，標題為 「 使用 Entity Framework 建立模型類別。


## <a name="the-controller-class"></a>控制器類別

我們會使用清單電影主控制器，並建立新的電影。 此類別的程式碼會包含在程式碼範例 1。

**列表 1-Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

列表 1 中的首頁控制器類別包含兩個 create （） 動作。 第一個動作顯示的 HTML 表單建立新的電影。 第二個 create （） 動作會執行新電影的實際插入至資料庫。 第一個 create （） 動作所顯示的表單提交到伺服器時，會叫用第二個 create （） 動作。

請注意第二個 create （） 動作，包含下列程式碼行：

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

驗證錯誤時，則 IsValid 屬性會傳回 false。 在此情況下，會重新顯示建立檢視，其中包含用於建立電影的 HTML 表單。

## <a name="creating-a-partial-class"></a>建立部分類別

影片類別是由 Entity Framework 產生的。 如果您展開 MoviesDBModel.edmx 檔案，在 方案總管 視窗中的，並在程式碼編輯器中開啟 MoviesDBModel.Designer.cs 檔案，您可以看到電影類別的程式碼 （請參閱 圖 2）。


[![電影實體的程式碼](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**圖 02**： 電影實體的程式碼 ([按一下以檢視完整大小的影像](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


影片類別是部分類別。 這表示我們可以加入至擴充功能的電影類別同名的其他部分類別。 我們會將我們的驗證邏輯加入新的部分類別。

在 列表 2 中新增的類別，到 Models 資料夾。

**列表 2-Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

請注意，在 列表 2 中的類別包含*部分*修飾詞。 任何方法或屬性，您將加入這個類別會成為 Entity Framework 所產生之電影類別的一部分。

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>新增 OnChanging 和 OnChanged 部分方法

當 Entity Framework 產生的實體類別時，Entity Framework 部分方法類別會自動新增。 Entity Framework 會產生 OnChanging 和 OnChanged 對應類別的每一個屬性的部分方法。

如果影片類別是 Entity Framework 會建立下列方法：

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

OnChanging 方法稱為權限，才會變更對應的屬性。 變更屬性之後，稱為右 OnChanged 方法。

您可以利用這些部分方法，將驗證邏輯新增至電影類別。 更新列表 3 中的電影類別會確認 標題 和 導演屬性，會指派非空值。

> [!NOTE] 
> 
> 部分方法是在您不需要實作類別中定義的方法。 如果您未實作部分方法，編譯器會移除方法簽章，並因此方法的所有呼叫都都沒有與部分方法相關聯的執行階段成本。 在 Visual Studio 程式碼編輯器 」 中，您可以藉由輸入關鍵字加入部分方法*部分*後面空間，以檢視來實作的部分清單。


**列表 3-Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

比方說，如果您嘗試將空字串指派給 Title 屬性，則錯誤訊息指派給名為字典\_錯誤。

此時，實際上沒有任何動靜當您將空字串指派給 Title 屬性，並將錯誤加入至私用\_錯誤欄位。 我們必須實作 IDataErrorInfo 介面，公開 （expose） 這些的驗證錯誤，以 ASP.NET MVC 架構。

## <a name="implementing-the-idataerrorinfo-interface"></a>實作 IDataErrorInfo 介面

IDataErrorInfo 介面已自第一版.NET framework 的一部分。 這個介面是一個非常簡單的介面：

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

如果類別實作的 IDataErrorInfo 介面，ASP.NET MVC 架構會使用此介面，建立類別的執行個體時。 例如，主控制器 create （） 動作會接受影片類別的執行個體：

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

ASP.NET MVC 架構會建立使用模型繫結 (DefaultModelBinder) 傳遞給 create （） 動作電影的執行個體。 模型繫結器會負責建立電影物件的執行個體繫結至電影物件的執行個體的 HTML 表單欄位。

DefaultModelBinder 會偵測類別實作的 IDataErrorInfo 介面。 如果類別實作此介面的模型繫結器會叫用類別的每一個屬性的 IDataErrorInfo.this 索引子。 如果索引子會傳回錯誤訊息的模型繫結器就會加入此錯誤訊息，來自動建立模型的狀態。

DefaultModelBinder 也會檢查 IDataErrorInfo.Error 屬性。 這個屬性被要代表與類別相關聯的非屬性特定的驗證錯誤。 例如，您可能要強制套用驗證規則，取決於影片類別的多個屬性的值。 在此情況下，您也會從 [錯誤] 屬性中傳回驗證錯誤。

在 列表 4 中更新的電影類別會實作 IDataErrorInfo 介面。

**列表 4-Models\Movie.cs （實作 IDataErrorInfo）**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

在 [列表 4] 的索引子屬性會檢查\_錯誤集合，以查看它是否包含索引鍵對應至屬性名稱傳遞給索引子。 如果不沒有與屬性有關聯任何驗證錯誤則會傳回空字串。

您不需要修改 Home 控制器中使用修改過的電影類別的任何方法。 圖 3 中所顯示的網頁說明的標題或主管的表單欄位不輸入任何值時，會發生什麼事。


[![自動建立動作方法](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**圖 03**： 有遺漏值的表單 ([按一下以檢視完整大小的影像](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


請注意 DateReleased 值會自動驗證。 DateReleased 屬性不接受 NULL 值，所以 DefaultModelBinder 驗證錯誤，這個屬性會自動產生它沒有值時。 如果您想要修改 DateReleased 屬性的錯誤訊息，則您需要建立自訂模型繫結。

## <a name="summary"></a>總結

在本教學課程中，您已了解如何使用 IDataErrorInfo 介面來產生驗證錯誤訊息。 首先，我們會建立擴充功能的 Entity Framework 所產生的部分影片類別的部分影片類別。 接下來，我們加入驗證邏輯的電影類別 OnTitleChanging() 和 OnDirectorChanging() 部分方法。 最後，我們實作 IDataErrorInfo 介面，以便公開 （expose） 這些 ASP.NET MVC framework 的驗證訊息。

> [!div class="step-by-step"]
> [上一頁](performing-simple-validation-cs.md)
> [下一頁](validating-with-a-service-layer-cs.md)
