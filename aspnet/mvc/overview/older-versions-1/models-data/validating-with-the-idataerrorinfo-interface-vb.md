---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: 驗證與 IDataErrorInfo 介面 (VB) |Microsoft 文件
author: StephenWalther
description: 作者： Stephen Walther 會示範如何在模型類別中實作 IDataErrorInfo 介面中顯示自訂的驗證錯誤訊息。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 60df0f934432484e0c97e0caef25c15605beb14f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>驗證與 IDataErrorInfo 介面 (VB)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 作者： Stephen Walther 會示範如何在模型類別中實作 IDataErrorInfo 介面中顯示自訂的驗證錯誤訊息。


本教學課程的目標是要說明的其中一個方法，以在 ASP.NET MVC 應用程式中執行驗證。 您了解如何防止某人提交 HTML 表單，而不需要的表單欄位提供值。 在本教學課程中，您可以了解如何使用 IErrorDataInfo 介面來執行驗證。

## <a name="assumptions"></a>假設

在本教學課程中，我將使用 MoviesDB 資料庫及電影資料庫資料表。 此資料表具有下列資料行：

<a id="0.6_table01"></a>


| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | Int | False |
| 標題 | Nvarchar(100) | False |
| 導向器 | Nvarchar(100) | False |
| DateReleased | DateTime | False |


在本教學課程中，我可以使用 Microsoft Entity Framework 來產生我的資料庫模型類別。 Entity Framework 所產生的電影類別會顯示在圖 1。


