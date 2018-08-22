---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: EF Database First 與 ASP.NET MVC： 增強資料驗證 |Microsoft Docs
author: tfitzmac
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: dbff33a1c4f47474adda82e796d9c292392142f2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825157"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>EF Database First 與 ASP.NET MVC： 增強資料驗證
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。
> 
> 系列的這個部分會著重於將資料註解加入至資料模型，以指定驗證需求，並顯示格式設定。 它已改善根據意見反應的註解區段中的使用者。


## <a name="add-data-annotations"></a>新增資料註解

如您所見在先前主題中，某些資料驗證規則會自動套用到使用者輸入。 比方說，您只可以提供一些級屬性。 若要指定更多的資料驗證規則，您可以新增至您的模型類別的資料註解。 這些註解會套用在您的 web 應用程式，針對指定的屬性。 您也可以套用變更之屬性的顯示方式; 的格式屬性例如，變更用於文字標籤的值。

在本教學課程中，您將加入資料註解提供的 FirstName、 LastName 和 MiddleName 屬性的值將長度限制。 在資料庫中，這些值僅限於 50 個字元;不過，在您的 web 應用程式中的字元限制目前不會強制執行。 如果使用者提供超過 50 個字元的其中一個值，當您嘗試將值儲存至資料庫時，會損毀頁面。 您也會限制級為 0 到 4 之間的值。

開啟**Student.cs**中的檔案**模型**資料夾。 將下列反白顯示的程式碼新增至類別。

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

在 Enrollment.cs，加入下列反白顯示的程式碼。

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

建置方案。

瀏覽至 編輯或建立學生的頁面。 如果您嘗試輸入超過 50 個字元，則會顯示一則錯誤訊息。

![顯示錯誤訊息](enhancing-data-validation/_static/image1.png)

瀏覽至這個網頁進行編輯註冊，並嘗試提供一個等級，上述 4。

![等級範圍錯誤](enhancing-data-validation/_static/image2.png)

您可以套用至屬性和類別的資料驗證註解的完整清單，請參閱 < [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。

## <a name="add-metadata-classes"></a>加入中繼資料類別

當您不想要變更; 的資料庫模型類別中直接加入驗證屬性運作不過，如果您的資料庫變更，而且您需要重新產生模型類別，您會遺失所有您已套用至模型類別的屬性。 這種方法可以是非常沒有效率且容易會遺失重要的驗證規則。

若要避免這個問題，您可以新增包含屬性的中繼資料類別。 當您建立關聯的中繼資料類別的模型類別時，這些屬性會套用至模型中。 這種方法，就可以重新產生模型類別而不會遺失所有已套用至中繼資料類別的屬性。

在 **模型**資料夾中，加入名為類別**Metadata.cs**。

![加入中繼資料類別](enhancing-data-validation/_static/image3.png)

Metadata.cs 中的程式碼取代為下列程式碼。

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

這些中繼資料類別可包含的所有驗證屬性，您先前套用至模型類別。 **顯示**屬性用來變更所使用的文字標籤的值。

現在，您必須將模型類別關聯的中繼資料類別。

在 **模型**資料夾中，加入名為類別**PartialClasses.cs**。

取代下列程式碼檔案的內容。

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

請注意，每個類別標示為`partial`類別，和每個符合的名稱和命名空間會自動產生的類別。 藉由套用至部分類別的中繼資料屬性，確定您的資料驗證屬性將會套用至自動產生的類別。 當您重新產生模型類別，因為中繼資料屬性會套用在不會重新產生的部分類別中，這些屬性將不會遺失。

若要重新產生的自動產生的類別，請開啟 ContosoModel.edmx 檔案。 同樣地，以滑鼠右鍵按一下 設計介面，然後選取**從資料庫更新模型**。 即使您未變更的資料庫，此程序會重新產生類別。 在 **重新整理**索引標籤上，選取**資料表**並**完成**。

![重新整理資料表](enhancing-data-validation/_static/image4.png)

儲存 ContosoModel.edmx 檔案以套用變更。

開啟 Student.cs 檔案或 Enrollment.cs 檔案，並請注意您在稍早所套用的資料驗證屬性已不在檔案中。 不過，執行應用程式，並請注意當您輸入的資料仍然會套用驗證規則。

> [!div class="step-by-step"]
> [上一頁](customizing-a-view.md)
> [下一頁](publish-to-azure.md)
