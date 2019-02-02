---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 教學課程：增強 EF Database First 與 ASP.NET MVC 應用程式的資料的驗證
description: 本教學課程著重於將資料註解加入至資料模型，以指定驗證需求，並顯示格式設定。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667618"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>教學課程：增強 EF Database First 與 ASP.NET MVC 應用程式的資料的驗證

您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。

本教學課程著重於將資料註解加入至資料模型，以指定驗證需求，並顯示格式設定。 它已改善根據意見反應的註解區段中的使用者。

在本教學課程中，您已：

> [!div class="checklist"]
> * 新增資料註解
> * 加入中繼資料類別

## <a name="prerequisites"></a>必要條件

* [自訂檢視](customizing-a-view.md)

## <a name="add-data-annotations"></a>新增資料註解

如您所見在先前主題中，某些資料驗證規則會自動套用到使用者輸入。 比方說，您只可以提供一些級屬性。 若要指定更多的資料驗證規則，您可以新增至您的模型類別的資料註解。 這些註解會套用在您的 web 應用程式，針對指定的屬性。 您也可以套用變更之屬性的顯示方式; 的格式屬性例如，變更用於文字標籤的值。

在本教學課程中，您將加入資料註解提供的 FirstName、 LastName 和 MiddleName 屬性的值將長度限制。 在資料庫中，這些值僅限於 50 個字元;不過，在您的 web 應用程式中的字元限制目前不會強制執行。 如果使用者提供超過 50 個字元的其中一個值，當您嘗試將值儲存至資料庫時，會損毀頁面。 您也會限制級為 0 到 4 之間的值。

選取 **模型** > **ContosoModel.edmx** > **ContosoModel.tt** ，然後開啟*Student.cs*檔案。 將下列反白顯示的程式碼新增至類別。

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

開啟*Enrollment.cs*並新增下列醒目提示的程式碼。

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

建置方案。

按一下 **學生的清單**，然後選取**編輯**。 如果您嘗試輸入超過 50 個字元，則會顯示一則錯誤訊息。

![顯示錯誤訊息](enhancing-data-validation/_static/image1.png)

回到首頁。 按一下 **註冊清單**，然後選取**編輯**。 嘗試提供一個等級，上述 4。 您會收到此錯誤：*等級必須是介於 0 到 4 之間的欄位。*

## <a name="add-metadata-classes"></a>加入中繼資料類別

當您不想要變更; 的資料庫模型類別中直接加入驗證屬性運作不過，如果您的資料庫變更，而且您需要重新產生模型類別，您會遺失所有您已套用至模型類別的屬性。 這種方法可以是非常沒有效率且容易會遺失重要的驗證規則。

若要避免這個問題，您可以新增包含屬性的中繼資料類別。 當您建立關聯的中繼資料類別的模型類別時，這些屬性會套用至模型中。 這種方法，就可以重新產生模型類別而不會遺失所有已套用至中繼資料類別的屬性。

在 **模型**資料夾中，加入名為類別*Metadata.cs*。

中的程式碼取代*Metadata.cs*為下列程式碼。

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

這些中繼資料類別可包含的所有驗證屬性，您先前套用至模型類別。 **顯示**屬性用來變更所使用的文字標籤的值。

現在，您必須將模型類別關聯的中繼資料類別。

在 **模型**資料夾中，加入名為類別*PartialClasses.cs*。

取代下列程式碼檔案的內容。

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

請注意，每個類別標示為`partial`類別，和每個符合的名稱和命名空間會自動產生的類別。 藉由套用至部分類別的中繼資料屬性，確定您的資料驗證屬性將會套用至自動產生的類別。 當您重新產生模型類別，因為中繼資料屬性會套用在不會重新產生的部分類別中，這些屬性將不會遺失。

若要重新產生的自動產生的類別，請開啟*ContosoModel.edmx*檔案。 同樣地，以滑鼠右鍵按一下 設計介面，然後選取**從資料庫更新模型**。 即使您未變更的資料庫，此程序會重新產生類別。 在 **重新整理**索引標籤上，選取**資料表**並**完成**。

儲存*ContosoModel.edmx*檔案，以套用所做的變更。

開啟*Student.cs*檔案或*Enrollment.cs*檔案，並請注意，您在稍早所套用的資料驗證屬性已不在檔案中。 不過，執行應用程式，並請注意當您輸入的資料仍然會套用驗證規則。

## <a name="conclusion"></a>結論

這一系列將提供如何從現有的資料庫，可讓使用者編輯、 更新、 建立和刪除的資料產生程式碼的簡單範例。 它使用 ASP.NET MVC 5、 Entity Framework 與 ASP.NET Scaffolding 來建立專案。 

Code First 開發簡介範例，請參閱[Getting Started with ASP.NET MVC 5](../introduction/getting-started.md)。 

如需更進階的範例，請參閱 <<c0> [ 建立 ASP.NET MVC 4 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 請注意，您使用第一個資料庫中的資料使用 DbContext API 與您用來處理在 Code First 資料的 API 相同。 即使您想要使用第一個資料庫，您可以了解如何處理更複雜的案例，例如讀取及更新的相關的資料，處理並行衝突，Code First 的教學課程中，依此類推。 唯一的差別是在建立資料庫、 內容類別和實體類別的方式。

## <a name="additional-resources"></a>其他資源

您可以套用至屬性和類別的資料驗證註解的完整清單，請參閱 < [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 已新增的資料註解
> * 已新增的中繼資料類別

若要了解如何部署至 Azure App Service 的 web 應用程式和 SQL database，請參閱本教學課程：
> [!div class="nextstepaction"]
> [將.NET 應用程式部署至 Azure App Service](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
