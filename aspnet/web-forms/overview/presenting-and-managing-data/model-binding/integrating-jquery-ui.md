---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: 使用模型繫結和 web form 整合 JQuery UI 日期選擇器 |Microsoft 文件
author: tfitzmac
description: 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>使用模型繫結和 web form 整合 JQuery UI 日期選擇器
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。
> 
> 本教學課程示範如何將 JQuery UI[日期選擇器 widget](http://jqueryui.com/datepicker/) Web 表單，並使用模型繫結至選取的值以更新資料庫。
> 
> 本教學課程中建立的專案是[第一個](retrieving-data.md)和[第二個](updating-deleting-and-creating-data.md)數列的組件。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。


## <a name="what-youll-build"></a>您將建置

在此教學課程中，您將會：

1. 將屬性加入至您的模型，以記錄學生註冊日期
2. 可讓使用者選取的 JQuery UI 日期選擇器 widget 註冊日期
3. 註冊日期，強制執行驗證規則

JQuery UI 日期選擇器小工具可讓使用者輕鬆地從使用者互動的欄位時，快顯行事曆選取日期。 使用這個小工具可能會更方便使用者比手動輸入的日期。 將 [日期選擇器] widget 中整合至模型繫結使用的資料作業的網頁需要只有少量的額外工作。

## <a name="add-a-new-property-to-the-model"></a>將新屬性加入至模型

首先，您將加入**Datetime**您學生的屬性建立的模型，並將該變更移轉到資料庫。 開啟**UniversityModels.cs**，並將反白顯示的程式碼加入至學生模型。

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute**是否包含強制執行驗證規則的屬性。 此教學課程中，我們會假設 Contoso 大學創立在 2013 年 1 月 1 日，並因此先前註冊日期不是有效。

在封裝管理視窗中，加入在移轉執行命令**新增移轉 AddEnrollmentDate**。 請注意，移轉程式碼會將新的日期時間資料行加入學生資料表。 反白顯示的下列程式碼所示，以符合您在 RangeAttribute 中指定的值，加入新的資料行的預設值。

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

將您的變更儲存至移轉檔案。

您不需要重新植入資料。 因此，開啟**configuration.cs 中**Migrations 資料夾中，移除或註解中的程式碼**種子**方法。 儲存並關閉檔案。

現在，執行命令**更新資料庫**。 請注意，資料行存在於資料庫中的所有現有的記錄都有預設值為 EnrollmentDate。

## <a name="add-dynamic-controls-for-enrollment-date"></a>註冊日期集將動態控制項

您現在要加入控制項，顯示與編輯註冊日期。 此時，這個值是透過文字方塊中編輯。 稍後在教學課程中，您會變更為 [JQuery 日期選擇器] widget 中的文字方塊。

首先，它是很重要的一點，您不需要進行任何變更**AddStudent.aspx**檔案。 Dynamicentity 自動會轉譯為新的屬性。

開啟**Students.aspx**，並加入下列反白顯示的程式碼。

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

執行應用程式，請注意，您可以設定註冊日期的值所輸入的日期。 當加入新的學生：

![設定的日期](integrating-jquery-ui/_static/image1.png)

或者，您也可以編輯現有的值：

![編輯日期](integrating-jquery-ui/_static/image2.png)

輸入日期是有效的但它可能不是您想要提供的客戶體驗。 在下一步 區段中，您將啟用選取透過行事曆日期。

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>安裝 NuGet 套件，才能使用 JQuery UI

**汁 UI** NuGet 封裝可讓您輕鬆整合 JQuery UI widgets 到 web 應用程式。 若要使用此封裝，請透過 NuGet 安裝。

![新增汁 UI](integrating-jquery-ui/_static/image3.png)

汁 UI，您所安裝的版本可能與您的應用程式中的 JQuery 版本衝突。 此教學課程之前，請嘗試執行您的應用程式。 如果您遇到 JavaScript 錯誤，您需要調解的 JQuery 版本。 您可以預期的 JQuery 版本加入到指令碼資料夾 （版本 1.8.2 撰寫本教學課程中的每次），或在 Site.master 指定 JQuery 檔案的路徑。

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>自訂 DateTime 範本，以包括日期選擇器 widget

您會將 [日期選擇器] widget 中，加入動態資料範本進行編輯，日期時間值。 將 widget 新增至範本，它會自動轉譯加入新的學生表單和格線檢視中編輯學生版。 開啟**DateTime\_Edit.ascx**，並加入下列反白顯示的程式碼。

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

在程式碼後置檔案中，您會將日期選擇器的最小和最大日期。 藉由設定這些值，您將會防止使用者瀏覽至無效的日期。 您將擷取的最小和最大值**RangeAttribute**上所提供的日期時間屬性。 開啟**DateTime\_Edit.ascx.cs**，並將下列反白顯示的程式碼加入至頁面\_載入方法。

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

執行 web 應用程式，並瀏覽至 AddStudent 頁面。 提供的欄位值，請注意，當您按一下文字方塊上註冊日期時，會顯示行事曆。

![日期選擇器](integrating-jquery-ui/_static/image4.png)

選取一個日期，然後按一下**插入**。 RangeAttribute 會強制執行伺服器上的驗證。 藉由設定 minDate 屬性上的日期選擇器，您也適用於驗證用戶端上。 行事曆不讓使用者瀏覽至之前的 minDate 值的日期。

當您編輯資料格檢視中的記錄時，也會顯示行事曆。

![在 GridView 中的日期選擇器](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>結論

在本教學課程中，您將學會如何使用模型繫結的 web 表單中併入 JQuery widget。

在接下來[教學課程](using-query-string-values-to-retrieve-data.md)，選取資料時，您將使用的查詢字串值。

> [!div class="step-by-step"]
> [上一頁](sorting-paging-and-filtering-data.md)
> [下一頁](using-query-string-values-to-retrieve-data.md)
