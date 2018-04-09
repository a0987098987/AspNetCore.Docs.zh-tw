---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 第 6 單元： ASP.NET 成員資格 |Microsoft 文件
author: JoeStagner
description: 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 6 單元新增 ASP.NET 成員資格。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-6-aspnet-membership"></a>第 6 單元： ASP.NET 成員資格
====================
由[Joe stagner 以](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。 它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。
> 
> 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 6 單元新增 ASP.NET 成員資格。


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

請注意，ASPNETDB。已建立 MDF 沒問題。 此檔案包含支援核心 ASP.NET 服務，例如成員資格的資料表。

現在我們可以開始實作在結帳程序。

開始建立 CheckOut.aspx 頁面。

只能為 CheckOut.aspx 頁面可讓我們將會限制存取權登入使用者而不會記錄在登入頁面重新導向使用者登入使用者。

若要這樣做我們會將下列新增到我們的 web.config 檔案的組態區段。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

ASP.NET Web Form 應用程式範本會自動加入我們的 web.config 檔案中的驗證 區段，並建立預設登入頁面。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

我們必須修改 Login.aspx 背後的程式碼來移轉匿名的購物車，當使用者登入的檔案。 變更頁面\_載入事件，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

然後加入像這樣將工作階段名稱設定為新登入的使用者，並變更使用者的購物車中的暫存工作階段識別碼，我們 MyShoppingCart 類別中呼叫 MigrateCart 方法的"LoggedIn"事件處理常式。 （在.cs 檔案中實作）

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

實作 MigrateCart() 方法以這種方式。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

在 checkout.aspx 我們將使用 EntityDataSource 和 GridView 我們簽出 頁面中一樣我們在我們的購物車網頁。

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

請注意，我們的 GridView 控制項指定名為 MyList"ondatabound"事件處理常式\_RowDataBound 現在讓我們來實作該事件處理常式，像這樣。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

累計總數的購物車為每個資料列繫結，而底部資料列的 GridView 會更新這個方法會保留。

在這個階段中，我們已經實作 「 評論 」 簡報的放置順序。

讓我們將幾行程式碼加入至我們的頁面處理空白的購物車案例\_載入事件：

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

當使用者在"Submit"按鈕我們會送出按鈕 Click 事件處理常式中執行下列程式碼。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

"肉"訂單送出程序是在 SubmitOrder() 我們 MyShoppingCart 類別方法中實作。

SubmitOrder 將會：

- 採取購物車中的所有明細項目，並使用它們來建立新的訂單記錄和相關的訂單明細記錄。
- 計算出貨日期。
- 清除購物車。


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

基於此範例應用程式的我們會計算出貨日期，只要新增到目前日期的兩天。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

執行應用程式現在將會允許我們購物程序從開始到完成的測試。

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-5.md)
> [下一頁](tailspin-spyworks-part-7.md)
