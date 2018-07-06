---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 第 6 節： ASP.NET 成員資格 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 6 部分加入 ASP.NET 成員資格。
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 303c1edf548db1da9ef61d94bdd8157d6afbb2e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804416"
---
<a name="part-6-aspnet-membership"></a>第 6 節： ASP.NET 成員資格
====================
藉由[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。 它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。
> 
> 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 6 部分加入 ASP.NET 成員資格。


## <a id="_Toc260221672"></a>  使用 ASP.NET 成員資格

![](tailspin-spyworks-part-6/_static/image1.png)

按一下 [安全性]

![](tailspin-spyworks-part-6/_static/image1.jpg)

請確定我們使用表單驗證。

![](tailspin-spyworks-part-6/_static/image2.jpg)

您可以使用 [建立使用者] 連結來建立幾個使用者。

![](tailspin-spyworks-part-6/_static/image3.jpg)

完成時，請參閱 [方案總管] 視窗，並重新整理檢視。

![](tailspin-spyworks-part-6/_static/image2.png)

請注意，ASPNETDB。已建立好的 MDF。 此檔案包含的資料表，以支援核心 ASP.NET 服務，例如成員資格。

現在我們可以開始實作結帳程序。

藉由建立 CheckOut.aspx 頁面開始。

CheckOut.aspx 頁面應僅供已登入，因此我們將會限制存取權登入使用者以及未登入的登入頁面的重新導向使用者的使用者。

若要這樣做我們會將下列加入我們的 web.config 檔案的組態區段。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

ASP.NET Web Form 應用程式範本會自動加入我們的 web.config 檔案中的 [驗證] 區段，並建立預設登入頁面。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

我們必須修改 Login.aspx 背後的程式碼移轉匿名的購物車，當使用者登入的檔案。 變更頁面\_載入事件，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

然後新增"LoggedIn"事件處理常式如下設定新登入的使用者工作階段名稱，並變更使用者的購物車中的暫時工作階段識別碼，我們 MyShoppingCart 類別中呼叫 MigrateCart; 方法。 （在.cs 檔案中實作）

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

實作 MigrateCart() 方法如下所示。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

在 checkout.aspx 我們將使用 EntityDataSource 和 GridView 中我們查看頁面就像我們在我們購物車 頁面。

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

請注意，我們的 GridView 控制項指定名為 MyList"ondatabound"事件處理常式\_RowDataBound 現在讓我們來實作該事件處理常式，就像這樣。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

加總的購物車每個資料列繫結，並更新 GridView 的底端列這個方法會保留。

在這個階段中，我們已實作放置順序 「 檢閱 」 的簡報。

讓我們加入我們的頁面中的幾行程式碼處理空白的購物車案例\_Load 事件：

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

當使用者按一下 [提交] 按鈕時我們會執行下列程式碼中的提交按鈕 Click 事件處理常式。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

「"訂單提交程序是在 SubmitOrder() 方法中，我們 MyShoppingCart 類別的實作。

SubmitOrder 將會：

- 購物車中採取所有明細項目，並使用它們來建立新的訂單記錄和相關聯的 OrderDetails 記錄。
- 計算出貨日期。
- 清除 購物車。


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

基於此範例應用程式的中，我們會計算出貨日期只要目前的日期加上兩天。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

執行應用程式現在將允許我們購物程序從開始到完成的測試。

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-5.md)
> [下一頁](tailspin-spyworks-part-7.md)