[![影片實體](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**圖 01**: 電影實體 ([按一下以檢視完整大小的影像](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> 若要深入了解使用 Entity Framework 來產生您的資料庫模型類別，請參閱我教學課程詳述與 Entity Framework 建立模型類別。


## <a name="the-controller-class"></a>控制器類別

我們會使用清單電影主控制器，並建立新的電影。 這個類別的程式碼會包含在程式碼範例 1。

**Listing 1 - Controllers\HomeController.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

列出 1 中的首頁控制器類別包含兩個 create （） 動作。 第一個動作顯示的 HTML 表單建立新的電影。 第二個 create （） 動作執行新的電影實際插入至資料庫。 第一個 create （） 動作所顯示的表單提交給伺服器時，會叫用第二個 create （） 動作。

請注意第二個 create （） 動作，包含下列程式碼：

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

驗證錯誤時，則 IsValid 屬性會傳回 false。 在此情況下，會重新顯示建立檢視，其中包含建立電影的 HTML 表單。

## <a name="creating-a-partial-class"></a>建立部分類別

影片類別是 Entity Framework 所產生的。 如果您展開 MoviesDBModel.edmx 檔案，在 [方案總管] 視窗中的，並在程式碼編輯器中開啟 MoviesDBModel.Designer.vb 檔案，您可以看到影片類別的程式碼 （請參閱圖 2）。


[![影片實體的程式碼](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**圖 02**： 電影實體的程式碼 ([按一下以檢視完整大小的影像](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


影片類別是部分類別。 這表示我們可以加入另一個部分類別，具有相同的名稱，以擴充影片類別的功能。 我們會將我們的驗證邏輯新增至新的部分類別。

將清單 2 中的類別，加入 模型 資料夾。

**列出 2-Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

請注意，在列出 2 類別包含*部分*修飾詞。 任何方法或屬性加入至這個類別會變成 Entity Framework 所產生的電影類別的一部分。

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>將 OnChanging 和 OnChanged 部分方法

在 Entity Framework 產生實體類別時，Entity Framework 部分方法的類別會自動新增。 Entity Framework 會產生 OnChanging 和 OnChanged 部分方法對應至每一個屬性的類別。

在影片類別中，Entity Framework 會建立下列方法：

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

變更對應的屬性之前，稱為右 OnChanging 方法。 變更屬性之後，稱為右 OnChanged 方法。

您可以利用這些影片類別加入驗證邏輯的部分方法。 更新中列出的 3 影片類別會驗證的標題和主管屬性指派非空值。

> [!NOTE] 
> 
> 部分方法是，您就不需要實作在類別中定義的方法。 如果您不要只實作部分方法，編譯器會移除方法簽章，並因此方法的所有呼叫都都沒有與部分方法相關聯的執行階段成本。 在 Visual Studio 程式碼編輯器中，您可以輸入關鍵字加入部分方法*部分*後面接著一個空格，以檢視來實作 partials 的清單。


**Listing 3 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

例如，如果您嘗試將空字串指派給 Title 屬性，則錯誤訊息會指派給名為字典\_錯誤。

此時，會有任何作用實際上當您將空字串指派給 Title 屬性，並將錯誤加入至私人\_錯誤欄位。 我們需要實作 IDataErrorInfo 介面來公開這些驗證錯誤，以 ASP.NET MVC 架構。

## <a name="implementing-the-idataerrorinfo-interface"></a>實作 IDataErrorInfo 介面

IDataErrorInfo 介面已的第一版.NET framework 的一部分。 這個介面是非常簡單的介面：

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

如果類別實作 IDataErrorInfo 介面，ASP.NET MVC 架構會使用此介面，建立類別的執行個體時。 例如，主控制器動作 create （） 接受影片類別的執行個體：

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

ASP.NET MVC 架構會建立影片所使用的模型繫結器 (DefaultModelBinder) 傳遞給 create （） 動作的執行個體。 模型繫結器會負責建立電影物件的執行個體繫結至電影物件的執行個體的 HTML 表單欄位。

DefaultModelBinder 會偵測類別實作 IDataErrorInfo 介面。 如果類別實作此介面的模型繫結器叫用類別的每個屬性的 IDataErrorInfo.this 索引子。 如果索引子會傳回錯誤訊息的模型繫結器將加入此錯誤訊息，來自動建立模型的狀態。

DefaultModelBinder 也會檢查 IDataErrorInfo.Error 屬性。 這個屬性會代表類別相關聯的非屬性特定的驗證錯誤。 比方說，您可能想要強制套用驗證規則，取決於影片類別的多個屬性的值。 在此情況下，您會傳回驗證錯誤的錯誤屬性。

列出的 4 中更新的影片類別會實作 IDataErrorInfo 介面。

**列出 4-Models\Movie.vb （實作 IDataErrorInfo）**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

在列出 4 中，檢查索引子屬性\_錯誤以查看它是否包含對應至屬性名稱的索引鍵的集合傳遞給索引子。 如果沒有任何與屬性相關聯的驗證錯誤則會傳回空字串。

您不需要修改主控制器，以任何方式修改過的電影類別使用。 圖 3 顯示的網頁說明的標題或主管的表單欄位不輸入任何值時，會發生什麼事。


[![自動建立動作方法](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**圖 03**： 有遺漏值的表單 ([按一下以檢視完整大小的影像](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


請注意 DateReleased 值會自動驗證。 DateReleased 屬性不接受 NULL 值，所以 DefaultModelBinder 驗證錯誤，這個屬性會自動產生它沒有值時。 如果您想要修改 DateReleased 屬性的錯誤訊息，則您需要建立自訂的模型繫結。

## <a name="summary"></a>總結

在本教學課程中，您將學會如何使用 IDataErrorInfo 介面來產生驗證錯誤訊息。 首先，我們建立擴充功能的 Entity Framework 所產生的部分影片類別的部分影片類別。 接下來，我們加入驗證邏輯的電影類別 OnTitleChanging() 和 OnDirectorChanging() 部分方法。 最後，我們會實作 IDataErrorInfo 介面以公開至 ASP.NET MVC framework 這些驗證訊息。

> [!div class="step-by-step"]
> [上一頁](performing-simple-validation-vb.md)
> [下一頁](validating-with-a-service-layer-vb.md)
