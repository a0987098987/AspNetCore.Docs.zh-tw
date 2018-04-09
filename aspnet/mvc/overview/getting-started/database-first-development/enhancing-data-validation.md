---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 第一個使用 ASP.NET MVC 的 EF 資料庫： 強化資料驗證 |Microsoft 文件
author: tfitzmac
description: 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>第一個使用 ASP.NET MVC 的 EF 資料庫： 強化資料驗證
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。
> 
> 數列的這個部分著重在將資料註解加入至資料模型，以指定驗證需求，並顯示格式設定。 它已根據上改善的註解區段中的使用者意見。


## <a name="add-data-annotations"></a>新增資料註解

如同先前的主題，一些資料的驗證規則會自動套用到使用者輸入。 例如，您可以只提供數字等級屬性。 若要指定詳細資料的驗證規則，您可以將資料註解加入至模型類別。 這些註解會套用在 web 應用程式指定的屬性。 您也可以套用變更之屬性的顯示方式; 的格式屬性例如，變更文字標籤所使用的值。

在本教學課程中，您將加入資料註解，FirstName、 LastName 和 MiddleName 屬性提供的值長度限制。 在資料庫中，這些值會限制為 50 個字元。不過，web 應用程式中，字元是目前未強制限制。 使用者提供超過 50 個字元的其中一個值，如果嘗試將值儲存至資料庫時，會損毀頁面。 您也會限制等級為 0 到 4 之間的值。

開啟**Student.cs**檔案**模型**資料夾。 將下列反白顯示的程式碼加入至類別。

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

在 Enrollment.cs，加入下列反白顯示的程式碼。

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

建置方案。

瀏覽頁面，以便進行編輯或建立一位學生。 如果您嘗試輸入超過 50 個字元，則會顯示錯誤訊息。

![顯示錯誤訊息](enhancing-data-validation/_static/image1.png)

瀏覽至網頁進行編輯的註冊項目，並會嘗試提供 4 上方的等級。

![等級範圍錯誤](enhancing-data-validation/_static/image2.png)

資料驗證註解，您可以將套用至屬性和類別的完整清單，請參閱[System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。

## <a name="add-metadata-classes"></a>新增中繼資料類別

驗證屬性直接加入模型類別運作時不想要變更; 因此，資料庫不過，如果您的資料庫變更，且您需要重新產生模型類別，您會遺失所有您已套用至模型類別的屬性。 這個方法可能非常沒有效率且容易會遺失重要的驗證規則。

若要避免這個問題，您可以加入包含屬性的中繼資料類別。 當您建立關聯的中繼資料類別的模型類別時，這些屬性會套用至模型。 這種方法，而不會遺失的所有屬性已套用的中繼資料類別可以會重新產生模型類別。

在**模型**資料夾中，加入名為類別**Metadata.cs**。

![新增中繼資料類別](enhancing-data-validation/_static/image3.png)

Metadata.cs 中的程式碼取代為下列程式碼。

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

這些中繼資料類別可包含的所有驗證屬性，您先前套用至模型類別。 **顯示**屬性用來變更所使用的文字標籤的值。

現在，您必須將模型類別關聯的中繼資料類別。

在**模型**資料夾中，加入名為類別**PartialClasses.cs**。

取代下列程式碼檔案的內容。

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

請注意，每個類別標示為`partial`類別，而每個符合名稱和命名空間為自動產生的類別。 將中繼資料屬性套用至部分類別，您可以確保資料的驗證屬性，會套用至自動產生的類別。 當您重新產生模型類別，因為中繼資料已套用該屬性不會重新產生的部分類別中，這些屬性將不會遺失。

若要重新產生的自動產生的類別，請開啟 ContosoModel.edmx 檔案。 同樣地，以滑鼠右鍵按一下 設計介面，並選取**從資料庫更新模型**。 即使沒有變更資料庫，此程序會重新產生的類別。 在**重新整理**索引標籤上，選取**資料表**和**完成**。

![重新整理的資料表](enhancing-data-validation/_static/image4.png)

儲存 ContosoModel.edmx 檔案才能套用變更。

開啟 Student.cs 檔案或 Enrollment.cs 檔案，並請注意，先前所套用的資料驗證屬性不再是檔案中。 不過，執行應用程式，並注意當您輸入的資料仍會套用驗證規則。

> [!div class="step-by-step"]
> [上一頁](customizing-a-view.md)
> [下一頁](publish-to-azure.md)
