---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "組件 8： 最後的頁面、 例外狀況處理，以及結論 |Microsoft 文件"
author: JoeStagner
description: "此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 8 部將加入連絡人的頁面，頁面上，以及例外狀況的相關..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: fce1a20f9d1093b6c60542d8a786ddf54fdc922c
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>組件 8： 最後的頁面、 例外狀況處理，以及結束時
====================
由[Joe stagner 以](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。 它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。
> 
> 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 8 部將加入連絡人的頁面，頁面上和例外狀況處理的相關。 這是數列的結論。


## <a id="_Toc260221680"></a>請連絡 頁面 （從 ASP.NET 傳送電子郵件）

建立名為 ContactUs.aspx 的新頁面

使用設計工具，建立採用特殊附註，以包含 ToolkitScriptManager 和 AjaxdControlToolkit 編輯器控制項的格式如下。 。

![](tailspin-spyworks-part-8/_static/image1.jpg)

按兩下"Submit"按鈕，以產生 click 事件處理常式程式碼後置檔案，並實作一個方法來以電子郵件傳送的連絡資訊。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

此程式碼需要您的 web.config 檔案包含指定要用來傳送郵件的 SMTP 伺服器的組態區段中的項目。

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>有關頁面

建立名為 AboutUs.aspx 的頁面，並新增任何您喜歡的內容。

## <a id="_Toc260221682"></a>全域例外狀況處理常式

最後，我們有整個應用程式擲回例外狀況，並發生未預期的情況下，冷也有我們的 web 應用程式中造成未處理的例外狀況。

我們不想要顯示網站訪客處理的例外狀況。

![](tailspin-spyworks-part-8/_static/image2.jpg)

除了正在可怕的使用者體驗未處理例外狀況也可以是安全性問題。

若要解決此問題，我們會實作全域例外狀況處理常式。

若要這樣做，請開啟 Global.asax 檔案並請注意下列預先產生的事件處理常式。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

加入程式碼來實作應用程式\_錯誤處理常式，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

然後加入至方案名為 Error.aspx 的頁面，並加入此標記程式碼片段。

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

此時網頁\_載入錯誤訊息的事件處理常式擷取要求的物件。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>結論

我們已看到，ASP.NET WebForms 輕鬆建立複雜的網站與資料庫存取權，成員資格、 AJAX 等。 很快。

希望本教學課程已提供您要開始建置您自己的 ASP.NET WebForms 應用程式所需的工具 ！

>[!div class="step-by-step"]
[上一步](tailspin-spyworks-part-7.md)
