---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: 開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form-第 8 節 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 的 ASP.NET Web Forms 應用程式。 範例應用程式是...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 249677c9e0eca248710bf730e57a7aeca5a9b5ce
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831412"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>開始使用 Entity Framework 4.0 Database 中第一次和 ASP.NET 4 Web Form 第 8 節
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>使用動態資料功能來格式化與驗證資料

在上一個教學課程中，您會實作預存程序。 本教學課程將說明如何動態資料功能可以提供下列優點：

- 欄位會自動格式化顯示根據其資料類型。
- 欄位會自動驗證根據其資料類型。
- 您可以加入自訂格式化與驗證行為的資料模型的中繼資料。 當您這樣做，您可以在一個位置，來新增格式化與驗證規則而 everywhere 自動套用您存取使用動態資料控制項欄位。

若要查看其運作方式，您要變更您用來顯示和編輯現有欄位的控制項*Students.aspx*頁面上，而且您會將格式化與驗證的中繼資料新增至的名稱和日期欄位`Student`實體類型。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>使用 DynamicField 和 DynamicControl 控制項

開啟*Students.aspx*頁面並在`StudentsGridView`控制項取代**名稱**並**註冊日期**`TemplateField`以下列標記的項目：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

此標記會使用`DynamicControl`控制的位置`TextBox`並`Label`學生中的控制項的命名範本 欄位中，並使用`DynamicField`個註冊日期的控制項。 不指定任何格式字串。

新增`ValidationSummary`控制後`StudentsGridView`控制項。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

在`SearchGridView`控制項取代的標記**名稱**並**註冊日期**為您的資料行未`StudentsGridView`控制項，但省略`EditItemTemplate`項目。 `Columns`項目`SearchGridView`控制項現在包含下列標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

開啟*Students.aspx.cs*並新增下列`using`陳述式：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

新增網頁的處理常式`Init`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

此程式碼可讓您指定格式化與驗證這些欄位的資料繫結控制項中，會提供動態資料`Student`實體。 如果當您執行頁面時，您會收到錯誤訊息，如下列範例所示，這通常表示您忘記呼叫`EnableDynamicData`方法中的`Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

執行網頁。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

在 **註冊日期**資料行，因為屬性型別顯示的時間和日期`DateTime`。 您將會修正該問題稍後。

現在，請注意，動態資料就會自動提供基本的資料驗證。 例如，按一下**編輯**，清除 [日期] 欄位，按一下**更新**，，您會看到，動態資料會自動將此必填的欄位因為值不是可為 null 的資料模型。 此頁面會顯示欄位和中的錯誤訊息之後的星號`ValidationSummary`控制項：

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

您可以省略`ValidationSummary`控制項，因為您可以也將滑鼠指標停留星號，以查看錯誤訊息：

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

動態資料也會驗證該輸入中的資料**註冊日期**欄位是有效的日期：

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

如您所見，這是一般錯誤訊息。 下一節中，您會看到如何自訂訊息，以及驗證和格式化規則。

## <a name="adding-metadata-to-the-data-model"></a>加入資料模型的中繼資料

一般而言，您會想要自訂動態資料所提供的功能。 例如，您可能會變更資料的顯示方式和錯誤訊息的內容。 您通常也會自訂資料驗證規則，以提供更多的功能比動態資料所提供的功能會自動根據資料型別。 若要這樣做，您會建立對應至實體類型的部分類別。

中**方案總管**，以滑鼠右鍵按一下**ContosoUniversity**專案，然後選取**加入參考**，並將參考加入`System.ComponentModel.DataAnnotations`。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

在  *DAL*資料夾中，建立新的類別檔案，將其命名*Student.cs*，並以下列程式碼取代範本程式碼中的。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

此程式碼建立的部分類別`Student`實體。 `MetadataType`套用至這個部分類別的屬性會識別您用來指定中繼資料的類別。 中繼資料類別可以具有任何名稱，但使用實體名稱再加上 「 中繼資料 」 是常見的作法。

套用至中繼資料類別中屬性的屬性會指定格式、 驗證、 規則及錯誤訊息。 如下所示的屬性將會有下列結果：

- `EnrollmentDate` 將會顯示為日期 （不含時間）。
- 這兩個名稱欄位必須是 25 個字元，或小於長度和自訂錯誤訊息中提供。
- 這兩個名稱欄位是必要的並提供自訂的錯誤訊息。

執行*Students.aspx*頁面，然後您會看到，日期現在會顯示不含時間：

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

編輯資料列，然後再次嘗試清除 [名稱] 欄位中的值。 只要您再按一下 [保留] 欄位中，會顯示指出欄位錯誤星號**更新**。 當您按一下 **更新**，此頁面會顯示您所指定的錯誤訊息文字。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

嘗試輸入超過 25 個字元的名稱，請按一下**更新**，和此頁面會顯示您所指定的錯誤訊息文字。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

既然您已設定的資料模型中繼資料中的這些格式化與驗證規則，規則將會自動套用可顯示或允許這些欄位中，變更每個頁面上，只要您使用`DynamicControl`或`DynamicField`控制項。 這會減少的多餘的程式碼撰寫，您必須讓程式設計和測試更為容易，並且確保資料格式化與驗證是整個應用程式一致。

## <a name="more-information"></a>更多資訊

以上總結這一系列的 Entity Framework 使用者入門教學課程。 取得可協助您了解如何使用 Entity Framework 的更多資源，繼續進行[下一步 的 Entity Framework 教學課程系列的第一個教學課程](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)或瀏覽下列網站：

- [Entity Framework 常見問題集](http://www.ef-faq.org/introduction.html)
- [Entity Framework 小組部落格](https://blogs.msdn.com/b/adonet/)
- [MSDN Library 中的 entity Framework](https://msdn.microsoft.com/library/bb399572.aspx)
- [在 MSDN 資料開發人員中心中的 entity Framework](https://msdn.microsoft.com/data/ef.aspx)
- [MSDN Library 中的 EntityDataSource Web 伺服器控制項概觀](https://msdn.microsoft.com/library/cc488502.aspx)
- [EntityDataSource 控制項 MSDN Library 中的 API 參考](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [MSDN 上的 entity Framework 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Julie Lerman 的部落格](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [上一步](the-entity-framework-and-aspnet-getting-started-part-7.md)
