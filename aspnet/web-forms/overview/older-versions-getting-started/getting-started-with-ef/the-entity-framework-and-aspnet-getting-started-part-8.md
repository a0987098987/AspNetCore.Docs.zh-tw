---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: 開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-部分 8 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 035cce022d1b3697b825a96487529dbc9675d90e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form 一部分 8
====================
由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>使用動態資料功能格式化以及驗證資料

在上一個教學課程中，您會實作預存程序。 本教學課程將示範如何動態資料功能可以提供下列優點：

- 欄位會自動格式化顯示根據其資料類型。
- 欄位會自動驗證其資料型別。
- 您可以將中繼資料加入資料模型，以自訂格式化與驗證行為。 當您這樣做，在一個位置，您可以加入格式設定和驗證規則而自動在所有地方套用您存取使用的 Dynamic Data 控制項的欄位。

若要查看其運作方式，您要變更您用來顯示和編輯中的現有欄位的控制項*Students.aspx*  頁面上，而且您會加入格式設定和驗證的中繼資料的名稱和日期欄位`Student`實體類型。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>使用 DynamicField 和 DynamicControl 控制項

開啟*Students.aspx*頁面和`StudentsGridView`控制項取代**名稱**和**註冊日期**`TemplateField`以下列標記的項目：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

這個標記會使用`DynamicControl`取代控制`TextBox`和`Label`學生中的控制項名稱範本欄位，並使用`DynamicField`註冊日期的控制項。 不指定任何格式字串。

新增`ValidationSummary`控制後`StudentsGridView`控制項。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

在`SearchGridView`控制項取代的標記**名稱**和**註冊日期**做為您的資料行未`StudentsGridView`控制，但省略`EditItemTemplate`項目。 `Columns`元素`SearchGridView`控制項現在包含下列標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

開啟*Students.aspx.cs*並加入下列`using`陳述式：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

新增網頁的處理常式`Init`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

此程式碼會指定動態資料將提供格式化與這些資料繫結控制項中的欄位驗證`Student`實體。 如果執行頁面時，您會收到錯誤訊息，如下列範例所示，這通常表示您忘了呼叫`EnableDynamicData`方法中的`Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

執行網頁。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

在**註冊日期**資料行，因為屬性類型就是顯示的時間和日期`DateTime`。 您可以稍後修正的。

現在請注意，動態資料自動提供基本的資料驗證。 例如，按一下**編輯**，請清除 [日期] 欄位中按一下**更新**，而且您會看到，動態資料自動讓這個成為必要的欄位的值不是可為 null 的資料模型，因為。 頁面會顯示星號的欄位中的錯誤訊息後`ValidationSummary`控制項：

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

您可以省略`ValidationSummary`控制項，因為您也可以將滑鼠指標以查看錯誤訊息的星號上：

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

動態資料也會驗證輸入中的資料**註冊日期**欄位是有效的日期：

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

如您所見，這是一般錯誤訊息。 下一節中，您會看到如何自訂訊息，以及驗證以及格式化規則。

## <a name="adding-metadata-to-the-data-model"></a>加入資料模型的中繼資料

一般而言，您會想要自訂動態資料所提供的功能。 例如，您可能會變更資料的顯示方式和錯誤訊息的內容。 您通常也自訂資料驗證規則，以提供更多的功能比動態資料所提供的功能會自動根據資料型別。 若要這樣做，您可以建立對應至實體類型的部分類別。

在**方案總管 中**，以滑鼠右鍵按一下**ContosoUniversity**專案，然後選取**加入參考**，並將參考加入`System.ComponentModel.DataAnnotations`。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

在*DAL*資料夾中，建立新的類別檔案，其命名*Student.cs*，並在它的範本程式碼取代為下列程式碼。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

此程式碼建立的部分類別`Student`實體。 `MetadataType`套用至這個部分類別的屬性會識別您用來指定中繼資料的類別。 中繼資料類別可以具有任何名稱，但使用實體名稱再加上 「 中繼資料 」 是常見的作法。

套用至中繼資料類別中屬性的屬性指定的格式、 驗證、 規則和錯誤訊息。 如下所示的屬性將會產生下列結果：

- `EnrollmentDate` 將會顯示為日期 （而不是時間）。
- 這兩個名稱欄位必須是 25 個字元，或小於長度，以及自訂錯誤訊息中提供。
- 這兩個名稱欄位是必要的並提供自訂錯誤訊息。

執行*Students.aspx*頁面上，而且您會看到日期現在會顯示不含時間：

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

編輯資料列，然後試著清除名稱欄位中的值。 只要您之前按一下 [離開] 欄位中，會出現表示欄位錯誤星號**更新**。 當您按一下**更新**，頁面會顯示您所指定的錯誤訊息文字。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

若要輸入超過 25 個字元的名稱再試一次，請按一下**更新**，和頁面會顯示您所指定的錯誤訊息文字。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

既然您已經設定這些格式設定和驗證規則，在資料模型中繼資料中，規則就會自動套用會顯示或變更這些欄位，可讓每一頁上，只要您使用`DynamicControl`或`DynamicField`控制項。 如此能縮短的多餘的程式碼撰寫，您必須進行程式設計和測試更為容易，並且確保資料格式和驗證是整個應用程式一致。

## <a name="more-information"></a>更多資訊

這一系列的 Entity Framework 使用者入門教學課程的總結。 如需可協助您了解如何使用 Entity Framework 的多個資源，繼續進行[下一步的 Entity Framework 教學課程系列的第一個教學課程](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)或瀏覽下列網站：

- [Entity Framework 常見問題集](http://www.ef-faq.org/introduction.html)
- [Entity Framework 小組部落格](https://blogs.msdn.com/b/adonet/)
- [MSDN Library 中的 entity Framework](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework 中 MSDN 資料開發人員中心](https://msdn.microsoft.com/data/ef.aspx)
- [MSDN Library 中的 EntityDataSource Web 伺服器控制項概觀](https://msdn.microsoft.com/library/cc488502.aspx)
- [EntityDataSource 控制項 MSDN Library 中的應用程式開發介面參考](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [MSDN 上的實體架構論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Julie Lerman 部落格](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [上一步](the-entity-framework-and-aspnet-getting-started-part-7.md)
