---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: "更新、 刪除和建立資料模型繫結與 web form |Microsoft 文件"
author: tfitzmac
description: "此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 18c065b44524e7738c048b5908fa50c592188064
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>更新、 刪除和建立資料模型繫結與 web form
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。
> 
> 本教學課程會示範如何建立、 更新和刪除與模型繫結的資料。 您會設定下列屬性：
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> 這些屬性會接收對應的作業會處理方法的名稱。 在該方法中，您提供的邏輯與資料互動。
> 
> 本教學課程是在第一個建立的專案[一部分](retrieving-data.md)的數列。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。


## <a name="what-youll-build"></a>您將建置

在此教學課程中，您將會：

1. 加入動態資料範本
2. 透過模型繫結方法啟用更新和刪除資料
3. 將資料驗證規則套用-啟用在資料庫中建立新的記錄

## <a name="add-dynamic-data-templates"></a>加入動態資料範本

若要提供最佳使用者體驗，並減少重複的程式碼，您將使用動態資料範本。 您還可以安裝 NuGet 封裝，將預先建立的動態資料範本輕鬆整合到現有的站台。

從**管理 NuGet 封裝**，安裝**DynamicDataTemplatesCS**。

![動態資料範本](updating-deleting-and-creating-data/_static/image1.png)

請注意，您的專案現在會包含名為的資料夾**動態**。 在該資料夾中，您會發現會自動套用到動態控制項，web form 中的範本。

![動態資料資料夾](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>啟用更新和刪除

讓使用者更新和刪除資料庫中的記錄會擷取資料的程序非常類似。 在**UpdateMethod**和**DeleteMethod**屬性，指定執行這些作業的方法名稱。 GridView 控制項，您可以也指定自動產生編輯和刪除按鈕。 下列反白顯示的程式碼顯示新增內容至 GridView 程式碼。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

在程式碼後置檔案中，加入 using 陳述式**System.Data.Entity.Infrastructure**。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

接著，新增下列更新與刪除方法。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**TryUpdateModel**方法相符的繫結的資料值從 web 表單套用至資料的項目。 根據識別碼參數的值，擷取資料的項目。

## <a name="enforce-validation-requirements"></a>強制執行驗證需求

更新資料時，會自動強制執行您套用至 Student 類別中的 FirstName、 LastName 和年份屬性的驗證屬性。 DynamicField 控制項加入驗證屬性為基礎的用戶端和伺服器驗證。 FirstName 和 LastName 屬性都需要。 FirstName 不能超過 20 個字元的長度，和 [姓氏] 不能超過 40 個字元。 年份必須 AcademicYear 列舉的有效值。

如果使用者違反了其中一項驗證需求，就不會繼續更新。 若要查看錯誤訊息，新增 ValidationSummary 控制項上方 GridView。 若要顯示在模型繫結的驗證錯誤，請設定**ShowModelStateErrors**屬性設定為**true**。 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

執行 web 應用程式中，更新和刪除任何記錄。

![更新資料](updating-deleting-and-creating-data/_static/image3.png)

請注意，當編輯模式中年度屬性的值會自動轉譯為下拉式清單。 [年] 屬性是列舉值，並列舉值的動態資料範本指定的下拉式清單進行編輯。 您可以找到該範本開啟**列舉\_Edit.ascx**檔案**動態**/**FieldTemplates**資料夾。

如果您提供有效的值，更新就會順利完成。 如果您違反其中一項驗證需求，不會繼續更新和方格上方顯示錯誤訊息。

![錯誤訊息](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>新增新的記錄

GridView 控制項不包含**InsertMethod**屬性，因此無法用來加入新的記錄與模型繫結。 您可以找到 InsertMethod 屬性**FormView**， **DetailsView**，或**ListView**控制項。 在本教學課程中，您將使用 FormView 控制項來加入新的記錄。

首先，加入新的頁面，您將加入新的記錄建立的連結。 Aggregate 上方加入：

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

新的連結會出現在頂端的學生頁面的內容。

![新的連結](updating-deleting-and-creating-data/_static/image5.png)

然後，加入新的 web 表單使用主版頁面中，並將其命名**AddStudent**。 選取 Site.Master 做為主版頁面。

您將轉譯的欄位加入使用新的學生**DynamicEntity**控制項。 Dynamicentity 呈現 ItemType 屬性中指定的類別中的可編輯的屬性。 StudentID 屬性被標記為**[ScaffoldColumn(false)]**屬性，讓它不會轉譯。 在 AddStudent 頁面 MainContent 預留位置，加入下列程式碼。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

在程式碼後置檔案 (AddStudent.aspx.cs) 中，加入**使用**陳述式**ContosoUniversityModelBinding.Models**命名空間。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

然後，加入下列方法來指定如何插入新的記錄 和 取消 5d; 按鈕的事件處理常式。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

將儲存的所有變更。

執行 web 應用程式並建立新的學生。

![加入新的學生](updating-deleting-and-creating-data/_static/image6.png)

按一下**插入**，並注意已建立新的學生。

![顯示新的學生](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>結論

在本教學課程中，您可以啟用更新、 刪除和建立的資料。 您可以確保與資料互動時，會套用驗證規則。

在接下來[教學課程](sorting-paging-and-filtering-data.md)在此數列，您將啟用排序、 分頁和篩選資料。

>[!div class="step-by-step"]
[上一頁](retrieving-data.md)
[下一頁](sorting-paging-and-filtering-data.md)
