---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: 與模型繫結和 web form 整合 JQuery UI Datepicker |Microsoft Docs
author: tfitzmac
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: fc278575615e63af2fc7284e51814cf318165110
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807573"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>與模型繫結和 web form 整合 JQuery UI Datepicker
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。
> 
> 本教學課程示範如何新增 JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) Web 表單，並使用模型繫結至選取的值以更新資料庫。
> 
> 本教學課程中建立的專案是根據[第一個](retrieving-data.md)並[第二個](updating-deleting-and-creating-data.md)系列的一部分。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。


## <a name="what-youll-build"></a>您將建置

在本教學課程中，您將會：

1. 將屬性加入至您的模型，來記錄學生的註冊日期
2. 讓使用者選取使用 JQuery UI Datepicker 小工具的註冊日期
3. 強制執行驗證規則的註冊日期

JQuery UI Datepicker 小工具可讓使用者輕鬆地從使用者所互動的欄位時快顯行事曆選取日期。 使用這個小工具可能會更方便使用者比手動輸入的日期。 Datepicker 小工具整合的資料作業中使用模型繫結的頁面要求只有少量額外的工作。

## <a name="add-a-new-property-to-the-model"></a>將新屬性加入至模型

首先，您將在其中加入**Datetime**屬性，以讓學生模型，並將該變更移轉至資料庫。 開啟**UniversityModels.cs**，並將醒目提示的程式碼新增至學生的模型。

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute**是否包含強制執行驗證規則的屬性。 本教學課程中，我們假設 Contoso 大學創立在 2013 年 1 月 1 日起，並因此先前的註冊日期不正確。

在 [套件管理] 視窗中，新增移轉執行命令**新增移轉 AddEnrollmentDate**。 請注意，移轉程式碼會將新的日期時間資料行新增至 Student 資料表。 若要符合您在 RangeAttribute 中指定的值，請新增新的資料行的預設值，下列反白顯示的程式碼所示。

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

將變更儲存到移轉檔案中。

您不需要重新植入資料。 因此，開啟**Configuration.cs**在 [移轉] 資料夾並移除或註解中的程式碼**種子**方法。 儲存並關閉檔案。

現在，執行命令**更新資料庫**。 請注意，資料行存在於資料庫中的所有現有的記錄都有預設值的 EnrollmentDate。

## <a name="add-dynamic-controls-for-enrollment-date"></a>新增動態控制項個註冊日期

您現在要加入控制項來顯示和編輯註冊日期。 到目前為止，是可以透過 [文字] 方塊中編輯值。 稍後在教學課程中，您會變更至 JQuery Datepicker 小工具的文字方塊。

首先，請務必請注意，您不需要進行任何變更**AddStudent.aspx**檔案。 DynamicEntity 控制項將自動呈現新的屬性。

開啟**Students.aspx**，並新增下列醒目提示的程式碼。

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

執行應用程式，並請注意，您可以設定的註冊日期值所輸入的日期。 當加入新的學生：

![設定的日期](integrating-jquery-ui/_static/image1.png)

或者，您也可以編輯現有的值：

![編輯日期](integrating-jquery-ui/_static/image2.png)

輸入日期的運作方式，但它可能不是您想要提供的客戶體驗。 在下一步 區段中，您將啟用選取透過行事曆的日期。

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>安裝 NuGet 套件，才能使用 JQuery UI

**汁 UI** NuGet 套件可讓您輕鬆整合到 web 應用程式的 JQuery UI widgets。 若要使用此套件，請透過 NuGet 安裝。

![新增汁 UI](integrating-jquery-ui/_static/image3.png)

汁 UI，您所安裝的版本可能與您的應用程式中的 JQuery 版本衝突。 繼續進行本教學課程之前，請嘗試執行您的應用程式。 如果您遇到 JavaScript 錯誤時，您需要調解的 JQuery 版本。 您可以預期的 JQuery 版本加入您的指令碼資料夾 （版本 1.8.2 撰寫本教學課程中的每次），或在 Site.master 指定 JQuery 檔案的路徑。

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>自訂日期時間的範本，以包含 Datepicker 小工具

編輯日期時間值的動態資料範本，您將加入 Datepicker 小工具。 將小工具加入至範本，它會自動轉譯這兩種格式來新增新的學生和格線檢視中編輯學生版。 開啟**DateTime\_Edit.ascx**，並新增下列醒目提示的程式碼。

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

在程式碼後置檔案中，您將設定的最小和最大日期 datepicker。 藉由設定這些值，將會導致使用者無法瀏覽到無效的日期。 您將擷取的最小和最大值**RangeAttribute**上的日期時間屬性，如果提供的其中一個。 開啟**DateTime\_Edit.ascx.cs**，並將下列反白顯示的程式碼新增至頁面\_負載方法。

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

執行 web 應用程式，並瀏覽至 AddStudent 頁面。 提供的欄位值，並請注意，當您按一下文字方塊上個註冊日期時，會顯示行事曆。

![日期選擇器](integrating-jquery-ui/_static/image4.png)

挑選日期，然後按一下**插入**。 RangeAttribute 會強制執行伺服器上的驗證。 藉由設定 Datepicker minDate; 屬性，您也適用於驗證用戶端上。 行事曆不會讓使用者瀏覽至 minDate 值之前的日期。

當您編輯格線檢視中的記錄時，也會顯示行事曆。

![在 GridView 的 Datepicker](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>結論

在本教學課程中，您已了解如何使用模型繫結的 web 表單中納入 JQuery 小工具。

在接下來[教學課程](using-query-string-values-to-retrieve-data.md)，選取資料時，您將使用的查詢字串值。

> [!div class="step-by-step"]
> [上一頁](sorting-paging-and-filtering-data.md)
> [下一頁](using-query-string-values-to-retrieve-data.md)
